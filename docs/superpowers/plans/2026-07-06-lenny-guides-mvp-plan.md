# Lenny_Guides MVP Implementation Plan

> **For agentic workers:** REQUIRED SUB-SKILL: Use `superpowers:subagent-driven-development` (recommended) or `superpowers:executing-plans` to implement this plan task-by-task. Steps use checkbox (`- [ ]`) syntax for tracking.

**Goal:** Build the Lenny_Guides public bilingual static site (MkDocs Material + GitHub Pages) with StateDD governance and the first student-setup guide.

**Architecture:** Plain Markdown source files live in `docs/`. MkDocs Material renders them into a static site. `mkdocs-static-i18n` generates English (default) and Dutch (`/nl/`) versions. GitHub Actions builds and deploys to GitHub Pages on every push to `main`. Custom CSS applies the Lenny color palette.

**Tech Stack:** Python, MkDocs, Material for MkDocs, mkdocs-static-i18n, GitHub Pages, GitHub Actions.

---

## File Map

| File | Responsibility |
|---|---|
| `requirements.txt` | Pinned Python dependencies for reproducible builds |
| `mkdocs.yml` | Site config, nav, theme, i18n, extra CSS |
| `docs/assets/css/extra.css` | Lenny brand colors and small UI tweaks |
| `docs/index.md` | English landing page |
| `docs/index.nl.md` | Dutch landing page |
| `docs/getting-started/student-setup.md` | English student setup guide |
| `docs/getting-started/student-setup.nl.md` | Dutch student setup guide |
| `docs/guides/*.md` | Per-topic guide stubs (EN + NL) |
| `docs/safety/boundaries.md` | English safety/boundary page |
| `docs/safety/boundaries.nl.md` | Dutch safety/boundary page |
| `.github/workflows/pages.yml` | CI/CD build and deploy to GitHub Pages |
| `AGENTS.md` | Agent read-order and invariants for this repo |
| `STATUS.md` | Short current-truth snapshot |
| `PROJECT_STATE.yaml` | Structured current state |
| `PROJECT_DNA.yaml` | Stable architecture contract |
| `NEXT_ACTIONS.md` | Active queue |
| `BACKLOG.md` | Strategic backlog with IDs |
| `WORKLOG.md` | Append-only history |

---

### Task 1: Create StateDD Governance Skeleton

**Files:**
- Create: `AGENTS.md`
- Create: `STATUS.md`
- Create: `PROJECT_STATE.yaml`
- Create: `PROJECT_DNA.yaml`
- Create: `NEXT_ACTIONS.md`
- Create: `BACKLOG.md`
- Create: `WORKLOG.md`

**Context:** These files follow StateDD principles but stay lightweight for a guides repo. Keep them honest — the repo is in bootstrap mode until the site is live.

- [ ] **Step 1: Write `AGENTS.md`**

```markdown
---
repo_role: documentation_site
repo_mode: bootstrap
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
`bootstrap` until the first GitHub Pages deployment is verified.
```

- [ ] **Step 2: Write `STATUS.md`**

```markdown
# Lenny_Guides Status

**Updated At:** 2026-07-06
**Mode:** bootstrap
**Goal:** Public bilingual guides site on GitHub Pages.

## Snapshot

- Empty repo initialized on `main`.
- Design spec approved: MkDocs Material, GitHub Pages, English + Dutch.
- Implementation plan exists at `docs/superpowers/plans/2026-07-06-lenny-guides-mvp-plan.md`.
- No deployment yet.

## Product Truth

- No published site yet.
- First guide: student setup for Herdr, Tailscale, Termux, StudyDD, StateDD.

## Runtime Truth

- No application runtime. Build verification is local `mkdocs build --strict`.

## Current Quality Gate

- Not yet passing; build is unverified.

## Open Blockers

- None.

## Immediate Priorities

1. Add MkDocs Material skeleton.
2. Add bilingual landing pages and first guide.
3. Add GitHub Actions deploy workflow.
4. Verify local build.
5. Push and confirm Pages deployment.
```

- [ ] **Step 3: Write `PROJECT_DNA.yaml`**

