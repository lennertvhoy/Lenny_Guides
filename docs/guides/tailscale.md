# Tailscale

Tailscale creates a private mesh network between your devices.

## What Tailscale does

Lets your phone connect to your host over an encrypted tunnel without public IP exposure.

## When to use it

- Remote SSH
- File sync
- Accessing services between devices

## Basic setup

```bash
curl -fsSL https://tailscale.com/install.sh | sh && sudo tailscale up
```

## Safety notes

!!! warning "Do not open port 22 on your router"
    Tailscale removes the need for port forwarding.

## Common mistakes

- Forgetting to authenticate the phone into the same tailnet
- Relying on MagicDNS names before they propagate

## What to do next

- Read the [Termux guide](termux.md) and the [student setup guide](../getting-started/student-setup.md)
