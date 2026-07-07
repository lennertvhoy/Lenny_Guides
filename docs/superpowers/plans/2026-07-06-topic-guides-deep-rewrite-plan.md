> **For agentic workers:** REQUIRED SUB-SKILL: Use superpowers:subagent-driven-development (recommended) or superpowers:executing-plans to implement this plan task-by-task. Steps use checkbox (`- [ ]`) syntax for tracking.

# Topic Guides Deep Rewrite Implementation Plan

**Goal:** Rewrite the six thin topic guides (`Herdr`, `Tailscale`, `Termux`, `StudyDD`, `StateDD`, `Boundaries`) into full-depth, student-friendly references with troubleshooting tables and concrete workflows, then update navigation and Dutch translations.

**Architecture:** Each guide becomes a standalone Markdown file following a strict 10-section template. Guides share a consistent example world (`studybox`, `lenny`, `study-project`) and cross-link to each other. Quality is enforced by a section checklist and `mkdocs build --strict`.

**Tech Stack:** MkDocs, Material for MkDocs, mkdocs-static-i18n, Markdown.

---

## File Structure

**English guides (rewrite to full depth):**
- `docs/guides/herdr.md` — persistent terminal workspace for agents
- `docs/guides/tailscale.md` — private mesh network
- `docs/guides/termux.md` — Android terminal / SSH client
- `docs/guides/studydd.md` — repo-first learning workflow
- `docs/guides/statedd.md` — repo-first project workflow
- `docs/safety/boundaries.md` — safety rules and decision scenarios
- `docs/index.md` — landing page with updated browse links and summaries

**Dutch translations (update after English approval):**
- `docs/guides/herdr.nl.md`
- `docs/guides/tailscale.nl.md`
- `docs/guides/termux.nl.md`
- `docs/guides/studydd.nl.md`
- `docs/guides/statedd.nl.md`
- `docs/safety/boundaries.nl.md`
- `docs/index.nl.md`

---

## Required 10-Section Template

Every rewritten guide must contain, in order:

1. One-line summary
2. What it does
3. When to use it
4. What you need
5. Install and verify
6. Your first real workflow
7. Common problems and fixes
8. Safety notes
9. Phone vs laptop
10. What to read next

---

## Task 0: Establish Baseline

**Files:**
- Read: `docs/superpowers/specs/2026-07-06-topic-guides-deep-rewrite-design.md`

- [ ] **Step 1: Verify current build passes**

Run:
```bash
mkdocs build --strict
```

Expected: exit 0, no warnings.

- [ ] **Step 2: Commit baseline (optional, with user approval)**

```bash
git add docs/superpowers/specs/2026-07-06-topic-guides-deep-rewrite-design.md
git commit -m "docs: add topic guides deep rewrite design spec"
```

---

## Task 1: Rewrite Herdr Guide

**Files:**
- Modify: `docs/guides/herdr.md`

- [ ] **Step 1: Research Herdr**

Check the official install script at `https://herdr.dev/install.sh` and any available docs. Document:
- Install command
- How to start a session
- How to detach (`Ctrl+B q`)
- How to stop (`herdr server stop`)
- Common errors (e.g., tmux conflict, session not found)

- [ ] **Step 2: Rewrite `docs/guides/herdr.md` using the 10-section template**

Include a troubleshooting table with at least 5 entries. Use example names `studybox` and `lenny`.

- [ ] **Step 3: Verify required sections exist**

Run:
```bash
for section in "One-line summary" "What it does" "When to use it" "What you need" "Install and verify" "Your first real workflow" "Common problems and fixes" "Safety notes" "Phone vs laptop" "What to read next"; do
  grep -q "$section" docs/guides/herdr.md || echo "MISSING: $section"
done
```

Expected: no "MISSING" lines.

- [ ] **Step 4: Build check**

Run:
```bash
mkdocs build --strict
```

Expected: exit 0.

- [ ] **Step 5: Commit (with user approval)**