```yaml
---
# PROJECT_DNA.yaml — Stable architecture contract for Lenny_Guides

project:
  name: Lenny_Guides
  type: public_documentation_site
  purpose: Practical bilingual guides for safe AI-coding-agent workflows
  canonical_url: https://lennertvhoy.github.io/Lenny_Guides/
  languages:
    canonical: en
    translations:
      - nl

architecture:
  generator: MkDocs
  theme: Material for MkDocs
  i18n_plugin: mkdocs-static-i18n
  hosting: GitHub Pages
  deploy_trigger: push to main
  content_format: Markdown
  custom_assets:
    - docs/assets/css/extra.css

invariants:
  - Source of truth is the Markdown in docs/.
  - No backend, database, comments, login, or analytics in MVP.
  - No custom domain in MVP.
  - English pages are canonical; Dutch pages are peer translations.
  - All rendered nav links must resolve.
  - Build must pass `mkdocs build --strict` before push.

directories:
  docs: Markdown source content
  docs/assets/css: Custom styles
  docs/assets/images: Screenshots and diagrams
  docs/getting-started: Onboarding guides
  docs/guides: Topic-specific guides
  docs/safety: Safety and boundary guidance
  .github/workflows: CI/CD definitions
  docs/superpowers/specs: Approved design specs
  docs/superpowers/plans: Implementation plans
```

- [ ] **Step 4: Write `PROJECT_STATE.yaml`**

```yaml
---
# PROJECT_STATE.yaml — Current truth for Lenny_Guides

metadata:
  updated_at: 2026-07-06T17:00:00+00:00
  updated_by: agent
  version: "0.1.0-bootstrap"

workflow:
  repo_role: documentation_site
  repo_mode: bootstrap

current_state:
  site:
    status: not_built
    generator: MkDocs
    theme: Material for MkDocs
    i18n: planned
    url: https://lennertvhoy.github.io/Lenny_Guides/
    custom_domain: false

  content:
    status: planned
    languages:
      en: planned
      nl: planned
    first_guide: student-setup
    guide_topics:
      - herdr
      - tailscale
      - termux
      - studydd
      - statedd

  deployment:
    status: not_deployed
    host: GitHub Pages
    workflow: .github/workflows/pages.yml
    ci_status: not_verified

  quality_gate:
    status: not_passed
    local_build: not_verified
    strict_build: not_verified
    link_check: not_verified

  repository:
    path: /home/ff/Documents/Projects/Lenny_Guides
    branch: main
    head: unknown
    worktree_status: unknown

active_problems: []

residual_risks:
  - GitHub Pages may require enabling in repository settings.
  - mkdocs-static-i18n config may need tuning for first build.
```

- [ ] **Step 5: Write `NEXT_ACTIONS.md`**

```markdown
# NEXT_ACTIONS.md — Active Queue

1. **LG-MVP-001** Create MkDocs Material skeleton and requirements.
2. **LG-MVP-002** Add bilingual landing pages.
3. **LG-MVP-003** Add student setup guide structure (EN + NL).
4. **LG-MVP-004** Add safety/boundaries page (EN + NL).
5. **LG-MVP-005** Add GitHub Actions deploy workflow.
6. **LG-MVP-006** Verify `mkdocs build --strict` passes locally.
7. **LG-MVP-007** Commit and push; confirm Pages deployment.
```

- [ ] **Step 6: Write `BACKLOG.md`**

```markdown
# BACKLOG.md — Strategic Roadmap

## LG-MVP — Site Skeleton + First Guide
Build and publish the first version of Lenny_Guides.

- LG-MVP-001 MkDocs Material skeleton and requirements
- LG-MVP-002 Bilingual landing pages
- LG-MVP-003 Student setup guide structure (EN + NL)
- LG-MVP-004 Safety/boundaries page (EN + NL)
- LG-MVP-005 GitHub Actions deploy workflow
- LG-MVP-006 Local build verification
- LG-MVP-007 Push and confirm Pages deployment

## LG-POST — After MVP

- LG-CUSTOM-001 Custom domain support (only if default URL is not enough)
- LG-CONTENT-002 Add polished screenshots to student setup guide
- LG-CONTENT-003 Printable student checklist
- LG-CONTENT-004 Trainer-mode annotations
- LG-CONTENT-005 Herdr-specific troubleshooting expansion
- LG-CONTENT-006 Termux Android troubleshooting expansion
- LG-CONTENT-007 Safe AI-agent workflow examples
- LG-CONTENT-008 StateDD template download/setup path
```

