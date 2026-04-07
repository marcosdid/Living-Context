# Full Claude Code Support — Implementation Plan

> **For agentic workers:** REQUIRED SUB-SKILL: Use superpowers:subagent-driven-development (recommended) or superpowers:executing-plans to implement this plan task-by-task. Steps use checkbox (`- [ ]`) syntax for tracking.

**Goal:** Give the Living Context plugin first-class Claude Code support — three slash commands, two new skills, expanded triggers, stronger vault-as-second-brain language, and updated CLAUDE.md.

**Architecture:** Commands (`commands/*.md`) are thin wrappers discovered by Claude Code automatically. Skills (`skills/*/SKILL.md`) contain all logic and are auto-triggered by their `description:` field. No changes to `plugin.json` or the 5-phase workflow body of the existing skill. `references/templates.md` receives one update (Task 4) to stay in sync with the SKILL.md Phase 4 template — required by the CLAUDE.md convention.

**Tech Stack:** Markdown only — no build step, no dependencies.

**Spec:** `docs/superpowers/specs/2026-04-07-full-claude-code-support-design.md`

---

## File Map

| Action | File | Responsibility |
|--------|------|----------------|
| Create | `commands/living-context.md` | Slash command `/living-context` |
| Create | `commands/update-context.md` | Slash command `/update-context` |
| Create | `commands/context-status.md` | Slash command `/context-status` |
| Modify | `skills/living-context/SKILL.md` | Expand trigger phrases + second-brain language in Phase 4 |
| Create | `skills/update-context/SKILL.md` | New skill: audit and update existing vault |
| Create | `skills/context-status/SKILL.md` | New skill: read-only vault gap report |
| Modify | `prompt/living-context-prompt.md` | Second-brain framing in Seção A and Seção B |
| Modify | `CLAUDE.md` | Reflect 3 skills + commands/ directory |

---

## Task 1: Create the `commands/` directory and three command files

**Files:**
- Create: `commands/living-context.md`
- Create: `commands/update-context.md`
- Create: `commands/context-status.md`

- [ ] **Step 1: Create `commands/living-context.md`**

```markdown
---
name: living-context
description: Create a living context vault for this project
---

Use the living-context skill to create a living documentation vault for the current project.
```

- [ ] **Step 2: Create `commands/update-context.md`**

```markdown
---
name: update-context
description: Audit and update the living context vault with recent code changes
---

Use the update-context skill to audit the existing context vault and update it with any undocumented changes since the last changelog entry.
```

- [ ] **Step 3: Create `commands/context-status.md`**

```markdown
---
name: context-status
description: Check what is outdated in the context vault (read-only)
---

Use the context-status skill to check the context vault for gaps — commits and changes that haven't been documented yet. Makes no modifications.
```

- [ ] **Step 4: Verify the three files exist**

Run: `ls commands/`
Expected output:
```
context-status.md
living-context.md
update-context.md
```

- [ ] **Step 5: Commit**

```bash
git add commands/
git commit -m "feat: add slash commands for living-context, update-context, context-status"
```

---

## Task 2: Expand trigger phrases in `skills/living-context/SKILL.md`

**Files:**
- Modify: `skills/living-context/SKILL.md` (frontmatter `description:` block, lines 1–11)

- [ ] **Step 1: Replace the existing `description:` frontmatter block**

Current block (lines 1–11):
```markdown
---
name: living-context
description: >
  Create and maintain a living context vault for any project — an evolving documentation system
  that stays synchronized with code changes. Use this skill when the user asks to set up project
  context, create documentation structure, initialize a context vault, or mentions "living context",
  "context vault", "project documentation setup", "context-v2", or "living docs". Also trigger when
  the user wants to organize project knowledge, set up an Obsidian vault for a project, or create
  a self-maintaining documentation system. Works for ANY project type — backend, frontend, fullstack,
  data science, mobile, infrastructure, monorepo, or any other.
---
```

Replace with:
```markdown
---
name: living-context
description: >
  Create and maintain a living context vault for any project — an evolving documentation system
  that stays synchronized with code changes. Use this skill when the user asks to set up project
  context, create documentation structure, initialize a context vault, or mentions "living context",
  "context vault", "project documentation setup", "context-v2", or "living docs". Also trigger when
  the user wants to organize project knowledge, set up an Obsidian vault for a project, or create
  a self-maintaining documentation system. Also trigger for: "set up project documentation",
  "create context vault", "initialize living docs", "document this project", "set up knowledge base",
  "create project docs", "set up living documentation". Works for ANY project type — backend,
  frontend, fullstack, data science, mobile, infrastructure, monorepo, or any other.
  NOTE: If a `context/` directory already exists in the project, do NOT run this skill —
  instead, suggest the user run /update-context or /context-status.
---
```

