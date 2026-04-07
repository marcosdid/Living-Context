# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## What This Repo Is

A Claude Code plugin that ships three skills and three slash commands for creating and maintaining living documentation vaults. There is no build step, no test runner, and no compiled output — the deliverable is the skill files themselves.

## Project Structure

```
.claude-plugin/plugin.json         # Plugin manifest (name, version, author, repo)
commands/
  living-context.md                # /living-context slash command
  update-context.md                # /update-context slash command
  context-status.md                # /context-status slash command
skills/living-context/
  SKILL.md                         # Create vault (5-phase workflow)
  references/templates.md          # File templates referenced by SKILL.md (Phase 3)
skills/update-context/
  SKILL.md                         # Audit and update an existing vault
skills/context-status/
  SKILL.md                         # Read-only vault gap report
prompt/living-context-prompt.md    # Standalone reusable prompt for non-Claude-Code AI tools
README.md                          # Installation, usage, and tool compatibility matrix
```

## How the Plugin Works

1. **`plugin.json`** registers the plugin with the Claude Code harness.
2. **`SKILL.md`** is the skill body — it is loaded verbatim when a user triggers `living-context`. The frontmatter `description:` field controls when Claude auto-invokes it.
3. **`commands/*.md`** registers three slash commands (`/living-context`, `/update-context`, `/context-status`). Each is a thin wrapper that delegates to the corresponding skill. Commands give users discoverability via autocomplete and `/help`.
4. **`references/templates.md`** is a supporting reference that `SKILL.md` explicitly tells Claude to read during Phase 3 (Write the Content). It must stay in sync with `SKILL.md`.
5. **`skills/update-context/SKILL.md`** and **`skills/context-status/SKILL.md`** are the two additional skills. `update-context` writes; `context-status` is read-only. Both may reference `skills/living-context/references/templates.md` for format consistency.
6. **`prompt/living-context-prompt.md`** is an independent artifact for users not on Claude Code — it duplicates some logic from `SKILL.md` intentionally.

## Key Conventions

- **Language**: README and prompt are in Portuguese (pt-BR). `SKILL.md` and `references/templates.md` are in English. Keep this separation.
- **Skill trigger phrases** are defined in `SKILL.md`'s `description:` frontmatter field. If you add trigger phrases, update them there.
- **Templates in `references/templates.md`** must remain consistent with the structure described in `SKILL.md` Phases 2–3. If you change the vault structure in one, update the other.
- **The CLAUDE.md maintenance section template** exists in two places: `SKILL.md` (Phase 4) and `references/templates.md` (last section). Keep them in sync.
- **Command files** in `commands/*.md` are thin wrappers — all logic lives in the skill. Do not add workflow logic to command files.
- **Skills `update-context` and `context-status`** share the same "significant change" definition and may cross-reference `skills/living-context/references/templates.md` for entry formats.

## Installing / Testing the Plugin Locally

```bash
# Install for local testing in Claude Code
/plugin install ./path/to/this-repo

# Trigger the skill manually
crie um living context para este projeto
```

There is no automated test suite. Validation is manual: install the plugin, trigger the skill on a sample project, and verify the generated `context/` vault matches the structure in `SKILL.md`.

## When Modifying the Skill

- **Adding a new vault file type**: add the template to `references/templates.md` AND document when to create it in `SKILL.md` Phase 2.
- **Changing maintenance rules**: update Phase 4 in `SKILL.md` AND the CLAUDE.md template in `references/templates.md`.
- **Adding trigger phrases**: edit the `description:` block in `SKILL.md` frontmatter.
- **Bumping version**: update `version` in `.claude-plugin/plugin.json`.
- **Changing the "significant change" definition**: update it in both `skills/update-context/SKILL.md` (Step 3) and `skills/context-status/SKILL.md` (Step 3).
- **Adding a slash command**: create a `.md` file in `commands/` and a corresponding skill in `skills/`.