- [ ] **Step 7: Write `WORKLOG.md`**

```markdown
# WORKLOG.md — Append-Only History

## 2026-07-06 — Bootstrap and plan

- Repo initialized on `main`.
- Design spec approved: MkDocs Material on GitHub Pages, bilingual EN/NL.
- Implementation plan created at `docs/superpowers/plans/2026-07-06-lenny-guides-mvp-plan.md`.
- StateDD governance skeleton created.
```

- [ ] **Step 8: Verify files exist**

Run:
```bash
ls -1 AGENTS.md STATUS.md PROJECT_STATE.yaml PROJECT_DNA.yaml NEXT_ACTIONS.md BACKLOG.md WORKLOG.md
```
Expected: all seven files listed.

- [ ] **Step 9: Commit**

```bash
git add AGENTS.md STATUS.md PROJECT_STATE.yaml PROJECT_DNA.yaml NEXT_ACTIONS.md BACKLOG.md WORKLOG.md
git commit -m "chore: add StateDD governance skeleton"
```

---

### Task 2: Create MkDocs Material Skeleton

**Files:**
- Create: `requirements.txt`
- Create: `mkdocs.yml`
- Create: `docs/assets/css/extra.css`
- Create: `docs/assets/images/.gitkeep`

- [ ] **Step 1: Write `requirements.txt`**

```text
mkdocs==1.6.1
mkdocs-material==9.6.12
mkdocs-static-i18n==1.3.0
```

- [ ] **Step 2: Write `mkdocs.yml`**

```yaml
site_name: Lenny Guides
site_description: Practical guides for safe, effective AI-coding-agent workflows.
site_url: https://lennertvhoy.github.io/Lenny_Guides/
repo_url: https://github.com/lennertvhoy/Lenny_Guides
edit_uri: edit/main/docs/

theme:
  name: material
  palette:
    - media: "(prefers-color-scheme)"
      scheme: slate
      primary: custom
      accent: custom
      toggle:
        icon: material/brightness-auto
        name: Switch to light mode
    - media: "(prefers-color-scheme: light)"
      scheme: default
      primary: custom
      accent: custom
      toggle:
        icon: material/brightness-7
        name: Switch to dark mode
    - media: "(prefers-color-scheme: dark)"
      scheme: slate
      primary: custom
      accent: custom
      toggle:
        icon: material/brightness-4
        name: Switch to system preference
  font:
    text: Open Sans
    code: Roboto Mono
  features:
    - navigation.tabs
    - navigation.sections
    - navigation.expand
    - navigation.top
    - search.suggest
    - search.highlight

extra_css:
  - assets/css/extra.css

plugins:
  - search
  - i18n:
      docs_structure: suffix
      fallback_to_default: true
      languages:
        - locale: en
          name: English
          default: true
          build: true
        - locale: nl
          name: Nederlands
          build: true
      navigation:
        - Home: index.md
        - Getting Started:
            - Student Setup: getting-started/student-setup.md
        - Guides:
            - Herdr: guides/herdr.md
            - Tailscale: guides/tailscale.md
            - Termux: guides/termux.md
            - StudyDD: guides/studydd.md
            - StateDD: guides/statedd.md
        - Safety:
            - Boundaries: safety/boundaries.md

nav:
  - Home: index.md
  - Getting Started:
      - Student Setup: getting-started/student-setup.md
  - Guides:
      - Herdr: guides/herdr.md
      - Tailscale: guides/tailscale.md
      - Termux: guides/termux.md
      - StudyDD: guides/studydd.md
      - StateDD: guides/statedd.md
  - Safety:
      - Boundaries: safety/boundaries.md

extra:
  social:
    - icon: fontawesome/brands/github
      link: https://github.com/lennertvhoy/Lenny_Guides
```

- [ ] **Step 3: Write `docs/assets/css/extra.css`**