- [ ] **Step 2: Commit**

```bash
git add skills/living-context/SKILL.md
git commit -m "feat: expand living-context trigger phrases with English variants"
```

---

## Task 3: Replace Phase 4 maintenance template in `skills/living-context/SKILL.md` with second-brain language

**Files:**
- Modify: `skills/living-context/SKILL.md` (Phase 4 section, lines 144–171)

- [ ] **Step 1: Replace the Phase 4 block**

Find and replace this block (starting at `### Phase 4: Configure CLAUDE.md Maintenance Rules`):

Current text:
```markdown
### Phase 4: Configure CLAUDE.md Maintenance Rules

This is the **most critical step** — without maintenance rules, the vault dies.

Add the following section to the project's CLAUDE.md (create it if it doesn't exist). Adapt the paths and domain names to match the vault structure you created:

```markdown
## Living Context Maintenance

After ANY significant code modification, you MUST:

1. Update `context/changelog.md` with:
   - Date: `[YYYY-MM-DD]`
   - Type: Feature | Bugfix | Refactor | UI/UX | Infrastructure | Documentation
   - Affected files
   - What changed and why

2. Update the relevant domain evolution file (e.g., `context/01-backend/evolution.md`)

3. If an architectural decision was made, add a new ADR to the relevant `decisions.md`

4. If data models changed, update the data models file

5. If a feature was added or completed, update the features tracking file

The `context/` vault is alive — create, rename, split, or remove files as needed.
Use Obsidian format: YAML frontmatter, wikilinks `[[]]`, callouts `> [!type]`.
```
```

Replace with:
```markdown
### Phase 4: Configure CLAUDE.md Maintenance Rules

This is the **most critical step** — without maintenance rules, the vault dies.

Add the following section to the project's CLAUDE.md (create it if it doesn't exist). Adapt the paths and domain names to match the vault structure you created:

```markdown
## Living Context — Vault as Second Brain

The `context/` vault is your **second brain** for this project. It is the authoritative source
of knowledge about architecture, decisions, history, and current state. You — the AI — know less
about this project without it. Future sessions and future developers depend on it being accurate.

> [!important] Two hard rules — no exceptions
>
> **RULE 1 — Read before working:** Before starting any non-trivial task, read `context/HOME.md`.
> If working in a specific domain, also read that domain's `estrutura.md` and `evolucao.md`.
> Do this even if you think you already know the project. The vault may have changed.
>
> **RULE 2 — Write after changing:** After any significant code change, update the vault.
> This is not optional. Skipping this degrades the vault for every session that comes after.

### What counts as a significant change (triggers RULE 2)

Update the vault after: new or deleted source files, changed function signatures or API contracts,
schema/model changes, new dependencies, config changes that affect behavior.

Skip for: formatting fixes, comment edits, test fixture changes, lockfile updates.

### Update checklist (RULE 2 procedure)

1. Update `context/changelog.md`:
   - Date: `[YYYY-MM-DD]`
   - Type: Feature | Bugfix | Refactor | UI/UX | Infrastructure | Documentation
   - Affected files
   - What changed and why

2. Update the relevant domain evolution file (e.g., `context/01-backend/evolution.md`)

3. If an architectural decision was made, add a new ADR to the relevant `decisions.md`

4. If data models changed, update the data models file

5. If a feature was added or completed, update the features tracking file

The vault is alive — create, rename, split, or remove files as needed.
Use Obsidian format: YAML frontmatter, wikilinks `[[]]`, callouts `> [!type]`.
```
```

- [ ] **Step 2: Commit**

```bash
git add skills/living-context/SKILL.md
git commit -m "feat: replace Phase 4 template with vault-as-second-brain hard rules"
```

---

## Task 4: Update CLAUDE.md maintenance template in `references/templates.md`

**Files:**
- Modify: `skills/living-context/references/templates.md` (last section: "CLAUDE.md Maintenance Section Template")

This template must stay in sync with the Phase 4 template in SKILL.md (per CLAUDE.md convention).

- [ ] **Step 1: Replace the "CLAUDE.md Maintenance Section Template" block**

