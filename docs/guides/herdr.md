# Herdr

## One-line summary

Herdr keeps your terminal workspace—agents, servers, and shells—running on a host so you can detach, move to another device, and reattach without losing work.

## What it does

Herdr is a terminal multiplexer built for coding agents. It runs as a background server on your machine. Every pane, tab, and workspace lives in that server, not in the terminal window you are looking at. When you close your laptop, shut your terminal, or switch from laptop to phone, the server keeps the processes alive. When you open a new terminal and run `herdr`, you reconnect to the same session.

Herdr also shows the state of each coding agent in a sidebar: `working`, `blocked`, `done`, or `idle`. This helps you see which agent needs attention without clicking through every terminal window.

You can think of it as tmux redesigned for AI agents: persistent sessions, pane splits, and remote reattach, plus awareness of what your agents are doing.

## When to use it

- **Long-running agent tasks** — Start a refactor or test run on `studybox`, detach, and check progress later from your phone.
- **Remote work** — Leave agents running on a home server or VM and attach from anywhere over SSH.
- **Switching between laptop and phone** — Start work on your laptop, continue approvals or status checks from `student-phone`.
- **Running several agents at once** — Keep one agent per pane and use the sidebar to see which one is blocked.
- **Protecting work from accidental closes** — Closing a terminal tab detaches you; it does not kill the session.

## What you need

- A computer running Linux, macOS, or WSL. This guide calls it `studybox`.
- An Android phone for the reattach step. This guide calls it `student-phone`.
- A working SSH connection from the phone to `studybox` over Tailscale or your local network.
- About 15 minutes.
- Basic terminal skills: running commands, pressing key combinations, and editing files.
- The example username on `studybox` is `lenny`. Replace it with your actual username when you run `ssh lenny@studybox`.

If you have not set up SSH or Tailscale yet, follow the links at the end of this guide first.

## Install and verify

These commands run on `studybox`.

1. Download and run the official installer.

    ```bash
    curl -fsSL https://herdr.dev/install.sh | sh
    ```

    `curl -fsSL` fetches the install script silently and follows redirects. The pipe `| sh` runs it in your current shell. The installer downloads a single binary and places it in a directory on your `PATH`, usually `~/.local/bin`.

2. Reload your shell so it sees the new binary.

    ```bash
    exec $SHELL -l
    ```

    `exec` replaces the current shell with a fresh one. The `-l` flag makes it a login shell, which re-reads your profile and `PATH`.

3. Check that Herdr is installed.

    ```bash
    herdr --version
    ```

    Expected output is a version number, for example:

    ```text
    herdr 0.6.6
    ```

    The exact version number may differ.

4. Make sure the Herdr server is not already running from an older install.

    ```bash
    herdr status
    ```

    If no server is running, it reports that. If a server is running, it shows the version and session state.

## Your first real workflow

In this workflow you will start Herdr, run a small web server in one pane, detach, reattach from your phone, and stop everything cleanly.

### 1. Start Herdr from your project directory

```bash
cd ~/Projects/study-project
herdr
```

`cd` changes to your project folder. `herdr` starts the background server and opens the terminal user interface (TUI). If a session already exists, it reattaches instead.

You should see a sidebar on one side and a single pane with your shell prompt.

### 2. Split the pane and start a simple server

Press `Ctrl+B`, release, then press `v`.

This creates a second pane to the right. The combination `Ctrl+B` is called the *prefix*. Herdr waits for the prefix, then interprets the next key as a command.

In the new pane, start a small web server:

```bash
python3 -m http.server 8080
```

This serves the current directory on port `8080`. It keeps running, which is what we want to test.

### 3. Detach and leave the server running

Press `Ctrl+B`, release, then press `q`.

The Herdr TUI closes. Your terminal returns to the normal shell. The server is still running inside the Herdr server on `studybox`.

Verify that the server is still responding:

```bash
curl http://localhost:8080
```

You should see HTML listing the files in `~/Projects/study-project`. If the directory is empty, you see an empty directory listing. The important thing is that `curl` gets a response.

### 4. Reattach from your phone

On `student-phone`, open Termux and SSH into `studybox`:

```bash
ssh lenny@studybox
```

Then reattach to the same session:

```bash
cd ~/Projects/study-project
herdr
```

You see the same two panes. The web server is still running on the right. This is the same session you left on the laptop.

