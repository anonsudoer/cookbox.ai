Docs · Navigation cheat sheet

# cookbox.ai user cheat sheet

A phone has no Ctrl key, no Esc, no Tab and no scroll wheel, and the soft keyboard covers half of what you came to read. The key bar is the answer to that. This page is the whole of it: what each control does, what a long press does instead, and what the orange means.

The controls below are drawn exactly as the app draws them, in its dark identity. In the light theme the shapes are rounder and the orange is a shade warmer; nothing moves.
## Connection stats

The chip beside the crown shows the transport (`mp`, `ssh` or `mixed`) and the average round trip; tap it for the full panel. A round trip is the time from a key you type to the first output after it. Output that arrives more than 30 seconds after a key is not counted: the key drew nothing, or the phone slept, and that output is not its echo. A real stall still shows in the terminal as “no answer for…”. Every number counts from the app's launch.

## The key bar

Two rows under the terminal, always there. The top row is tools, the bottom row is keys.

Ctrl

Tab

Esc

| Key | Tap | Held or long-pressed |
| --- | --- | --- |
|  | Arrow keys. `↑` sits on the tools row directly above `↓`, within one thumb's reach of each other. | Repeats, about eighteen a second after a short hold — for walking back through shell history or a long line. |
|  | Pick images from the gallery and upload them to `~/.cookbox/uploads` on the host, then paste their paths into the prompt. | Latch it to **any file** — see [below](#states). |
|  | Paste the Android clipboard into the session. Says so plainly when the clipboard is empty or the system refuses it. | Latch it to **clear** — see [below](#states). |
|  | Raise or drop the soft keyboard. | Switch the button to dictation — see [below](#dictation). |
| Ctrl | Arm Ctrl for the next key. | — |
| Tab | Tab. | Latch it to **⇧Tab** until you long-press again. |
|  | Enter — which is *submit* to an agent. | Latch it to **⌥⏎**: a newline that does not send. |
| Esc | Escape. Interrupts a Claude Code turn, leaves an editor's insert mode. | — |

## What the orange means

A key turns orange when it is **armed** — when it will do something other than what it says, or when a mode is running. Nothing else in the bar is coloured, so an orange key is always a live mode you are in and may have forgotten about.

Ctrl Ctrl
**Ctrl, armed.** Tap it, then a letter, for `^C` and friends. There is no modifier to *hold* on a soft keyboard, so it latches instead: it is spent by the first character you type, and stays armed across the arrow keys on purpose, which is what makes the second tap of `←` or `→` below work.

 pu
**The arrows, with Ctrl armed.** `↑` and `↓` relabel themselves to **pu** and **pd** — page up and page down — so you can see what Ctrl turned them into before you commit. `←` and `→` keep their arrows and become word-left and word-right; tap the same one twice inside about half a second and it goes to the start or end of the line instead. That escalation is deliberately not extended to `↑`/`↓`: paging up twice is an ordinary thing to do and must not fly to the top of the buffer.

Tab ⇧Tab
**Back-tab, latched.** Long-press Tab and the label changes to ⇧Tab and stays there — it is a real key in every TUI that cycles a selection, and a phone has nowhere else to put it. Long-press again to go back.

 ⌥
**Enter that does not submit.** Agents read Enter as "send this now", so writing a second line of a prompt needs a different key. Long-press Enter to latch **⌥⏎**; the small ⌥ in the corner is there so a latch left on overnight is visible before it eats your next message.

 
**The clip, latched to any file.** Tapping it picks photos; long-press and the icon changes from a gallery to a stack of sheets, and it picks anything on the phone instead. The icon is what tells you which, not the colour — the two modes take different roads underneath. A photo crosses to the host whole, which is fine for a screenshot; anything else is streamed a slice at a time, so a file larger than the phone can hold in memory still arrives. Long-press again to go back.

 
**Paste, latched to clear.** Long-press paste and it becomes a clear key: one tap throws away the line you are typing. It sends `^E` then `^U` — end of the line, then kill back to the start of it — so the whole line goes wherever the cursor happens to be, in a shell and in every agent. Neither half interrupts anything, so a turn already running is left alone; that is the difference between this and `^C`. A prompt of several lines takes one tap per line.

 
**The soft keyboard.** Orange while the keyboard is up. Tap its key or double-tap the terminal to raise or drop it. A single tap on the terminal leaves the keyboard alone.

 
**Dictation.** Long-press the keyboard key and it becomes a microphone. Tap to start listening — orange means the phone is recording you — and tap again to stop. Words land in the prompt as they are recognised. Long-press again to go back to the keyboard.

## Gestures on the terminal

| Gesture | Does | Why that one |
| --- | --- | --- |
| Swipe up / down | Scroll — see the next section, because this is the one thing that is not the same everywhere. | The obvious gesture for the obvious job. |
| Single tap | Move the cursor to where you tapped — but only along the line the cursor is already on, wrapped rows included. Anywhere else it does nothing. | Faster than holding an arrow. It is horizontal only because **Up** at a shell is the previous command, not a cursor move: a tap two rows up would quietly throw away what you had typed. A terminal is a grid of cells and nothing in it says which are the line being edited, so the cursor's own line is the one honest answer. |
| Double tap | Raise or drop the keyboard. | A *single* tap is how a selection is dismissed. A keyboard arriving on a single tap would cover the text you just tapped to read. |
| Long press | Select the word under your finger; keep dragging to extend the selection either way. Letting go copies it. | Selection has to be deliberate. A drag that starts before the press lands is treated as a swipe instead. |
| Pinch | Resize the terminal text, 8–22 px. | It is the same value as **Terminal size** in settings — set it either way, they stay in step. |

## Scrolling, which is not the same everywhere

There is no scrollback to slide around on a phone: the terminal you are looking at is a live screen on a machine somewhere else, and what "up" means depends on who is drawing it. cookbox.ai looks at what is in the pane and picks:

| What is in the pane | A swipe sends | What you see |
| --- | --- | --- |
| **Anything in zellij** — an agent, vim, htop, a plain shell | A screenful of mouse wheel notches, at the cell under your finger. | Zellij scrolls that pane's scrollback — or hands the notch to a full-screen program that asked for the mouse, and that program scrolls itself. Zellij decides, because zellij is the one that knows. Agents are not a special case: none of the three page themselves, and treating them as if they did is a swipe that does nothing. |
| **A full-screen TUI in tmux** | `PageUp` / `PageDown`, straight to the program. | The program's own history. tmux's mouse is off unless you turned it on, so this side has to do the picking. |
| **A plain shell in tmux** | `Ctrl-B [` to enter copy mode, then paging. | The multiplexer's scrollback. The mode ends by itself the moment you type. |

A pane stays wherever you left it scrolled, and the only thing that snaps it back to the live screen is a keypress — so opening a session puts it back at the bottom for you. Without that, a session left scrolled to the top of its history opens there, and swiping back does nothing at all until you press a key, which looks exactly like a broken swipe.

One thing no swipe can reach: a program that runs on the **alternate screen** keeps no history in the multiplexer at all, so there is nothing there to scroll. That is a property of the program, not of cookbox.ai — the Codex TUI is the usual one, and its own issue tracker carries the request to render inline instead.

Ctrl armed plus `↑`/`↓` sends `PageUp`/`PageDown` whatever is in the pane, for when you would rather press a key than swipe, and it is the way to reach the paging of a program that has some of its own. You never have to leave tmux's copy mode by hand: the first key you type ends it, because a shell that silently swallows your command is a worse bug than a scrollback you left open. Zellij has no mode to leave at all.

## Voice: dictation and recordings

Two different things, in two different places.

### Dictation, inside a terminal

Long-press  to turn it into , tap to listen, tap to stop. It is Android's own speech recognition, so the transcript is whatever the service on your phone produces — nothing is sent to cookbox.ai. Good for a sentence of instruction; not for a paragraph of thinking out loud.

### Recordings, from the sessions screen

The  in the bottom deck of the session list opens the recorder, which is for the paragraph of thinking out loud.

- Record, then name the file. Recordings are `.m4a`, kept on the phone under **Music** in the folder set in settings, and they stay there until you delete them.
- Pick what the agent should do with it — **transcribe**, **summarize**, **dig deeper**, **start cook** — and add a follow-up prompt if the checkboxes do not say it.
- Choose a target session and forward. The audio is uploaded to the host and handed to the agent; a Claude Code or Codex session can take it, other agents cannot, so only the ones that can appear in the list.
- The cap is 24 MB per recording — roughly an hour of speech.

## View: ports and forwarding

**View** is the answer to "the dev server is running, now what". It lists the TCP ports actually listening on the host, with the process that holds each one.

- Tap a port to forward it over the existing SSH connection and open its page in the browser's own view inside the app. No firewall change and nothing published — the tunnel exists only while the app holds it, and the page cannot reach the clipboard, recordings or other apps.
- Long-press a port to type a path such as `/admin`: ✓ opens it, ★ saves it as a favourite.
- On the page itself, **Add to favourites** in the browser's menu (Android) or share sheet (iPhone) saves the page you are on. Favourites sit at the top of View; one tap opens that exact page.
- The stop button on a row closes that forward and leaves the session alone.

### pm2

If the host runs **pm2**, View says so above the ports, in one line you can read without opening it: how many apps are up, how many are not, the load average against the core count, what pm2's apps cost between them, and free disk. Load and disk are not pm2's — it has no system numbers — they come off `/proc` and `df` on the same round trip.

- Tap the line to unfold a row per process: a dot for its state, what it is costing in CPU and memory, how long it has been up, and how many times it has restarted. A restart count that has run away is coloured, because that is the number that says something is crash-looping quietly.
- Each row carries its own controls — **restart** and **stop** for a running app, **start** for a stopped one. They address the process by pm2's own id rather than by name, because names repeat across namespaces and cluster instances.
- It refreshes while the panel is on screen — every six seconds unfolded, slower when folded — and stops when you leave the screen. Nothing is pushed; a reading is a round trip, and a round trip is about a fifth of a second and a quarter of a kilobyte.
- **Nothing here starts a pm2 daemon.** Every `pm2` command launches one if there is none, so cookbox.ai reads the daemon's pid file instead and only talks to pm2 once that names a live process. A host with pm2 installed and nothing running says exactly that, and offers no buttons.
- What crosses the link is eight numbers per process. `pm2 jlist` hands back every app's full environment — tens of kilobytes, and where a server's secrets live — so it is reduced on the host and the rest never leaves it.
- pm2 is per-user. You see the daemon belonging to the account you log in as, not every pm2 on the box.

### Files

- The folder button opens the host's files — from a terminal it starts in that session's own working directory, from View it starts at the SSH user's home.
- Each row's glyph says what the file is — page, picture, film, note, archive — because a long name loses its extension to the ellipsis long before you can read it.
- Text files preview in place, read-only, first 256 KB. Anything else — a PDF, an image, an archive — offers **Open in another app**: cookbox.ai pulls the file down, at any size, and hands it to whatever on the phone opens that type. There is no PDF viewer inside cookbox.ai and none inside the webview it draws in, so the phone's own viewer is the honest answer.
- The tick button turns the list into a picker: tap rows to choose them, folders included, then **Download**. One file arrives as itself, uncompressed — compressing a single file only buys a wait and an unpacking step. A folder, or several things at once, arrives as one `.zip`, built on the host and streamed straight out so nothing is written down there to clean up. A host with neither `zip` nor `python3` falls back to `.tar.gz`, which Android needs an app to open.
- A download counts up on a bar *above* the list while it runs. The bar does not fill to a percentage because there is no total to fill towards: the archive is built as it is sent. Everything lands in the phone's own **Downloads**, where it outlives the app, and the name it landed under is shown next to **Saved to Downloads**, with a folder button that opens that screen. The name is worth reading: a second download of the same folder is renamed rather than overwritten, the way a browser does it. There is no size ceiling: the bytes stream straight to the file on the phone rather than being held in memory, so what limits a download is the free space and the link.

## On a desktop

The Linux and WSL app has a real keyboard, so the key bar folds away. The mouse wheel scrolls a pane the way a swipe does, and Ctrl+wheel sizes the text. Right click pastes the clipboard into the terminal, as Ctrl+Shift+V does; selecting with the mouse copies. Forwarded pages open in your browser.

## The bar above the terminal

| Control | Does |
| --- | --- |
|  | Back to the session list. The session stays live and the agent keeps working — this is not a way out of anything. |
|  | What the agent is doing: **green** working, **amber** waiting for an answer from you, **grey** idle or unknown. The session list spells the same thing out in words. |
|  | The files this session has been writing, starting in its own working directory. |
|  | Leave, which opens three choices: detach **others** (kick other clients off this session), **detach** yourself, or **close** the session for good. It folds back into the button after six seconds, because a menu that replaced the button it came from leaves nothing to cancel with. |
| `mp` / `ssh` | Which transport the terminal is on. `mp` is moshpit — local echo, survives a roam; `ssh` means plain SSH, a fact rather than a fault. A new pane opens on `ssh` at once and moves to `mp` a moment later, so a brief `ssh` is expected. Tap it to redraw the screen: it blinks and the session repaints, which clears leftover artifacts. |

## Home-screen premium crown

Tap the crown between connection stats and Settings to open premium status, current receipt dates and IDs, and available paid amounts. A faded crown is still clickable. Tap it again to close; opening connection stats replaces this panel.

Full past history is available through the Google Play history button. The benefits button opens [premium features and benefits](https://cookbox.ai/premium). Returning from the browser keeps you on the app's home screen. Receipt reads stay in native code and Rust; forwarded web pages cannot request them.

[← Overview](https://cookbox.ai/docs/overview)[← Server setup](https://cookbox.ai/docs/server-setup)
