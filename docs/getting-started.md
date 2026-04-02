# Getting Started 

Welcome to bcMAPPER! This guide walks you through everything you need to go from a fresh install to a configured preset on your hardware — covering license activation, the Welcome screen, connecting your device, Virtual Mode, and the basic editing workflow.

---

## Step 1: Activate Your License

The very first thing you'll see on launch is the License Activation screen. Enter the **email address** you used when purchasing bcMAPPER and your **license code**, then click **Activate**.

> **Note:** An internet connection is required the first time you activate. After that, your license is stored locally and bcMAPPER runs fully offline.

Once activated, you'll land on the Welcome screen.

---

## The Welcome Screen

The Welcome screen is your starting point every time you launch without an open preset. It offers four paths:

### Create New BCR2000 Preset
Spins up a blank preset for the BCR2000 — 32 encoders (8 with push buttons, 24 without), 32 buttons, and 4 foot switch inputs. All controls start at sensible defaults.

### Create New BCF2000 Preset
Same idea, but for the BCF2000 — 8 push encoders, 8 motorized faders, 32 buttons, and 4 foot switch inputs.

### Import from Hardware
A guided, step-by-step flow that listens for SysEx data from your connected device. Use this to pull presets you've already programmed into the hardware and bring them into bcMAPPER for editing. See the [Import from Hardware](#import-from-hardware) section below.

### Open Existing Bank
Opens a file picker to load a `.json` or `.syx` bank file from disk. Use this when picking up a previous session or loading a bank someone shared with you.

### Setting a Save Directory
Before you dive in, it's worth telling bcMAPPER where to keep your files. The Welcome screen includes a **Save Directory** field — click it to choose a folder. New presets and banks will auto-save there. You can change this later in Preferences.

---

## Import from Hardware

If your BCR2000 or BCF2000 already has presets programmed onto it, this guided flow brings them into bcMAPPER so you can edit them visually.

1. **Select your device type** — BCR2000 or BCF2000. This tells bcMAPPER which control layout to expect when parsing the incoming data.

2. **Choose an import mode:**
   - **Full Bank** — Imports all 32 preset slots from the device in one go
   - **Single Preset** — Imports only the currently active preset on the hardware

3. **Connect your hardware** — Use the input device selector in the dialog to pick your BCR/BCF's MIDI input port. Both the device and the cable need to be connected and powered on.

4. **Click Start Listening** — bcMAPPER is now waiting for incoming SysEx data. It won't time out — it just waits.

5. **Trigger a dump from the hardware** — 
   - Hold **Edit** and press the **preset (right)** button.

6. **Watch the presets arrive** — As SysEx comes in, bcMAPPER parses it on the fly and shows you each preset being received with a progress indicator.

7. **Click Done** — The imported data is loaded into the editor, ready to go.

> **Tip:** If you're not sure how to trigger a dump from your hardware, the Behringer BCR2000/BCF2000 manual covers the SysEx dump procedure in the MIDI implementation section. Most copies are freely available as PDFs online.

---

## Connecting Your Hardware

Once past the Welcome screen, you can connect hardware at any time using the **Device Selector** in the header toolbar.

1. Click the device selector (top right of the header)
2. From the **Input** dropdown, pick your BCR2000 or BCF2000
3. From the **Output** dropdown, pick the same device
4. The connection status indicator turns green when both are active

Both input and output must be selected for full bidirectional communication. Input alone lets you receive from hardware; output alone lets you send to it. You need both for the complete workflow.

### What If My Device Isn't Listed?

- Make sure the device is powered on and connected via USB
- Try a different USB cable or port
- Close any other application that might be holding the MIDI port open — only one app can own a port at a time (your DAW, a browser tab with Web MIDI, another MIDI utility)
- Click the **refresh icon** next to the device selector to re-scan for connected devices
- If nothing works, quit and relaunch bcMAPPER, then check again

- Note: Newer Macs don't seem to support bi-directional communication over the USB MIDI port on teh BCR/BCF hardware. If you are using a newer Mac, you will need to use a MIDI interface via 5-pin DIN connections. The available Bluetooth offerings are pretty handy and make moving around with these units a bit easier.

---

## Virtual Mode: Testing Without Hardware

Don't have your hardware nearby, or just want to verify a preset's behavior before committing it to the device? **Virtual Mode** turns the on-screen controller into a playable, MIDI-outputting instrument. It's not tuned for performance, but is great for making mappings to other software (or hardware) in the absence of the controller.

Toggle Virtual Mode from the *Hardware* menu in the header toolbar. Once active:

- **Encoders** respond to click-and-drag (drag upward to increase the value, downward to decrease) or to the mouse wheel
- **Buttons** toggle on click, respecting the configured Controller Mode and Start State — a Toggle button behaves like a toggle, a Momentary button sends its On value on mousedown and Off on mouseup
- **Faders** (BCF2000) respond to vertical click-and-drag
- MIDI output is sent to whichever output device is selected in the header

The **LED display** on the visual controller shows the value being transmitted in real-time and clears automatically after 2 seconds of inactivity.

