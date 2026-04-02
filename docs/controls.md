# Configuring Controls 

Every physical control on your BCR2000 or BCF2000 — every encoder, button, fader, and foot switch — can be independently configured with its own MIDI parameters. Click any control on the visual controller to open its settings in the Parameter Panel.

Changes you make in the Parameter Panel take effect in the application immediately. They're written to the preset file when you save. What gets sent to the hardware is BCL (B-Control Language) generated from those settings — so if something behaves unexpectedly on the hardware, the BCL Preview section in the Parameter Panel is always a good first debugging stop.

---

## Encoders

Rotary knobs with LED rings. The BCR2000 has 32 total — 8 push encoders in the top row (each with a built-in push button) plus 24 standard encoders in three rows below. The BCF2000 has 8 push encoders.

### Encoder Parameters

**Channel** (1–16)
The MIDI channel this encoder transmits on.

**Data Type**
What kind of MIDI message the encoder sends when turned:
- **CC** — Control Change. The most common and versatile type. Turn the encoder, it sends a CC message with your chosen CC number and a value.
- **NRPN** — Non-Registered Parameter Number. Addresses up to 16,384 unique parameters using an MSB/LSB pair. Values expand to 14-bit resolution (0–16383).
- **Program Change** — Steps through program/patch numbers. Turning clockwise sends higher numbers.
- **Pitch Bend** — Simulates a pitch wheel with full 14-bit resolution (0–16383).
- **Aftertouch** — Sends channel pressure values.
- **GS/XG** — Access Roland GS and Yamaha XG synthesis parameters directly (see GS/XG section below).

**CC Number** (0–127) — CC mode only
The specific controller number being targeted. CC7 is the General MIDI volume convention, CC10 is pan, but you can use any number for any purpose.

**NRPN MSB / LSB** (0–127 each) — NRPN mode only
The parameter address, split across a high byte (MSB) and a low byte (LSB). Together they identify one of 16,384 possible NRPN parameters.

**GS/XG Parameter** — GS/XG mode only
Select from a preset list of Roland GS and Yamaha XG synthesis parameters: Filter Cutoff, Resonance, Vibrato Rate, Vibrato Depth, Vibrato Delay, EG Attack, EG Decay, EG Release, Modulation, Portamento Time, Volume, Pan, Reverb Send, Chorus Send, Delay Send.

**Mode**
How the encoder reports its position:
- **Absolute** — Sends the actual knob position as a value within the min–max range. The hardware's LED ring reflects the physical position directly.
- **Relative 1** — Encodes movement as an offset from 64. Clockwise rotation sends values above 64 (how far above indicates speed); counterclockwise sends values below 64. This is the most common relative encoding for DAW integration.
- **Relative 2** — Two's complement encoding. Clockwise = small positive numbers (1–63); counterclockwise = large numbers near 127.
- **Relative 3** — Sign-bit encoding. Clockwise = 1–63 (increment); counterclockwise = 65–127 (decrement with the sign bit set).

> **Which relative mode?** It depends on what your target software expects. Ableton Live, Logic Pro, and most modern DAWs accept Relative 1. Check your software's MIDI learn documentation if you're unsure.

**LED Ring Mode**
The visual style of the LED ring on the hardware:
- **1dot** — A single LED lights at the current position. Clean and precise.
- **12dot** — LEDs fill from the minimum position up to the current value. Good for level-style controls.
- **Bar** — A sweeping bar from the left edge. Similar to 12dot but starting from a fixed left edge.
- **Pan** — Two LEDs expand outward from center. Purpose-built for panning controls.
- **Spread** — LEDs spread symmetrically from center outward in both directions. Good for symmetrical effects parameters.
- **Qual** — A quality/level indicator style. Useful for mix bus or mastering-type controls.

**Min Value / Max Value**
The output range. Standard range is 0–127; expands to 0–16383 for NRPN and Pitch Bend. The **↔ swap button** next to the pair instantly inverts the range — handy for making an encoder feel "backwards" for certain parameters, or for creating a reverse-pan.

