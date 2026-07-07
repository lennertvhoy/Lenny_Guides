# Termux

## One-line summary

Termux turns your Android phone into a small Linux terminal so you can SSH into your study machine and check Git repos on the go.

## What it does

Termux is a Linux-like terminal environment for Android. It gives you tools you normally find on a laptop:

- `ssh` to log into remote machines like your study server.
- `git` to inspect repos, pull changes, and review handoffs.
- `nano` or other editors for tiny edits.
- Standard Unix commands (`ls`, `cd`, `cat`, `grep`) so your muscle memory still works.

You do not get a full desktop or IDE, but you do get enough to stay unblocked when you only have your phone.

## When to use it

Use Termux for short, low-risk tasks away from your laptop:

- Running `git status` or `git log` on `~/Projects/study-project`.
- Approving a small change after reading the handoff.
- Checking whether a long-running job on `studybox` finished.
- Reading logs or config files on the go.

Avoid it for large refactors, complicated merges, or anything that needs fast typing or a big screen.

## What you need

- An Android phone or tablet (`student-phone`).
- Termux installed from F-Droid, not from Google Play. The Play Store version is old and often broken.
- Your study machine (`studybox`) reachable over the network, with SSH already enabled.
- Your SSH private key copied to the phone, or password access temporarily enabled.
- About fifteen minutes for the first setup.

## Install and verify

1. Install Termux from [F-Droid](https://f-droid.org/packages/com.termux/).
2. Open Termux and update the package list:

   ```bash
   pkg update && pkg upgrade -y
   ```

   This downloads package indexes and upgrades installed base packages.

3. Install the tools you need:

   ```bash
   pkg install -y openssh git nano
   ```

   `openssh` gives the `ssh` and `ssh-keygen` commands, `git` lets you work with repos, and `nano` is a simple editor.

4. Verify each tool:

   ```bash
   ssh -V
   git --version
   nano --version
   ```

   Each command should print a version number. If any prints "command not found", run the install command again.

## Your first real workflow

Goal: from your phone, log into `studybox` as `lenny` and run `git status` in `~/Projects/study-project`.

### 1. Create or copy an SSH key

If you already have an SSH key pair you trust, copy the private key (for example `id_ed25519`) into Termux's home directory at `~/.ssh/`. If you do not have one, generate it on the phone:

```bash
ssh-keygen -t ed25519 -C "lenny@student-phone"
```

When asked for a file, press Enter to accept the default `~/.ssh/id_ed25519`. When asked for a passphrase, choose one you can type on a phone, or press Enter twice for no passphrase. A passphrase is safer; no passphrase is more convenient.

### 2. Add the public key to `studybox`

Display the public key:

```bash
cat ~/.ssh/id_ed25519.pub
```

Copy the printed line and append it to `~/.ssh/authorized_keys` on `studybox` for user `lenny`. If you are doing this from the laptop, the line looks like:

```text
ssh-ed25519 AAAAC3NzaC... lenny@student-phone
```

If you skip this step, you can still log in with a password, but keys are safer and faster.

### 3. Connect to `studybox`

```bash
ssh lenny@studybox
```

If this is the first connection, Termux asks you to confirm the host fingerprint. Type `yes`. If you set a passphrase, enter it now. You should land at a prompt on `studybox`.

### 4. Check the project

```bash
cd ~/Projects/study-project
git status
```

You should see output like:

```text
On branch main
Your branch is up to date with 'origin/main'.

nothing to commit, working tree clean
```

When you are done, log out:

```bash
exit
```

You are now back on the phone. Keep this workflow: open Termux, SSH to `studybox`, run a quick Git or status command, then disconnect.

## Common problems and fixes

| Symptom | Cause | Fix |
| --- | --- | --- |
| `ssh: command not found` | OpenSSH is not installed. | Run `pkg install -y openssh`. |
| `Connection refused` on `ssh lenny@studybox` | SSH server on `studybox` is not running or a firewall blocks port 22. | Start the SSH service on `studybox` and confirm the same network. |
| `Permission denied (publickey)` | The public key is missing on `studybox` or the private key permissions are wrong. | Add the public key to `~/.ssh/authorized_keys` on `studybox`; on the phone run `chmod 600 ~/.ssh/id_ed25519`. |
| Termux closes immediately after opening | Battery optimization or an old Play Store install. | Reinstall from F-Droid and disable battery optimization for Termux in Android settings. |
| `git status` shows weird line endings | Git's `core.autocrlf` is set differently on the phone and laptop. | Set `git config --global core.autocrlf input` on `studybox`. |
| Special characters type wrong on the phone keyboard | The virtual keyboard sends different key codes. | Use a programmer keyboard like Hacker's Keyboard, or paste long commands (unverified). |
| Copy-paste from Termux does not work | Android version or Termux build issue. | Long-press the screen and choose Paste; update Termux through F-Droid if it fails (unverified). |

## Safety notes

!!! warning "Phone is for short tasks"
    Large diffs, security decisions, and risky operations belong on a laptop.

- Keep your SSH private key (`~/.ssh/id_ed25519`) on the phone only if the phone is encrypted and has a lock screen.
- Do not store passwords or API keys in plain-text notes on the phone.
- If you lose the phone, remove that device's key from `~/.ssh/authorized_keys` on `studybox`.
- On public Wi-Fi, prefer connecting through a VPN or Tailscale rather than exposing port 22 to the internet.
- Always read a command before pasting it; virtual keyboards make typos easy and hard to spot.

## Phone vs laptop

| Task | Phone with Termux | Laptop |
| --- | --- | --- |
| `git status`, `git log` | Good | Good |
| Reading handoffs | Good | Good |
| Writing or reviewing large diffs | Hard | Easy |
| Running tests or builds | Slow or impossible | Easy |
| Typing long commands | Error-prone | Comfortable |
| Secure key handling | Riskier if phone is unlocked | Usually safer |

Treat Termux as a read-only or quick-check companion, not a replacement for your main development machine.

## What to read next

- Read the [Herdr guide](herdr.md) for running background jobs on your study machine.
- Read the [Tailscale guide](tailscale.md) for reaching `studybox` safely from anywhere without opening ports.
