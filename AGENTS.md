---
repo_role: documentation_site
repo_mode: operating
initialized_on: 2026-07-06
---

# Lenny_Guides — Agent Operating Contract

**Purpose:** Public bilingual guides site for safe, effective AI-coding-agent workflows.

## Agent Read Order
1. `AGENTS.md`
2. `STATUS.md`
3. `PROJECT_STATE.yaml`
4. `PROJECT_DNA.yaml`
5. `NEXT_ACTIONS.md`
6. `BACKLOG.md`

## Invariants
- Repo-first truth: guides live as Markdown source in `docs/`.
- No fake completeness: claim only what is built, tested, or deployed.
- Public-facing content must be accurate and safe; mark assumptions clearly.
- English is canonical; Dutch is a parallel translation.
- No secrets, tracking, backends, or custom domains in the MVP slice.
- End each session with a handoff: changed files, build result, known limits, next action.

## Current Mode
`operating` — the site is deployed and the first guide is live.