```bash
git add docs/guides/herdr.md
git commit -m "docs: rewrite Herdr guide with troubleshooting"
```

---

## Task 2: Rewrite Tailscale Guide

**Files:**
- Modify: `docs/guides/tailscale.md`

- [ ] **Step 1: Research Tailscale**

Check `https://tailscale.com/kb/` for current install steps and common issues. Document:
- Install command for Linux
- `sudo tailscale up`
- `tailscale status` and `tailscale ip -4`
- MagicDNS propagation delay
- Common errors (authentication, different tailnet, ACL blocks)

- [ ] **Step 2: Rewrite `docs/guides/tailscale.md` using the 10-section template**

Include a troubleshooting table with at least 5 entries. Show SSH from phone to `studybox`.

- [ ] **Step 3: Verify required sections exist**

Run:
```bash
for section in "One-line summary" "What it does" "When to use it" "What you need" "Install and verify" "Your first real workflow" "Common problems and fixes" "Safety notes" "Phone vs laptop" "What to read next"; do
  grep -q "$section" docs/guides/tailscale.md || echo "MISSING: $section"
done
```

Expected: no "MISSING" lines.

- [ ] **Step 4: Build check**

Run:
```bash
mkdocs build --strict
```

Expected: exit 0.

- [ ] **Step 5: Commit (with user approval)**

```bash
git add docs/guides/tailscale.md
git commit -m "docs: rewrite Tailscale guide with troubleshooting"
```

---

## Task 3: Rewrite Termux Guide

**Files:**
- Modify: `docs/guides/termux.md`

- [ ] **Step 1: Research Termux**

Check `https://termux.dev/` and F-Droid page. Document:
- F-Droid install recommendation
- `pkg update && pkg upgrade`
- `pkg install -y openssh git nano`
- SSH key generation and `~/.ssh/config`
- Common errors (outdated Termux, keyboard issues, package conflicts)

- [ ] **Step 2: Rewrite `docs/guides/termux.md` using the 10-section template**

Include a troubleshooting table with at least 5 entries. Workflow must end with `ssh studybox` and `git status`.

- [ ] **Step 3: Verify required sections exist**

Run:
```bash
for section in "One-line summary" "What it does" "When to use it" "What you need" "Install and verify" "Your first real workflow" "Common problems and fixes" "Safety notes" "Phone vs laptop" "What to read next"; do
  grep -q "$section" docs/guides/termux.md || echo "MISSING: $section"
done
```

Expected: no "MISSING" lines.

- [ ] **Step 4: Build check**

Run:
```bash
mkdocs build --strict
```

Expected: exit 0.

- [ ] **Step 5: Commit (with user approval)**

```bash
git add docs/guides/termux.md
git commit -m "docs: rewrite Termux guide with troubleshooting"
```

---

## Task 4: Rewrite StudyDD Guide

**Files:**
- Modify: `docs/guides/studydd.md`

- [ ] **Step 1: Review existing repo-first patterns**

Read:
- `AGENTS.md`
- `PROJECT_DNA.yaml`
- `STATUS.md`
- `NEXT_ACTIONS.md`
- `BACKLOG.md`

Document the minimum file set and the agent read order.

- [ ] **Step 2: Rewrite `docs/guides/studydd.md` using the 10-section template**

Workflow: create `~/Projects/study-project` → add `AGENTS.md`, `STATUS.md`, `NEXT_ACTIONS.md`, `BACKLOG.md`, `WORKLOG.md`, `SOURCES.md`, `REVIEW.md`, `evidence/README.md` → add a learning topic → write evidence → review.

Include a troubleshooting table with at least 5 entries.

- [ ] **Step 3: Verify required sections exist**

Run:
```bash
for section in "One-line summary" "What it does" "When to use it" "What you need" "Install and verify" "Your first real workflow" "Common problems and fixes" "Safety notes" "Phone vs laptop" "What to read next"; do
  grep -q "$section" docs/guides/studydd.md || echo "MISSING: $section"
done
```