```css
:root {
  --lenny-navy: #12192c;
  --lenny-orange: #f2ad18;
  --lenny-red: #e93325;
}

[data-md-color-scheme="slate"] {
  --md-primary-fg-color: var(--lenny-navy);
  --md-primary-fg-color--light: #1a2440;
  --md-primary-fg-color--dark: #0d1220;
  --md-accent-fg-color: var(--lenny-orange);
  --md-accent-fg-color--transparent: rgba(242, 173, 24, 0.1);
}

[data-md-color-scheme="default"] {
  --md-primary-fg-color: var(--lenny-navy);
  --md-primary-fg-color--light: #1a2440;
  --md-primary-fg-color--dark: #0d1220;
  --md-accent-fg-color: var(--lenny-orange);
  --md-accent-fg-color--transparent: rgba(242, 173, 24, 0.1);
}

.md-typeset a {
  color: var(--lenny-orange);
}

.md-typeset a:hover {
  color: var(--lenny-red);
}

.md-header {
  background: linear-gradient(90deg, var(--lenny-navy) 0%, #1a2440 100%);
}

.md-tabs {
  background-color: var(--lenny-navy);
}

.md-button {
  background-color: var(--lenny-orange);
  color: var(--lenny-navy);
  border-color: var(--lenny-orange);
}

.md-button:hover {
  background-color: var(--lenny-red);
  border-color: var(--lenny-red);
  color: #fff;
}

.admonition.warning {
  border-left-color: var(--lenny-red);
}

.admonition.warning > .admonition-title {
  background-color: rgba(233, 51, 37, 0.1);
}
```

- [ ] **Step 4: Create image placeholder**

```bash
mkdir -p docs/assets/images && touch docs/assets/images/.gitkeep
```

- [ ] **Step 5: Install dependencies and test MkDocs**

Run:
```bash
python3 -m venv .venv
source .venv/bin/activate
pip install -r requirements.txt
mkdocs --version
```
Expected output includes `mkdocs, version 1.6.1` and no errors.

- [ ] **Step 6: Commit**

```bash
git add requirements.txt mkdocs.yml docs/assets/css/extra.css docs/assets/images/.gitkeep
git commit -m "feat: add MkDocs Material skeleton and Lenny styling"
```

---

### Task 3: Add Bilingual Landing Pages

**Files:**
- Create: `docs/index.md`
- Create: `docs/index.nl.md`

- [ ] **Step 1: Write English landing page `docs/index.md`**

```markdown
# Lenny Guides

Practical, opinionated guides for using AI coding agents safely and effectively.

## What is this?

Lenny Guides is a public knowledge base for students, teachers, and solo builders who want to work with AI coding agents without losing control of their projects, secrets, or sanity.

Each guide explains **why** a step matters, not just **what** to type. We prefer safe defaults, clear boundaries, and honest limits over hype.

## Start here

- [Student setup](./getting-started/student-setup.md) — Build a safe, mobile-friendly workspace with Herdr, Tailscale, Termux, StudyDD, and StateDD.

## Browse guides

- [Herdr](./guides/herdr.md) — terminal workspace for coding agents
- [Tailscale](./guides/tailscale.md) — private device network
- [Termux](./guides/termux.md) — terminal on Android
- [StudyDD](./guides/studydd.md) — repo-first learning workflow
- [StateDD](./guides/statedd.md) — repo-first project workflow

## Safety first

Before you let an agent touch code, money, or production data, read [Boundaries](./safety/boundaries.md).

---

*English is the canonical language. A Dutch translation is available via the language switcher.*
```

- [ ] **Step 2: Write Dutch landing page `docs/index.nl.md`**

```markdown
# Lenny Guides

Praktische, opinievolle gidsen voor het veilig en effectief gebruiken van AI-coding-agents.

## Wat is dit?

Lenny Guides is een publieke kennisbank voor studenten, leerkrachten en solobouwers die met AI-coding agents willen werken zonder controle te verliezen over hun projecten, geheimen of gezond verstand.

Elke gids legt uit **waarom** een stap belangrijk is, niet alleen **wat** je moet typen. We verkiezen veilige standaarden, duidelijke grenzen en eerlijke beperkingen boven hype.

## Start hier

- [Studentenopstelling](./getting-started/student-setup.nl.md) — Bouw een veilige, mobielvriendelijke werkplek met Herdr, Tailscale, Termux, StudyDD en StateDD.

## Gidsen

- [Herdr](./guides/herdr.nl.md) — terminalwerkruimte voor coding agents
- [Tailscale](./guides/tailscale.nl.md) — privé-netwerk tussen toestellen
- [Termux](./guides/termux.nl.md) — terminal op Android
- [StudyDD](./guides/studydd.nl.md) — repo-first leerworkflow
- [StateDD](./guides/statedd.nl.md) — repo-first projectworkflow

## Veiligheid eerst

Laat een agent geen code, geld of productiedata aanraken voor je [Grenzen](./safety/boundaries.nl.md) hebt gelezen.

---

*Engels is de canonieke taal. Een Nederlandse vertaling is beschikbaar via de taalschakelaar.*
```

