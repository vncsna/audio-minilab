## REAPER + MiniLab 3 setup

Settings that live outside any config file, kept here so the setup can be rebuilt on a fresh
machine. Version-specific: REAPER 7.84, Arch Linux, PipeWire.

### Transport (MCU)

- Hardware: in the WebMIDI configurator (`minilab3.klangsoft.com`), set **Transport mode → MCU**.
  With `None`, `Shift` + pads 4–7 send plain `CC 105–108` on the `MIDI` port with 127 on press
  and 0 on release; REAPER shortcuts fire on both, so toggle actions undo themselves.
- The controller must be in **DAW** mode (`Shift` + pad 3 until the display reads `DAW`).
- `Preferences → Control/OSC/web → Add`: **Mackie Control Universal**, MIDI input and output
  `Minilab3: MCU/HUI`. Only one MCU surface in the list.
- No MIDI CC shortcuts for the transport (`KEY 176 …` in `reaper-kb.ini`), or the transport
  double-triggers.
- Close Chrome (WebMIDI) and any `aseqdump` before starting REAPER; the launches that stopped
  before initialising audio coincided with both holding the MiniLab ports (suspected, not proven).

### MIDI devices

- `Minilab3: MIDI` — both checkboxes: **Enable input for track record input** and
  **Enable input for control messages**. The second one is required for MIDI action shortcuts;
  without it REAPER silently drops the message.
- `MCU/HUI`, `DIN THRU`, `ALV` — input disabled (they cause double triggers). The MCU surface
  opens `MCU/HUI` by itself.

### Audio

JACK, served by `pipewire-jack`: 48 kHz, 256 samples (≈10.6 ms round trip).

### Track and FX chain

- Track input: `MIDI → Minilab3: MIDI → All channels`.
- FX chain order matters:

  ```
  IX → MIDI Tool II     (Input Channel 10, Output Channel 1)
  LV2i → AVL Drums      (Black Pearl 4)
  ```

  The AVL Drums `.sf2` has no preset on bank 128, so the drum channel (10) is silent; the
  remap sends pad notes to channel 1, where the kit is mapped by note number.

### Hardware settings (stored on the MiniLab 3)

- Pad velocity curve: `Log` — or `Fixed` (~100) for finger drumming.
- Set through the WebMIDI configurator `https://minilab3.klangsoft.com` in Chrome, since MIDI
  Control Center has no Linux build. These settings persist in the device.
