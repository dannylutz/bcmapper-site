# The User Interface 

bcMAPPER's layout is built around one principle: the visual controller should take center stage. Every other panel — the preset sidebar, the parameter editor, the MIDI monitor — opens and closes contextually so it only occupies space when you actually need it.

The main areas of the interface are:

- **Header Toolbar** — Preset name, slot strip, undo/redo, save, file menu, hardware menu, and device selector
- **Preset Sidebar** — Left-side panel listing all standalone presets and banks
- **Visual Controller** — The center stage: an accurate, clickable layout of your BCR2000 or BCF2000
- **Parameter Panel** — Right-side panel for editing a selected control's settings (or the Bulk Edit Panel when multiple controls are selected)
- **Preset Globals** — Collapsible panel below the controller for sweeping changes across an entire preset
- **MIDI Monitor** — Live feed of MIDI traffic; hidden by default, opens automatically in Virtual Mode
- **Footer** — Status bar showing preset info, dirty indicator, slot number, and device connection status

The layout adapts based on what you're doing:

| Panel | Edit Mode | Virtual Mode |
|-------|-----------|--------------|
| **Preset Sidebar** | Expanded on launch; auto-collapses when a preset is loaded | Same behavior |
| **Parameter Panel** | Fixed width, always visible | Collapses to a narrow rail; Shift+Click any control to expand it |
| **MIDI Monitor** | Hidden by default | Opens automatically |

---

## The Header Toolbar

The header runs across the top and is home to all major navigation and action controls.

### Preset Name

Displays the name of the currently loaded preset. **Click it to rename inline** — just click, type, and press Enter. The rename takes effect immediately and saves with the preset.

### Slot Strip

A compact row of 32 small indicators sitting in the header, each corresponding to a preset slot in the current bank. Each indicator shows its slot number and reflects one of the following states:

- **Filled slots** — Displayed with the primary accent color; this slot contains a preset
- **Empty slots** — Dimmed; nothing is here yet
- **Current slot** — Highlighted with a ring outline
- **Dirty indicator** — The current slot's ring changes color when there are unsaved changes
- **Hover** — Shows a tooltip with the slot number and preset name
- **Click** — Jumps directly to that slot

The Slot Strip stays visible even when the sidebar is collapsed, so you always have a sense of where you are in the bank and what's filled.

### Undo / Redo

Step through your editing history with the undo/redo buttons (or `Cmd/Ctrl + Z` / `Cmd/Ctrl + Shift + Z`). bcMAPPER tracks up to 100 history steps.

### Save

Saves the current preset — same as `Cmd/Ctrl + S`. The dirty indicator in the footer clears when the save completes.

### File Menu

A dropdown of file operations:

- **New Bank** — Create a fresh 32-slot bank (prompts for a name and device type)
- **New Preset** — Create a new standalone preset
- **Export** — Submenu with export options:
  - **Preset (.syx)** — Save the current preset in standard SysEx format (hardware-compatible, no labels or colors)
  - **Preset (.json)** — Save the current preset in bcMAPPER's native format (includes labels, colors, and metadata)
  - **Bank (.syx)** — Export the entire current bank as a SysEx file
  - **Bank (.json)** — Export the entire current bank in native format
  - **Preset BCL (.txt)** — Export the BCL code for the current preset as a plain text file
  - **Bank BCL (.txt)** — Export the BCL code for the entire bank as a plain text file
  - **All (.zip)** — Compress all presets and banks into a single zip archive
- **Import** — Submenu with import options:
  - **Preset (.json)** — Load a single preset from a bcMAPPER-format file
  - **Preset (.syx)** — Load a single preset from a SysEx file
  - **Bank (.json)** — Load a bank from a bcMAPPER-format file
  - **Bank (.syx)** — Load a full 32-slot bank from a SysEx dump file
  - **Preset BCL (.txt)** — Import a preset from a BCL text file
  - **Bank BCL (.txt)** — Import a bank from a BCL text file
  - **From ZIP** — Import a zip archive of bcMAPPER presets and banks
- **Print Scribble Strips...** — Open the print dialog where you can print labels and colors for any preset or bank, formatted to cut and place on your hardware

### Help Menu