Find the section starting at `## CLAUDE.md Maintenance Section Template` (line 329 of `references/templates.md`) and replace the markdown code block inside it with the new second-brain template below.

The surrounding header and instruction line stay the same. Only the inner markdown block changes.

Replace the inner block with:

````markdown
## CLAUDE.md Maintenance Section Template

Add this to the project root CLAUDE.md:

```markdown
## Living Context — Vault as Second Brain

The `context/` vault is your **second brain** for this project. It is the authoritative source
of knowledge about architecture, decisions, history, and current state. You — the AI — know less
about this project without it. Future sessions and future developers depend on it being accurate.

> [!important] Two hard rules — no exceptions
>
> **RULE 1 — Read before working:** Before starting any non-trivial task, read `context/HOME.md`.
> If working in a specific domain, also read that domain's `estrutura.md` and `evolucao.md`.
> Do this even if you think you already know the project. The vault may have changed.
>
> **RULE 2 — Write after changing:** After any significant code change, update the vault.
> This is not optional. Skipping this degrades the vault for every session that comes after.

### What counts as a significant change (triggers RULE 2)

Update the vault after: new or deleted source files, changed function signatures or API contracts,
schema/model changes, new dependencies, config changes that affect behavior.

Skip for: formatting fixes, comment edits, test fixture changes, lockfile updates.

### Update checklist (RULE 2 procedure)

1. Update `context/changelog.md`:
   - Date: `[YYYY-MM-DD]`
   - Type: Feature | Bugfix | Refactor | UI/UX | Infrastructure | Documentation
   - Affected files
   - What changed and why

2. Update the relevant domain evolution file:
   {list each domain folder and when to update it}

3. If an architectural decision was made, add a new ADR to the relevant `decisions.md`

4. If data models/schemas changed, update `context/{path}/data-models.md`

5. If a feature was added or completed, update `context/{path}/features.md`

The vault is alive — create, rename, split, or remove files as needed.
Use Obsidian format: YAML frontmatter, wikilinks `[[]]`, callouts `> [!type]`.
```
````

- [ ] **Step 2: Commit**

```bash
git add skills/living-context/references/templates.md
git commit -m "chore: sync CLAUDE.md template in references/templates.md with SKILL.md Phase 4"
```

---

## Task 5: Create `skills/update-context/SKILL.md`

**Files:**
- Create: `skills/update-context/SKILL.md`

- [ ] **Step 1: Create the file**

```markdown
---
name: update-context
description: >
  Audit and update an existing living context vault with undocumented changes.
  Use this skill when the user asks to update the context, sync the vault, or says the context
  is outdated. Trigger phrases: "update context", "update the context", "update my context",
  "sync the vault", "sync context", "context is outdated", "docs are outdated",
  "o vault está desatualizado", "atualize a documentação", "atualiza o context", "atualizar o vault".
  Only trigger when a `context/` directory already exists in the project.
---

# Update Context — Audit and Sync the Living Vault

Use this skill to bring an existing living context vault up to date with recent code changes.

## Prerequisites

Before running: verify `context/HOME.md` exists. If it does not, stop and tell the user:
> "No context vault found in this project. Run `/living-context` first to create one."

## Step 1: Orient yourself

Read `context/HOME.md` and all vault files. Note the domains, current state, and the most recent
date entry in `context/changelog.md`. This date is your baseline.

## Step 2: Find undocumented changes

Run the following to see all commits and modified files since the last changelog entry:

```bash
git log --since="YYYY-MM-DD" --name-only --pretty=format:"## %h %s (%ad)" --date=short
```

Replace `YYYY-MM-DD` with the most recent date from `context/changelog.md`.

## Step 3: Identify significant changes

For each modified file outside `context/`, decide if the change is significant:

**Update the vault for:**
- New or deleted source files
- Changed function signatures or API contracts
- Schema or data model changes
- New dependencies added to package files
- Config changes that affect runtime behavior

**Skip:**
- Formatting, whitespace, comments
- Test fixture changes
- Lockfile updates (package-lock.json, yarn.lock, etc.)

## Step 4: Update the vault

For each significant change found, update the relevant vault files.

Read `skills/living-context/references/templates.md` for the correct format of each file type
before writing new entries.

Files to update as applicable:
- `context/changelog.md` — new entry per significant change (date, type, files, what changed)
- `context/{domain}/evolucao.md` — domain-specific history entry
- `context/{domain}/decisoes.md` — new ADR if an architectural decision was made
- `context/{domain}/data-models.md` — if schema changed
- `context/{path}/features.md` — if a feature was added or completed

The vault is alive — create new domain files or folders if the project has grown into new areas.

## Step 5: Report

After updating, tell the user:
- How many commits were reviewed
- Which vault files were updated and what was added
- Any significant changes you found but couldn't fully document (ask for clarification if needed)
```

