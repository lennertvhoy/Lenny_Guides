# Lenny_Guides Status

**Updated At:** 2026-07-06
**Mode:** bootstrap
**Goal:** Public bilingual guides site on GitHub Pages.

## Snapshot

- Local build verified: `mkdocs build --strict` exits 0 with no warnings.
- Site skeleton, landing pages, student-setup guide, safety page, and guide stubs are in `docs/`.
- English and Dutch versions are generated under `site/` and `site/nl/`.
- GitHub Actions deploy workflow (`.github/workflows/pages.yml`) is ready.
- Deployment to GitHub Pages is the remaining step.

## Product Truth

- No published site yet.
- First guide: student setup for Herdr, Tailscale, Termux, StudyDD, StateDD.

## Runtime Truth

- No application runtime. Build verification is local `mkdocs build --strict`.

## Current Quality Gate

- Local build passed (`--strict`, exit 0).
- No placeholders in authored rendered content.
- Ready to push and confirm Pages deployment.

## Open Blockers

- None.

## Immediate Priorities

1. Push and confirm Pages deployment.
