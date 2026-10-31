# GUIDE.md — From zero to a first track (MiniLab 3 + Reaper + Linux)

> **Living document.** It is both the course map and the logbook: it shows where you are,
> what is done, and what the next step is. The agent updates it at the end of every lesson.
> Update rules are at the bottom of this file.

---

## Current state

| Field | Value |
|---|---|
| **Progress** | `3 / 18` lessons |
| **Level** | 0 — starting from scratch |
| **Last session** | 2026-10-31 — lessons 0.1, 0.2 and 1.1 (diagnostics, live MIDI messages, hardware map) |
| **Next lesson** | **1.2 — What velocity, aftertouch, and CC are** |
| **Blockers** | none |
| **Final goal** | compose, mix, and export 1 original track (~3 min), on your own |

**How to resume:** read this block → find the next lesson in the roadmap → do only that one.

---

## Learning roadmap

Legend: `[ ]` pending · `[~]` in progress · `[x]` done

### Module 0 — Foundation: the keyboard talking to the system (2 lessons)

**Goal:** know that the MiniLab 3 really sends MIDI, and where that is visible.

- [x] **0.1 — Keyboard diagnostics**
  - **Concept:** the MiniLab 3 is a MIDI controller (it makes no sound). Linux already recognizes it, no driver needed.
  - **Exercise:** with the keyboard plugged in, run
    ```bash
    cat /proc/asound/seq/clients
    pw-link -o | grep -i minilab
    ```
    and recognize the 4 ports (`MIDI`, `DIN THRU`, `MCU/HUI`, `ALV`).
  - **Criterion:** you can point out which client/port is the keyboard.

- [x] **0.2 — See the messages arriving**
  - **Concept:** MIDI is a list of messages (`note on`, `note off`, `velocity`, `CC`).
  - **Exercise:** `sudo pacman -S alsa-utils && aseqdump -p 16:0`; press keys, turn knobs,
    hit pads and identify in the terminal: note, velocity, CC.
  - **Criterion:** you can tell from the terminal output the difference between pressing a key and turning a knob.

### Module 1 — MIDI and the MiniLab 3 (3 lessons)

- [x] **1.1 — Hardware map**
  - **Concept:** 25 keys, 8 pads (2 banks), 4 faders, 8 knobs, touch strips (pitch/mod),
    `Octave`, `Hold`, `Shift`, arpeggiator, OLED display.
  - **Exercise:** touch every control and name what it does; find out what `Shift` modifies.
  - **Criterion:** you can find octave, hold, arp, and pad bank switching without the manual.
  - **Findings (verified):** one octave = 12 semitones; `Shift` + `Octave` = transpose (semitones,
    not octaves); `Shift` + arp button = arpeggiator; `Shift` + pad 2 = pad bank switch, signalled by
    `CC 27` (127/0) plus an Arturia SysEx message; pads send on channel 9 with notes 36–43 (bank A)
    and 44–51 (bank B); keys send on channel 0; button presses appear as `CC 105–109` at 127/0;
    `Shift` also produces Arturia SysEx (`F0 00 20 6B …`).

- [ ] **1.2 — What velocity, aftertouch, and CC are**
  - **Concept:** velocity = intensity; aftertouch = pressure after pressing; CC = continuous control.
  - **Exercise:** play the same note soft and hard and compare the velocity number in `aseqdump`.
  - **Criterion:** you can explain why the same note "sounds different" depending on force.

- [ ] **1.3 — MIDI is not audio**
  - **Concept:** MIDI describes *what to play*; the sound comes from a virtual instrument (VSTi).
  - **Exercise:** draw (on paper/in your head) the chain: MiniLab → Reaper → VSTi → speaker.
  - **Criterion:** you know that changing the instrument changes the sound without changing the MIDI.

### Module 2 — First sound in Reaper (2 lessons)

- [ ] **2.1 — Configure Reaper**
  - **Concept:** MIDI input, audio device, buffer, and latency.
  - **Exercise:** `Options → Preferences → MIDI Devices`: enable `Minilab3: MIDI (capture)`.
    Check `Audio → Device` (ALSA/JACK) and play with comfortable latency.
  - **Criterion:** key light on the track = immediate sound, no double trigger, no annoying delay.

- [ ] **2.2 — First recorded note**
  - **Concept:** armed track + `Record: input (audio or MIDI)` + monitoring.
  - **Exercise:** insert a VSTi (Surge XT, Vital, or Dexed), record 4 bars of loose notes,
    and see the events in the MIDI Editor.
  - **Criterion:** there is a MIDI item with your notes in the project.

### Module 3 — Rhythm: pads and drums (2 lessons)