- [ ] **Step 2: Verify the file exists**

Run: `ls skills/update-context/`
Expected: `SKILL.md`

- [ ] **Step 3: Commit**

```bash
git add skills/update-context/
git commit -m "feat: add update-context skill for auditing and syncing existing vaults"
```

---

## Task 6: Create `skills/context-status/SKILL.md`

**Files:**
- Create: `skills/context-status/SKILL.md`

- [ ] **Step 1: Create the file**

```markdown
---
name: context-status
description: >
  Check what is outdated in the living context vault — read-only, no modifications.
  Use this skill when the user asks to check or inspect the vault, asks what is missing,
  or wants to know if the context is up to date. Trigger phrases: "context status", "vault status",
  "is the vault up to date", "is the context up to date", "o que está desatualizado",
  "checar documentação", "check context", "check vault", "what's missing from the context".
  Disambiguation: if uncertain between this skill and living-context, and `context/` exists,
  this skill wins. This skill never creates or modifies files.
---

# Context Status — Vault Gap Report (Read-Only)

Use this skill to audit the living context vault and report what is missing or outdated.
**This skill makes zero file modifications.**

## Prerequisites

Before running: verify `context/HOME.md` exists. If it does not, stop and tell the user:
> "No context vault found in this project. Run `/living-context` first to create one."

## Step 1: Orient yourself

Read `context/HOME.md` and all vault files. Note the most recent date entry in
`context/changelog.md`. This date is your baseline.

## Step 2: Find commits since last documentation

Run:

```bash
git log --since="YYYY-MM-DD" --name-only --pretty=format:"## %h %s (%ad)" --date=short
```

Replace `YYYY-MM-DD` with the most recent date from `context/changelog.md`.

## Step 3: Apply the significance filter

For each commit and modified file, apply the same rule as update-context:

**Significant (would need documentation):**
- New or deleted source files
- Changed function signatures or API contracts
- Schema or data model changes
- New dependencies
- Config changes that affect runtime behavior

**Not significant (skip):**
- Formatting, comments, test fixtures, lockfiles

## Step 4: Produce the gap report

Output a structured read-only report. Do not modify any files.

Format:

```
## Context Vault Status Report — [today's date]

### Baseline
Last documented: [date from changelog]
Commits reviewed: [N commits since that date]

### Undocumented Changes
[For each significant commit:]
- `[hash]` [commit message] ([date])
  - Files: [list of significant files changed]
  - Likely affects: [domain folder(s) in vault]

### Vault Files That Likely Need Updating
- [ ] `context/changelog.md` — [N] undocumented entries
- [ ] `context/[domain]/evolucao.md` — [reason]
- [ ] `context/[domain]/features.md` — [reason]
(list only files that need updates)

### Verdict
[One of:]
- ✅ Vault is current — no significant undocumented changes found.
- ⚠️ Vault has gaps — run `/update-context` to sync.

### Suggested next step
[If gaps: "Run `/update-context` to document the changes above."]
[If current: "No action needed."]
```
```

- [ ] **Step 2: Verify the file exists**

Run: `ls skills/context-status/`
Expected: `SKILL.md`

- [ ] **Step 3: Commit**

```bash
git add skills/context-status/
git commit -m "feat: add context-status skill for read-only vault gap reporting"
```

---

## Task 7: Update `prompt/living-context-prompt.md` with second-brain language

**Files:**
- Modify: `prompt/living-context-prompt.md`

Two edits in this file:

### Edit A — Seção A, section "3. Regras de manutenção"

- [ ] **Step 1: Replace the intro of section "3. Regras de manutenção no CLAUDE.md"**

Find the block starting at `### 3. Regras de manutenção no CLAUDE.md` inside the Seção A code block.

Current text of the injected CLAUDE.md block:
```
## Manutenção do Context Vault

Após QUALQUER modificação significativa de código, OBRIGATORIAMENTE:
```

