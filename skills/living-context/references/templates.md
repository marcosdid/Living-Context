# Living Context — File Templates

Use these templates when creating vault files. Adapt language (Portuguese/English) and content to the specific project.

---

## HOME.md

```markdown
---
title: "{PROJECT_NAME} — Home"
tags:
  - index
  - {project-tag}
aliases:
  - Home
  - Início
---

# {PROJECT_NAME} — Context Vault

> [!important] Living Vault
> This vault is alive and evolves alongside the codebase. Every significant code change
> triggers mandatory updates here. Consult this vault before making changes and update it after.

## Navigation

### {Domain 1 Name}
- [[01-domain/structure|Structure & Conventions]]
- [[01-domain/evolution|Evolution History]]
- [[01-domain/decisions|Architecture Decisions]]

### {Domain 2 Name}
- [[02-domain/structure|Structure & Conventions]]
- [[02-domain/evolution|Evolution History]]
- [[02-domain/decisions|Architecture Decisions]]

### {Domain-specific files}
- [[03-domain/data-models|Data Models]]
- [[03-domain/business-rules|Business Rules]]

### History & Tracking
- [[changelog|Changelog]]
- [[04-features/features|Features Status]]

## Stack

| Layer | Technology |
|-------|-----------|
| {Layer 1} | {Tech} |
| {Layer 2} | {Tech} |
| {Layer 3} | {Tech} |

## Quick Links
- Repository: {repo-url}
- Documentation: {docs-url}
- CI/CD: {ci-url}
```

---

## changelog.md

```markdown
---
title: Changelog
tags:
  - changelog
  - history
aliases:
  - Changes
  - History
---

# Changelog

> [!info] Format
> ## [YYYY-MM-DD] Type: Short Description
> - **Affected files**: list of files
> - **What changed**: description of what was altered and why
>
> **Types**: Feature | Bugfix | Refactor | UI/UX | Infrastructure | Documentation

---

## [YYYY-MM-DD] Infrastructure: Context vault initialized

- **Affected files**: `context/` (all files)
- **What changed**: Created living context vault with initial project documentation
```

---

## structure.md (per domain)

```markdown
---
title: "Structure — {Domain Name}"
tags:
  - {domain-tag}
  - structure
aliases:
  - "{Domain} Structure"
---

# {Domain Name} — Structure & Conventions

## Stack

| Component | Technology | Version |
|-----------|-----------|---------|
| {Component} | {Tech} | {Ver} |

## Folder Structure

```
{root}/
├── {folder}/          # {description}
│   ├── {subfolder}/   # {description}
│   └── {file}         # {description}
└── {folder}/          # {description}
```

## Commands

| Command | Description |
|---------|------------|
| `{cmd}` | {what it does} |

> [!tip] Conventions
> - {Convention 1}
> - {Convention 2}
> - {Convention 3}

## Environment Variables

| Variable | Description | Example |
|----------|------------|---------|
| `{VAR}` | {desc} | `{example}` |
```

---

## evolution.md (per domain)

```markdown
---
title: "Evolution — {Domain Name}"
tags:
  - {domain-tag}
  - evolution
  - history
aliases:
  - "{Domain} Evolution"
---

# {Domain Name} — Evolution

> [!info] Format
> ## [YYYY-MM-DD] Type: Short Description
> - **What changed**: detailed description
> - **Why**: motivation for the change
> - **Decisions**: [[decisions#AD-XXX]] (if applicable)
> - **Files affected**: list

---
```

---

## decisions.md (per domain — ADRs)

```markdown
---
title: "Architecture Decisions — {Domain Name}"
tags:
  - {domain-tag}
  - adr
  - architecture
aliases:
  - "{Domain} ADRs"
---

# Architecture Decisions — {Domain Name}

> [!info] Format
> Each decision is numbered (AD-001, AD-002...) and includes:
> Date, Context, Decision, and Consequence.

---

### AD-001 — {Decision Title}

- **Date**: YYYY-MM-DD
- **Context**: {What problem or situation prompted this decision?}
- **Decision**: {What was decided and how?}
- **Consequence**: {What are the trade-offs, implications, and follow-ups?}

> [!warning] {Optional warning about this decision}
> {Details}
```

---

## features.md

```markdown
---
title: Features Status
tags:
  - features
  - roadmap
aliases:
  - Feature Tracking
  - Status
---

# Features — Implementation Status

> [!tip] Usage
> Check items as they are completed. Add new items as they are planned.

## {Module/Area 1}

- [x] {Completed feature}
- [x] {Completed feature}
- [ ] {Planned feature}
- [ ] {Planned feature}

## {Module/Area 2}

- [ ] {Planned feature}
- [ ] {Planned feature}

---

## Roadmap

> [!todo] Upcoming
> - {Future initiative 1}
> - {Future initiative 2}
```

---

## data-models.md (if applicable)

```markdown
---
title: Data Models
tags:
  - domain
  - data
  - models
aliases:
  - Schema
  - ERD
---

# Data Models

## Entity Relationship Diagram

```mermaid
erDiagram
    {ENTITY_A} ||--o{ {ENTITY_B} : "{relationship}"
    {ENTITY_A} {
        int id PK
        string name
        datetime created_at
    }
    {ENTITY_B} {
        int id PK
        int entity_a_id FK
        string value
    }
```

## {Entity A}

| Field | Type | Description |
|-------|------|------------|
| `id` | int | Primary key |
| `name` | string | {description} |

> [!note] {Important note about this model}
> {Details}
```

---

## business-rules.md (if applicable)

```markdown
---
title: Business Rules
tags:
  - domain
  - rules
  - business
aliases:
  - Domain Rules
---

# Business Rules

## {Domain Area 1}

### {Rule Category}

- {Rule 1}
- {Rule 2}

> [!example] Example
> {Concrete example of the rule in action}

### {Rule Category 2}

- {Rule 1}
- {Rule 2}

> [!important] Critical Rule
> {A rule that must never be violated}
```

---

## CLAUDE.md Maintenance Section Template

Add this to the project root CLAUDE.md:

```markdown
## Living Context Maintenance

The `context/` directory is a living documentation vault that evolves with the codebase.

After ANY significant code modification, you MUST:

1. Update `context/changelog.md` with:
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

---

## .obsidian/app.json

```json
{
  "showLineNumber": true,
  "strictLineBreaks": true,
  "showFrontmatter": false
}
```

## .obsidian/core-plugins.json

```json
[
  "file-explorer",
  "global-search",
  "switcher",
  "graph",
  "backlink",
  "outgoing-link",
  "tag-pane",
  "properties",
  "page-preview",
  "templates",
  "command-palette",
  "outline",
  "word-count",
  "file-recovery"
]
```
