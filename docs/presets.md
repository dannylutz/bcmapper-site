# Working with Presets 

## What's in a Preset?

A **preset** is a complete snapshot of your controller's configuration — every parameter for every control, all in one file. It contains:

- All encoder settings — data type, CC number, NRPN address, mode, LED ring style, min/max values, default value, and resolution — for all 32 encoders on the BCR2000 or 8 on the BCF2000
- All button settings — 32 buttons on both controllers, plus 8 push encoder buttons on the BCR2000; includes controller mode, increment settings, bank select values, and MMC configuration
- All fader settings — 8 faders for BCF2000 only
- Foot switch configurations — 4 inputs on both controllers
- Hardware behavior flags — **Snapshot** (hardware auto-transmits all values when this preset is selected; manually triggered in Virtual Mode via the Send Snapshot button) and **Lock** (prevent hardware editing)
- Scribble strip labels and colors — your organizational layer, stored per-preset and visible on the controller layout

A **bank** is a collection of up to 32 preset slots — exactly matching the hardware's internal memory structure. Think of a bank as a "page" of 32 presets. You can have as many banks as you like.

---

## Two Ways to Organize: Standalone vs Bank

### Standalone Presets

A standalone preset is an independent file, not attached to any bank. Use them for:
- Quick experiments and one-off configurations
- Building blocks you plan to organize into banks later
- Sharing a single configuration with someone else without handing over an entire bank

Standalone presets appear at the top of the sidebar, sorted newest-first by modification time.

### Bank Presets

A bank holds up to **32 preset slots**, numbered 1–32. This mirrors the hardware exactly — the BCR2000 and BCF2000 both store presets in banks of 32. If you're planning to send presets to hardware and use them live, you'll want them organized in banks.

You can have an unlimited number of banks. A bank might represent a DAW, an instrument, a project, or a workflow — whatever makes sense for how you work.

> **Moving between the two:** Any standalone preset can be copied into a bank slot at any time via right-click → **Copy to Existing Bank**. And any bank slot can be exported as a standalone file. The two formats are fully interchangeable.

---

## Creating Presets

### New Empty Preset

Start from scratch with a blank slate:

1. Use the **Welcome screen**, the **File menu** in the header, or the **New Preset** button in the sidebar
2. Enter a name and choose the device type (BCR2000 or BCF2000)
3. A preset is created with all controls at their default values, and auto-saved immediately to your configured save directory
4. Click any control on the visual controller and start configuring

> **No save directory set?** bcMAPPER will prompt you to choose one. This also happens if the save directory has been moved or deleted.

### From Factory Presets

Both the BCR2000 and BCF2000 factory banks are built into bcMAPPER. They're created automatically on first launch and pinned to the bottom of the sidebar under **BCR Factory Presets** and **BCF Factory Presets**.

To use a factory preset as a starting point:
1. Expand the appropriate factory bank in the sidebar
2. Click any slot to load it into the editor
3. Modify as needed

Factory presets are fully editable. If you change one and want the original back, right-click it → **Revert to Factory Preset**.

### Copying Existing Presets

Duplicate a preset within a bank, or move a copy to another bank:

1. Right-click any preset slot in the sidebar
2. **Copy to Slot** — Opens a submenu showing all 32 slots with their preset names. Click any slot to place a copy there. Use this to make quick variations within the same bank or to move a copy into another slot.

### From Hardware

Pull presets off a connected BCR2000 or BCF2000:

1. Use the **Welcome screen's Import from Hardware** flow for a guided first-time experience
2. Or click **Receive from Hardware** in the header (`Cmd/Ctrl + R`) at any time
3. Select which slots to import, or choose All for the entire bank
4. Trigger the dump from the hardware — bcMAPPER listens for the incoming SysEx and parses it on the fly
5. The received preset(s) land in the current bank at the requested slot(s)

