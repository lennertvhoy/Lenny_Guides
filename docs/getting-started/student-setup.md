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

- Read the [Herdr](../guides/herdr.md), [Tailscale](../guides/tailscale.md), and [Termux](../guides/termux.md) guides for deeper setup help.
- Read [StudyDD](../guides/studydd.md) for repo-first learning.
- Read [StateDD](../guides/statedd.md) for repo-first project work.
- Read [Safety Boundaries](../safety/boundaries.md) before letting an agent touch real code.
