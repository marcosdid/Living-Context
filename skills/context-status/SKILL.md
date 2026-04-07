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
