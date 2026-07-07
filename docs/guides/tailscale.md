# Tailscale

## One-line summary

Tailscale connects your devices into a single private network, so your phone can reach your host from anywhere without opening ports on your router.

## What it does

Tailscale is a mesh VPN. It installs a small daemon on each device and links them into a private "tailnet." Every device gets a stable `100.x.y.z` IP address and, if MagicDNS is enabled, a name like `studybox`. Traffic between your devices is encrypted end-to-end. You do not need a public IP address, a static IP, or port forwarding.

For this guide, that means `student-phone` can SSH into `studybox` over the internet as if they were on the same desk.

## When to use it

- **Remote SSH** — Connect from your phone, a friend's laptop, or a VM back to `studybox`.
- **File sync or Git push** — Work with repositories on `studybox` from another device.
- **Accessing local services** — Reach a dev server, notebook, or API running on `studybox` from your phone.
- **Working with Herdr from outside** — Leave agents running on `studybox` and reattach from `student-phone`.
- **Avoiding port forwarding on your router** — You do not have to forward port 22 to the internet.

You can skip Tailscale when both devices are on the same trusted local network and you already know the local IP. Even then, Tailscale keeps working if you move to a different network.

## What you need

- A computer running Linux, macOS, or WSL. This guide calls it `studybox`.
- An Android phone. This guide calls it `student-phone`.
- Admin rights on `studybox` so you can run `sudo`.
- A Tailscale account. You can sign up with Google, Microsoft, GitHub, or an email address.
- About 15 minutes.
- The example username on `studybox` is `lenny`. Replace it with your actual username when you see `lenny@studybox`.
- An SSH server running on `studybox`. If you have not installed one, follow the [student setup guide](../getting-started/student-setup.md) first.
- An SSH client on `student-phone`. This guide uses Termux; see the [Termux guide](termux.md) for setup.

## Install and verify

These steps run on `studybox` unless noted.

### 1. Install Tailscale

```bash
curl -fsSL https://tailscale.com/install.sh | sh
```

`curl -fsSL` downloads the install script silently and follows redirects. `| sh` runs the script, which adds Tailscale's package repository and installs the `tailscale` command.

### 2. Start Tailscale and log in

```bash
sudo tailscale up
```

`sudo tailscale up` starts the Tailscale daemon and asks you to authenticate. A link appears in the terminal. Open it in a browser, sign in to your Tailscale account, and approve the device. When you are done, the terminal shows a success message.

If you are on a headless server, use:

```bash
sudo tailscale up --operator=$USER
```

`--operator=$USER` lets your own user manage Tailscale without typing `sudo` for every later command. You still need `sudo` for the first run.

### 3. Check that Tailscale is running

```bash
tailscale status
```

Expected output lists your machine and its connection state, for example:

```text
100.x.y.z   studybox            lenny@       linux   -
```

The exact IP and name may differ. If the state shows `NeedsLogin`, the authentication step is not finished yet.

### 4. Find your Tailscale IP

```bash
tailscale ip -4
```

Expected output is a single address, for example:

```text
100.x.y.z
```

This is the address you will use from `student-phone` to reach `studybox`. Write it down or save it in your password manager.

### 5. Install Tailscale on your phone

On `student-phone`:

1. Install the Tailscale app from the Google Play Store or Tailscale's download page.
2. Open the app and sign in with the **same account** you used on `studybox`.
3. Leave the slider on to keep Tailscale connected.

The phone now has its own Tailscale IP and can see other devices in the same tailnet.

### 6. Verify both devices see each other

On `studybox`, ping the phone's Tailscale IP:

```bash
ping -c 3 <phone-tailscale-ip>
```

Replace `<phone-tailscale-ip>` with the address shown in the Tailscale app on `student-phone`.

On `student-phone`, ping `studybox`:

```bash
ping -c 3 <studybox-tailscale-ip>
```

A few replies on each side mean the tailnet is working. If pings fail, continue to the troubleshooting table below.

## Your first real workflow

