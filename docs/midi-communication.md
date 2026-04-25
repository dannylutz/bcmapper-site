# MIDI Communication 

bcMAPPER sits between you and your hardware — or between you and your DAW in Virtual Mode. This section covers how MIDI data flows in and out: connecting devices, sending and receiving presets, monitoring MIDI traffic, and using Virtual Mode to test without hardware.

---

## Connecting Your Device

### Selecting MIDI Devices

1. Click the **device selector** in the header toolbar (top right)
2. From the **Input** dropdown, choose your BCR2000 or BCF2000 (or an IAC bus for Virtual Mode)
3. From the **Output** dropdown, choose the same device (or the destination port)
4. The connection status indicator turns green when both are active and communicating

Both input and output must be selected for full bidirectional operation. Input lets you receive from hardware; output lets you send to it. You need both for the complete workflow.

Both dropdowns include a **None** option. Selecting None for a direction disconnects just that direction without affecting the other. This is particularly useful in Virtual Mode — for example, setting Output to an IAC bus and leaving Input as None avoids potential feedback loops if your DAW echoes messages back.

### What the Status Indicator Means

- **Connected** (green) — Device selected and responding
- **Disconnected** — No device selected, or the previously connected device was unplugged

### Troubleshooting Connection Problems

If your device isn't appearing in the dropdown, or you're seeing connection errors:

1. **Check power and cables.** Confirm the BCR/BCF is powered on and connected via USB.
2. **Click the refresh button** next to the device selector to re-scan for MIDI ports.
3. **Close other MIDI-using applications.** Only one application can own a MIDI port at a time. Your DAW, a browser tab with Web MIDI active, or any MIDI utility could be holding the port. Close them and refresh.
4. **Try a different USB port.** Ports on USB hubs can be unreliable — try connecting directly to the computer.
5. **Restart bcMAPPER.** MIDI system state can get stale. A relaunch often resolves detection issues.
6. **Check system-level MIDI settings.** On macOS, Audio MIDI Setup (in `/Applications/Utilities/`) shows connected devices and their status. On Windows, Device Manager confirms whether the USB MIDI device driver is correctly installed.

---

## Sending Presets to Hardware

When you're happy with a preset's configuration, sending it to the hardware is what makes it real.

### How to Send

1. Confirm your hardware is connected (both input and output selected in the device selector)
2. Load or create the preset you want to transmit
3. Click **Send to Hardware** in the header, or press `Cmd/Ctrl + T`
4. In the dialog, select the **target slot** (1–32) where this preset should land on the hardware
5. Click **Send**

The hardware receives the data, parses it, and updates that preset slot. On the BCR2000, you'll see the LED rings update. On the BCF2000, the motor faders will move to their default positions.

> **Important:** Sending to a slot overwrites whatever was previously stored there on the hardware. If the hardware has something valuable in that slot that you haven't backed up, receive it into bcMAPPER first.

### Sending Selected Controls

You can send one or more individual controls to hardware without resending the entire preset. This is useful when you've tweaked a few controls and want to test them without triggering a full 32-slot overwrite.

1. Select the control(s) you want to send — click one, or Shift-click / drag to select multiple
2. Right-click any selected control
3. Choose **Send N control(s) to hardware** from the context menu

bcMAPPER generates partial BCL covering only the selected controls and transmits it immediately — no dialog, no slot selection. The controls land in their natural positions within the current preset slot on the hardware.

> **Note:** A MIDI output device must be connected. The context menu item is disabled (greyed out) if no output is selected.

### What's Actually Being Sent

bcMAPPER translates your preset configuration into **BCL (B-Control Language)** — Behringer's own text-based configuration format — and wraps it in a SysEx message for transmission.

The SysEx packet structure:
```
F0 00 20 32 7F 7F 20 [BCL payload as ASCII bytes] F7
```

- `F0` — SysEx start byte
- `00 20 32` — Behringer's registered MIDI manufacturer ID
- `7F` — Device ID (broadcast, targets all connected B-Control devices)
- `7F` — Model ID (targets any B-Control model)
- `20` — BCL command identifier
- BCL payload as ASCII-encoded bytes
- `F7` — SysEx end byte

