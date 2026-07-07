# Design: Deep rewrite of Lenny Guides topic pages

**Date:** 2026-07-06  
**Status:** Approved  
**Related backlog items:** LG-CONTENT-002, LG-CONTENT-005, LG-CONTENT-006, LG-CONTENT-007

## Goal

Transform the current thin topic guides (`Herdr`, `Tailscale`, `Termux`, `StudyDD`, `StateDD`, `Boundaries`) from stub overviews into standalone, student-friendly references that a beginner can actually follow, debug, and learn from.

## Scope

Rewrite these six English guides to full depth:

- `docs/guides/herdr.md`
- `docs/guides/tailscale.md`
- `docs/guides/termux.md`
- `docs/guides/studydd.md`
- `docs/guides/statedd.md`
- `docs/safety/boundaries.md`

Update `docs/index.md` cross-links and summaries once the guides are rewritten. Dutch translations follow after the English versions are approved.

## Audience

Students learning to code with AI coding agents. Assume beginner terminal/Git skills. Explain every command and concept. Prefer safe defaults over speed.

## Success criteria

1. Every rewritten guide follows the same 10-section template.
2. Every guide contains at least one complete "first real workflow" a student can run.
3. Every guide contains a troubleshooting table with 5–8 real problems, symptoms, and fixes.
4. No mixed Dutch/English in examples.
5. `mkdocs build --strict` passes for English.
6. All internal links resolve.

## Phased approach

### Phase 1 — Core stack
Rewrite the three tools that `student-setup.md` depends on:

- **Herdr** — install, first session, attach/detach/stop, agent pane model, common errors.
- **Tailscale** — install, authenticate, MagicDNS, SSH between devices, ACL basics, common errors.
- **Termux** — install from F-Droid, SSH client setup, key management, common errors.

### Phase 2 — Repo-first workflows

- **StudyDD** — the learning workflow using `~/Projects/study-project` and evidence rules.
- **StateDD** — the project workflow using `~/Projects/study-project` and validation rules.

### Phase 3 — Safety and navigation

- **Boundaries** — add concrete scenarios and a quick decision checklist.
- **index.md** — update browse links and add a "who each guide is for" summary.

### Phase 4 — Dutch translations

Translate the rewritten guides to Dutch, matching structure and examples.

## Guide template

Each guide uses the following sections in order:

1. **One-line summary** — what problem this tool/workflow solves.
2. **What it does** — plain explanation, no marketing copy.
3. **When to use it** — 3–5 concrete scenarios.
4. **What you need** — hardware, accounts, time, prior guides.
5. **Install and verify** — copy-paste commands with a check command after each step.
6. **Your first real workflow** — a complete mini-example a student can run.
7. **Common problems and fixes** — table: symptom → cause → fix.
8. **Safety notes** — the non-negotiables specific to this tool/workflow.
9. **Phone vs laptop** — when this works well on a phone and when it does not.
10. **What to read next** — exactly two links.

## Content standards

### Consistent example world

Use the same names across guides so students do not relearn context:

- Host machine: `studybox`
- User: `lenny`
- Study repo: `~/Projects/study-project`
- Android phone: `student-phone`

### Command blocks

- Explain every command before or after the block.
- Every block must be copy-pasteable or clearly marked with an inline comment like `# <-- change this to your value` if it needs editing.
- Show the expected output or a verification command after installation steps.

### Workflows

Each guide's "first real workflow" must be concrete:

- **Herdr:** start a session → run a simple command → detach → reattach from phone → stop safely.
- **Tailscale:** install → authenticate → find your IP → SSH from phone → verify both devices see each other.
- **Termux:** install → set up SSH keys → connect to `studybox` → run `git status`.
- **StudyDD:** create a study repo → add today's topic → write evidence → review.
- **StateDD:** create a project repo → file a task → let the agent propose → validate → close.
- **Boundaries:** walk through three real decisions (install a package, commit a `.env`, run a shell command you do not understand).

### Troubleshooting tables

Each table contains 5–8 real problems. Format:

| Symptom | Likely cause | Fix |
|---|---|---|
| `ssh: Could not resolve hostname studybox` | MagicDNS has not propagated yet | Use the Tailscale IP instead, or wait 60 seconds and retry. |

Fixes are verified where possible. Unverified fixes are marked with `(unverified)` and a note explaining why.

## Research plan

Before writing, research each tool's current install path and common failure modes:

- **Herdr:** official install script, pane commands, detach/stop behavior, known issues.
- **Tailscale:** `tailscale up` failures, MagicDNS delays, ACL gotchas, SSH setup.
- **Termux:** F-Droid vs Play Store differences, package install errors, SSH key setup on Android.
- **StudyDD / StateDD:** refine file templates against the existing repo's `AGENTS.md` and `PROJECT_DNA.yaml`.
- **Boundaries:** collect concrete unsafe agent requests and safe alternatives.

Research sources: official documentation, project GitHub issues, install scripts. Mark anything that cannot be locally verified.

## Internationalization

- English is canonical. Write and validate English first.
- Dutch translations update after each phase is approved.
- Maintain a glossary for recurring terms (e.g., "repo first" → "repo first", "source of truth" → "bron van waarheid").
- Commands, screenshots, and example names stay identical across languages.

## Verification

- `mkdocs build --strict` must pass with no warnings.
- All internal links must resolve in both EN and NL builds.
- No `TODO`, `TBD`, or placeholder text in rendered content.
- Final review checklist per guide: template present, workflow complete, troubleshooting table present.

## Out of scope

- Adding screenshots in this pass. Use placeholder notes where a screenshot would prevent a mistake.
- Printable checklist (backlog item LG-CONTENT-003); may be added later.
- Custom domain or analytics.
- Trainer-mode annotations (backlog item LG-CONTENT-004).

## Open questions

None remaining; approved by user before spec write-up.