Expected: no "MISSING" lines.

- [ ] **Step 4: Build check**

Run:
```bash
mkdocs build --strict
```

Expected: exit 0.

- [ ] **Step 5: Commit (with user approval)**

```bash
git add docs/guides/studydd.md
git commit -m "docs: rewrite StudyDD guide with workflow and troubleshooting"
```

---

## Task 5: Rewrite StateDD Guide

**Files:**
- Modify: `docs/guides/statedd.md`

- [ ] **Step 1: Review existing repo-first patterns**

Same files as Task 4, plus `PROJECT_STATE.yaml` and `docs/EVIDENCE_LOG.md`.

- [ ] **Step 2: Rewrite `docs/guides/statedd.md` using the 10-section template**

Workflow: create `~/Projects/study-project` → add StateDD file set → file a task in `NEXT_ACTIONS.md` → let the agent propose a plan → validate with evidence → close the task.

Include a troubleshooting table with at least 5 entries.

- [ ] **Step 3: Verify required sections exist**

Run:
```bash
for section in "One-line summary" "What it does" "When to use it" "What you need" "Install and verify" "Your first real workflow" "Common problems and fixes" "Safety notes" "Phone vs laptop" "What to read next"; do
  grep -q "$section" docs/guides/statedd.md || echo "MISSING: $section"
done
```

Expected: no "MISSING" lines.

- [ ] **Step 4: Build check**

Run:
```bash
mkdocs build --strict
```

Expected: exit 0.

- [ ] **Step 5: Commit (with user approval)**

```bash
git add docs/guides/statedd.md
git commit -m "docs: rewrite StateDD guide with workflow and troubleshooting"
```

---

## Task 6: Rewrite Boundaries Guide

**Files:**
- Modify: `docs/safety/boundaries.md`

- [ ] **Step 1: Design three concrete decision scenarios**

Scenarios to cover:
1. Agent asks to install a package you do not recognize.
2. Agent wants to commit a file that looks like `.env`.
3. Agent suggests a shell command you do not understand.

For each, write the safe response and why.

- [ ] **Step 2: Rewrite `docs/safety/boundaries.md` using the 10-section template**

Keep the existing non-negotiables, but expand with scenarios and a quick decision checklist. Troubleshooting table here is a "common mistakes" table: unsafe action → why it is unsafe → safe alternative.

- [ ] **Step 3: Verify required sections exist**

Run:
```bash
for section in "One-line summary" "What it does" "When to use it" "What you need" "Install and verify" "Your first real workflow" "Common problems and fixes" "Safety notes" "Phone vs laptop" "What to read next"; do
  grep -q "$section" docs/safety/boundaries.md || echo "MISSING: $section"
done
```

Expected: no "MISSING" lines.

- [ ] **Step 4: Build check**

Run:
```bash
mkdocs build --strict
```

Expected: exit 0.

- [ ] **Step 5: Commit (with user approval)**

```bash
git add docs/safety/boundaries.md
git commit -m "docs: rewrite Boundaries guide with scenarios and checklist"
```

---

## Task 7: Update Landing Page

**Files:**
- Modify: `docs/index.md`

- [ ] **Step 1: Rewrite browse guide descriptions**

Update the "Browse guides" and "Safety first" sections to reflect the rewritten content. Add one-line "who this is for" blurbs.

Example format:
```markdown
- [Herdr](./guides/herdr.md) — persistent terminal workspace for agents. Start here if you want long-running agent sessions.
```

- [ ] **Step 2: Verify all linked files exist**

Run:
```bash
mkdocs build --strict
```

Expected: exit 0.

- [ ] **Step 3: Commit (with user approval)**

```bash
git add docs/index.md
git commit -m "docs: update landing page for rewritten guides"
```

---

## Task 8: Translate Core Stack Guides to Dutch