Replace with:
```
## Living Context — Vault como Segundo Cérebro

O vault `context/` é o seu **segundo cérebro** neste projeto. É a fonte autoritativa de
conhecimento sobre arquitetura, decisões, histórico e estado atual. Você — a IA — sabe menos
sobre este projeto sem ele. Sessões futuras e próximos desenvolvedores dependem de que ele
esteja preciso.

> **REGRA 1 — Leia antes de trabalhar:** Antes de iniciar qualquer tarefa não-trivial, leia
> `context/HOME.md`. Se for trabalhar em um domínio específico, leia também o `estrutura.md`
> e `evolucao.md` daquele domínio. Faça isso mesmo que ache que já conhece o projeto.
>
> **REGRA 2 — Escreva após modificar:** Após qualquer mudança significativa de código, atualize
> o vault. Isso não é opcional. Pular esta etapa degrada o vault para todas as sessões futuras.

### Checklist de atualização (procedimento da REGRA 2)

Após QUALQUER modificação significativa de código, OBRIGATORIAMENTE:
```

### Edit B — Seção B (maintenance reminder block)

- [ ] **Step 2: Replace the content of the Seção B code block**

Find the Seção B code block (lines ~144–157). Current content:
```
IMPORTANTE: Este projeto possui um Living Context Vault na pasta `context/`.

Antes de começar, leia `context/HOME.md` para entender o projeto.

Após QUALQUER modificação significativa de código, você DEVE:
1. Atualizar `context/changelog.md` (data, tipo, arquivos, o que mudou)
2. Atualizar o `evolucao.md` do domínio afetado
3. Adicionar ADR em `decisoes.md` se decisão arquitetural foi tomada
4. Atualizar `modelos-dados.md` se schema mudou
5. Atualizar `features.md` se feature foi adicionada/completada

O vault é vivo — crie novos arquivos se necessário. Use formato Obsidian (frontmatter, wikilinks, callouts).
```

Replace with:
```
CONTEXTO: Este projeto possui um Living Context Vault na pasta `context/`.

O vault é o seu SEGUNDO CÉREBRO para este projeto — a fonte autoritativa de conhecimento sobre
arquitetura, decisões e histórico. Você sabe menos sobre este projeto sem ele.

**REGRA 1 — Leia antes de trabalhar:**
Antes de iniciar qualquer tarefa não-trivial, leia `context/HOME.md`.
Se for trabalhar em um domínio específico, leia também o `estrutura.md` e `evolucao.md` dele.

**REGRA 2 — Escreva após modificar (obrigatório, não opcional):**
Após qualquer mudança significativa de código, atualize o vault:
1. Atualizar `context/changelog.md` (data, tipo, arquivos, o que mudou)
2. Atualizar o `evolucao.md` do domínio afetado
3. Adicionar ADR em `decisoes.md` se decisão arquitetural foi tomada
4. Atualizar `modelos-dados.md` se schema mudou
5. Atualizar `features.md` se feature foi adicionada/completada

Pular a REGRA 2 degrada o vault para todas as sessões futuras.
O vault é vivo — crie novos arquivos se necessário. Use formato Obsidian (frontmatter, wikilinks, callouts).
```

- [ ] **Step 3: Commit**

```bash
git add prompt/living-context-prompt.md
git commit -m "feat: add vault-as-second-brain framing to prompt Seção A and Seção B"
```

---

## Task 8: Update `CLAUDE.md` to reflect new structure

**Files:**
- Modify: `CLAUDE.md`

- [ ] **Step 1: Update "What This Repo Is" section**

Find exact text:
```
A Claude Code plugin that ships a single skill (`living-context`) for creating and maintaining living documentation vaults.
```

Replace with:
```
A Claude Code plugin that ships three skills and three slash commands for creating and maintaining living documentation vaults.
```

- [ ] **Step 2: Replace "Project Structure" code block**

