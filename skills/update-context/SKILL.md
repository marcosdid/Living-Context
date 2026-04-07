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
