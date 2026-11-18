Guide · English · October 15, 2026

# Where cookbox.ai keys and terminal traffic go

The Android, SSH host, agent-provider, QR-pairing and Google Play trust boundaries in cookbox.ai.

cookbox.ai is a client for hosts you control. The useful privacy claim is specific: cookbox.ai-operated services do not relay your SSH terminal session. That does not remove the access your chosen host and agent provider need to do their work.

## The normal terminal connection

1. The Android app holds the SSH private key.
2. Your host accepts the corresponding public key for the SSH user you chose.
3. The app connects directly to that host over SSH. Optional moshpit carries terminal traffic over its negotiated transport when available.
4. The agent runs on the host under your account and sends requests to its own provider according to that provider's configuration.

Compare the SSH host-key fingerprint on first connection. cookbox.ai pins host keys and refuses unexpected changes. Investigate a changed fingerprint through a trusted route; do not accept a new identity solely to make an error disappear.

## In-app tunnels

The optional AmneziaWG tunnel runs inside the app to your own server; nothing else sees it. The optional Tailscale tunnel makes the app a device in your tailnet, so Tailscale's coordination service knows about it as it knows any device of yours, while terminal traffic stays end-to-end encrypted between the app and your host.

## QR pairing is a separate temporary service

Optional pairing uses a cookbox.ai Cloudflare service to exchange connection details, installer choices and the phone's public key. It does not exchange the SSH private key or carry the terminal session. Pairings expire after five minutes.

A live QR code is a temporary pairing credential: anyone who can scan it may be able to pair with that host. Do not publish it in a tutorial, demo or support screenshot. Compare the public key printed by the host before confirming installation.

## Purchases are separate from terminal traffic

Google Play or the App Store handles purchases. The cookbox.ai receipt verifier receives the store's purchase token to validate access; it does not need the host's SSH key, prompts, repository files or terminal contents. A permanent premium purchase unlocks guided AWG 3.1 and Tailscale setup. Existing connections remain usable without that purchase.

## Your host and your agent still matter

Use an SSH account with the permissions your work needs. Agent authorization and approval policies remain the agent's own policies; a phone terminal does not make every command safe. Remote agents can read or modify files available to their host account and may send material to their configured provider.

Do not describe optional networking containers as an agent sandbox. If you need process isolation, configure and assess it on your host separately.

## What happens on the website

The website collects iPhone test or cloud-setup requests only when you submit the corresponding form, and uses Cloudflare Web Analytics. That is distinct from the terminal data path. See the [privacy policy](https://cookbox.ai/privacy) for the current details.

Before sharing a diagnostic report or recording, remove host addresses, usernames, keys, QR codes, account details and private repository contents. For questions, contact [hello@cookbox.ai](mailto:hello@cookbox.ai).

[All guides](https://cookbox.ai/guides)[Server setup](https://cookbox.ai/docs/server-setup)[Get it on Google Play](https://play.google.com/store/apps/details?id=ai.cookbox.app)
