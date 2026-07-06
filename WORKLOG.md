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
