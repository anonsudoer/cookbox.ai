Guide · English · October 15, 2026

# Control coding agents from Android over SSH

A practical cookbox.ai walkthrough for Claude Code, Codex, Cursor and shell sessions on your own Linux host.

cookbox.ai is an Android SSH terminal for coding agents already running on machines you control. The agent executes on the host; the phone lets you send prompts, resume sessions, inspect files and open forwarded web previews.

## Before you start

- An Android phone with cookbox.ai from [Google Play](https://play.google.com/store/apps/details?id=ai.cookbox.app).
- A host you are authorized to administer and can reach over SSH. Linux is the full target; SSH must terminate inside Linux for WSL 2. macOS is a reduced preview.
- A multiplexer: tmux, zellij or herdr. Setup adds tmux when the host has none and installs nothing else.
- Your chosen agent CLI installed and authenticated on the host. Its subscription and permissions remain separate from cookbox.ai.

## 1. Prepare and pair your host

In the app, choose **New connection**, the host type and the route you want. On the host, run:

```
curl -fsSL https://cookbox.ai/install | sh
```
This downloads and runs the installer; its first choice, the default, pairs with the app. Only run it on a machine you administer, and review the script or the [full setup guide](https://cookbox.ai/docs/server-setup) first if you want to inspect the steps.

Scan the QR code with the app. The host shows the proposed installer actions and the public key it received. Compare that key before confirming, save the connection and verify the SSH host-key fingerprint on first connection. A live pairing QR is sensitive and expires after five minutes; do not include it in screenshots.

Direct setup and existing connections are free. Guided AWG 3.1 and Tailscale setup use the same permanent premium unlock. They do not include a VPS or an agent subscription.

## 2. Open or resume a session

Connect to the saved host. Pick the multiplexer and an existing session, or choose the installed agent and create a session in your working directory. Existing herdr workspaces are discovered where a herdr server is already running. You can keep agent sessions and ordinary shells side by side.

The terminal is the actual command-line interface on that host. Send a prompt the way you would from your desktop. The key bar supplies modifiers and terminal keys; [the navigation cheat sheet](https://cookbox.ai/docs/navigation) explains taps, long presses and the orange armed state.

## 3. Inspect the result from the phone

Open Files to browse the working directory. Use View to forward a discovered TCP service and inspect a web preview. If the host already runs PM2, its panel exposes supported process controls; probing for it does not start a new PM2 daemon.

Agent state and usage appear when the host exposes enough information. Notifications cover sessions being watched by the phone after it has observed work begin. Android background restrictions and lost connectivity can delay them.

## 4. Leave the host working

Detach when you want to leave work running. Explicitly closing a session is a different action. The multiplexer and agent still need a powered, running host; the phone does not migrate work off a sleeping or failed machine. [Read about persistence and reconnects](https://cookbox.ai/guides/persistent-agent-sessions).

## If the first connection fails

- Check the address, SSH port, host login and key installation.
- For a tailnet route, confirm the companion app and the host are connected to the same network.
- If UDP roaming is unavailable, plain SSH is the expected fallback. A proxy or restrictive network can permit SSH while blocking UDP.
- Read the app diagnostics and the host setup guide. Remove secrets, addresses and live pairing codes before sharing logs.

[Where keys and terminal data go](https://cookbox.ai/guides/ssh-keys-and-data-flow) explains the trust boundaries.

[All guides](https://cookbox.ai/guides)[Server setup](https://cookbox.ai/docs/server-setup)[Get it on Google Play](https://play.google.com/store/apps/details?id=ai.cookbox.app)
