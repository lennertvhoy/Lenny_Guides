# Herdr

Herdr is a terminal multiplexer designed for coding agents. It keeps panes and agents alive across disconnects.

## What Herdr does

Maintains persistent terminal workspaces with visible agent status.

## When to use it

- Long-running agent tasks
- Remote work
- Switching between laptop and phone

## Basic setup

```bash
curl -fsSL https://herdr.dev/install.sh | sh
```

## Safety notes

!!! warning "Detach is not stop"
    Detaching with Ctrl+B q keeps the session running. Use `herdr server stop` to shut down deliberately.

## Common mistakes

- Running tmux inside Herdr confuses status detection
- Stopping the terminal instead of detaching kills work

## What to do next

- Read the [Tailscale guide](tailscale.md) and the [student setup guide](../getting-started/student-setup.md)