**Default Value**
The value the encoder starts at when the preset loads. The hardware uses this to know where to "wake up" when you switch presets.

---

## Push Encoder Buttons

Each push encoder is physically one control but behaves as two independent MIDI sources: the **rotary encoder** and the **push button** built into it.

When you select a push encoder in bcMAPPER, the Parameter Panel shows a **toggle** at the top to switch between editing:
- **Encoder** — All the rotary parameters described above
- **Enc Btn** — Push button parameters, configured just like a standard button

The encoder and its push button operate on completely separate MIDI channels, data types, and parameters. They have no dependency on each other.

---

## Buttons

Both the BCR2000 and BCF2000 have **32 buttons**. The BCR2000 also has 8 push encoder buttons, giving up to 40 button-type controls on that device. Foot switches also use this same parameter set.

### Button Parameters

**Channel** (1–16)
The MIDI channel.

**Data Type**
What kind of MIDI message the button sends:
- **CC** — Control Change, with configurable on/off values
- **Note** — Note On when activated, Note Off when deactivated
- **Program Change** — Sends a program/patch number
- **NRPN** — Non-Registered Parameter Number, with on/off values
- **Aftertouch** — Channel or per-note pressure
- **Pitch Bend** — Sends a pitch bend value on press and/or release
- **MMC** — MIDI Machine Control transport commands
- **GS/XG** — Roland GS / Yamaha XG synthesis parameters

**Controller Mode**
How the button behaves:
- **Toggle** — Alternates between On and Off values with each press. The button LED reflects which state it's currently in.
- **Toggle On** — Always sends the On value. LED stays lit. Useful for buttons that should only ever trigger one side of a parameter.
- **Toggle Off** — Always sends the Off value. LED stays dark.
- **Momentary (Up/Down)** — Sends the On value when the button is pressed down, and the Off value when released. Behaves like a piano key.
- **Increment** — Each press advances through a configurable number of steps, cycling through the min-to-max range before wrapping. Useful for cycling through states or programs.

**Increment Steps** (1–127) — Increment mode only
How many steps to cycle through. For example, if you want a button to cycle through 8 program changes, set Increment Steps to 8.

**Note / CC Number** (0–127)
The target note number or CC number.

**NRPN MSB / LSB** — NRPN mode only
The parameter address (same MSB/LSB pair concept as encoders).

**Aftertouch Scope** — Aftertouch mode only
- **Channel** — A single pressure value applies to all notes on the channel (monophonic/channel aftertouch)
- **Note** — Per-note pressure (polyphonic aftertouch)

**MMC Command** — MMC mode only
Selects the transport action:
- **Stop** — Halts transport
- **Play** — Starts playback
- **Fast Forward (Deferred)** — Fast-forwards
- **Rewind** — Rewinds
- **Record** — Engages record mode
- **Pause** — Pauses transport
- **Locate** — Jumps to a specific SMPTE timecode position

**Frame Rate** — MMC mode only
Controls whether a Locate (SMPTE timecode jump) is sent before the MMC command:
- **Off** — Only the MMC command is transmitted. No position is sent.
- **24 / 25 / 30 / 29.97 Drop Frame** — A Locate message is transmitted first (at the specified frame rate), then the MMC command. This allows any transport command — not just Locate — to jump to a specific position before executing.

> For example: with Frame Rate set to 30 and the command set to **Play**, pressing the button will first send a Locate jump to your specified timecode position, then send Play. This is useful for jump-to-bar-and-play type triggers.

**Locate Position** — Shown when Frame Rate is not Off
The SMPTE timecode position: Hours (0–23), Minutes (0–59), Seconds (0–59), Frames (0–29), Subframes (0–99).

- For the **Locate** command: the sequencer jumps to this position and stops
- For **Play**, **Rewind**, and other transport commands: the sequencer jumps to this position first, then executes the command

