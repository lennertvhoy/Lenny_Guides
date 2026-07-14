# Lenny_Guides Status

**Updated At:** 2026-07-14T00:00:00Z
**Mode:** operating
**Goal:** Public bilingual guides site on GitHub Pages.

## Snapshot

- GitHub Pages is enabled with source set to GitHub Actions.
- Latest `Deploy Lenny Guides to GitHub Pages` workflow run completed successfully.
- Live site returns HTTP 200:
  - EN: https://lennertvhoy.github.io/Lenny_Guides/
  - NL: https://lennertvhoy.github.io/Lenny_Guides/nl/
- Local build verified: `mkdocs build --strict` exits 0 with no warnings.
- Site skeleton, landing pages, student-setup guide, safety page, and guide stubs are in `docs/`.
- English and Dutch versions are generated under `site/` and `site/nl/`.
- Topic guides (Herdr, Tailscale, Termux, StudyState, StateSpec) and the safety boundaries page have been rewritten to full depth with troubleshooting tables.
- Theme: cozy Catppuccin Mocha base with Dracula accents; dark mode by default.
- Public naming uses Stateware, State-Centric Engineering, StateSpec,
  StudyState, and ClassState while stable legacy guide URLs remain compatible.

## Product Truth

- Site is published at https://lennertvhoy.github.io/Lenny_Guides/.
- First guide: student setup for Herdr, Tailscale, Termux, StudyState, StateSpec.

## Runtime Truth

- No application runtime. Build verification is local `mkdocs build --strict`.
- Deployment is handled by GitHub Actions (`.github/workflows/pages.yml`).

## Current Quality Gate

- Local build passed (`--strict`, exit 0).
- No placeholders in authored rendered content.
- GitHub Actions workflow passing; live URLs return HTTP 200.

## Open Blockers

- None.

## Immediate Priorities

1. Add polished screenshots to the student setup guide (LG-CONTENT-002-post-rewrite).
