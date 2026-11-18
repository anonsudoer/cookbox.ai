# cookbox.ai

**Control your AI coding agents from your phone, over SSH.**

cookbox.ai connects straight to Claude Code, Codex, Cursor and shell sessions on servers you own. Resume tmux, zellij or herdr sessions, send prompts, browse and upload files, and open forwarded web previews. There is no cookbox.ai account, and your SSH private key never leaves the phone.

This repository holds the user documentation and the downloadable releases. The app's source code is not published here.

## Get the app

- **Android:** [Google Play](https://play.google.com/store/apps/details?id=ai.cookbox.app), or the APK from [Releases](../../releases/latest) for phones without Google Play.
- **Linux PC:** the `.deb` from [Releases](../../releases/latest) (x86_64): `sudo apt install ./cookbox-v*-amd64.deb`
- **iPhone:** TestFlight beta. [Request an invite](https://cookbox.ai/?signup=ios).

## Connect a server

On the server, as the user the app will log in as:

```sh
curl -fsSL https://cookbox.ai/install | sh
```

Choose **Connect the cookbox.ai app**, then scan the QR code it prints from **New connection** in the app. The [overview](docs/overview.md) has the four steps and which network to pick.

## Docs

- [Overview: connect your first server](docs/overview.md)
- [Server setup: every option](docs/server-setup.md)
- [Navigation cheat sheet](docs/navigation.md)
- [Premium](docs/premium.md)
- Guides: [control agents from Android](docs/guides/control-ai-agents-from-android.md) · [persistent sessions](docs/guides/persistent-agent-sessions.md) · [SSH keys and data flow](docs/guides/ssh-keys-and-data-flow.md)

The same pages, in English, German and Russian, are at [cookbox.ai/docs](https://cookbox.ai/docs/overview).

## Help

[hello@cookbox.ai](mailto:hello@cookbox.ai) · [Telegram](https://t.me/cookboxai) · [X](https://x.com/cookboxai)

Never post private keys, live QR codes, relay keys, purchase tokens or private host details in an issue.