> **Note:** The **Locate** command always requires a Frame Rate to be set (not Off). If Frame Rate is Off, the Locate position is ignored.

**On Value / Off Value** (0–127)
The values sent for the active and inactive states. A **↔ swap button** is available to instantly invert them — useful for reversing the polarity of a toggle without manually re-entering values.

**Start State**
Whether the button's LED starts **On** or **Off** when the preset loads. This matters especially in Toggle mode, because it determines the button's initial state and therefore what value will be sent on the very first press.

---

## Faders (BCF2000 Only)

The BCF2000's 8 motorized faders slide vertically and are touch-sensitive. They behave similarly to encoders in terms of MIDI output — the difference is the physical form factor and the fact that the BCF2000's motors will physically move the fader to the Default Value position when the preset loads.

### Fader Parameters

**Channel** (1–16)

**Data Type** — CC, NRPN, Program Change, Pitch Bend, or GS/XG

**CC Number** (0–127) — CC mode

**NRPN MSB / LSB** — NRPN mode

**GS/XG Parameter** — GS/XG mode

**Mode** — Absolute or Relative

**Min / Max Values** — With **↔ swap button**. Inverting the range on a fader makes moving it up decrease the value — useful for certain mixing conventions.

**Default Value** — The starting position when the preset loads. The BCF2000 physically moves its motor faders to this position when the preset is selected.

---

## Foot Switches

The 4 foot switch inputs on both controllers accept external expression/switch pedals. They're configured exactly like buttons — every button parameter listed above applies to foot switches as well. A foot switch can toggle, fire MMC transport commands, send notes, or anything else a button can do.

---

## Scribble Strips

Every control has a **scribble strip** — a small label displayed directly on the visual controller layout, right above or below the control.

**To edit a label:**
1. Double-click the scribble strip area of the control you want to label
2. A text field appears — type your label (up to **12 characters**)
3. Press **Enter** to confirm, or **Escape** to cancel without changes

Labels are stored per-preset, persist between app sessions, and survive undo/redo. They are never transmitted to hardware — they exist purely for your own reference in bcMAPPER.

### Strip Colors and Opacity

From the **Bulk Edit Panel** (select two or more controls), you can assign background colors to scribble strips:

- **Built-in color palette** — A set of curated colors to choose from
- **Custom colors** — Click the **pipette icon** to open the color picker, type any hex value, and click Add & Apply. Custom colors are saved to your preferences and appear in the palette for all future sessions.
- **Opacity slider** — Controls how saturated the background color appears. Low opacity creates a subtle tint; full opacity gives a solid fill. A subtle tint often looks cleaner in a dense layout while still conveying the grouping.
- **No-color option** — The first item in the palette (marked with ✕) clears any existing color from selected strips.

Color-coding tips:
- Assign one color per functional group (all mixer controls = blue, all synth controls = teal, all transport = red)
- Use lower opacity for secondary controls within a group — same color family, less visual weight
- Apply colors after you've finished labeling, so you can see the full picture before deciding on groups

---

## MIDI Data Types: A Deeper Look

### CC (Control Change)

The workhorse of MIDI control. Sends a three-byte message: status byte (including channel number), CC number (0–127), and a value (0–127).

```
Status:  Bn  (where n = channel − 1, in hex; B0 = channel 1, BF = channel 16)
Data 1:  CC number (0–127)
Data 2:  Value (0–127)
```

Every CC number is technically equivalent from the protocol's perspective. The conventional assignments (CC7 = volume, CC10 = pan, CC11 = expression, CC64 = sustain pedal, etc.) are General MIDI standards and DAW defaults — not protocol rules. You can use any number for any purpose, as long as your target software is configured to listen for it.

### Note

Sends a Note On message when activated, and a Note Off when deactivated. In Momentary mode, press = Note On and release = Note Off. In Toggle mode, the first press sends Note On and the second sends Note Off.

