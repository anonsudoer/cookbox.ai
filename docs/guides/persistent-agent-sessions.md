Guide · English · October 15, 2026

# Keep agent sessions running after your phone disconnects

How cookbox.ai uses tmux, zellij and existing herdr workspaces, and what persistence cannot recover.

The phone connection and the host session are different things. A multiplexer keeps the terminal session on the host while cookbox.ai connects and detaches from it. This lets you leave a running agent and return later, provided the host and its processes keep running.

## Choose a supported session host

cookbox.ai creates sessions in tmux or zellij, whichever the host has (setup adds tmux when it has neither). cookbox.ai also discovers existing herdr workspaces where a herdr server is already running; it does not install herdr for you. Use the same host login and multiplexer when looking for a session you opened earlier.

As a simple host-side example, create a named tmux session:

```
tmux new-session -s phone-work
```
Run your agent inside it. Detach from tmux with Ctrl-B then D, and inspect it with:

```
tmux list-sessions
tmux attach-session -t phone-work
```
These are ordinary tmux commands, not a special cookbox.ai service. Do not run the attach command inside an unrelated multiplexer unless you intentionally want nested sessions.

## Detach, close and disconnect are different

- **Detach:** leave the session running on the host and stop viewing it from this client.
- **Close session:** explicitly end that session. Treat it as a destructive action, not another way to hide the terminal.
- **Lose the network:** the phone may reconnect, but the host session survives only while the host and multiplexer remain healthy.

Do not assume that leaving a server running also keeps a phone-side watcher active. Reopen the correct host/session to inspect its current state.

## What moshpit adds

When available, moshpit adds a UDP transport with predicted local input and roaming. cookbox.ai can fall back to SSH when that transport cannot work. A terminal that says SSH does not mean the remote session has been replaced or lost.

A phone proxy, captive portal or network that blocks UDP can prevent moshpit while still allowing SSH. Do not use a successful SSH connection as proof that the UDP path is open.

## What this does not recover

Persistence is not checkpointing an autonomous workflow. It cannot restore a process killed by the host, wake a sleeping laptop, undo a closed session or recover work after a server failure. Agent-specific resume features are separate from terminal reconnection.

Keep valuable work in your normal version control and backup workflow. To get started, use the [Android walkthrough](https://cookbox.ai/guides/control-ai-agents-from-android) and [host setup guide](https://cookbox.ai/docs/server-setup).

[All guides](https://cookbox.ai/guides)[Server setup](https://cookbox.ai/docs/server-setup)[Get it on Google Play](https://play.google.com/store/apps/details?id=ai.cookbox.app)
