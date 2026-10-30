# OBS Notes: Setup Guide

## Commands

| Command | Also | Who | What it does |
|---------|------|-----|--------------|
| `!note <text>` | `!notiz`, `!postit`, `!aufschreiben`, `!merken` | everyone | Saves a note and answers with its number. |
| `!zettel` | `!schmierzettel`, `!merkheft`, `!shownotes` | everyone | Posts the last five notes in chat; `!zettel 3` posts note 3. |
| `!radier <number>` | `!delnote`, `!deletenote` | Moderators, VIPs | Deletes one note; the ones after it move up. |
| `!clearnotes` | `!allesradieren` | Moderators, VIPs | Deletes every note of the current game. |

The commands work on Twitch, Kick and YouTube, and the answer goes to the chat the command came from, with an
emote of that platform: Twitch and Kick global emotes, plain emoji on YouTube. Untick a source in a command to
leave that platform out. The list follows the Twitch category. Without Twitch it follows the Kick category, read from
Kick's public channel page (the live stream's category, offline the latest one); without either, every note goes to
one list.

Each command has a 5 second cooldown; the broadcaster is never held back. The replies in chat are in German
or English, see `language` under [Settings](#settings).

---

## Setup

1. Import [Import-String.md](Import-String.md) in Streamer.bot (*Import*, paste the string), or drop
   `OBS-Notes.sb` from the [latest release](https://github.com/aaskjer/OBSNotes/releases/latest) onto the
   import window.
2. Turn on the four commands in the group **OBS Notes** (*Commands* tab). They are imported switched off.
3. Connect OBS in Streamer.bot (*Stream Apps > OBS*). OBS has to run on the same PC, because the browser source
   reads a local file.
4. In the action **OBS Notes**, right-click the trigger **Test** > *Test Trigger*. It adds the browser source
   **OBS Notes** (600 x 800, or 900 x 560 with `layout` set to `horizontal`) to the scene OBS is showing. Move and
   scale it wherever you like.

The Test trigger's variable `AddOBSOverlayTo` names the OBS connection: `0` is the first one in Streamer.bot.
Running the trigger again refreshes the source, or adds it to the scene that is showing now. Nothing in OBS?
See the [FAQ](FAQ.md#the-post-its-do-not-show-up-in-obs).

Updates keep the post-its in OBS: the first command after an import looks in every connected OBS for a browser
source that still shows the page of an older version, points it to the current page and says so in the log.

---

## Requirements

- Streamer.bot 1.0.7 or newer with Twitch or Kick connected (its category picks the list, Twitch first); YouTube optional
- OBS Studio 28 or newer (tested with 33), connected in Streamer.bot, on the same PC

---

<!-- tab: Settings -->

## Settings

Set Argument sub-actions in the action **OBS Notes**, under *Settings (Important to check!)*.

**Chat**

| Argument | Default | |
|----------|---------|---|
| `useBot` | `True` | Answers from the bot account; `False` = from the broadcaster account. Without a connected bot account Streamer.bot uses the broadcaster account. |
| `sendAsMe` | `False` | `True` = answers as a /me message (Twitch only; Kick and YouTube have none). |
| `language` | `DE` | `DE` (German) or `EN` (English), for the chat answers and the post-its. |

**Overlay**

| Argument | Default | |
|----------|---------|---|
| `hideAfterMinutes` | `10` | Minutes without any command until the post-its fade out. The next command brings them back. `0` = always visible. |
| `colorfulNotes` | `True` | `False` = black text on white post-its. |
| `showGameName` | `True` | The game name on the top left post-it. |
| `layout` | `vertical` | `vertical` = two post-its side by side in three rows (600 x 800), `horizontal` = three side by side in two rows (900 x 560). The browser source in OBS takes the new size with the next command. |

Changes reach OBS with the next command or Test trigger.

---

## Files

Everything lives in `OBS Notes` inside the Streamer.bot folder:

- `<Game>-Notes.json`: the notes of one game.
- `<Game>-Notes.json.bak`: every note ever added to that game, one per line, in case one was wiped by mistake.
- `Overlay\notes_overlay.html` and `notes_state.json`: the page OBS shows and what it shows.