```
Note On:   9n kk vv   (channel, key number 0–127, velocity 0–127)
Note Off:  8n kk vv
```

The "On Value" field sets the velocity of the Note On; the "Off Value" is the velocity of the Note Off (usually 0 or 64).

### Program Change

Sends a patch/program selection. One data byte containing the program number (0–127). Useful for switching sounds in a hardware synth, selecting a DAW scene, or triggering a plugin preset change.

```
Status:  Cn  (channel)
Data 1:  Program number (0–127)
```

### NRPN (Non-Registered Parameter Number)

NRPN dramatically extends CC's 128-controller limit by using four CC messages in sequence to address and set a parameter. The address space is 16,384 parameters (MSB 0–127 × LSB 0–127), each capable of carrying a 14-bit value (0–16383).

The four messages bcMAPPER sends for each NRPN event:
1. CC 99 — NRPN MSB (parameter address high byte)
2. CC 98 — NRPN LSB (parameter address low byte)
3. CC 6 — Value MSB (data entry high byte)
4. CC 38 — Value LSB (data entry low byte)

bcMAPPER handles all of this automatically — you just set the MSB, LSB, and value range, and the rest is handled behind the scenes.

### Pitch Bend

A dedicated 14-bit MIDI message for smooth pitch control, with a range of 0–16383 (center position is 8192). Pitch Bend doesn't use a CC number — it's its own message type. Great for any parameter where high resolution and smooth response matter.

```
Status:  En  (channel)
Data 1:  Value LSB
Data 2:  Value MSB
```

### Aftertouch

Pressure sensitivity applied after an initial button press. Two scope options:
- **Channel Aftertouch** — A single pressure value applies uniformly to all notes on the channel (monophonic). One data byte.
- **Note Aftertouch** — Per-note pressure for polyphonic expression. Two data bytes (note number + pressure value). Useful with instruments that support MPE or polyphonic aftertouch.

### GS/XG Parameters

Roland GS and Yamaha XG are synthesis extensions to General MIDI that expose synthesis parameters for direct MIDI control. When a control is set to GS/XG mode, you pick from a list that includes: Filter Cutoff, Resonance, Vibrato Rate, Vibrato Depth, Vibrato Delay, EG Attack, EG Decay, EG Release, Modulation Depth, Portamento Time, Volume, Pan, Reverb Send Level, Chorus Send Level, Delay Send Level.

GS/XG parameters are transmitted as NRPN sequences using the Roland GS or Yamaha XG-specified parameter addresses — bcMAPPER generates the correct sequence for whichever parameter you select.

### MMC (MIDI Machine Control)

Transport control for DAWs and recorders, using SysEx rather than standard MIDI channel messages. This means MMC buttons use a **Device ID** (0–127, where 127 broadcasts to all devices) instead of a MIDI channel. MMC buttons are always momentary — there's no toggle behavior for transport commands.

