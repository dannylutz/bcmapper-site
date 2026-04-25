# bcMAPPER 

Hello! Meet your new best friend for Behringer B-Control programming. bcMAPPER is a visual MIDI controller editor built specifically for the BCR2000 and BCF2000 — those satisfying, knob-packed beasts of hardware that have been a studio staple for decades.

Instead of squinting at a tiny LED display and tapping through cryptic menus on the device itself, you get a beautiful, clickable representation of your hardware on screen. Click a control, configure it, send it to the box. Done.

## What bcMAPPER Can Do

- **Visual Controller Editing** — Click any control on a realistic hardware layout to open its parameters. No menu-diving required.
- **Preset & Bank Management** — Organize up to 32 presets per bank, matching the hardware's memory structure exactly. A collapsible sidebar and slot strip keep everything accessible.
- **Scribble Strips** — Label every control directly on the visual layout, with custom background colors and opacity. Because "Encoder 7" is not a useful label.
- **Bulk Editing** — Select a group of controls and update them all at once. The Value Distribution tool can assign CC numbers, channels, or values across your entire selection in a single operation.
- **Preset Globals** — Apply MIDI channel, data type, or value ranges across all controls — or just encoders, or just buttons — without touching each one individually.
- **Global Setup Profiles** — Store and send device-wide settings (BCL `$global` section): MIDI mode, startup preset, foot switch wiring, SysEx device ID, TX interval, and dead time. Link a profile to a bank so the global setup is automatically sent together with the presets.
- **Real-time MIDI Monitor** — Watch every MIDI message flowing in and out, with direction and type filters to cut through the noise.
- **Virtual Mode** — No hardware nearby? Interact with controls on-screen and route MIDI to your DAW via a virtual MIDI port.
- **BCL Preview** — See the raw B-Control Language code being generated for any selected control. Useful for debugging and for the curious.
- **Custom Output** — Attach extra MIDI or SysEx messages to any control. Write `.tx` expressions for raw byte sequences, or build them with the Message Builder — structured forms for Roland SysEx, Yamaha SysEx, RPN sequences, direction-pair encoders, and more.
- **Copy & Paste** — Capture a control's full configuration and stamp it onto others. Great for building templated layouts quickly.
- **Factory Presets** — Both the BCR2000 and BCF2000 factory banks are built in and automatically restored, giving you a ready-made reference or starting point.
- **Undo & Redo** — Step freely through your edit history (up to 100 steps) without fear.
- **Import & Export** — Read and write both `.syx` (standard SysEx) and `.json` (bcMAPPER native) for presets and full banks.

## Quick Start

1. **Launch bcMAPPER** and enter your license email and code when prompted
2. **Choose your device type** on the Welcome screen (BCR2000 or BCF2000)
3. **Connect your hardware** using the device selector in the header
4. **Click any control** on the visual controller to open the Parameter Panel
5. **Configure it** — set the MIDI channel, data type, CC number, and any other relevant parameters
6. **Save** with `Cmd/Ctrl + S`, then **send to hardware**!

That's the core loop. The rest of this documentation covers everything you can do from there.

## System Requirements

| Platform | Minimum Version |
|----------|-----------------|
| macOS    | 10.15 (Catalina) or later |
| Windows  | 10 or 11 |

A USB or MIDI interface is required for hardware communication. No special drivers are needed beyond what your operating system already provides for the BCR2000 and BCF2000. Newer versions of macOS don't seem to support bidirectional MIDI over the internal USB port of the BCR/BCF units, which you probably already know if this applies to you, but it's worth mentioning in the event that you just dusted off one of these workhorse controllers and didn't know that to be true. There are some solid Bluetooth MIDI interfaces out there in addition to USB

## Documentation

1. [Getting Started](./getting-started.md) — First launch, licensing, connecting hardware, and creating your first preset
2. [User Interface](./user-interface.md) — A full tour of every panel, button, and section
3. [Working with Presets](./presets.md) — Creating, organizing, saving, and sending presets and banks
4. [Control Configuration](./controls.md) — Every parameter for every control type, explained in detail
5. [MIDI Communication](./midi-communication.md) — Hardware sync, Virtual Mode, the MIDI monitor, and file formats
6. [Keyboard Shortcuts](./shortcuts.md) — Every shortcut, all in one place

## Support

Something broken? Have a feature idea? Just want to say hello? Email **support@full-crystal.com** — we will get back to you as quickly as possible.