- [ ] **Step 3: Verify pages render**

Run:
```bash
source .venv/bin/activate
mkdocs serve &
sleep 5
curl -s http://127.0.0.1:8000/ | head -20
curl -s http://127.0.0.1:8000/nl/ | head -20
kill %1
```
Expected: both requests return HTML containing the page titles.

- [ ] **Step 4: Commit**

```bash
git add docs/index.md docs/index.nl.md
git commit -m "feat: add bilingual landing pages"
```

---

### Task 4: Add Student Setup Guide

**Files:**
- Create: `docs/getting-started/student-setup.md`
- Create: `docs/getting-started/student-setup.nl.md`

- [ ] **Step 1: Write English guide `docs/getting-started/student-setup.md`**

```markdown
# Student Setup

Build a safe, mobile-friendly workspace for learning and building with AI coding agents.

## What you will build

- A Linux/macOS/WSL host with SSH and Git.
- A private Tailscale network so your phone can reach your host without public port forwarding.
- Herdr as a persistent terminal workspace for coding agents.
- Termux on Android as a mobile SSH client.
- StudyDD and StateDD workflows to keep learning and project state in the repo.

## Before you start

You need:

- A computer running Linux, macOS, or WSL.
- An Android phone.
- A GitHub or GitLab account.
- About 45 minutes for the first setup.

!!! warning "Do not expose SSH to the public internet"
    Use Tailscale or your local network. Never forward port 22 on your router for this setup.

## 1. Install Git and SSH on the host

### Debian / Ubuntu / WSL

```bash
sudo apt update
sudo apt install -y git openssh-server curl
sudo systemctl enable --now ssh || true
```

### Fedora

```bash
sudo dnf install -y git openssh-server curl
sudo systemctl enable --now sshd
```

### macOS

```bash
xcode-select --install
```

Then enable Remote Login in System Settings if you want SSH access.

## 2. Install Tailscale

On the host:

```bash
curl -fsSL https://tailscale.com/install.sh | sh
sudo tailscale up
```

Check:

```bash
tailscale status
tailscale ip -4
```

Optional Tailscale SSH (only if your tailnet policy allows it):

```bash
sudo tailscale up --ssh
```

## 3. Install Herdr

```bash
curl -fsSL https://herdr.dev/install.sh | sh
```

Start Herdr in your project directory:

```bash
cd ~/Projects/mijn-project
herdr
```

Detach without killing the session: press `Ctrl+B`, then `q`.
Stop the server deliberately with:

```bash
herdr server stop
```

## 4. Install a coding agent

Example: OpenCode.

```bash
curl -fsSL https://opencode.ai/install | bash
```

Or with npm:

```bash
npm install -g opencode-ai
```

Start it inside your project:

```bash
cd ~/Projects/mijn-project
opencode
```

## 5. Set up Termux on Android

Install:

- Tailscale from Google Play or the official download page.
- Termux, preferably from F-Droid.

Open Termux and update packages:

```bash
pkg update
pkg upgrade
pkg install -y openssh git nano
```

Test the connection to your host:

```bash
ssh gebruikersnaam@machine-naam
```

Or with a MagicDNS name:

```bash
ssh gebruikersnaam@studybox
```

Or with the Tailscale IP:

```bash
ssh gebruikersnaam@100.x.y.z
```

## 6. Make SSH easier

Create an SSH config in Termux:

```bash
mkdir -p ~/.ssh
nano ~/.ssh/config
```

Example:

```sshconfig
Host studybox
  HostName studybox
  User jouw-linux-gebruiker
  ServerAliveInterval 30
  ServerAliveCountMax 3
