# Lenny Guides

Public bilingual guides for safe, effective AI-coding-agent workflows.

Current public terminology uses **Stateware**, **State-Centric Engineering**,
**StateSpec**, and **StudyState**. Existing guide URLs keep their legacy paths
so bookmarks continue to work.

**Live site:** https://lennertvhoy.github.io/Lenny_Guides/

## What is this?

Lenny Guides is a knowledge base for students, teachers, and solo builders who want to work with AI coding agents without losing control of their projects, secrets, or sanity.

Each guide explains **why** a step matters, not just **what** to type. English is the canonical language; Dutch translations are available for students who need them.

## Start here

- [Student setup](https://lennertvhoy.github.io/Lenny_Guides/getting-started/student-setup/) — Build a safe, mobile-friendly workspace with Herdr, Tailscale, Termux, StudyState, and StateSpec.

## Repository structure

- `docs/` — Markdown source for all guides.
- `mkdocs.yml` — Site configuration.
- `requirements.txt` — Pinned Python dependencies.
- `.github/workflows/pages.yml` — GitHub Actions deploy pipeline.
- StateSpec files (`AGENTS.md`, `STATUS.md`, `PROJECT_STATE.yaml`, etc.) — Repo-first project governance.
- [Naming and compatibility](https://lennertvhoy.github.io/Lenny_Guides/guides/naming/) — the category, method, specification, and legacy aliases.

## Local development

```bash
python3 -m venv .venv
source .venv/bin/activate
pip install -r requirements.txt
mkdocs serve
```

Then open http://127.0.0.1:8000/Lenny_Guides/.

## License

See [LICENSE](LICENSE) for details.
