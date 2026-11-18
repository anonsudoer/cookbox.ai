Docs · Server setup

# Connect a server to cookbox.ai

cookbox.ai needs a POSIX SSH endpoint and at least one supported multiplexer. Linux is the full target; WSL 2 works when SSH terminates inside Linux, and macOS is a reduced preview. Everything below runs as the user cookbox.ai will log in as — never as root by habit.

## The short path: QR pairing

This is the normal route, and the one **New connection** in the app walks you down.

Guided AWG 3.1 and Tailscale setup require the same one-time premium unlock. An existing premium purchase covers both. Check or restore access using the home-screen crown before preparing the server. Direct and WireGuard setup remain free; manually configured tunnels remain usable. See [premium benefits](https://cookbox.ai/premium).
1. In the app, tap **New connection** and choose where the host runs and the network route. Agents, zellij and herdr are yours to install; the app finds whatever is there.
2. On the host, run the short command the page shows, and choose **Connect the cookbox.ai app**:

```
curl -fsSL https://cookbox.ai/install | sh
```
3. It prints a QR code. Scan it with the app. Name, login user, address and SSH port fill themselves in, and the host receives the installer flags and your public key.
4. The host prints what it is about to install and the public key it received. Compare it on screen, then confirm.
5. Save the connection in the app and compare the SSH host-key fingerprint the first time you connect.

The pairing expires after five minutes and never carries a password or a private key. Treat a live QR code like a five-minute password: anyone who can photograph it can pair with that host.

## The long path: the installer by hand

The same installer, run with the flags spelled out. The app shows this command behind the fold on the same page, filled in with your key.

```
curl -fsSL https://cookbox.ai/install | sh -s -- \
  --install --mode direct \
  --public-key 'ssh-ed25519 AAAA… cookbox'
```

`--describe` prints what would happen and installs nothing. The installer:

- supports apt, dnf, pacman and Homebrew;
- installs OpenSSH, tmux when the host has no multiplexer at all, and the usual upload and probe tools — never an agent CLI, zellij or herdr;
- appends the public key once and repairs SSH file permissions;
- installs the cookbox.ai skill for a detected Claude Code or Codex CLI;
- installs and persists `mps` on Linux x86_64;
- configures only the network route you chose.

Run it again at any time: it updates what is there and keeps keys and sessions. A different `--mode` switches the network in place. `mps` runs for the account that ran the installer, so one account per server gets moshpit; the others use SSH. A shared server for several accounts is in the works.

Agent authentication stays separate: run `claude`, `codex` or `cursor-agent login` on the host after installing. The phone's private key never leaves the app.

## The cookbox command

The installer leaves a `cookbox` command in `~/.local/bin`. Run it alone for a menu (arrow keys, space, Enter), or: `cookbox NAME` opens or starts a session, `cookbox status` shows what runs, and `cookbox pair`, `setup`, `update` and `uninstall` do what they say.

## Network routes

`--mode` decides what the host exposes and what you type into the app's **Host** field. `core` installs no route at all.

| Mode | Public exposure | Address in cookbox.ai |
| --- | --- | --- |
| `direct` | SSH TCP, and UDP `50000:50009` | The VPS public IP or DNS name |
| `wireguard` | UDP `51820` | `10.88.0.1` |
| `awg2` | A randomised AmneziaWG 3.1 UDP port | The address in the printed AWG 3.1 profile |
| `tailscale` | Nothing from cookbox.ai | The Tailscale IPv4 or MagicDNS name |
| `relay` | Nothing: the host connects out to cookbox.ai | Filled in by pairing |

WireGuard prints a QR code for the WireGuard app. With AmneziaWG, pairing hands the app its own tunnel profile and it connects by itself; the printed QR code is for another device's AmneziaWG app. With Tailscale, the app joins your tailnet by itself with an auth key, and pairing fills **Host** with the host's tailnet address when Tailscale is running. Another device needs its own AmneziaWG profile, never a shared one: run `cookbox add-peer` once per device.

The moshpit control port `40404` stays on loopback and is reached through SSH. Direct mode opens the UDP data range publicly; the private modes restrict it to their own interface or container. One UDP port is used per live moshpit session.

The installer leaves an existing public SSH path open after setting up a private route. Close it by hand only once cookbox.ai connects successfully through the private address — otherwise you lock yourself out of your own machine.
## Windows through WSL 2

Native Windows is not a cookbox.ai host: Windows sshd with `wsl.exe` as the shell does not give the POSIX exec, uploads, process metadata and multiplexer behaviour cookbox.ai expects.

1. Run `wsl --install`, open the Linux distribution once, and make sure systemd is active.
2. In the app choose **Linux PC / WSL**, which selects Tailscale.
3. Run the generated command inside the WSL terminal.
4. Scan the QR code; the app fills **Host** with the WSL node's tailnet address and joins the tailnet itself with an auth key (the **Get an auth key** button opens the right page).

## macOS preview

In the app choose **macOS**, install the Tailscale app on the Mac and sign in, then run the shown command in Terminal and scan the QR. The installer turns on **Remote Login** (or says how, when Terminal lacks Full Disk Access) and fills in the Mac's tailnet address. Terminal sessions, uploads and SSH forwarding work; agent and activity probes, port discovery and moshpit do not, so terminals run over SSH.

## Check the result

1. Connect from the app and compare the pinned SSH host fingerprint.
2. Create a session, leave it, and reopen it.
3. Upload a small file, and open one forwarded service from **View**.
4. On Linux x86_64 with `mps` installed, confirm the terminal says `mp`. Anywhere else, `ssh` is the expected answer.

If the host sits behind a provider firewall, mirror the rules the installer printed. A local UFW or firewalld rule cannot open the provider's edge.

## Uninstall

An installation records its own mode, SSH port and public key, so removing it needs no flags:

```
curl -fsSL https://cookbox.ai/install | sh -s -- --uninstall
```

That removes the cookbox.ai-owned moshpit, route state and skills. It keeps OpenSSH, system packages, multiplexers, agent CLIs and any shared Tailscale enrollment, and it removes AmneziaWG resources only when cookbox.ai created them.

[Next: navigation cheat sheet →](https://cookbox.ai/docs/navigation)[← Overview](https://cookbox.ai/docs/overview)
