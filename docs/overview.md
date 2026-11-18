Docs · Overview

# Connect your first server

On the server, as the user the app will log in as, run:

```
curl -fsSL https://cookbox.ai/install | sh
```

1. The installer asks what to set up: choose **Connect the cookbox.ai app**. It prints a QR code.
2. In the app, tap **New connection**, choose **Runs on** and **Network** (see below), then tap **Scan server QR**.
3. The server shows the key it received: check it, confirm, and wait for it to finish. Then tap **Save**, and compare the host-key fingerprint the first time you connect.
4. Open a session. For an agent, install it and sign in once on the server (`claude`, `codex` or `cursor-agent login`); the app finds it.

## Which network to pick

- **VPS** with a public address: **Direct** is free and simplest. **WireGuard** (free), **AmneziaWG** or **Tailscale** keep SSH off the public internet.
- **PC or Mac** behind a home router: the **cookbox.ai relay** needs nothing opened on the router; **Tailscale** suits you if you already use it.
- AmneziaWG, Tailscale and the relay come with the one-time [premium](https://cookbox.ai/premium) unlock. Direct and WireGuard are free.

## What you need

- Android 7.0 or newer, or iOS 15 or newer.
- A Linux server you can reach by SSH, with `python3`. WSL 2 works; macOS is a reduced preview.
- Nothing else: the installer adds tmux, moshpit and the network route. Run it again any time to update.

## Good to know

- There is no cookbox.ai account, and your SSH private key never leaves the phone. See the [privacy policy](https://cookbox.ai/privacy).
- Notifications ride on polling: a turn that starts and finishes between two polls can be missed, and Android Doze applies.
- Uploads stay in `~/.cookbox/uploads` on the host until you remove them.

cookbox.ai-created Codex sessions run with `--yolo`, which disables Codex approvals and sandboxing. If that policy does not suit a host, start Codex there yourself and let cookbox.ai attach to the session instead.
[Every setup option: server setup →](https://cookbox.ai/docs/server-setup)[Navigation cheat sheet →](https://cookbox.ai/docs/navigation)
