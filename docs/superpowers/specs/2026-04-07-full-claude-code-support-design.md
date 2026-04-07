# Design: Full Claude Code Support for Living Context Plugin

**Date:** 2026-04-07  
**Status:** Approved  

---

## Goal

Give the Living Context plugin first-class Claude Code support: slash commands the user can discover and invoke explicitly, plus automatic skill triggering via natural language (expanded English trigger phrases).

Maintenance automation (updating the vault after code changes) stays as CLAUDE.md rules in the target project — no hooks added to the plugin itself.

---

## How Claude Code Resolves Components

- **Commands**: auto-discovered from `commands/*.md` files — no manifest registration needed
- **Skills**: auto-discovered from `skills/*/SKILL.md` — no manifest registration needed
- **`plugin.json`**: metadata only (name, version, author, keywords) — not changed by this work

---

## File Structure

```
.claude-plugin/
  plugin.json                    # No changes — metadata only

commands/                        # New directory
  living-context.md              # /living-context command
  update-context.md              # /update-context command
  context-status.md              # /context-status command

skills/
  living-context/
    SKILL.md                     # Expand trigger phrases with English variants
    references/templates.md      # No changes

  update-context/
    SKILL.md                     # New skill — audit and update an existing vault

  context-status/
    SKILL.md                     # New skill — check what's outdated (read-only)

CLAUDE.md                        # Update: reflect 3 skills + commands directory
```

---

## Commands (`commands/*.md`)

Each command file is a thin wrapper that invokes the corresponding skill. The `description:` field is what the user sees in autocomplete.

**`commands/living-context.md`:**
```markdown
---
name: living-context
description: Create a living context vault for this project
---

Use the living-context skill to create a living documentation vault for the current project.
```

**`commands/update-context.md`:**
```markdown
---
name: update-context
description: Audit and update the living context vault with recent code changes
---

Use the update-context skill to audit the existing context vault and update it with any undocumented changes since the last changelog entry.
```

**`commands/context-status.md`:**
```markdown
---
name: context-status
description: Check what is outdated in the context vault (read-only)
---

Use the context-status skill to check the context vault for gaps — commits and changes that haven't been documented yet. Makes no modifications.
```

Commands give users discoverability (autocomplete, `/help`) and explicit invocation. The skill does the actual work.

---

## Skill: `living-context` (existing — expand triggers only)

**Change:** Expand the `description:` frontmatter field with additional English trigger phrases. No changes to the 5-phase workflow body.

**Phrases to add:**
- "set up project documentation"
- "create context vault"
- "initialize living docs"
- "document this project"
- "set up knowledge base"
- "create project docs"
- "set up living documentation"

**Disambiguation rule:** These phrases all imply *creating from scratch*. If a `context/` directory already exists, the skill should detect it and offer to run `update-context` instead.

---

## Skill: `update-context` (new)

**File:** `skills/update-context/SKILL.md`

**Trigger phrases:**
- "update context", "update the context", "update my context"
- "sync the vault", "sync context"
- "o vault está desatualizado", "atualize a documentação"
- "atualiza o context", "atualizar o vault"
- "context is outdated", "docs are outdated"

**Disambiguation:** These phrases imply *updating existing docs*. No overlap with `living-context` triggers (which imply creation). No overlap with `context-status` triggers (which imply read-only auditing).

**Behavior:**
1. Check that `context/HOME.md` exists — if not, abort with: *"No context vault found. Run `/living-context` first."*
2. Read all vault files to understand current documented state
3. Find the most recent date entry in `context/changelog.md`
4. Run `git log --since="<that date>" --name-only` to list commits and modified files since last documentation
5. For each modified file outside `context/`: determine if the change is significant (new feature, changed API, renamed module, schema change, deleted component — not style/typo fixes)
6. Update: `changelog.md` (new entries), relevant `evolution.md` (domain history), `features.md` (status), `decisions.md` (new ADR if architectural decision was made)
7. Report what was updated: list of files changed in vault + summary of commits that were documented

**What counts as "significant" (update-worthy):**
- New or deleted source files
- Changed function signatures or API contracts
- Schema/model changes
- New dependencies added
- Config changes that affect behavior
- NOT: formatting, comments, test fixtures, lockfile updates

**Template reference:** May reference `skills/living-context/references/templates.md` for ADR and changelog entry formats. The SKILL.md body should instruct Claude to read that file when creating new vault entries.

---

## Skill: `context-status` (new)

**File:** `skills/context-status/SKILL.md`

**Trigger phrases:**
- "context status", "vault status"
- "is the vault up to date", "is the context up to date"
- "o que está desatualizado", "checar documentação"
- "check context", "check vault"
- "what's missing from the context"

**Behavior:**
1. Check that `context/HOME.md` exists — if not, abort with: *"No context vault found. Run `/living-context` first."*
2. Read all vault files — note the most recent date in `context/changelog.md`
3. Run `git log --since="<that date>" --name-only` to identify undocumented commits
4. Apply the same "significant change" rule as `update-context` to filter noise
5. Output a **read-only** report — no file modifications:
   - Undocumented commits (hash + message + files)
   - Domains/files that likely need updating
   - Features that may have changed status
   - Suggested next action: run `/update-context` if gaps found, or confirm vault is current

**Disambiguation rule:** Any phrase that implies checking, auditing, or inspecting existing docs (rather than creating or updating) maps to `context-status` when `context/HOME.md` exists. If Claude is uncertain between `living-context` and `context-status`, the presence of `context/` is the tiebreaker — `context-status` wins.

**Makes zero file modifications.**

---

## CLAUDE.md Updates

Update the "Project Structure" section to reflect:
- `commands/` directory with 3 command files
- 3 skills instead of 1
- Remove the phrase "a single skill"

Update the "Key Conventions" section to add:
- Commands in `commands/*.md` are thin wrappers — logic lives in skills
- New skills (`update-context`, `context-status`) may reference `skills/living-context/references/templates.md` for format consistency

Update the "When Modifying the Skill" section to cover all 3 skills.

---

## Out of Scope

- No hooks (post-edit, post-commit) added to the plugin
- No changes to `references/templates.md`
- No changes to `prompt/living-context-prompt.md`
- No agent definitions
- No changes to `plugin.json`