```

Then connect with:

```bash
ssh studybox
```

## 7. Open your Herdr workspace from your phone

```bash
ssh studybox
cd ~/Projects/mijn-project
herdr
```

You now see the same Herdr workspace as on your laptop.

!!! tip "Use your phone for short tasks only"
    Good for checking if an agent is blocked, giving quick approval, or reading a handoff. Large diffs, merges, and security decisions belong on a laptop.

## 8. Add StudyDD and StateDD to your repo

Create the minimal file set in your project root:

```text
study-project/
  AGENTS.md
  STATUS.md
  NEXT_ACTIONS.md
  BACKLOG.md
  WORKLOG.md
  PROJECT_STATE.yaml
  SOURCES.md
  REVIEW.md
  evidence/
    README.md
```

Minimum rule: the agent must read `AGENTS.md`, `STATUS.md`, `NEXT_ACTIONS.md`, and `BACKLOG.md` before it starts work.

## 9. First correct run assignment

Prove you can start and close an agent workflow safely:

1. Screenshot of Herdr with at least one agent pane.
2. Screenshot or text of `git status --short`.
3. Short `WORKLOG.md` entry.
4. An `evidence/` folder with `runtime.md`.
5. Answer: what is the single source of truth in your repo?
6. Answer: why is "the agent said it was done" not enough?

## What to do next

- Read the [Herdr](../../guides/herdr.md), [Tailscale](../../guides/tailscale.md), and [Termux](../../guides/termux.md) guides for deeper setup help.
- Read [StudyDD](../../guides/studydd.md) for repo-first learning.
- Read [StateDD](../../guides/statedd.md) for repo-first project work.
- Read [Safety Boundaries](../../safety/boundaries.md) before letting an agent touch real code.
```

- [ ] **Step 2: Write Dutch guide `docs/getting-started/student-setup.nl.md`**

Translate the English guide to Dutch, keeping the same sections, warnings, and code blocks. Ensure the relative links point to `.nl.md` pages where they exist.

Key Dutch terminology:

- source of truth = bron van waarheid
- workspace = werkruimte
- persistent = blijvend
- warning = waarschuwing
- tip = tip
- what to do next = wat nu?

- [ ] **Step 3: Verify internal links**

Run:
```bash
source .venv/bin/activate
mkdocs build --strict
```
Expected: build succeeds with no warnings about unresolved links.

- [ ] **Step 4: Commit**

```bash
git add docs/getting-started/student-setup.md docs/getting-started/student-setup.nl.md
git commit -m "feat: add student setup guide (EN + NL)"
```

---

### Task 5: Add Per-Topic Guide Stubs

**Files:**
- Create: `docs/guides/herdr.md`
- Create: `docs/guides/herdr.nl.md`
- Create: `docs/guides/tailscale.md`
- Create: `docs/guides/tailscale.nl.md`
- Create: `docs/guides/termux.md`
- Create: `docs/guides/termux.nl.md`
- Create: `docs/guides/studydd.md`
- Create: `docs/guides/studydd.nl.md`
- Create: `docs/guides/statedd.md`
- Create: `docs/guides/statedd.nl.md`

- [ ] **Step 1: Write English stubs**

Each stub follows this pattern:

```markdown
# <Topic>

Short description of what this tool/workflow does and why it matters for agent work.

## What <topic> does

One-paragraph explanation.

## When to use it

- Bullet list of scenarios.

## Basic setup

```bash
# key install or start command
```

## Safety notes

!!! warning "Title"
    Important boundary or risk.

## Common mistakes

- Mistake one
- Mistake two

## What to do next

- Link to related guide or next step.
```

Create all five English stubs:

- `docs/guides/herdr.md`
- `docs/guides/tailscale.md`
- `docs/guides/termux.md`
- `docs/guides/studydd.md`
- `docs/guides/statedd.md`

- [ ] **Step 2: Write Dutch stubs**

Translate each English stub to Dutch. Use the same structure. Create:

- `docs/guides/herdr.nl.md`
- `docs/guides/tailscale.nl.md`
- `docs/guides/termux.nl.md`
- `docs/guides/studydd.nl.md`
- `docs/guides/statedd.nl.md`

- [ ] **Step 3: Verify nav links resolve**

Run:
```bash
source .venv/bin/activate
mkdocs build --strict
```
Expected: build succeeds.