Find the exact fenced block (between the two ` ``` ` fences after `## Project Structure`):
```
.claude-plugin/plugin.json         # Plugin manifest (name, version, author, repo)
skills/living-context/
  SKILL.md                         # The skill Claude Code loads and executes
  references/templates.md          # File templates referenced by SKILL.md (Phase 3)
prompt/living-context-prompt.md    # Standalone reusable prompt for non-Claude-Code AI tools
README.md                          # Installation, usage, and tool compatibility matrix
```

Replace with:
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

- [ ] **Step 3: Update "How the Plugin Works" section**

Find exact text:
```
2. **`SKILL.md`** is the skill body — it is loaded verbatim when a user triggers `living-context`. The frontmatter `description:` field controls when Claude auto-invokes it.
3. **`references/templates.md`** is a supporting reference that `SKILL.md` explicitly tells Claude to read during Phase 3 (Write the Content). It must stay in sync with `SKILL.md`.
4. **`prompt/living-context-prompt.md`** is an independent artifact for users not on Claude Code — it duplicates some logic from `SKILL.md` intentionally.
```

Replace with:
```
2. **`SKILL.md`** is the skill body — it is loaded verbatim when a user triggers `living-context`. The frontmatter `description:` field controls when Claude auto-invokes it.
3. **`commands/*.md`** registers three slash commands (`/living-context`, `/update-context`, `/context-status`). Each is a thin wrapper that delegates to the corresponding skill. Commands give users discoverability via autocomplete and `/help`.
4. **`references/templates.md`** is a supporting reference that `SKILL.md` explicitly tells Claude to read during Phase 3 (Write the Content). It must stay in sync with `SKILL.md`.
5. **`skills/update-context/SKILL.md`** and **`skills/context-status/SKILL.md`** are the two additional skills. `update-context` writes; `context-status` is read-only. Both may reference `skills/living-context/references/templates.md` for format consistency.
6. **`prompt/living-context-prompt.md`** is an independent artifact for users not on Claude Code — it duplicates some logic from `SKILL.md` intentionally.
```

- [ ] **Step 4: Update "Key Conventions" section**

Find exact text:
```
- **The CLAUDE.md maintenance section template** exists in two places: `SKILL.md` (Phase 4) and `references/templates.md` (last section). Keep them in sync.
```

Replace with:
```
- **The CLAUDE.md maintenance section template** exists in two places: `SKILL.md` (Phase 4) and `references/templates.md` (last section). Keep them in sync.
- **Command files** in `commands/*.md` are thin wrappers — all logic lives in the skill. Do not add workflow logic to command files.
- **Skills `update-context` and `context-status`** share the same "significant change" definition and may cross-reference `skills/living-context/references/templates.md` for entry formats.
```

- [ ] **Step 5: Update "When Modifying the Skill" section**

Find exact text:
```
- **Bumping version**: update `version` in `.claude-plugin/plugin.json`.
```

Replace with:
```
- **Bumping version**: update `version` in `.claude-plugin/plugin.json`.
- **Changing the "significant change" definition**: update it in both `skills/update-context/SKILL.md` (Step 3) and `skills/context-status/SKILL.md` (Step 3).
- **Adding a slash command**: create a `.md` file in `commands/` and a corresponding skill in `skills/`.
```

- [ ] **Step 6: Commit**

```bash
git add CLAUDE.md
git commit -m "docs: update CLAUDE.md to reflect 3 skills and commands/ directory"
```

---

## Task 9: Final verification

- [ ] **Step 1: Confirm full file tree**

Run: `find . -not -path './.git/*' -not -path './docs/*' -name '*.md' | sort`

Expected output includes:
```
./CLAUDE.md
./README.md
./commands/context-status.md
./commands/living-context.md
./commands/update-context.md
./prompt/living-context-prompt.md
./skills/context-status/SKILL.md
./skills/living-context/SKILL.md
./skills/living-context/references/templates.md
./skills/update-context/SKILL.md
```

- [ ] **Step 2: Confirm `commands/` files have valid frontmatter**

Each file in `commands/` must have `name:` and `description:` fields in YAML frontmatter. Spot-check:

Run: `head -5 commands/living-context.md`
Expected: starts with `---`, contains `name: living-context`

- [ ] **Step 3: Confirm all three SKILL.md files have frontmatter**

Run: `head -3 skills/update-context/SKILL.md && head -3 skills/context-status/SKILL.md`
Expected: both start with `---`

- [ ] **Step 4: Confirm SKILL.md Phase 4 template contains "second brain" language**

Run: `grep -n "second brain\|RULE 1\|RULE 2" skills/living-context/SKILL.md`
Expected: at least 3 matches

- [ ] **Step 5: Confirm prompt has second-brain language**

Run: `grep -n "segundo cérebro\|REGRA 1\|REGRA 2" prompt/living-context-prompt.md`
Expected: at least 4 matches

- [ ] **Step 6: Confirm `references/templates.md` was synced**

Run: `grep -n "second brain\|RULE 1\|RULE 2" skills/living-context/references/templates.md`
Expected: at least 3 matches

- [ ] **Step 7: Final commit if any loose files**

```bash
git status
# If clean: done. If any unstaged: git add -A && git commit -m "chore: final cleanup"
```
