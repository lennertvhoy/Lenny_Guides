# Lenny_Guides — Public Bilingual Static Site Design

**Date:** 2026-07-06  
**Scope:** First implementation slice (MVP)  
**Goal:** Build `Lenny_Guides` as a public, bilingual, StateDD-governed static documentation site.

---

## Decision

Use **MkDocs Material on GitHub Pages via GitHub Actions**, with the default URL first:
`https://lennertvhoy.github.io/Lenny_Guides/`

No custom domain in the first slice. Custom-domain support is a small backlog item for later, because GitHub Pages supports custom domains but DNS/CNAME work is not needed for the MVP.

MkDocs is the right fit because it is documentation-first, Markdown-native, and configured through a single YAML file. Material for MkDocs adds the polished docs UI, search, mobile-friendly layout, and theme features. For bilingual support, use `mkdocs-static-i18n`, which generates the default language plus language-specific paths such as `/nl/`.

GitHub Pages is fine for this scale: public repos on GitHub Free are supported, and the site/repo limits are far above what this guide site should need.

---

## Project Description

`Lenny_Guides` is a public, bilingual (English primary + Dutch) knowledge base of practical, opinionated guides for working safely and effectively with AI coding agents. The first guide is a student setup walkthrough for Herdr, Tailscale, Termux, StudyDD, and StateDD. The repo follows StateDD governance principles and publishes automatically to a free GitHub Pages static site.

---

## Default Choices

| Aspect | Choice |
|---|---|
| URL | `https://lennertvhoy.github.io/Lenny_Guides/` |
| Custom domain | No custom domain in the first slice |
| Stack | MkDocs Material |
| Hosting | Free GitHub Pages |
| Deploy | GitHub Actions on push to `main` |
| Content source | Plain Markdown in the repo |
| Language model | English is canonical first; Dutch translations follow as parallel pages |
| Governance | Repo-first StateDD files remain the source of truth |

Constraint: this is a clean guides site, not a platform. Do not overengineer it into a web app.

---

## Visual Style

- Dark navy base, close to `#12192c`.
- Warm orange/yellow accent, close to `#f2ad18`.
- Red/orange secondary accent, close to `#e93325`.
- Clear Open-Sans-like typography.
- Practical, opinionated, no corporate fluff.
- Use strong guide cards, warnings, checklists, and “what to do next” sections.
- Avoid cringe AI-copywriting language.

## Content Style

- Direct, practical, teacher-friendly.
- Explain why steps matter.
- Prefer safe defaults.
- Mark dangerous or irreversible steps clearly.
- Use “student path” and “trainer path” where helpful.
- Keep English primary, but make Dutch available for students who need it.
- Do not make exaggerated AI claims.
- Do not claim a tool is safe, private, or working unless the guide explains the boundary.

---

## Initial Content

Student setup walkthrough covering:

1. Herdr
2. Tailscale
3. Termux
4. StudyDD
5. StateDD

---

## Recommended Repository Structure

```text
.
├── AGENTS.md
├── STATUS.md
├── PROJECT_STATE.yaml
├── PROJECT_DNA.yaml
├── NEXT_ACTIONS.md
├── BACKLOG.md
├── WORKLOG.md
├── docs/
│   ├── index.md
│   ├── index.nl.md
│   ├── getting-started/
│   │   ├── student-setup.md
│   │   └── student-setup.nl.md
│   ├── guides/
│   │   ├── herdr.md
│   │   ├── herdr.nl.md
│   │   ├── tailscale.md
│   │   ├── tailscale.nl.md
│   │   ├── termux.md
│   │   ├── termux.nl.md
│   │   ├── studydd.md
│   │   ├── studydd.nl.md
│   │   ├── statedd.md
│   │   └── statedd.nl.md
│   ├── safety/
│   │   ├── boundaries.md
│   │   └── boundaries.nl.md
│   └── assets/
│       ├── css/
│       │   └── extra.css
│       └── images/
├── mkdocs.yml
├── requirements.txt
└── .github/
    └── workflows/
        └── pages.yml
```

---

## Implementation Requirements

- Use pinned Python dependencies in `requirements.txt`.
- Configure MkDocs Material with dark mode enabled by default.
- Configure `mkdocs-static-i18n` for English and Dutch.
- Configure GitHub Actions to build and deploy to GitHub Pages.
- Add a custom CSS file for Lenny style.
- Keep the site readable directly on GitHub.
- Do not add unnecessary JavaScript frameworks.
- Do not use Jekyll unless MkDocs fails for a concrete reason.
- Do not add a custom domain yet.
- Do not add analytics yet.
- Do not add a backend.
- Do not add comments, login, database, or user tracking.

---

## First Implementation Slice

1. Create the MkDocs Material skeleton.
2. Add bilingual landing pages.
3. Add the first student setup guide stub with the full structure.
4. Add safety/boundary page.
5. Add GitHub Actions deploy workflow.
6. Add StateDD status updates.
7. Add evidence that the site builds locally.
8. Commit the complete slice.

---

## Acceptance Criteria

- `mkdocs build --strict` passes.
- GitHub Actions deploy workflow exists.
- Site has English and Dutch navigation.
- Landing page explains what Lenny_Guides is.
- Student setup guide exists and is structured, even if not fully complete yet.
- No broken nav links.
- No placeholder TODOs visible in rendered content.
- StateDD files reflect current truth.
- Final handoff includes changed files, build result, known limits, and next action.

---

## Backlog After MVP

- Add custom domain only if the default URL feels too unprofessional.
- Add polished screenshots.
- Add printable student checklist.
- Add “trainer mode” annotations.
- Add Herdr-specific troubleshooting.
- Add Termux Android troubleshooting.
- Add safe AI-agent workflow examples.
- Add StateDD template download/setup path.

---

## One Strong First Slice

**Site skeleton + bilingual landing + student setup guide structure + deploy pipeline.** Do not spend the first pass perfecting copy; the important closure is a real published site with the right structure.

---

## References

- GitHub Pages custom domains: https://docs.github.com/en/pages/configuring-a-custom-domain-for-your-github-pages-site/managing-a-custom-domain-for-your-github-pages-site
- MkDocs: https://www.mkdocs.org/
- MkDocs static i18n plugin: https://ultrabug.github.io/mkdocs-static-i18n/
- GitHub Pages limits: https://docs.github.com/en/pages/getting-started-with-github-pages/github-pages-limits