- [ ] **Step 4: Commit**

```bash
git add docs/guides/
git commit -m "feat: add per-topic guide stubs (EN + NL)"
```

---

### Task 6: Add Safety / Boundaries Page

**Files:**
- Create: `docs/safety/boundaries.md`
- Create: `docs/safety/boundaries.nl.md`

- [ ] **Step 1: Write English boundaries page `docs/safety/boundaries.md`**

```markdown
# Boundaries

A coding agent is a fast junior colleague in your terminal. It needs context, limits, and review.

## The non-negotiables

- **Repo first.** The Git repo is the source of truth. Herdr, StudyDD, and StateDD only organize work around it.
- **Plan before build.** For any non-trivial task, the agent proposes a plan before changing files.
- **Evidence before closure.** A task is not done because the agent says so. You need tests, screenshots, logs, or a clean diff.
- **No secrets in chat or Git.** API keys, tokens, and passwords do not belong in prompts, screenshots, or commits.
- **One task at a time.** Parallel agents only when they do not touch the same files or decisions.
- **Human stays reviewer.** The agent proposes; you accept or reject.

## What not to let the agent do

```text
- Paste API keys into Discord or prompts
- Commit .env files
- Take screenshots where tokens are visible
- Expose SSH via router port forwarding
- Run force push
- Change passwords
- Read production data without permission
- Run shell commands without you understanding them
```

## What to do instead

```text
- Use .env.example without real secrets
- Keep .gitignore clean
- Use Tailscale for private access
- Use small branches
- Read git diff before committing
- Treat agent output as a proposal, not truth
- Ask for evidence before closure
```

## Example .gitignore

```gitignore
.env
.env.*
!.env.example
*.key
*.pem
secrets/
node_modules/
.venv/
```

## Phone vs laptop

- **Phone:** good for checking status, short approvals, reading handoffs.
- **Laptop:** required for large diffs, merge conflicts, security decisions, complex refactors.

## Remember

Repo first. Plan first. Evidence before closure.
```

- [ ] **Step 2: Write Dutch boundaries page `docs/safety/boundaries.nl.md`**

Translate the English page to Dutch, keeping the same structure and code blocks.

- [ ] **Step 3: Verify build**

Run:
```bash
source .venv/bin/activate
mkdocs build --strict
```
Expected: build succeeds.

- [ ] **Step 4: Commit**

```bash
git add docs/safety/
git commit -m "feat: add safety boundaries page (EN + NL)"
```

---

### Task 7: Add GitHub Actions Deploy Workflow

**Files:**
- Create: `.github/workflows/pages.yml`

- [ ] **Step 1: Create workflow directory**

```bash
mkdir -p .github/workflows
```

- [ ] **Step 2: Write `.github/workflows/pages.yml`**

```yaml
name: Deploy Lenny Guides to GitHub Pages

on:
  push:
    branches:
      - main
  workflow_dispatch:

permissions:
  contents: read
  pages: write
  id-token: write

concurrency:
  group: pages
  cancel-in-progress: false

jobs:
  build:
    runs-on: ubuntu-latest
    steps:
      - name: Checkout
        uses: actions/checkout@v4

      - name: Set up Python
        uses: actions/setup-python@v5
        with:
          python-version: '3.12'

      - name: Install dependencies
        run: |
          python -m pip install --upgrade pip
          pip install -r requirements.txt

      - name: Build site
        run: mkdocs build --strict

      - name: Upload artifact
        uses: actions/upload-pages-artifact@v3
        with:
          path: site/

  deploy:
    needs: build
    runs-on: ubuntu-latest
    environment:
      name: github-pages
      url: ${{ steps.deployment.outputs.page_url }}
    steps:
      - name: Deploy to GitHub Pages
        id: deployment
        uses: actions/deploy-pages@v4
```

- [ ] **Step 3: Validate workflow syntax**

Run locally if `actionlint` is available:
```bash
command -v actionlint && actionlint .github/workflows/pages.yml || echo "actionlint not installed; skipping"
```
Expected: either passes or skips gracefully.

- [ ] **Step 4: Commit**

```bash
git add .github/workflows/pages.yml
git commit -m "ci: add GitHub Pages deploy workflow"
```

---