**Files:**
- Modify: `docs/guides/herdr.nl.md`
- Modify: `docs/guides/tailscale.nl.md`
- Modify: `docs/guides/termux.nl.md`

- [ ] **Step 1: Create translation glossary**

Create or append to `docs/TRANSLATION_GLOSSARY.md`:
```markdown
| English | Dutch | Notes |
|---|---|---|
| repo first | repo first | keep in English |
| source of truth | bron van waarheid |  |
| troubleshooting | probleemoplossing |  |
| workflow | werkstroom |  |
```

- [ ] **Step 2: Translate Herdr, Tailscale, and Termux**

Match structure and examples to the English versions. Keep commands and file names identical.

- [ ] **Step 3: Verify build with Dutch**

Run:
```bash
mkdocs build --strict
```

Expected: exit 0.

- [ ] **Step 4: Commit (with user approval)**

```bash
git add docs/guides/herdr.nl.md docs/guides/tailscale.nl.md docs/guides/termux.nl.md docs/TRANSLATION_GLOSSARY.md
git commit -m "docs: add Dutch translations for core stack guides"
```

---

## Task 9: Translate Workflow and Safety Guides to Dutch

**Files:**
- Modify: `docs/guides/studydd.nl.md`
- Modify: `docs/guides/statedd.nl.md`
- Modify: `docs/safety/boundaries.nl.md`
- Modify: `docs/index.nl.md`

- [ ] **Step 1: Translate StudyDD, StateDD, Boundaries, and index**

Match structure and examples to the English versions.

- [ ] **Step 2: Verify build with Dutch**

Run:
```bash
mkdocs build --strict
```

Expected: exit 0.

- [ ] **Step 3: Commit (with user approval)**

```bash
git add docs/guides/studydd.nl.md docs/guides/statedd.nl.md docs/safety/boundaries.nl.md docs/index.nl.md
git commit -m "docs: add Dutch translations for workflow and safety guides"
```

---

## Task 10: Final Verification

**Files:**
- All files listed above

- [ ] **Step 1: Run strict build**

Run:
```bash
mkdocs build --strict
```

Expected: exit 0, no warnings.

- [ ] **Step 2: Verify all guides have required sections**

Run:
```bash
for file in docs/guides/herdr.md docs/guides/tailscale.md docs/guides/termux.md docs/guides/studydd.md docs/guides/statedd.md docs/safety/boundaries.md; do
  echo "Checking $file"
  for section in "One-line summary" "What it does" "When to use it" "What you need" "Install and verify" "Your first real workflow" "Common problems and fixes" "Safety notes" "Phone vs laptop" "What to read next"; do
    grep -q "$section" "$file" || echo "  MISSING: $section"
  done
done
```

Expected: no "MISSING" lines.

- [ ] **Step 3: Verify no placeholders in rendered content**

Run:
```bash
grep -R "TODO\|TBD\|FIXME" docs/guides docs/safety docs/index.md || echo "No placeholders found"
```

Expected: "No placeholders found".

- [ ] **Step 4: Update project state files**

Update `STATUS.md`, `PROJECT_STATE.yaml`, `NEXT_ACTIONS.md`, and `BACKLOG.md` to reflect completion of LG-CONTENT-002 and related items.

- [ ] **Step 5: Final commit (with user approval)**

```bash
git add -A
git commit -m "docs: complete topic guides deep rewrite"
```

---

## Plan Self-Review

**Spec coverage:**
- Full-depth rewrite of all six topic guides → Tasks 1–6.
- 10-section template → repeated in each task.
- Troubleshooting tables with 5–8 entries → Step 2 of Tasks 1–6.
- Consistent example world (`studybox`, `lenny`, `study-project`) → embedded in task instructions.
- Dutch translations → Tasks 8–9.
- Verification → Task 10.

**Placeholder scan:**
- No `TBD`, `TODO`, `implement later`, or vague "add error handling" steps.
- Each task has exact file paths and verification commands.

**Type consistency:**
- Section names match across all verification commands.
- File paths are consistent.