- [ ] **3.1 — Drums with the pads**
  - **Concept:** General MIDI note map (kick 36, snare 38, closed hat 42…).
  - **Exercise:** record a 2-bar groove (kick/snare/hat) using the pads.
  - **Criterion:** the groove is in time, with at least 3 pieces.

- [ ] **3.2 — Grid, quantization, and humanization**
  - **Concept:** quantize = align to the grid; 100% sounds robotic.
  - **Exercise:** quantize the drums at 60–80% strength and compare with 100%.
  - **Criterion:** you consciously choose the quantize strength.

### Module 4 — Applied minimal harmony (2 lessons)

- [ ] **4.1 — Scale and triads**
  - **Concept:** major/minor scale, intervals, triad (root–third–fifth).
  - **Exercise:** play the C major and A minor triads on the MiniLab.
  - **Criterion:** you can build any major/minor triad from its root.

- [ ] **4.2 — Four-chord progression**
  - **Concept:** scale degrees, the I–V–vi–IV progression, and why it "sticks".
  - **Exercise:** record the progression with a pad/keys and loop it.
  - **Criterion:** a 4-chord loop recorded, in tune, and in time.

### Module 5 — Arrangement and form (2 lessons)

- [ ] **5.1 — Track structure**
  - **Concept:** intro, verse, chorus, bridge, outro; counting in bars (4/8/16).
  - **Exercise:** build a 2-minute "map" on paper for your track.
  - **Criterion:** map with sections and durations defined.

- [ ] **5.2 — Layers: bass, harmony, melody**
  - **Concept:** roles (low/mid/high) and how to avoid frequency clashes.
  - **Exercise:** add a bass (mono) and a simple melody over the loop.
  - **Criterion:** three layers sounding distinct, none masking another.

### Module 6 — Basic mixing (2 lessons)

- [ ] **6.1 — Volume, pan, and space**
  - **Concept:** gain staging (peaks ~-6 dB), pan, volume hierarchy.
  - **Exercise:** balance the track with faders, no plugins at all.
  - **Criterion:** the mix sounds balanced at both low and high volume.

- [ ] **6.2 — EQ, compression, and reverb (the minimum)**
  - **Concept:** cut before boosting; compression controls dynamics; reverb creates space.
  - **Exercise:** cut the pad's low end, compress the bass, and send a short reverb to the melody.
  - **Criterion:** you can justify every plugin you inserted.

### Module 7 — Finishing (2 lessons)

- [ ] **7.1 — Reference and loudness**
  - **Concept:** compare against a commercial track, integrated LUFS, true peak.
  - **Exercise:** analyze your track and the reference with a meter; adjust level.
  - **Criterion:** track at competitive volume without distortion.

- [ ] **7.2 — Export**
  - **Concept:** 24-bit WAV for the master, MP3 for listening on the phone.
  - **Exercise:** render the correct region and check the final file.
  - **Criterion:** file exported, listened to from start to finish, and approved.

### Module 8 — After the first track (1 lesson)

- [ ] **8.1 — Next paths**
  - **Concept:** sound design, recording voice/instruments, collaboration, advanced MIDI Learn.
  - **Exercise:** choose the next focus and write 3 goals.
  - **Criterion:** goals written at the bottom of this file.

---

## Session log

| Date | Lesson | What was done | Blocked? |
|---|---|---|---|
| — | — | Course created; no lesson taught yet | — |
| 2026-10-31 | 0.1 | Identified the keyboard as ALSA client 16 with 4 ports (0–3); learned `cat /proc/asound/seq/clients` and `pw-link -o \| grep -i minilab`; mapped the role of `MIDI`, `DIN THRU`, `MCU/HUI`, `ALV` | — |
| 2026-10-31 | 0.2 | Installed `alsa-utils` and used `aseqdump -p 16:0`; saw note on/off with velocity (60 soft vs 96–99 hard), CC 74 from the first knob, CC 1 from the mod strip, and pitch bend centered on 0; distinguished discrete events from continuous streams | — |
| 2026-10-31 | 1.1 | Mapped the surface with `aseqdump`: 12 semitones per octave, `Shift`+`Octave` = transpose, `Shift`+arp = arpeggiator, `Shift`+pad 2 = pad bank (`CC 27`). Verified pad 1 = note 44 in bank B and 36 in bank A; pads send on channel 9, keys on channel 0 | Arp pattern looked like a repeated held triad with identical velocities; definitive on/off comparison deferred to module 3 |

---

## Student notes

_(free space — questions, music references, track ideas)_

---

## Update rules for this document

1. When a lesson is completed: replace `[ ]` with `[x]`.
2. Update the **Current state** table (progress, date, next lesson, blockers).
3. Add one row to **Session log** — never edit old rows.
4. New lessons go **at the end of the matching module**; new modules go at the end of the roadmap.
5. If a lesson is skipped, justify it in the "What was done" column.