The BCL inside describes every control in the preset. You can inspect the BCL for any individual control using the **BCL Preview** section in the Parameter Panel.

---

## Receiving Presets from Hardware

### Via the Header Toolbar

1. Click **Receive from Hardware** in the header, or press `Cmd/Ctrl + R`
2. In the dialog, select which slots you want to receive from (choose individual slots, or **All** for the full 32-slot bank)
3. Click **Receive** — bcMAPPER sends a SysEx dump request to the hardware
4. The hardware responds with one SysEx message per preset
5. bcMAPPER parses the BCL from each message and reconstructs the control configurations
6. The received preset(s) appear in the current bank at the requested slot(s)

If you receive all 32 slots, the result is loaded as a new bank. Individual slots are loaded into their corresponding positions in the current bank.

### Via the Welcome Screen

The Welcome screen offers a more guided import experience — particularly useful the first time you're bringing hardware presets into bcMAPPER.

1. **Select device type** — BCR2000 or BCF2000
2. **Choose import mode:**
   - **Full Bank** — All 32 slots
   - **Single Preset** — Just the currently active slot
3. **Select your MIDI input** — Pick the port connected to your hardware
4. **Click Start Listening** — bcMAPPER enters listening mode and waits for incoming SysEx (no timeout)
5. **Trigger the dump from your hardware** — Hold **Edit** and press the RIGHT (>) preset button on the BCR2000, or use the hardware's dump function via Global Setup (EDIT + STORE), selecting the appropriate mode (SnGL or ALL) on encoder 6, and pushing the encoder to start the transfer.
6. **Watch presets arrive** — A progress indicator shows each preset being received and parsed
7. **Click Done** — All received presets are loaded into the editor

### How Parsing Works

When SysEx arrives, bcMAPPER:
1. Strips the SysEx wrapper and extracts the BCL text payload
2. Parses each BCL command (`$encoder`, `$button`, `$fader`, `.easypar`, `.mode`, `.default`, etc.)
3. Reconstructs the complete configuration for each control
4. Builds a preset from the result and loads it

The round-trip — bcMAPPER → hardware → receive back → bcMAPPER — is lossless for all hardware parameters. Scribble strip labels and colors survive only in `.json` format; they can't be stored on or retrieved from the hardware.

---

## The MIDI Monitor

The MIDI Monitor displays a live feed of every MIDI message flowing through bcMAPPER — incoming from your hardware and outgoing from it (or from the virtual controller in Virtual Mode).

### When It's Visible

- **Automatically opens** when you enable Virtual Mode
- **Automatically closes** when you disable Virtual Mode
- **Toggleable manually** from the header toolbar at any time

In Edit Mode, the monitor is hidden by default to maximize controller space. If you want to see live traffic while editing — for example, to watch how your hardware responds to control movements — you can toggle it on manually.

### Filtering the Feed

**Direction filter** — choose what direction of traffic to show:
- **In** — Messages arriving from your hardware or IAC input
- **Out** — Messages going out to hardware or to the selected output in Virtual Mode
- **Both** — Everything. This is set automatically when Virtual Mode is enabled.

**Type filter** — toggle individual message types on or off:
- Note On
- Note Off
- Control Change (CC)
- Program Change
- SysEx
- Other (covers Pitch Bend, Aftertouch, and any other message types)

MIDI clock and other system real-time messages (active sensing, timing clock, etc.) are filtered out automatically regardless of the type filter setting — they generate too much noise to be useful in the monitor.

Use the type filter when you're working with a lot of traffic and only care about a specific kind — for example, filtering to CC only while testing encoder behavior, or showing only SysEx during a hardware dump.

### Reading Monitor Entries

Each entry follows this format:
```
[Direction]  [Timestamp]    [Type]        [Channel]   [Details]
OUT          12:34:56.789   CC            Ch:1        CC#7 = 100
IN           12:34:57.012   Note On       Ch:1        C3  vel:127
IN           12:34:58.456   SysEx         —           32 bytes
OUT          12:34:58.901   Program Chg   Ch:2        Program 5
```