See the [Button Parameters](#button-parameters) section above for the full list of MMC options and how Frame Rate and Locate Position work together.

---

## Editing Controls

### Single Control Editing

1. **Click a control** on the visual controller to select it
2. The **Parameter Panel** opens with all parameters for that control
3. **Modify any field** — changes are applied to the preset state immediately
4. **Save** the preset when you're done (`Cmd/Ctrl + S`)

Quick operations in the Parameter Panel:
- **Copy** (`Cmd/Ctrl + C`) — Captures the current control's full configuration
- **Paste** (`Cmd/Ctrl + V`) — Applies the clipboard configuration to the selected control (same type only)
- **Reset to Defaults** — Restores this control to its factory configuration

### Bulk Editing

1. **Select multiple controls** using any combination of:
   - `Cmd/Ctrl + Click` to add or remove individual controls
   - `Shift + Click` to select a range
   - Click and drag on empty space for a marquee selection
2. The **Bulk Edit Panel** replaces the Parameter Panel
3. **Modify shared parameters** — the value applies to every selected control simultaneously
4. Fields showing `Mixed...` have different values across the selection; changing the field sets a uniform value for all

### Value Distribution

The Range Tool is one of the most useful time-savers in bcMAPPER. Use it whenever you need sequential assignments across a row or group of controls.

1. **Select the controls** you want to distribute values across. The distribution goes in selection order — left-to-right and top-to-bottom as controls appear on the controller.
2. In the Bulk Edit Panel, **choose a target field**: CC Number, Note/CC, Channel, On Value, or Off Value
3. **Set a start value** and an **end value**
4. **Click Apply Range**

The math: `value[i] = start + (end − start) × i / (n − 1)` where `i` is the control's index and `n` is the total count in the selection.

Example: 16 buttons selected, start CC = 0, end CC = 15 → buttons get CC 0, 1, 2, 3... 15 in order.
Reversed: start CC = 15, end CC = 0 → buttons get CC 15, 14, 13... 0.

### Preset Globals

For changes that should sweep across an entire preset:

1. Open the **Preset Globals** panel below the visual controller (click to expand if collapsed)
2. Choose **Apply To**: All Controls, Encoders Only, Buttons Only, or Faders Only
3. Set the desired **MIDI Channel**, **Data Type**, or **Values & Ranges**
4. Click **Apply** (or **Apply All** / **Apply Ranges** for the values section)

This is the power move at the start of a new preset — set the channel for all controls globally in two seconds, then change the handful of controls that need to be on a different channel.

---

## Swapping Values

Both the Parameter Panel (single control) and the Bulk Edit Panel (multi-selection) show a **↔ swap button** next to Min/Max and On/Off value pairs. Click it to instantly invert the pair — no retyping needed.

Common uses:
- Invert an encoder's range so clockwise rotation decreases the value instead of increasing it
- Flip a button's On/Off values to reverse its logic (what was "full on" becomes "full off")
- Correct a fader that feels backwards

---

## Copy & Paste

- **Copy** (`Cmd/Ctrl + C` or the Copy button in the Parameter Panel) — Copies the selected control's full configuration to the clipboard
- **Paste** (`Cmd/Ctrl + V` or the Paste button) — Applies the clipboard configuration to the currently selected control(s)

Rules:
- Copy/paste only works between **controls of the same type** — encoder to encoder, button to button, fader to fader. Attempting to paste across types does nothing.
- When multiple controls are selected, paste applies the clipboard configuration to all of them simultaneously.
- Scribble strip labels, colors, and font settings **are** included in the copy — everything gets transferred.

---

## Pro Tips

**Match LED ring mode to the parameter.** Use Bar for volume and level controls. Use Pan for anything centered around a midpoint. Use Spread or Pan for symmetric effects like width or reverb. The right ring mode makes the hardware feel intuitive.

**Use relative mode for endless rotation in DAWs.** Absolute mode can jump when your DAW's parameter position doesn't match the encoder's physical position. Relative 1 avoids this entirely — every turn sends an increment, not a position.

**Reserve MIDI channels by function.** Putting all mixer controls on channel 1, all synth parameters on channel 2, and transport on channel 3 makes DAW routing far more manageable than mixing everything on channel 1.

**Test before you've finished.** Send to hardware after configuring your first row of controls, not after all 32. It's much easier to catch a misconfigured CC number when there are 8 to check.

**Preset Globals first, then specialize.** Set the channel for all controls globally at the start, then change the specific controls that need to be different. Top-down is faster than one at a time.

**Color groups, not individual controls.** Assign a color to a category of related controls — all mixer channels in blue, all effects in teal, all transport in red. Use opacity to distinguish primary from secondary controls within a group.

**The swap button saves time.** If you're building a control that works in reverse (like an inverted filter sweep), use the ↔ swap button on Min/Max instead of remembering which number goes where.
