# Keyboard Shortcuts 

All keyboard shortcuts in bcMAPPER, organized by category. On Mac, use `Cmd` wherever you see `Cmd/Ctrl`. On Windows and Linux, use `Ctrl`.

---

## File Operations

| Shortcut | Action |
|----------|--------|
| `Cmd/Ctrl + N` | Create a new preset — lands in the next empty bank slot if a bank is loaded, or creates a standalone preset otherwise |
| `Cmd/Ctrl + S` | Save the current preset |
| `Cmd/Ctrl + ,` | Open Preferences |

---

## Editing

| Shortcut | Action |
|----------|--------|
| `Cmd/Ctrl + Z` | Undo the last change (up to 100 steps) |
| `Cmd/Ctrl + Shift + Z` | Redo a previously undone change |
| `Ctrl + Y` | Redo (Windows alternative) |
| `Cmd/Ctrl + C` | Copy the selected control(s) configuration — includes MIDI parameters, labels, colors, and font settings |
| `Cmd/Ctrl + V` | Paste the clipboard configuration onto the selected control(s) — same control type only |
| `Cmd/Ctrl + X` | Cut the selected control(s) — copies their configuration, then resets them to defaults |
| `Cmd/Ctrl + D` | Duplicate the selected control(s) into the next adjacent control(s) of the same type |
| `Cmd/Ctrl + Shift + S` | Swap two selected controls — must select exactly 2 controls of the same type |

---

## Control Selection

| Shortcut / Interaction | Action |
|------------------------|--------|
| `Click` | Select a single control; deselects everything else |
| `Cmd/Ctrl + Click` | Add or remove that control from the current multi-selection |
| `Shift + Click` | Select all controls from the previously selected control to this one (range select) |
| `Click + Drag` on empty space | Draw a marquee selection box — all controls inside get selected |
| `Click` on empty space (< 4px drag) | Deselect all controls |
| `Escape` | Cancel an active marquee draw, or cancel an inline text edit (scribble strip label or preset name) |

> **On the 4px threshold:** A click on empty space only counts as "deselect" if your mouse moved less than 4 pixels from where you pressed down. Anything more starts a marquee selection. This prevents accidental deselects when you slightly wobble at the start of a drag.

---

## Slot Navigation

| Shortcut | Action |
|----------|--------|
| `←` | Move to the previous preset slot |
| `→` | Move to the next preset slot |

> **Focus note:** Arrow key shortcuts require keyboard focus outside of any text input. If they're not responding, click somewhere on the controller canvas first. Text input fields in the Parameter Panel capture keyboard input while focused — click away from a field before using these shortcuts.

---

## Hardware Communication

| Shortcut | Action |
|----------|--------|
| `Cmd/Ctrl + T` or `Cmd/Ctrl + Shift + S` | Open the Send to Hardware dialog |
| `Cmd/Ctrl + R` or `Cmd/Ctrl + Shift + R` | Open the Receive from Hardware dialog |
| `Cmd/Ctrl + Shift + V` | Toggle Virtual Mode on/off |

---

## Virtual Mode Interactions

Virtual Mode replaces hardware interaction with on-screen control. These aren't traditional keyboard shortcuts — they're mouse interactions that send real MIDI output.

| Interaction | Control Type | What It Does |
|-------------|---|---|
| Click + drag up | Encoder | Increases the value |
| Click + drag down | Encoder | Decreases the value |
| Mouse wheel | Encoder | Scrolls the value up or down |
| Click | Button | Toggles the button state (follows configured Controller Mode) |
| Click | Foot Switch | Activates the foot switch (same behavior as a button) |
| Click + drag up | Fader | Increases the fader value |
| Click + drag down | Fader | Decreases the fader value |
| Mouse wheel | Fader | Scrolls the value up or down |
| `Shift + Click` | Any control | Expands the Parameter Panel from its collapsed rail — use this to inspect a control's settings without leaving Virtual Mode |