The monitor auto-scrolls to show the most recent messages. Click **Clear** to reset the log. There's no message limit — if you run it for a while with a lot of activity, filter by type to reduce the volume.

### RAW Mode

Click the **RAW** button in the monitor header to switch to RAW mode. Instead of parsed message rows, you get a hex dump of every incoming MIDI byte, with a human-readable label on each line.

RAW mode is particularly useful when working with SysEx, NRPN, or RPN — message types that span multiple bytes or multiple messages and aren't fully decoded in the standard view.

**NRPN / RPN assembly** — RAW mode includes a per-channel state machine that watches for CC 99/98 (NRPN parameter MSB/LSB) and CC 101/100 (RPN parameter MSB/LSB) sequences. Once a parameter number is established, subsequent Data Entry messages (CC 6 for MSB, CC 38 for LSB) are labeled with the resolved parameter number rather than just the raw CC number.

**CLK toggle** — MIDI Clock and Active Sensing bytes are hidden by default in RAW mode (they generate too much noise). Click the **CLK** button — which appears next to RAW when RAW mode is active — to show or hide these system real-time messages.

To return to the standard monitor view, click **RAW** again to deactivate it.

---

## Virtual Mode

Virtual Mode turns the on-screen controller into a fully functional MIDI device. The controls become interactive — you can twist encoders, click buttons, and drag faders to send live MIDI messages — and they also respond to incoming MIDI from your DAW or softsynth, keeping the on-screen controls in sync with what the receiving software is actually doing.

### Enabling Virtual Mode

1. Click the **gamepad icon** in the header toolbar
2. Confirm or select an output device — this is where the MIDI output will go (a virtual port for your DAW, a hardware synth, a MIDI interface)
3. The controller becomes interactive and the MIDI Monitor opens automatically

Everything in Virtual Mode sends output exactly as the hardware would — on the configured MIDI channel, with the configured data type, within the configured min/max range.

### How to Interact with Virtual Controls

| Control Type | Interaction |
|---|---|
| **Encoder** | Click and drag vertically — drag up to increase the value, drag down to decrease it. Or use the mouse wheel. |
| **Button** | Click to activate. Behavior follows the configured Controller Mode and Start State: Toggle alternates states, Momentary sends On on mousedown and Off on mouseup, Increment steps through the range with each click. |
| **Fader** (BCF2000) | Click and drag vertically. |
| **Foot Switch** | Click, same as a button. |

Every interaction sends a real MIDI message to the selected output device. The MIDI Monitor shows each message as it goes out.

### The LED Display in Virtual Mode

The LED display area on the visual controller (lower area of the BCR/BCF layout) shows the **current value being sent** as you interact with controls. After 2 seconds of inactivity on that control, the display clears and returns to showing the current preset number.

### Live MIDI Feedback

When an input port is selected alongside an output port, Virtual Mode provides **live two-way feedback**. Incoming MIDI messages from your DAW or softsynth are matched against the current preset's control mappings, and any matched control updates its on-screen position in real time — without you touching bcMAPPER.

This is useful for:
- Seeing how automation in your DAW moves controls in the preset
- Confirming that a softsynth's state is actually changing when you send to it
- Using bcMAPPER as a visual overlay that mirrors the state of your software instruments

The visual update rate is throttled to approximately 30fps per control — fast enough to look smooth, but not so fast that it becomes a performance burden.

**Recommended setup for live feedback:**
- Set Output to the IAC bus your DAW is listening on
- Set Input to the same IAC bus (or a second one if your DAW echoes back on the same port)
- In your DAW, route MIDI output from the instrument track back to the IAC bus input that bcMAPPER is listening on