### Task 8: Final Local Build Verification

**Files:**
- Modify: `PROJECT_STATE.yaml`
- Modify: `STATUS.md`
- Modify: `NEXT_ACTIONS.md`
- Modify: `WORKLOG.md`

- [ ] **Step 1: Run strict build**

```bash
source .venv/bin/activate
mkdocs build --strict
```
Expected: exits 0, no warnings, `site/` directory created.

- [ ] **Step 2: Inspect generated site**

Run:
```bash
ls -1 site/
ls -1 site/nl/
```
Expected: `index.html`, `getting-started/`, `guides/`, `safety/` at top level; Dutch equivalents under `site/nl/`.

- [ ] **Step 3: Check for placeholder TODOs in rendered content**

Run:
```bash
grep -R "TODO\|TBD\|FIXME\|coming soon" site/ || echo "No placeholders found"
```
Expected: "No placeholders found" (stubs may say "expanded later", but not literal TODO/FIXME).

- [ ] **Step 4: Update `PROJECT_STATE.yaml`**

Change `current_state.site.status` to `built_locally`, `current_state.quality_gate.status` to `passed_locally`, `current_state.quality_gate.local_build` to `verified`, `current_state.quality_gate.strict_build` to `verified`.

- [ ] **Step 5: Update `STATUS.md`**

Add a line stating the local build passed and the deploy workflow is ready.

- [ ] **Step 6: Update `NEXT_ACTIONS.md`**

Mark LG-MVP-001 through LG-MVP-006 as done; keep LG-MVP-007 as active.

- [ ] **Step 7: Update `WORKLOG.md`**

Append the completed work.

- [ ] **Step 8: Commit**

```bash
git add PROJECT_STATE.yaml STATUS.md NEXT_ACTIONS.md WORKLOG.md
git commit -m "docs: update StateDD after local build verification"
```

---

### Task 9: Push and Confirm Pages Deployment

**Files:** none created; this is verification.

- [ ] **Step 1: Push to origin**

```bash
git push origin main
```
Expected: pushes cleanly with no rejected fast-forward.

- [ ] **Step 2: Enable GitHub Pages in repository settings**

If not already enabled, go to the repository settings on GitHub:
- Settings → Pages
- Source: GitHub Actions

This is a one-time manual step that cannot be done via this plan.

- [ ] **Step 3: Verify Actions workflow runs**

Open `https://github.com/lennertvhoy/Lenny_Guides/actions` and confirm the workflow runs.

- [ ] **Step 4: Confirm live URL**

Once the workflow is green, open `https://lennertvhoy.github.io/Lenny_Guides/` and `https://lennertvhoy.github.io/Lenny_Guides/nl/`.

Expected: both pages render, navigation works, no 404s on linked pages.

- [ ] **Step 5: Update StateDD after deployment**

Update `PROJECT_STATE.yaml`:
- `current_state.site.status` → `deployed`
- `current_state.deployment.status` → `deployed`
- `current_state.deployment.ci_status` → `verified`
- `current_state.quality_gate.status` → `passed`

Update `STATUS.md` with deployment snapshot.

Update `NEXT_ACTIONS.md`: remove LG-MVP-007; add next backlog item to review content quality.

Update `WORKLOG.md` with the deployment result.

- [ ] **Step 6: Final commit**

```bash
git add PROJECT_STATE.yaml STATUS.md NEXT_ACTIONS.md WORKLOG.md
git commit -m "docs: mark MVP deployed in StateDD"
git push origin main
```

---

## Self-Review

**Spec coverage:**
- MkDocs Material skeleton → Task 2
- Bilingual landing pages → Task 3
- Student setup guide → Task 4
- Per-topic guide stubs → Task 5
- Safety/boundaries page → Task 6
- GitHub Actions deploy → Task 7
- StateDD updates → Tasks 1, 8, 9
- Evidence (local build) → Task 8
- Custom CSS → Task 2
- No custom domain/analytics/backend → enforced by file choices and invariants

**Placeholder scan:** no TBD/TODO/FIXME in required steps. Stubs are allowed to say "expanded later" because they are explicitly scoped as stubs.

**Type consistency:** file paths and nav entries match `mkdocs.yml` and each other. English pages use `.md` links; Dutch pages use `.nl.md` links.