Make sure you have already completed [Install and verify](#install-and-verify) on both devices. You should know `studybox`'s Tailscale IP (we use the placeholder `100.x.y.z`) and both devices should appear in `tailscale status`.

### 1. SSH from your phone

Open Termux on `student-phone` and run:

```bash
ssh lenny@100.x.y.z
```

Replace `100.x.y.z` with `studybox`'s Tailscale IP and `lenny` with your username on `studybox`.

If this is the first time you connect, SSH asks you to confirm the host key. Type `yes` and press Enter. Then type your `studybox` password or use your SSH key.

### 2. Prove you are on `studybox`

In the SSH session from your phone, run one or both of:

```bash
whoami
hostname
```

`whoami` should print `lenny` (or your `studybox` username). `hostname` should print `studybox`. This confirms the command is running on the host, not on your phone.

### 3. Verify both sides see each other

While the SSH session is open, run:

```bash
tailscale status
```

You should see `studybox` and `student-phone` in the list, each with a state such as `direct` or `relay`.

Back on `studybox`, in a separate terminal, run:

```bash
tailscale status
```

Both devices should appear here too.

## Common problems and fixes

The fixes below are based on Tailscale documentation and common issue reports. I have not tested each one on this machine.

| Symptom | Likely cause | Fix |
|---|---|---|
| `tailscale up` hangs or says `NeedsLogin` | A captive portal, VPN, or firewall is blocking Tailscale's login servers. | Complete authentication in a browser on the same machine, or run `sudo tailscale up` from a graphical terminal. If headless, run `sudo tailscale up --operator=$USER`: `--operator` sets management rights so later commands do not need `sudo`, and the printed link is what you open on another device to log in. |
| Phone and `studybox` do not appear in each other's `tailscale status` | They are signed in to different Tailscale accounts or different tailnets. | Sign out of Tailscale on the phone and sign back in with the exact same account used on `studybox`. |
| `ssh lenny@100.x.y.z` fails with `Connection refused` or `Connection timed out` | The SSH server is not running on `studybox`, the host firewall blocks port 22, or Tailscale ACLs block access. | On `studybox`, run `sudo systemctl status ssh` (or `sshd`) to check the SSH server. Also run `tailscale status` to confirm `studybox` is connected. Check your Tailscale admin console ACLs to allow port 22 between devices. |
| `tailscale status` shows the device but `ping` does not work | A firewall or router is blocking UDP hole punching, so Tailscale cannot form a direct path. | Make sure outbound UDP is allowed and port `41641` is not blocked. As a fallback, Tailscale will use a DERP relay automatically, which may be slower but should still work. |
| The MagicDNS name `studybox` does not resolve | MagicDNS is disabled or the DNS change has not reached the phone yet. | Use the Tailscale IP (`100.x.y.z`) instead of the name, or enable MagicDNS in your Tailscale admin console and wait a minute. Depending on admin settings, the full MagicDNS name may include a tailnet suffix such as `studybox.<tailnet>.ts.net`. |
| Tailscale disconnects when the phone screen turns off | Android battery optimization is pausing the Tailscale app. | Open Android settings, find Tailscale in Apps, and disable battery optimization for it. |
| `tailscale: command not found` after installation | The installer did not update your current shell's `PATH`, or the install failed. | Open a new terminal or run `exec $SHELL -l` to reload your profile. If the command is still missing, re-run the installer and read the output for errors. |
| Enabling Tailscale SSH prevents normal SSH key login over the Tailscale IP | Tailscale SSH takes over SSH authentication on the Tailscale interface. | Use the Tailscale IP with `tailscale ssh user@host`, or connect via the local LAN IP if you need your existing SSH keys. |

## Safety notes

!!! warning "Do not open port 22 on your router"
    Tailscale removes the reason to forward SSH to the public internet. Keep port 22 closed on your router and use Tailscale instead.

- **Use the same Tailscale account on every device.** A device in a different tailnet cannot reach `studybox`.
- **Keep Tailscale updated.** Run `sudo tailscale update` on `studybox` when available, or update through your package manager. (unverified)
- **Do not share your tailnet with people you do not trust.** Use Tailscale ACLs to limit which devices can reach which ports.
- **Lock your phone when you step away.** Once connected, your phone has access to `studybox`.
- **Prefer the Tailscale IP in scripts.** MagicDNS names are convenient but can take a moment to propagate. IPs are immediate and stable.
- **Turn Tailscale off on public Wi-Fi if you do not need it.** This reduces the chance that a compromised network can interact with your tailnet.

## Phone vs laptop

| Task | Laptop (`studybox`) | Phone (`student-phone`) |
|---|---|---|
| Installing Tailscale and authenticating | Easy. Full browser and terminal access. | Easy through the Tailscale app, but typing account passwords on a small keyboard is slower. |
| Finding the Tailscale IP | Easy. Run `tailscale ip -4`. | Easy. Shown in the Tailscale app. |
| Configuring SSH and testing login | Easy. Keyboard and terminal make commands simple. | Hard. Typing long commands and host keys on a virtual keyboard is error-prone. |
| Daily connection | Good. Start Tailscale and SSH or Herdr normally. | Good for short checks once Termux and SSH config are set up. |
| Running troubleshooting commands | Good. Full terminal and copy-paste. | Hard. Complex output is difficult to read and edit on a small screen. |
| Security decisions | Good. | Avoid. Large changes and approving destructive actions belong on the laptop. |

Use the laptop for setup, troubleshooting, and heavy work. Use the phone for quick status checks and approvals.

## What to read next

- [Herdr guide](herdr.md) — leave coding agents running on `studybox` and reattach from `student-phone`.
- [Termux guide](termux.md) — turn `student-phone` into a usable SSH client.