> **Important:** If both Input and Output point to the same IAC bus, see the [Avoiding Feedback Loops](#avoiding-feedback-loops) section below before enabling Virtual Mode.

### Avoiding Feedback Loops

A MIDI feedback loop happens when:
1. bcMAPPER sends a message out on a port
2. The same port (or an IAC-routed version of it) delivers that message back to bcMAPPER's input
3. bcMAPPER processes the incoming message, possibly sending another — and the cycle repeats

Left unchecked, this can flood bcMAPPER with thousands of messages per second, locking up the UI.

**bcMAPPER has two defenses:**

**Echo suppression** — When bcMAPPER sends a MIDI message, it records what it sent in a short-lived buffer. If the same message arrives on the input within 50ms, it's recognized as its own echo and discarded. This handles the most common case: IAC buses that loop your own output back to you.

**Feedback loop detection** — If a large volume of non-echo matched messages keeps arriving, bcMAPPER concludes that there's a genuine loop from an external source. It will:
- Display a modal explaining what happened
- Automatically disable Virtual Mode
- Tell you which port combination is likely causing it and how to fix it

**How to prevent loops in the first place:**

| Situation | Recommendation |
|-----------|---------------|
| Sending to a DAW via IAC, not receiving feedback | Set Output to the IAC bus, set Input to **None** |
| Sending to a DAW and receiving DAW feedback | Use two IAC buses — one for bcMAPPER → DAW, one for DAW → bcMAPPER |
| Using with hardware only, no DAW | Use the physical USB MIDI ports directly; no IAC bus needed |

> **Warning:** Enabling Virtual Mode with the same IAC port selected for both Input and Output will generate a same-port warning. bcMAPPER will tell you this configuration is likely to produce a feedback loop and suggest setting one direction to **None**.

### Setting Up a Virtual MIDI Port

To route bcMAPPER's output into your DAW during Virtual Mode, you need a virtual MIDI port — a software-level "cable" that loops bcMAPPER's output back into your DAW's input.

**On macOS:**
1. Open **Audio MIDI Setup** (in `/Applications/Utilities/`)
2. Go to **Window → Show MIDI Studio**
3. Double-click the **IAC Driver** icon in the MIDI Studio
4. Check the box labeled **Device is online**
5. If there are no ports listed, click the **+** button to add one — you can name it anything
6. Close Audio MIDI Setup
7. The IAC port now appears in bcMAPPER's output device selector immediately

For two-way feedback with a DAW, create **two IAC ports** (e.g., "bcMAPPER → DAW" and "DAW → bcMAPPER") and configure each end accordingly. This keeps output and input on separate buses and eliminates self-echo.

**On Windows:**
1. Download **loopMIDI** from tobias-erichsen.de (it's free and widely used)
2. Install and launch it
3. Click the **+** button to create a new virtual MIDI port — give it a recognizable name
4. The port appears in bcMAPPER's output device list right away

If no virtual ports are detected when you enable Virtual Mode, bcMAPPER will offer to guide you through the setup for your platform.

> **Tip:** In your DAW, create a MIDI track with the virtual port as its input. Monitor the track and you'll hear bcMAPPER's virtual controller output in real time. This is a great way to test a preset configuration without the hardware in hand.

### MIDI Reconnect

IAC buses and virtual MIDI ports can silently drop after the system sleeps, after an audio interface is reconnected, or when another app restarts. If the MIDI Monitor shows no activity but you know MIDI should be flowing, click the **Reconnect** button (plug icon, next to the Refresh button in the device selector area). This re-establishes the connection to the current port without changing which port is selected.

---

## Global Setup Profiles

The BCR2000 and BCF2000 have a set of **device-wide settings** — stored in the hardware's `$global` section — that govern how the whole device behaves, independent of any individual preset. bcMAPPER lets you save these as named profiles, send them to hardware with one click, and link them to banks so the global setup travels with your presets automatically.

### The Seven Global Settings

| Setting | What it controls | Valid values |
|---------|-----------------|--------------|
| **MIDI Mode** | How MIDI data is routed between USB and DIN ports | U-1, U-2, U-3, U-4 (USB-only routing variants), S-1, S-2, S-3, S-4 (USB + DIN) |
| **Startup Preset** | Which preset slot loads when the device powers on | `last` (resume the last-used preset), or a fixed slot number 1–32 |
| **Foot Switch** | Wiring polarity of the foot switch input | `norm` (normally closed), `inv` (normally open), `auto` (autodetect) |
| **Receive Channel** | MIDI channel on which the device listens for Program Change messages to switch presets | `off` (disabled), or 1–16 |
| **SysEx Device ID** | The device ID used in all SysEx communication | 1–16. Must match what bcMAPPER uses when sending (set in the Send to Hardware dialog) |
| **TX Interval** | Minimum milliseconds between repeated MIDI outputs for the same control | 2, 5, 10, 20, 50, or 100 ms |
| **Dead Time** | How long (in ms) encoders and faders ignore incoming MIDI after being physically moved | 0–200 ms (BCR2000 default: 0 ms; BCF2000 default: 100 ms) |

> **U vs S modes:** U modes route MIDI data via USB only. S modes send MIDI data simultaneously over both USB and the DIN (5-pin) MIDI out. Within each group, the number (1–4) determines which USB sub-port or DIN output the data goes to. Consult the BCR/BCF MIDI implementation guide for exact routing tables.

### Creating a Profile

1. Open **Hardware → Global Setup Profiles...** in the header menu
2. Click **New Profile**
3. Enter a name (e.g., "Studio Rig – USB Only") and select the device type (BCR2000 or BCF2000)
4. Adjust the seven settings as needed — the form shows factory defaults for the selected device
5. Click **Create Profile**

The profile is saved immediately and persists across sessions.

### Editing and Deleting Profiles

All saved profiles appear in the profile list in the Global Setup dialog. Each profile card shows the device type, MIDI mode, and SysEx device ID at a glance.

- **Edit** (pencil icon) — Opens the profile back in the editor. All changes take effect when you click **Save Changes**.
- **Delete** (trash icon) — Permanently removes the profile. Any banks that were linked to it are automatically unlinked; no bank data is lost.

### Sending a Profile to Hardware

1. Open **Hardware → Global Setup Profiles...**
2. Find the profile you want to send
3. Click the **upload icon** on the profile card

bcMAPPER generates the BCL `$global` section and sends it to the currently connected output device. The device reboots its global state to match the settings in the profile.

You can also click the **copy icon** to copy the raw BCL to your clipboard, for pasting into BC Manager or MIDI-OX.

> **No output device connected?** The upload button is hidden when no MIDI output is available. Connect a device first via the device selector in the header.

### Linking a Profile to a Bank

The most powerful use of global setup profiles is linking one to a bank so everything stays in sync:

1. Load the bank you want to link (it must be the **active** bank)
2. In the sidebar, find the bank's card — below the Active badge, you'll see a small **Global Setup** dropdown
3. Select a profile from the dropdown (only profiles matching the bank's device type appear)

From this point on, whenever you use **Send to Hardware → All 32 Presets**, bcMAPPER automatically sends the linked global setup first, then the presets. No extra steps.

The **BCL Code** tab in Send to Hardware also shows the global setup BCL prepended to the bank BCL, so you can inspect or copy the complete sequence.

To unlink a profile, select **No global setup linked** from the dropdown.

### When to Use Global Setup Profiles

**Always link one when sending a full bank.** Even if you're happy with the hardware's current global state, having a profile linked makes your bank self-contained — when you come back to it six months later or share it with someone else, the global setup travels with it.

**Use separate profiles for different rigs.** If you use the same controller with multiple setups — one connected via USB, one via DIN — create one profile per rig with the appropriate MIDI mode and device ID. Switching rigs is then just loading the right bank.

**Match the SysEx device ID.** The device ID in your global profile must match the device ID you use in the Send to Hardware dialog. Mismatched IDs cause the hardware to ignore incoming SysEx. The factory default on both devices is ID 1 (0-based value 0 in SysEx headers).

---

## Global Setup Profiles

The BCR2000 and BCF2000 have a set of **device-wide settings** — stored in the hardware's `$global` section — that govern how the whole device behaves, independent of any individual preset. bcMAPPER lets you save these as named profiles, send them to hardware with one click, and link them to banks so the global setup travels with your presets automatically.

### The Seven Global Settings

| Setting | What it controls | Valid values |
|---------|-----------------|--------------|
| **MIDI Mode** | How MIDI data is routed between USB and DIN ports | U-1, U-2, U-3, U-4 (USB-only routing variants), S-1, S-2, S-3, S-4 (USB + DIN) |
| **Startup Preset** | Which preset slot loads when the device powers on | `last` (resume the last-used preset), or a fixed slot number 1–32 |
| **Foot Switch** | Wiring polarity of the foot switch input | `norm` (normally closed), `inv` (normally open), `auto` (autodetect) |
| **Receive Channel** | MIDI channel on which the device listens for Program Change messages to switch presets | `off` (disabled), or 1–16 |
| **SysEx Device ID** | The device ID used in all SysEx communication | 1–16. Must match what bcMAPPER uses when sending (set in the Send to Hardware dialog) |
| **TX Interval** | Minimum milliseconds between repeated MIDI outputs for the same control | 2, 5, 10, 20, 50, or 100 ms |
| **Dead Time** | How long (in ms) encoders and faders ignore incoming MIDI after being physically moved | 0–200 ms (BCR2000 default: 0 ms; BCF2000 default: 100 ms) |

> **U vs S modes:** U modes route MIDI data via USB only. S modes send MIDI data simultaneously over both USB and the DIN (5-pin) MIDI out. Within each group, the number (1–4) determines which USB sub-port or DIN output the data goes to. Consult the BCR/BCF MIDI implementation guide for exact routing tables.

### Creating a Profile

1. Open **Hardware → Global Setup Profiles...** in the header menu
2. Click **New Profile**
3. Enter a name (e.g., "Studio Rig – USB Only") and select the device type (BCR2000 or BCF2000)
4. Adjust the seven settings as needed — the form shows factory defaults for the selected device
5. Click **Create Profile**

The profile is saved immediately and persists across sessions.

### Editing and Deleting Profiles

All saved profiles appear in the profile list in the Global Setup dialog. Each profile card shows the device type, MIDI mode, and SysEx device ID at a glance.

- **Edit** (pencil icon) — Opens the profile back in the editor. All changes take effect when you click **Save Changes**.
- **Delete** (trash icon) — Permanently removes the profile. Any banks that were linked to it are automatically unlinked; no bank data is lost.

### Sending a Profile to Hardware

1. Open **Hardware → Global Setup Profiles...**
2. Find the profile you want to send
3. Click the **upload icon** on the profile card

bcMAPPER generates the BCL `$global` section and sends it to the currently connected output device. The device reboots its global state to match the settings in the profile.

You can also click the **copy icon** to copy the raw BCL to your clipboard, for pasting into BC Manager or MIDI-OX.

> **No output device connected?** The upload button is hidden when no MIDI output is available. Connect a device first via the device selector in the header.

### Linking a Profile to a Bank

The most powerful use of global setup profiles is linking one to a bank so everything stays in sync:

1. Load the bank you want to link (it must be the **active** bank)
2. In the sidebar, find the bank's card — below the Active badge, you'll see a small **Global Setup** dropdown
3. Select a profile from the dropdown (only profiles matching the bank's device type appear)

From this point on, whenever you use **Send to Hardware → All 32 Presets**, bcMAPPER automatically sends the linked global setup first, then the presets. No extra steps.

The **BCL Code** tab in Send to Hardware also shows the global setup BCL prepended to the bank BCL, so you can inspect or copy the complete sequence.

To unlink a profile, select **No global setup linked** from the dropdown.

### When to Use Global Setup Profiles

**Always link one when sending a full bank.** Even if you're happy with the hardware's current global state, having a profile linked makes your bank self-contained — when you come back to it six months later or share it with someone else, the global setup travels with it.

**Use separate profiles for different rigs.** If you use the same controller with multiple setups — one connected via USB, one via DIN — create one profile per rig with the appropriate MIDI mode and device ID. Switching rigs is then just loading the right bank.

**Match the SysEx device ID.** The device ID in your global profile must match the device ID you use in the Send to Hardware dialog. Mismatched IDs cause the hardware to ignore incoming SysEx. The factory default on both devices is ID 1 (0-based value 0 in SysEx headers).

---

## BCL: The Language Under the Hood

**B-Control Language (BCL)** is Behringer's text-based configuration format for the BCR2000 and BCF2000. Every preset bcMAPPER sends to hardware is expressed in BCL inside a SysEx wrapper. Every preset received from hardware arrives as BCL inside a SysEx wrapper.

You'll never need to write BCL by hand — that's what bcMAPPER is for. But understanding the format is helpful for debugging, and it's genuinely interesting to see what bcMAPPER is actually generating.

### Viewing the BCL for a Control

1. Select any control on the visual controller
2. In the Parameter Panel, scroll to the bottom and expand the **BCL Preview** section
3. The generated BCL for that specific control is displayed in full

### What BCL Looks Like

An encoder configured as CC#7 on channel 1, absolute mode, bar LED ring, range 0–127, default 0:
```
$encoder 1
  .easypar CC 1 7 0 127 absolute
  .showvalue on
  .default 0
  .mode bar
```

A button configured for Note On, channel 1, note C3 (MIDI note 36), toggle mode:
```
$button 1
  .easypar NOTE 1 36 0 127
  .showvalue on
  .default 0
  .mode toggle
```

An encoder in NRPN mode, relative-1, pan LED ring:
```
$encoder 5
  .easypar NRPN 2 0 32 0 16383 relative-1
  .showvalue on
  .default 8192
  .mode pan
```

### Key BCL Commands

| Command | Purpose |
|---------|---------|
| `$encoder N` | Selects encoder number N for configuration |
| `$button N` | Selects button number N |
| `$fader N` | Selects fader number N (BCF2000 only) |
| `.easypar` | Defines the MIDI parameter: `type channel number min max mode` |
| `.mode` | Sets the LED ring style (for encoders/faders) or button behavior mode |
| `.default N` | Sets the initial value when the preset loads |
| `.showvalue on` | Enables value display on the hardware's LED screen |

### Using BCL for Debugging

If a control is behaving unexpectedly on the hardware:

1. Select that control in bcMAPPER and open the BCL Preview
2. Compare the generated BCL with what you'd expect given your settings
3. If possible, receive the preset back from the hardware and compare the BCL it sends back
4. Any discrepancy between what you sent and what came back points to a parsing issue or an unsupported parameter combination

The receive-and-compare cycle is the most reliable debugging approach: configure in bcMAPPER, send, receive back, open BCL Preview, check that the roundtrip preserved everything correctly.

---

## File Formats

bcMAPPER works with two file formats: `.syx` (standard SysEx) and `.json` (bcMAPPER native).

### SysEx Files (.syx)

Standard binary MIDI SysEx format. Contains one or more presets encoded as BCL inside SysEx messages. This format:
- Is compatible with virtually all MIDI librarians, SysEx utilities, and hardware-compatible tools
- Can be loaded directly onto the BCR2000 or BCF2000 without bcMAPPER
- Is readable by other BCR/BCF editors
- Does **not** preserve scribble strip labels or colors (those are bcMAPPER-specific data)

Use `.syx` when interoperability with hardware or external tools is the priority.

### JSON Files (.json)

bcMAPPER's native format. Contains the full preset or bank in readable JSON, including:
- All control parameters for every control in the preset
- Scribble strip labels and colors
- Preset and bank name, device type
- Metadata: creation time, last modified time, version information

Use `.json` for backups and whenever you want to preserve the full picture including your labels and color-coding.

### Importing Files

**From the File menu:**
- **Open / Import Bank** — Opens a file picker; accepts `.json` or `.syx`
- **Import Preset** — Loads a single preset from `.json` or `.syx`
- **Import Bank (.syx)** — Specifically for importing a full 32-slot bank dump from a `.syx` file

**From the sidebar:**
- The **Open Bank** button opens a file picker directly

### Exporting Files

- **Export Preset as .syx** — Saves the current preset as a standard SysEx file, from the File menu
- **Export Preset as .json** — Saves the current preset in native format, from the File menu
- **Save Bank to File** — Exports the entire current bank as a `.json` file, from the File menu

> **Tip:** For important banks, export both formats. Keep the `.json` for full fidelity backups (labels, colors, metadata) and the `.syx` for hardware compatibility and portability to other tools.
