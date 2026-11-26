# OBS Notes: FAQ

Most answers start in the Streamer.bot log. Every line OBS Notes writes there starts with `[OBS Notes]`, and the
**Log** page of the settings window shows exactly those lines. The log files are in the `logs` folder inside the
Streamer.bot folder, one per day.

---

## Post-its in OBS

### The post-its do not show up in OBS

Commands never add the browser source on their own, only the button in the settings does
([Setup](Setup-Guide.md#setup), step 5). Press it: settings window, **Overlay** page, **Add the post-its to OBS**.
The box under the button says what happened, and the log has the same line:

| Log line | Cause | Fix |
|----------|-------|-----|
| `browser source 'OBS Notes' (600 x 800) added to scene '…'` | It worked. Without notes the page is fully transparent. | Look for **OBS Notes** in the sources of that scene and try `!note test`. |
| `OBS connection #0 is not connected. The overlay file is ready at …` | Streamer.bot does not reach OBS. A fresh OBS has its WebSocket server switched off. | In OBS, *Tools > WebSocket Server Settings*: tick *Enable WebSocket server*, then *Show Connect Info* for port and password. In Streamer.bot, *Stream Apps > OBS*: add a connection with them and connect. Press the button again. |
| `no connected OBS shows the post-its (checked connection #0). …` | A command ran, but the post-its were never added to OBS. | Press the button. |
| `AddOBSOverlayTo = '…' on the Test trigger is no OBS connection number …` | The trigger **Test** of the action **OBS Notes** was run, and its variable holds no number. | Set `AddOBSOverlayTo` to `0`, or empty it: then *OBS connection* from the window counts. |
| `no commandId, the action was not started by one of its commands.` | The action **OBS Notes** was run by hand. | Nothing to fix. Use a command in chat, or the button in the settings. |
| `OBS already has a source named 'OBS Notes' that is no browser source (…)` | The name is taken. | Rename that source in OBS and press the button again. |
| `OBS did not create the browser source: …` | OBS turned the request down; its answer follows in the same line and names the reason. | Fix what OBS names, then press the button again. |
| No `[OBS Notes]` line at all | The button never reached the action. | Switch on the action **OBS Notes**. |

*OBS connection*, next to the button, counts the OBS connections in Streamer.bot from `0`: with more than one OBS,
it picks which one gets the post-its. The trigger **Test** of the action **OBS Notes** adds the post-its too,
without the window; its variable `AddOBSOverlayTo` is that same number.

### The source is there, but it stays empty

- **No notes yet.** The page is transparent until the first note. Try `!note test`.
- **Faded out.** After the minutes in *Fade out after* without a command the post-its fade out, and the next command
  brings them back. `0` keeps them on screen.
- **Studio Mode.** The button adds the source to the scene on air (*Program*), not to the one in *Preview*.
- **Another scene.** The source goes to the scene showing when the button was pressed. Press it again in another
  scene to add it there too.
- **A folder Streamer.bot cannot write to**, such as `C:\Program Files`. The log says `overlay file write failed`.
  Move Streamer.bot to a folder of your own, for example in *Documents*.
- **OBS on another PC.** The browser source reads a local file, so OBS has to run on the PC with Streamer.bot.

### After an update the post-its stopped changing

The first command after an import points every browser source that still shows an older page to the current one,
and the log says `browser source '…' on OBS connection #0 showed …; it now shows …`. If OBS was not connected at
that moment, the first command a minute or more later tries again; the button in the settings does it straight
away.

### How do I make the post-its bigger or smaller?

Scale the source in OBS. The text on each post-it grows as large as the post-it allows, so it stays readable in a
corner of the screen. *Layout* in the settings picks the shape: vertical (600 x 800) or horizontal (900 x 560).

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

### A note has a typo. Can I fix it?

Yes, on the **Notes** page of the settings window: pick the game under *Notes of*, click the text, change it and
press `Save`. Notes your chat adds or deletes while the window is open stay as they are.

### A note was deleted by mistake. Can I get it back?

Yes. `OBS Notes\<Game>-Notes.json.bak` in the Streamer.bot folder keeps every note ever added to that game, one per
line. Find it there and add it again with `!note`, or with *Add row* on the **Notes** page of the settings window.

---

## Settings

### The settings window does not open

- **Only one at a time.** A window that is already open comes to the front instead of a second one.
- **The first open** asks whether to download `UIFramework.dll`, the window itself. Without it there is no window;
  run the trigger again and click `Yes`.
- **An older UIFramework.** OBS Notes needs 2.5.0 or newer; with an older one installed, the settings action offers
  the update instead of opening the window.
- Anything else is in the log, in the lines that start with `[OBS Notes/Settings]`.

### My settings from before the update are gone

Up to 1.0.2 the settings were `Set Argument` sub-actions of the action **OBS Notes**, and an import replaces that
action. The first time the window opens, it takes those values from Streamer.bot's own backup of your actions (the
folder `backup`) and shows them as unsaved changes: press `Save`. Without such a backup the window opens with the
defaults, and you set them once more.

### A setting is greyed out and says *Set Argument*

The action **OBS Notes** has a `Set Argument` sub-action with that name, and it counts instead of the window's
value. Delete or disable the sub-action to set the value in the window.
