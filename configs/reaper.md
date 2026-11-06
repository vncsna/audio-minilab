## REAPER + MiniLab 3 setup

Settings that live outside any config file, kept here so the setup can be rebuilt on a fresh
machine. Version-specific: REAPER 7.84, Arch Linux, PipeWire.

### Install the transport shortcuts

```bash
cp configs/Minilab3.ReaperKeyMap ~/.config/REAPER/KeyMaps/
```

Then `Actions → Show action list → Import/export… → Import shortcuts…` and pick the file.
Format: `KEY <modifier> <cc> <action> 0`, where modifier `176` = MIDI CC on channel 1.

| MiniLab control | Message | Action | REAPER command |
|---|---|---|---|
| `Shift` + pad 4 | CC 105 | Toggle repeat (loop) | 1068 |
| `Shift` + pad 5 | CC 106 | Stop | 1016 |
| `Shift` + pad 6 | CC 107 | Play/pause | 40073 |
| `Shift` + pad 7 | CC 108 | Record | 1013 |

The controller must be in **DAW** mode (`Shift` + pad 3 until the display reads `DAW`);
in ARTURIA mode the transport pads send nothing.

### MIDI devices

- `Minilab3: MIDI` — both checkboxes: **Enable input for track record input** and
  **Enable input for control messages**. The second one is required for MIDI action shortcuts;
  without it REAPER silently drops the message.
- `MCU/HUI`, `DIN THRU`, `ALV` — input disabled (they cause double triggers).

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