- **Documentation** — Opens the documentation in your browser
- **Getting Started...** — Reopens the Welcome screen where you can set your save directory or import presets from hardware
- **Preferences...** — Opens the Preferences panel. Settings are grouped into sections:
  - **Hardware** — Which device(s) you own (BCR2000, BCF2000, or both). Used to pre-select the device type when creating new banks and presets, skipping the prompt when you only use one controller.
  - **File Paths** — Where auto-saved files are written. The default is `~/Documents/BCMapper` on macOS/Linux and `Documents\BCMapper` on Windows; the folder is created automatically on first save. Enable **Use custom save location** and pick a folder to override. Also contains the **Back Up All Presets & Banks** button — see [Backup](#backup) below.
  - **Default MIDI Ports** — The input and output ports to connect on startup. If a saved port isn't found when the app launches, bcMAPPER falls back to auto-detecting a BCR/BCF device. Enable **Auto-connect on startup** to have ports connected immediately without clicking Reconnect each session.
  - **Updates** — Toggle automatic update checks. When enabled, bcMAPPER silently checks once per day and shows a banner in the toolbar if a newer version is available.
  - **Appearance** — Switch between the default dark theme and a light theme.
- **Check for Updates** — Check whether a newer version of bcMAPPER is available
- **About bcMAPPER...** — License info, version details, and support contact

#### Backup

The **Back Up All Presets & Banks** button in Preferences → File Paths creates a single timestamped zip archive containing every bank and standalone preset currently in bcMAPPER.

The zip is written to a `Backups/` subfolder inside your configured save location:

```
~/Documents/BCMapper/Backups/bcmapper-backup-2026-04-13-143022.zip
```

Inside the zip:

```
bcmapper-backup-2026-04-13-143022/
  manifest.json          ← summary: bank/preset names, counts, creation date
  banks/
    My-Bank.json
    Another-Bank.json
  presets/
    My-Standalone-Preset.json
```

Each file is the full bcMAPPER JSON for that bank or preset — the same format used by File → Save Bank to File. You can load any individual file back into bcMAPPER via File → Open Bank or File → Import Preset.

A confirmation toast appears when the backup completes, showing how many banks and presets were included and the last few path segments so you know exactly where the file landed.

> **Where is my data normally stored?** Banks and presets live in bcMAPPER's internal app storage (WebKit localStorage), not in the file folder. The save folder and the Backups folder contain *copies* written at specific moments — they are not the live source of truth. The backup button is the fastest way to export everything at once.

### Hardware Menu

- **Virtual Mode Toggle** — The **gamepad icon** switch toggles Virtual Mode on and off. When active, the on-screen controls become interactive and send live MIDI output to the selected output device. See [Virtual Mode](./midi-communication.md#virtual-mode) for the full rundown.

#### Transfer

- **Send to Hardware** — Opens a dialog to push the current preset (or all presets in the bank) to your connected hardware. When sending an entire bank, any linked Global Setup Profile is sent first automatically.
- **Receive from Hardware** — Opens a dialog to pull presets from the hardware into bcMAPPER. You need to set bcMAPPER to listen and manually trigger the dump from the hardware.

#### Device Setup

- **Global Setup Profiles...** — Opens the Global Setup Profile manager. Create named profiles for device-wide settings (MIDI mode, startup preset, foot switch, SysEx device ID, TX interval, dead time) and send them directly to hardware. See [Global Setup Profiles](./midi-communication.md#global-setup-profiles) for full details.

### Device Selector

Separate dropdowns for **Input** and **Output** MIDI devices. The connection status indicator shows whether bcMAPPER has an active link to the hardware.

- **None** — Both dropdowns include a **None** option. Selecting None disconnects that direction without affecting the other. This is useful in Virtual Mode when you want to route output to a softsynth via IAC without simultaneously receiving hardware input (which could cause a feedback loop).
- **Refresh** (↺ icon) — Re-scans for newly connected or disconnected devices.
- **Reconnect** (plug icon) — Attempts to re-establish the connection to the currently selected port without switching devices. Use this when an IAC bus or virtual port silently drops — a common occurrence after waking from sleep or reconnecting audio interfaces.

---

## The Preset Sidebar

The sidebar lives on the left side of the screen and holds all your presets and banks. It **expands on first launch**, then **auto-collapses** once a preset is loaded, to give the controller maximum room. The **toggle button** (a panel icon at the left edge of the content area) lets you show or hide it at any time.

The sidebar has two sections: Standalone Presets at the top, Banks below.

### Standalone Presets

Independent presets not attached to any bank. Listed **newest-first** by creation or modification time. Good for experiments, templates, or one-off configurations.

**Right-click any standalone preset for:**
- **Load Preset** — Load this preset into the editor
- **Copy to Existing Bank** — Opens a dialog to place a copy into any bank slot you choose
- **Create New Bank from Preset** — Creates a new bank with this preset placed in slot 1
- **Delete Preset** — Removes the preset and its file from disk (this is permanent)

### Banks

Each bank holds up to 32 preset slots. Click a bank name to expand or collapse its slot list.

**Loading a bank:** Click the **Load** link next to a bank name, or right-click and choose **Load Bank**. If there are unsaved changes to the current preset, bcMAPPER will prompt you to **Save & Continue**, **Discard**, or **Cancel** before proceeding. After any unsaved changes are resolved, it switches the active bank, expands it in the sidebar, and loads slot 1.

**Factory banks** are pinned to the bottom of the bank list. They're auto-restored on launch if they've been deleted. They're fully editable — but you can always restore any slot to its original factory state.

**Linking a Global Setup Profile:** When a bank is active, a small dropdown appears beneath the Active badge. Use it to link one of your saved Global Setup Profiles to this bank. When you send the entire bank via **Send to Hardware**, the global setup is automatically sent first — no extra steps needed. Only profiles matching the bank's device type (BCR2000 or BCF2000) appear in the dropdown. See [Global Setup Profiles](./midi-communication.md#global-setup-profiles).

**Right-click a bank name for:**
- **Load Bank** — Load this bank and switch to slot 1
- **Add Preset to Bank** — Create a new preset in the next empty slot of this bank
- **Rename Bank** — Edit the bank name
- **Duplicate Bank** — Creates a full copy with "(Copy)" appended
- **Delete Bank** — Removes the bank and its file

**Right-click a preset slot within a bank for:**

*Empty slot:*
- **Create Preset Here** — Create a new preset in this slot

*Filled slot:*
- **Rename Preset** — Edit the preset name
- **Copy to Slot** — Submenu showing all 32 slots with their names; click any slot to copy this preset there
- **Revert to Factory Preset** *(factory presets only)* — Restores the original Behringer factory configuration for this slot
- **Clear Slot** — Empties this slot (the slot stays; the preset data is wiped)

### Sidebar Action Buttons

At the bottom of the sidebar:
- **New Bank** — Create a fresh empty bank
- **New Preset** — Create a new standalone preset
- **Open Bank** — Load a `.json` or `.syx` bank file from disk

---

## The Visual Controller

The centerpiece of bcMAPPER. An accurate, interactive representation of your hardware where you click controls to select and edit them.

### BCR2000 Layout

The BCR2000 has four rows of controls:

- **Row 1 — 8 Push Encoders** — Rotary encoders with LED rings and a built-in push button. Each push encoder is two independent MIDI controls: the encoder and the button.
- **Rows 2–4 — 24 Standard Encoders** — Three rows of 8 rotary encoders with LED rings (no push button)
- **Buttons** — Two groups of 16 buttons each (32 total), arranged below the encoders
- **4 Foot Switch Inputs** — External pedal connections, displayed in the lower-left area
- **LED Display** — Shows the current preset number normally; shows the current value being sent in Virtual Mode

### BCF2000 Layout

The BCF2000 replaces the three standard encoder rows with 8 motorized faders:

- **8 Push Encoders** — Same dual encoder+button nature as the BCR2000, in the top row
- **8 Motorized Faders** — Touch-sensitive, vertically oriented; the BCF2000's motors physically move them to the Default Value position when a preset loads
- **32 Buttons** — Same layout and count as the BCR2000
- **4 Foot Switch Inputs**
- **LED Display**

### Scribble Strips

Every control has a **scribble strip** — a small label zone displayed directly on the controller layout. It sits above or below the control itself and is there for your reference.

- **Double-click** to start editing (up to 12 characters)
- **Enter** to confirm, **Escape** to cancel
- Labels are stored per-preset and persist between sessions
- Labels are not transmitted to hardware — they live in bcMAPPER only

Scribble strips can also carry a **custom background color**, **opacity**, and **font settings**, assigned from the Parameter Panel when a single control is selected, or from the Bulk Edit Panel when multiple controls are selected. Color-coding by function group is one of the fastest ways to make a complex preset readable at a glance.

### Selecting Controls

| Method | Result |
|--------|--------|
| **Click** | Selects one control, deselects everything else |
| **Cmd/Ctrl + Click** | Adds or removes that control from the current selection |
| **Shift + Click** | Selects all controls from the last-selected one to this one |
| **Click + Drag** on empty space | Draws a marquee selection box — everything inside gets selected |
| **Click empty space** (< 4px drag) | Deselects all controls |

> **On the 4px threshold:** bcMAPPER uses a small movement tolerance to distinguish between a "deselect click" on empty space and the start of a marquee drag. If you move less than 4 pixels from your mousedown point, it's treated as a click. More than 4 pixels starts a marquee.

### Push Encoder Groups (BCR2000)

Each BCR2000 push encoder is two controls in one physical unit — the **encoder** (rotary element) and a **push button** built into it. They're configured completely independently with their own MIDI parameters.

When you click a push encoder, the Parameter Panel shows a **toggle** at the top to switch between:
- **Encoder** — Configure the rotary parameters (data type, CC number, mode, LED ring, etc.)
- **Enc Btn** — Configure the push button (same options as a standard button)

Both sides of a push encoder can be on different MIDI channels, sending different data types, and behaving completely differently from each other.

---

## The Parameter Panel

The Parameter Panel lives on the right side of the screen. In **Edit Mode**, it's always visible (showing a placeholder when nothing is selected). In **Virtual Mode**, it collapses to a narrow rail — **Shift+Click** any control to expand the panel for that control without breaking the Virtual Mode workflow.

### Common Parameters (All Control Types)

- **Control Type Badge** — Identifies what you're editing: Encoder, Button, Fader, or Enc Btn
- **MIDI Channel** (1–16) — The channel this control transmits on
- **Copy / Paste** — Capture this control's settings (`Cmd/Ctrl + C`) and apply them to another control of the same type (`Cmd/Ctrl + V`). Copy includes MIDI parameters, label, color, and font settings.

### Encoder Parameters

- **Data Type** — CC, NRPN, Program Change, Pitch Bend, Aftertouch, or GS/XG
- **CC Number** (0–127) — The controller number, when in CC mode
- **NRPN MSB / LSB** (0–127 each) — The parameter address (high and low bytes), when in NRPN mode
- **GS/XG Parameter** — Pick from a list of Roland GS / Yamaha XG synthesis parameters, when in GS/XG mode
- **Mode** — How the encoder reports its position:
  - `Absolute` — Sends the actual knob position as a value from min to max
  - `Relative 1` — Encodes movement as an offset from 64 (clockwise = above 64, counterclockwise = below 64)
  - `Relative 2` — Two's complement encoding (clockwise = small positive numbers, counterclockwise = large numbers near 127)
  - `Relative 3` — Sign-bit encoding (clockwise = 1–63, counterclockwise = 65–127)
- **LED Ring Mode** — How the LED ring looks on the hardware:
  - `1dot` — One LED at the current position
  - `12dot` — LEDs fill from the start up to the current position
  - `Bar` — A filled bar sweeping from left to right
  - `Pan` — Two LEDs expand outward from center
  - `Spread` — LEDs spread symmetrically outward from center
  - `Qual` — A quality/level display style
- **Min Value / Max Value** — Output range (0–127 standard; 0–16383 for NRPN and Pitch Bend). The **↔ swap button** inverts the range instantly.
- **Default Value** — The encoder's starting position when the preset loads
- **Resolution** — Encoder responsiveness. Enter 1–4 space-separated values (each 1–32673), one per rotational speed level (slow → very fast). Standard: `96`. Larger = faster/coarser; smaller = finer. Click **Standard** to set it to the standard value. Leave blank to use the hardware default.

### Button Parameters

- **Data Type** — CC, Note, Program Change, NRPN, Aftertouch, Pitch Bend, MMC, or GS/XG
- **Controller Mode** — Behavior when pressed:
  - `Toggle` — Alternates between On and Off values with each press; LED reflects the current state
  - `Toggle On` — Always sends the On value; LED stays lit
  - `Toggle Off` — Always sends the Off value; LED stays dark
  - `Momentary (Up/Down)` — Sends On value on press, Off value on release
  - `Increment` — Each press advances the value by a configurable step, cycling through the range. Available for CC, NRPN, Aftertouch, and Program Change.
- **Increment Mode** — How to interpret the Increment Steps value:
  - `Step Value` — The exact MIDI amount added per press
  - `Number of Steps` — How many total steps divide the 0–127 range; bcMAPPER calculates the step size automatically
- **Increment Steps** (1–127) — Step size or step count, depending on Increment Mode
- **Note / CC Number** (0–127) — The target note or controller number
- **NRPN MSB / LSB** — For NRPN mode
- **Bank Select MSB / LSB** — For Program Change mode; enables CC 0 / CC 32 to be sent before the Program Change (use for multi-bank synths and devices)
- **Aftertouch Scope** — `Channel` (monophonic) or `Note` (polyphonic)
- **MMC Command** — Stop, Play, Fast Forward (Deferred), Rewind, Record, Pause, or Locate
- **Frame Rate** — Governs SMPTE locate behavior for MMC: Off (no locate), 24, 25, 29.97 Drop Frame, or 30 fps
- **Locate Position** — Hours, Minutes, Seconds, Frames, Subframes (shown when Frame Rate is not Off)
- **On Value / Off Value** (0–127) — Values for the two button states, with **↔ swap button**
- **Start State** — Whether the button LED starts On or Off when the preset loads

### Fader Parameters (BCF2000 Only)

- **Data Type** — CC, NRPN, Program Change, Pitch Bend, or GS/XG
- **CC Number** (0–127)
- **NRPN MSB / LSB** — For NRPN mode
- **GS/XG Parameter**
- **Mode** — Absolute or Relative
- **Min / Max Values** — With **↔ swap button**
- **Default Value** — Where the fader starts when the preset loads; BCF2000 motors will physically move to this position

### 14-bit Value Support

When a control's data type is set to **NRPN** or **Pitch Bend**, the Min/Max value range automatically expands from 0–127 to **0–16383** (14-bit resolution). This gives you far more granularity for parameters where 128 steps feels too coarse — think pitch, modulation depth, or precise filter control.

### BCL Preview

An expandable section at the bottom of the Parameter Panel showing the exact B-Control Language code bcMAPPER generates for the selected control. This is what gets transmitted to the hardware inside a SysEx message.

Useful for:
- Verifying that your settings will produce the expected behavior
- Debugging a control that's behaving unexpectedly on the hardware
- Learning BCL if you want to understand what's happening under the hood

### Reset to Defaults

A **Reset to Defaults** button at the bottom of the Parameter Panel restores the selected control to its factory configuration. Use this when you want to start over on a specific control without affecting the rest of the preset.

---

## The Bulk Edit Panel

Select two or more controls and the Parameter Panel gives way to the **Bulk Edit Panel** — a specialized view for editing shared settings across your selection and for distributing values automatically.

### Shared Parameters

The top section shows current values for parameters that can be set uniformly. If all selected controls share the same value, it shows that value. If they differ, it shows `Mixed...`. Changing any field here applies to **every control in the selection**.

Available bulk parameters:
- **MIDI Channel** — Same channel across all selected controls
- **Data Type** — Separate fields for encoders/faders and for buttons (each has a different set of options)
- **Mode** — Encoder/fader mode (Absolute, Relative 1/2/3)
- **Controller Mode** — Button behavior (Toggle, Momentary, Increment, etc.)
- **On / Off Values** — For buttons, with swap button
- **Default Value** — For encoders and faders

### Value Distribution

Instead of manually assigning sequential CC numbers or values to each control one by one, the Range Tool does it automatically.

1. **Choose a target field** — CC Number, Note/CC, Channel, On Value, or Off Value
2. **Set a start value** and an **end value**
3. **Click Apply Range** — bcMAPPER divides the range evenly across all selected controls

For example: select 8 buttons, set start CC to 0 and end CC to 7, click Apply Range — the buttons are assigned CC 0 through CC 7 in order. Flip start and end to get descending order.

Distribution is calculated as: `value = start + (end − start) × i / (n − 1)` where `i` is the control's index in the selection and `n` is the total number selected.

### Scribble Strip Colors

When multiple controls are selected, you can batch-assign strip colors:
- **Color palette** — A built-in set of colors, plus any custom colors you've added previously
- **Custom color** — Click the pipette icon to open a color picker and enter any hex value; custom colors are saved to your preferences and appear in the palette for future use
- **Opacity slider** — Adjusts how intense the background color appears; low opacity creates a subtle tint, full opacity gives a solid fill
- **No-color option** — The first entry in the palette clears any existing color from selected strips

### Bulk Reset

The **↺ Reset** button restores all selected controls to their factory defaults in one operation.

---

## Preset Globals

Below the visual controller, a collapsible **Preset Globals** panel lets you apply sweeping changes across an entire preset (or a filtered subset) without selecting individual controls.

### Preset Behavior

At the top of the expanded panel, two per-preset hardware settings control how the BCR2000/BCF2000 behaves when this preset is active:

- **Snapshot** — When checked, the BCR2000/BCF2000 automatically transmits all control values the moment this preset is selected via its hardware buttons. You can also trigger a snapshot manually at any time by pressing **EDIT + ◄** on the device. In **Virtual Mode**, the hardware send doesn't apply — use the **Send Snapshot** button that appears below this checkbox to push all current virtual control values to your MIDI output manually.
- **Lock** — When checked, the preset cannot be edited from the hardware itself. Controls still send MIDI normally; only the hardware's built-in edit functions are disabled. Useful for protecting performance presets from accidental changes during a set.

### Apply to Controls

- **Apply To** — Filter which controls are affected: **All Controls**, **Encoders Only**, **Buttons Only**, or **Faders Only** (BCF2000)
- **MIDI Channel** — Set the same channel across the entire target group
- **Data Type** — Apply a data type to all encoders/faders and/or all buttons in the group
- **Values & Ranges:**
  - **All** — Applies default value plus min/max (or on/off) ranges
  - **Ranges** — Applies min/max (or on/off) ranges only, leaving default values untouched

Preset Globals is particularly powerful at the start of a new preset — set the MIDI channel for every control in two seconds, then fine-tune individual controls from there.

---

## The MIDI Monitor

The MIDI Monitor displays a live stream of every MIDI message passing through bcMAPPER — incoming from your hardware and outgoing to it (or to any selected output device in Virtual Mode).

In **Edit Mode**, the monitor is hidden by default to keep the controller as large as possible. It **opens automatically when Virtual Mode is enabled** and closes when Virtual Mode is disabled. You can toggle it manually from the header at any time.

### Filtering

- **Direction** — Show messages going **In**, **Out**, or **Both**. When Virtual Mode is enabled, the direction filter automatically switches to **Both** so you can see the full picture of incoming feedback and outgoing control messages together.
- **Message Types** — Toggle visibility per type: Note On, Note Off, CC, Program Change, SysEx, Other (includes Pitch Bend, Aftertouch, etc.)

MIDI clock and system real-time messages (clock pulses, active sensing, etc.) are filtered out automatically — they don't appear in the monitor regardless of filter settings.

### Reading Entries

Each entry shows: direction (IN or OUT), timestamp, message type, MIDI channel, and message-specific details such as CC number and value, note name and velocity, or byte count for SysEx.

The monitor auto-scrolls to keep the most recent messages visible. **Clear** wipes the log. Use the type filter to reduce noise when a lot of traffic is flowing.

---

## The Footer

The footer bar at the bottom of the window shows at-a-glance status information:

- **Preset Info** — The name and device type of the currently loaded preset
- **Dirty Indicator** — An asterisk `*` appears when there are unsaved changes to the current preset
- **Slot Info** — The current slot number when working within a bank (e.g., "Slot 3 / 32")
- **Device Status** — Whether MIDI input and output devices are connected and active
