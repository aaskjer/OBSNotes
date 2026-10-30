# OBS Notes: FAQ

Most answers start in the Streamer.bot log. Every line OBS Notes writes there starts with `[OBS Notes]`; the log
files are in the `logs` folder inside the Streamer.bot folder, one per day.

---

## Post-its in OBS

### The post-its do not show up in OBS

Commands never add the browser source on their own, only the Test trigger does ([Setup](Setup-Guide.md#setup),
step 4). Run it: in the action **OBS Notes**, right-click the trigger **Test** > *Test Trigger*. Then look for the
line it wrote:

| Log line | Cause | Fix |
|----------|-------|-----|
| `browser source 'OBS Notes' (600 x 800) added to scene '…'` | It worked. Without notes the page is fully transparent. | Look for **OBS Notes** in the sources of that scene and try `!note test`. |
| `OBS connection #0 is not connected. The overlay file is ready at …` | Streamer.bot does not reach OBS. A fresh OBS has its WebSocket server switched off. | In OBS, *Tools > WebSocket Server Settings*: tick *Enable WebSocket server*, then *Show Connect Info* for port and password. In Streamer.bot, *Stream Apps > OBS*: add a connection with them and connect. Run the Test trigger again. |
| `no connected OBS shows the post-its (checked connection #0). …` | A command ran, but the Test trigger never did. | Run the Test trigger. |
| `the Test trigger needs the argument AddOBSOverlayTo …` | The trigger's variable is missing or no number. | Set `AddOBSOverlayTo` on the trigger **Test** to `0`. |
| `no commandId, the action was not started by one of its commands.` | The action was run some other way than by the Test trigger. | Right-click the trigger **Test** > *Test Trigger*. |
| `OBS already has a source named 'OBS Notes' that is no browser source (…)` | The name is taken. | Rename that source in OBS and run the Test trigger again. |
| `OBS did not create the browser source: …` | OBS turned the request down; its answer follows in the same line and names the reason. | Fix what OBS names, then run the Test trigger again. |
| No `[OBS Notes]` line at all | The trigger never reached the action. | Switch on the action **OBS Notes** and its trigger **Test**. |

`AddOBSOverlayTo` counts the OBS connections in Streamer.bot from `0`: with more than one OBS, it picks which one
gets the post-its.

### The source is there, but it stays empty

- **No notes yet.** The page is transparent until the first note. Try `!note test`.
- **Faded out.** After `hideAfterMinutes` without a command the post-its fade out, and the next command brings them
  back. `0` keeps them on screen.
- **Studio Mode.** The Test trigger adds the source to the scene on air (*Program*), not to the one in *Preview*.
- **Another scene.** The source goes to the scene showing when the trigger ran. Run it again in another scene to
  add it there too.
- **A folder Streamer.bot cannot write to**, such as `C:\Program Files`. The log says `overlay file write failed`.
  Move Streamer.bot to a folder of your own, for example in *Documents*.
- **OBS on another PC.** The browser source reads a local file, so OBS has to run on the PC with Streamer.bot.

### After an update the post-its stopped changing

The first command after an import points every browser source that still shows an older page to the current one,
and the log says `browser source '…' on OBS connection #0 showed …; it now shows …`. If OBS was not connected at
that moment, the first command a minute or more later tries again; the Test trigger does it straight away.

### How do I make the post-its bigger or smaller?

Scale the source in OBS. The text on each post-it grows as large as the post-it allows, so it stays readable in a
corner of the screen. `layout` picks the shape: `vertical` (600 x 800) or `horizontal` (900 x 560).

---

## Commands

### The commands get no answer

- **Switched off.** The four commands are imported switched off: *Commands* tab, group **OBS Notes**.
- **Cooldown.** Each command waits 5 seconds before it answers again; the broadcaster is never held back.
- **Not allowed.** `!radier` and `!clearnotes` are for moderators and VIPs only.
- **Platform left out.** A platform unticked in the command's sources gets no answer there.

### Why do all notes go to the list 'global'?

The list follows the Twitch category, without Twitch the Kick category. With neither connected in Streamer.bot,
every note goes to one list, and the log says `Neither Twitch nor Kick is connected, so the notes go to the list
'global'.`

### A note was deleted by mistake. Can I get it back?

Yes. `OBS Notes\<Game>-Notes.json.bak` in the Streamer.bot folder keeps every note ever added to that game, one per
line. Find it there and add it again with `!note`.