The **MIDI Monitor** opens automatically when you enable Virtual Mode so you can see every message going out.

### Setting Up a Virtual MIDI Port

To route bcMAPPER's output into your DAW during Virtual Mode, you'll need a virtual MIDI port — a software loopback that connects bcMAPPER's output to your DAW's input.

**macOS:**
1. Open **Audio MIDI Setup** (in `/Applications/Utilities/`)
2. Go to **Window → Show MIDI Studio**
3. Double-click the **IAC Driver** icon
4. Check **Device is online**
5. Add a port if one doesn't exist (click the **+** button in the ports list)
6. Close Audio MIDI Setup — the port now appears in bcMAPPER's output selector

**Windows:**
1. Download **loopMIDI** from tobias-erichsen.de (free, no fuss)
2. Install and launch it
3. Click the **+** button to create a virtual MIDI port
4. The port appears immediately in bcMAPPER's device list

If no virtual ports are detected when you enable Virtual Mode, bcMAPPER will offer to walk you through this setup.

> **Tip:** Virtual Mode is also great for demonstrating a preset configuration to a collaborator who doesn't have the hardware. Everything looks and behaves exactly as it would on the real device.

---

## Creating Your First Preset

After picking a device type on the Welcome screen (or using **File → New Preset**), you'll land in the editor with a blank preset ready to configure.

A typical first-preset workflow:

1. **Click any control** on the visual controller — an encoder, button, or fader. The **Parameter Panel** opens on the right showing all configurable parameters for that control.

2. **Set the MIDI Channel** (1–16) — which channel this control transmits on.

3. **Choose a Data Type** — CC is the most common for encoders and faders. Buttons support a wider set including Note, Program Change, NRPN, and MMC.

4. **Set the CC Number** (or note number, NRPN address, etc.) — this identifies the specific parameter you're targeting in your DAW or device.

5. **Adjust Min/Max values** if needed — the defaults are 0 and 127, but you can narrow the range for finer control or to match what a specific parameter expects.

6. **Double-click the scribble strip** above the control to add a label — up to 12 characters. This stays visible on the controller layout as a quick reference. Labels are stored in bcMAPPER only and are not transmitted to hardware.

7. **Repeat** for as many controls as you need.

8. **Save** with `Cmd/Ctrl + S`.

> **Tip:** Don't feel obligated to configure every single control before testing. Build incrementally — set up 8 controls, send them to hardware, see how they feel in your DAW, then come back and continue. Smaller iteration loops are much easier to debug.

---

## Two Ways to Organize Presets

bcMAPPER supports two types of presets. Understanding the difference early saves confusion later.

### Standalone Presets
An independent configuration not attached to any bank. It's saved as its own file. Standalone presets are great for quick experiments, sharing individual configurations, or building templates you'll later organize into banks. They appear at the top of the sidebar, sorted newest-first.

### Bank Presets
A bank is a collection of up to 32 preset slots — exactly mirroring the hardware's memory structure. If you're planning to send presets to your BCR2000 or BCF2000, you'll want them in a bank, because the hardware operates in banks of 32. Slots are numbered 1–32 and you can have as many banks as you like.

Standalone presets can be copied into bank slots at any time via right-click → **Copy to Bank**, so you can freely start standalone and organize into banks later.

---

## Saving and Auto-Save

bcMAPPER saves automatically when:
- A new preset or bank is first created (saved immediately to the configured directory)
- You switch between bank slots (the current slot is saved before the next one loads)

You can also save manually at any time:
- **`Cmd/Ctrl + S`** — Saves the current preset
- **File → Save Bank to File** — Exports the entire current bank as a `.json` file

The **dirty indicator** — a small asterisk `*` in the footer — appears whenever there are unsaved changes. A quick `Cmd/Ctrl + S` makes it disappear.

---

## Sending Presets to Hardware

Once a preset is configured and saved, sending it to the hardware is straightforward:

1. Confirm your hardware is connected (both input and output selected)
2. Click **Send to Hardware** in the header, or press `Cmd/Ctrl + T`
3. In the dialog, select the **target slot** (1–32) where the preset should land on the hardware
4. Click **Send**

The preset is translated into BCL (B-Control Language), wrapped in a SysEx message, and transmitted. The hardware parses it and updates that slot. You should see the LEDs update on the BCR2000, or hear the faders move on the BCF2000.

> **Important:** Sending to a slot overwrites whatever was previously there on the hardware. If the hardware has something valuable in that slot, receive it first before sending.

---

## Receiving Presets from Hardware

To pull a preset back from the hardware into bcMAPPER:

1. Connect your hardware (both input and output)
2. Click **Receive from Hardware** in the header, or press `Cmd/Ctrl + R`
3. In the dialog, select which slot(s) to receive — or choose **All** to import the entire bank (all 32 slots)
4. Click **Receive** — bcMAPPER sends a dump request to the hardware
5. The hardware responds with SysEx; bcMAPPER parses the BCL and populates the preset(s)

The received preset(s) are loaded into the current bank at the requested slot(s), or saved as a new bank if you received all 32 slots.
