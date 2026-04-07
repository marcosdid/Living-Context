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

# Living Context — Self-Maintaining Project Knowledge Vault

A living context vault is a structured documentation system that evolves alongside your codebase. Unlike static docs that rot, this system enforces maintenance through CLAUDE.md rules — every significant code change triggers mandatory context updates.

The vault uses Obsidian-flavored Markdown (frontmatter, wikilinks `[[]]`, callouts `> [!type]`) but works with any Markdown reader.

## Core Principles

1. **The context is alive** — it's updated with every significant code change, not written once and forgotten
2. **The CLAUDE.md enforces maintenance** — AI assistants are instructed to update the vault after modifications
3. **Cross-referencing is key** — changelog, evolution, decisions, and features all link to each other
4. **Adapted to the project** — the structure reflects YOUR project's domains, not a rigid template

## How to Use This Skill

### Phase 1: Discover the Project

Before creating anything, understand the project:

1. **Read the codebase** — explore the directory structure, package files, configs
2. **Identify domains** — what are the main areas of the project? (e.g., API, UI, data pipeline, infra)
3. **Identify the stack** — what languages, frameworks, tools, and services are used?
4. **Identify existing docs** — is there a README? Existing docs? CLAUDE.md?
5. **Ask the user** about the project's goals, team context, and what documentation matters most

### Phase 2: Create the Vault Structure

Create a `context/` directory (or `context-v2/` if there's a legacy version) at the project root.

#### Required Files (every project)

```
context/
├── HOME.md                  # Entry point — project overview + navigation
├── changelog.md             # Master changelog of all significant changes
└── .obsidian/               # Obsidian config (optional, for vault users)
    ├── app.json
    └── core-plugins.json
```

#### Domain Folders (adapted per project)

Create numbered folders for each domain area identified in Phase 1. Examples:

**Fullstack web app:**
```
├── 01-backend/
├── 02-frontend/
├── 03-domain/
└── 04-features/
```

**Data science project:**
```
├── 01-data-pipeline/
├── 02-models/
├── 03-experiments/
└── 04-results/
```

**Mobile app:**
```
├── 01-app/
├── 02-api/
├── 03-domain/
└── 04-releases/
```

**Infrastructure/DevOps:**
```
├── 01-architecture/
├── 02-services/
├── 03-deployments/
└── 04-runbooks/
```

**Monorepo:**
```
├── 01-package-a/
├── 02-package-b/
├── 03-shared/
└── 04-features/
```

Each domain folder should contain these files (adapted to context):

| File | Purpose |
|------|---------|
| `estrutura.md` (or `structure.md`) | Folder layout, conventions, stack details |
| `evolucao.md` (or `evolution.md`) | Chronological history of changes in this domain |
| `decisoes.md` (or `decisions.md`) | Architecture Decision Records (ADRs) |

Additional domain-specific files as needed (e.g., `api.md`, `data-models.md`, `design-system.md`, `routes.md`, `business-rules.md`, `auth.md`).

#### Features/Status Tracking

Always include a features or status tracking file:
- `XX-features/features.md` — checklist of implemented and planned features

### Phase 3: Write the Content

Read `references/templates.md` for the exact templates to use for each file type. Key rules:

#### HOME.md
- Title with project name
- `> [!important] Vault vivo` callout explaining the vault is alive
- Navigation organized by domain with wikilinks
- Stack summary table
- Quick links to key resources

#### changelog.md
- Each entry: `## [YYYY-MM-DD] Type: Description`
- Types: Feature | Bugfix | Refactor | UI/UX | Infrastructure | Documentation (adapt to project language)
- Each entry must include: affected files and what changed + why

#### Domain evolution files
- Same date format as changelog but domain-specific detail
- References to related ADRs via wikilinks: `[[decisions#AD-001]]`

#### Architecture Decision Records (ADRs)
- Numbered: AD-001, AD-002, etc.
- Each includes: Date, Context, Decision, Consequence
- Use callouts for warnings and important notes

#### Structure files
- Stack table
- Folder structure with descriptions
- Conventions section with `> [!tip]` callout
- Commands (build, test, lint, deploy)
- Environment variables

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

### Phase 5: Populate Initial State

After creating the structure, fill it with the **current state** of the project:
- Read existing code and document the current architecture
- Document existing decisions (even if they were made before the vault)
- Create initial changelog entry: `[YYYY-MM-DD] Infrastructure: Context vault initialized`
- Fill features checklist with current status

## File Format Conventions

All files must use:

### Frontmatter
```yaml
---
title: Page Title
tags:
  - relevant-tag
  - domain
aliases:
  - Alternate Name
---
```

### Wikilinks
- Internal navigation: `[[path/file|Display Label]]`
- Section references: `[[file#section-name]]`
- ADR references: `[[decisions#AD-001]]`

### Callouts
```markdown
> [!important] Title
> Critical information

> [!tip] Convention
> Best practices

> [!warning] Caution
> Potential issues

> [!info] Format
> Structural guidance
```

### Language
Follow the project's language convention. If the project is in Portuguese, use Portuguese for docs. If English, use English. If mixed, follow the user's preference. Code examples inside docs should match the codebase language.

## Maintenance Reminders

When working on a project that already has a living context vault:
- **Always check** `context/HOME.md` first for orientation
- **Always update** the vault after making significant changes
- **Cross-reference** between changelog, evolution, and decisions
- **Keep features tracking** up to date
- The vault structure can evolve — add new files/folders as the project grows
