# WORKLOG.md — Append-Only History

## 2026-07-06 — Bootstrap and plan

- Repo initialized on `main`.
- Design spec approved: MkDocs Material on GitHub Pages, bilingual EN/NL.
- Implementation plan created at `docs/superpowers/plans/2026-07-06-lenny-guides-mvp-plan.md`.
- StateDD governance skeleton created.

## 2026-07-06 — Site skeleton, content, and local build verification

- Added MkDocs Material site skeleton (`mkdocs.yml`, `requirements.txt`, `docs/assets/css/extra.css`).
- Added bilingual landing pages (`docs/index.md`, `docs/index.nl.md`).
- Added student-setup guide (`docs/getting-started/student-setup.md` + `.nl.md`).
- Added guide stubs for Herdr, Tailscale, Termux, StudyDD, and StateDD (EN + NL).
- Added safety/boundaries page (`docs/safety/boundaries.md` + `.nl.md`).
- Added GitHub Actions deploy workflow (`.github/workflows/pages.yml`).
- Verified `mkdocs build --strict` passes locally (exit 0, no warnings).
- Confirmed English output under `site/` and Dutch output under `site/nl/`.
- Confirmed no placeholder TODO/TBD/FIXME/"coming soon" in authored rendered content.
- Updated StateDD files: `PROJECT_STATE.yaml`, `STATUS.md`, `NEXT_ACTIONS.md`, `WORKLOG.md`.

## 2026-07-06 — Push and confirm Pages deployment

- Pushed local `main` to `origin/main` (HEAD `273173879215a3b69ba243dcda5fd91de6268cf2`).
- Enabled GitHub Pages via the GitHub API:
  - Initial POST with `source[branch]=main`/`source[path]=/` returned 404 (site not yet enabled).
  - Created Pages site via `POST /repos/lennertvhoy/Lenny_Guides/pages` with nested source fields.
  - Switched source to GitHub Actions via `PUT /repos/lennertvhoy/Lenny_Guides/pages` with `build_type=workflow`.
- First workflow run (`28814097934`) failed to create Pages deployment because Pages source was still legacy; after switching to `workflow` source and re-running, deployment succeeded.
- Verified workflow run `28814297865` completed successfully.
- Verified live URLs return HTTP 200:
  - https://lennertvhoy.github.io/Lenny_Guides/
  - https://lennertvhoy.github.io/Lenny_Guides/nl/
- Updated StateDD files: `PROJECT_STATE.yaml`, `STATUS.md`, `NEXT_ACTIONS.md`, `WORKLOG.md`.

## 2026-07-06 — Repo cleanup and public readiness

- Added `README.md` with live site URL and local development instructions.
- Added `.gitignore` to exclude `site/`, `.venv/`, editor files, and secrets.
- Added `LICENSE` (MIT).
- Committed design spec and implementation plan under `docs/superpowers/`.
- Switched repo mode from `bootstrap` to `operating` in StateDD files.