### 5. Stop the server and the Herdr session

In the pane running the web server, press `Ctrl+C` to stop `python3`.

Then, in any pane, stop the Herdr server:

```bash
herdr server stop
```

`herdr server stop` shuts down the background server and all its panes. It is the deliberate way to end a session. Detaching with `Ctrl+B q` leaves everything running; stopping the server ends it.

After stopping, confirm there is no running session:

```bash
herdr status
```

Expected output indicates that no server is running.

## Common problems and fixes

| Symptom | Likely cause | Fix |
|---|---|---|
| `herdr: command not found` after install | The new `~/.local/bin` directory is not in your current shell's `PATH`. | Restart the terminal or run `exec $SHELL -l`. (unverified) |
| The Herdr TUI shows a black screen or garbled text | Your terminal is too small or does not support truecolor. | Resize the terminal to at least 80 columns by 24 rows and use a modern terminal such as Ghostty, WezTerm, iTerm2, Windows Terminal, or GNOME Terminal. (unverified) |
| `Ctrl+B q` does nothing | The keys are being pressed together instead of as a sequence. | Press `Ctrl+B`, release both keys, then press `q`. (unverified) |
| The sidebar shows `tmux` instead of my agent | Your shell automatically enters tmux inside the Herdr pane, so Herdr sees tmux as the foreground process. | Disable tmux auto-attach for that shell, or launch the agent directly without wrapping it in tmux. (unverified) |
| An agent is asking a question but the sidebar shows `idle` | Herdr's screen manifest has not learned that agent's prompt shape yet. | Run `herdr agent explain <target>` to inspect the detection, then update Herdr or report the prompt shape. (unverified) |
| Processes disappear after I run `herdr server stop` | `server stop` deliberately ends the session and its panes. | Use `Ctrl+B q` to detach when you only want to disconnect. Only use `herdr server stop` when you want everything to shut down. (unverified) |
| `herdr --remote studybox` fails to install Herdr on the remote host | The remote shell is non-interactive, the platforms do not match, or `~/.local/bin` is not on the remote `PATH`. | Install Herdr manually on `studybox` first, or set `HERDR_REMOTE_BINARY` to a local binary path before running the command. (unverified) |
| The phone SSH client cannot send `Ctrl+B` | The on-screen keyboard lacks a control key, or the SSH client intercepts the shortcut. | Use the SSH client's Ctrl key button, or connect a Bluetooth keyboard, or use the mouse or touch UI if your client supports it. (unverified) |

These fixes come from the official Herdr documentation and GitHub issue discussions. They are marked `(unverified)` because I did not run them on this machine.

## Safety notes

!!! warning "Detach is not stop"
    Pressing `Ctrl+B q` detaches your client but leaves the Herdr server and all panes running. Use `herdr server stop` only when you deliberately want to end the session.

- **Do not run tmux inside a Herdr pane.** It confuses agent detection and makes the sidebar show `tmux` instead of the actual agent.
- **Do not leave sensitive agent sessions unattended.** Herdr keeps agents running even when you are not attached. Lock your device when you step away.
- **Stop the server before long breaks.** If you are done for the day, run `herdr server stop` so agents are not left running unsupervised.
- **Keep Herdr updated.** Run `herdr update` periodically for installs managed by Herdr's installer, or update through your package manager. (unverified)

## Phone vs laptop

| Task | Laptop | Phone |
|---|---|---|
| Starting a session and splitting panes | Easy. Keyboard and mouse both work well. | Hard. Small screen and virtual keyboard make precise pane work difficult. |
| Checking if an agent is blocked or done | Good. | Good. Herdr's sidebar fits narrow screens. |
| Approving a single prompt | Good. | Good. One tap or short command. |
| Reading handoffs or logs | Good. | Good for short checks. |
| Running long commands or editing files | Good. | Hard. Typing complex commands on a virtual keyboard is error-prone. |
| Security decisions | Good. | Avoid. Large diffs, merges, and approval of destructive actions belong on a laptop. |

Use your phone for quick status checks and approvals. Do the heavy setup, coding, and review work on your laptop.

## What to read next

- [Tailscale guide](tailscale.md) — connect your phone to `studybox` without exposing SSH to the internet.
- [Termux guide](termux.md) — turn `student-phone` into an SSH client for Herdr.