See [MIDI Communication](./midi-communication.md#receiving-presets-from-hardware) for detailed steps.

---

## Saving Your Work

### Auto-Save

bcMAPPER saves automatically when:
- A new preset or bank is created (saved immediately to the configured directory)

### Unsaved Changes Prompt

When you navigate away from a preset that has unsaved changes — by clicking a different bank slot, loading a different standalone preset, or switching to a different bank — bcMAPPER will ask what to do with those changes before proceeding:

- **Save & Continue** — Saves the current preset, then navigates to where you were going
- **Discard** — Abandons the unsaved changes and navigates immediately
- **Cancel** — Returns you to the current preset, unchanged, so you can decide what to do

The dirty indicator (a small colored dot or asterisk in the UI) signals when there are unsaved changes in the current slot.

### Manual Save

**`Cmd/Ctrl + S`** — Saves the current preset.

The **dirty indicator** — a small asterisk `*` in the footer — appears whenever there are unsaved changes. It disappears when you save.

### Back Up Everything at Once

**Preferences → File Paths → Back Up All Presets & Banks** — Creates a single timestamped zip containing every bank and standalone preset in bcMAPPER. A save dialog opens pre-filled with a suggested filename and location (`Backups/` subfolder of your configured save directory) — confirm to save there or navigate anywhere else.

Use this when:
- You want a full snapshot before making sweeping changes
- You're migrating to a new machine and want one file to carry everything
- You want a periodic archive of your entire library

Each run produces a new file — nothing is overwritten. A toast confirms the number of banks and presets included and shows the file location.

See [Backup](./user-interface.md#backup) in the UI reference for details on the zip structure and how to restore from it.

### Save Bank to File

**File → Save Bank to File** — Exports the entire current bank as a `.json` file. A system save dialog lets you choose the filename and destination.

Use this when:
- You want to share the whole bank with someone else
- You want a copy of a specific bank in a known location outside the Backups folder

### Export Options

- **Export Preset as .syx** — Saves the current preset as a standard SysEx file. Compatible with the hardware directly, other editors, and MIDI librarian apps.
- **Export Preset as .json** — Saves the preset in bcMAPPER's native format with full fidelity: all parameters, scribble strip labels, colors, and metadata (name, device type, timestamps).

> **Which format?** Use `.syx` when you need compatibility with hardware or external tools. Use `.json` when preserving labels and colors matters. The `.json` format is the only one that retains your scribble strip work.

---

## Opening and Importing

### Open a Bank File

1. Click **Open Bank** in the sidebar, or use **File → Open / Import Bank** in the header
2. Pick a `.json` or `.syx` file from your filesystem
3. The bank loads — you'll be asked to confirm if there are unsaved changes in the current session

### Import a Single Preset

**File → Import Preset** — Opens a file picker for `.json` or `.syx`. The preset is loaded into the current bank slot, or as a new standalone preset if no bank is active.

### Import Bank (.syx)

**File → Import Bank (.syx)** — Reads a SysEx file containing a full 32-preset bank dump and loads all 32 slots. This is the right choice when you've received a `.syx` from hardware or from a BCR/BCF community preset library.

### Loading a Bank from the Sidebar

Clicking **Load** next to a bank name in the sidebar:
1. Shows a confirmation dialog (to prevent accidentally losing unsaved work)
2. Switches the active working bank to the one you clicked
3. Expands that bank in the sidebar to show all 32 slots
4. Loads slot 1 automatically

---

## Managing Your Slots

### Navigating Between Slots

Multiple ways to move through the 32 slots in the active bank:

| Method | Action |
|--------|--------|
| Click a slot in the **Slot Strip** (header) | Jump to that slot |
| Click a preset slot in the **sidebar bank view** | Jump to that slot |
| `←` / `→` arrow keys | Previous / next slot |

### Filled vs Empty Slots

- **Filled slots** — Shown with the accent color in the Slot Strip, with the preset name listed in the sidebar
- **Empty slots** — Dimmed in the Slot Strip, showing "Empty" in the sidebar

### Renaming a Preset

Three ways:
1. Right-click the preset in the sidebar → **Rename**, type the new name, press Enter
2. Click the **preset name in the header** to edit it inline — same result
3. The name change takes effect immediately and saves with the preset

Good names include the target software, project, or function — "LogicPro-Mixer", "Prophet6-Editor", "Drum-Machine-Live" — rather than generic names like "Preset 1" that lose meaning over time.

### Clearing a Slot

1. Right-click the slot in the sidebar
2. Select **Clear Slot**

The slot's data is wiped and it becomes empty — but the slot itself remains as part of the bank. This is the right action when you want to erase a preset without deleting the bank.

### Deleting Presets

- **Standalone presets** — Right-click → **Delete**. Removes the preset and its file from disk. This is permanent.
- **Bank presets** — Use **Clear Slot**. The bank file stays intact.

### Duplicating a Bank

Right-click a bank name → **Duplicate Bank**. Creates a complete copy of the bank, all 32 slots included, with "(Copy)" appended to the name. Useful for creating a variant of an existing bank before making substantial changes.

---

## Factory Presets

bcMAPPER ships with the original Behringer factory content for both controllers, organized into two auto-managed banks.

### What's Included

**BCR2000 Factory Bank** — General MIDI controller configurations, DAW control templates, and synthesizer editor layouts.

**BCF2000 Factory Bank** — Mixer configurations, DAW control surface setups, and automation-focused controller templates.

### How They Work

**Auto-created on first launch** — You don't have to do anything. The factory banks appear in the sidebar the first time you run bcMAPPER, pinned to the bottom of the bank list.

**Auto-restored if deleted** — If the factory bank files are removed, renamed, or moved, bcMAPPER re-creates them the next time it launches. They'll always be there.

**Pinned position** — Factory banks always appear at the bottom of the sidebar bank list, separated from your own banks so they don't get lost in the shuffle.

**Fully editable** — Load any factory slot and change it however you like. bcMAPPER tracks whether the slot has been modified from its factory state and shows an "edited" indicator.

**Restorable** — Right-click any modified factory slot → **Revert to Factory Preset** to get the original Behringer configuration back. This doesn't affect any of your other presets or banks.

---

## Best Practices

**Save frequently.** `Cmd/Ctrl + S` takes less than a second. Use the dirty indicator (`*` in the footer) as a prompt — if it's there, save.

**Name things descriptively.** "Ableton-Live-Mix" is useful three months from now. "New Preset (2)" is not. Include the target software, instrument, or purpose in every name.

**Label every control.** Even a short label — "Vol", "Pan", "Cutoff", "Send A" — makes a preset dramatically more readable when you're in the middle of a session and need to find the right knob fast. Double-click any scribble strip to label it.

**Color-code by function group.** Assign strip colors to categories of controls — all mixer faders in blue, all effects in teal, transport controls in red. The visual grouping pays off immediately when you're looking at a complex preset.

**Organize by workflow, not by device.** One bank per project type (mixing, synthesis, live performance) tends to be more maintainable than one bank per session or one bank per date.

**Back up regularly.** Use **Preferences → File Paths → Back Up All Presets & Banks** to create a full snapshot zip in one click. For individual banks you want to share or archive separately, also export them as `.json` (preserves labels and colors) and `.syx` (hardware-compatible). Store copies somewhere safe outside of bcMAPPER's save directory.

**Use Preset Globals first, then specialize.** When starting a new preset from scratch, use Preset Globals to set the MIDI channel across all controls at once, then change the specific controls that need a different channel. Working top-down is faster than going control-by-control.

**Use encoder groups for denser BCR2000 presets.** The BCR2000's 8 push encoders can carry up to 4 separate assignments — one per group (G1–G4). In bcMAPPER, encoder slots E1–E8 are Group 1, E9–E16 are Group 2, E17–E24 are Group 3, and E25–E32 are Group 4. Pressing G1–G4 on the hardware activates the corresponding group's assignments on the physical knobs. Use this to pack 32 different encoder assignments onto 8 physical knobs. bcMAPPER auto-calculates the `.egroups` count from the highest populated group, so G-buttons beyond what you've actually used are automatically suppressed.

**Link a Global Setup Profile to every bank you send to hardware.** Device-wide settings like MIDI mode, foot switch polarity, and SysEx device ID live outside the presets — they belong to the hardware's global setup. Create a profile in **Hardware → Global Setup Profiles...** and link it to your bank via the dropdown in the sidebar. Then whenever you send that bank, the global setup goes first automatically. See [Global Setup Profiles](./midi-communication.md#global-setup-profiles).

**Test incrementally.** Configure your first row or group of controls, send them to hardware, verify they work in your DAW, then continue. Catching a mistake when you've set up 8 controls is much faster than debugging all 32.

**Duplicate before making big changes.** Before restructuring an existing preset, right-click the bank → Duplicate Bank. This gives you a safety copy you can fall back to if the changes don't work out.
