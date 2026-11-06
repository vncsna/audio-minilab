# GUIDE.md — From zero to a first track (MiniLab 3 + Reaper + Linux)

> **Living document.** It is both the course map and the logbook: it shows where you are,
> what is done, and what the next step is. The agent updates it at the end of every lesson.
> Update rules are at the bottom of this file.

---

## Current state

| Field | Value |
|---|---|
| **Progress** | `8 / 18` lessons |
| **Level** | 4 — a drum groove recorded with a GM kit; timing still needs the grid |
| **Last session** | 2026-11-06 — lesson 3.1 (2-bar drum groove with the pads) |
| **Next lesson** | **3.2 — Grid, quantization, and humanization** |
| **Blockers** | Reaper's metronome produces no click (settings checked, cause open); the student used a phone click. Groove recorded slightly off the beat — the target of 3.2 |
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
  - **Exercise:** `paru -S alsa-utils && aseqdump -p 16:0`; press keys, turn knobs,
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

- [x] **1.2 — What velocity, aftertouch, and CC are**
  - **Concept:** velocity = intensity; aftertouch = pressure after pressing; CC = continuous control.
  - **Exercise:** play the same note soft and hard and compare the velocity number in `aseqdump`.
  - **Criterion:** you can explain why the same note "sounds different" depending on force.
  - **Findings (verified):** note 48 kept its identity while velocity ranged 76–127 — the curve is
    compressed at the top, so "very soft" already lands near 76; the MiniLab 3 sends **no
    aftertouch** (nothing between `note on` and `note off`); knobs send their own CCs (74 = knob 1,
    82 = second control); `CC 9` and `CC 71` at startup are housekeeping, not performance data.
  - **Note:** the auditory part of the criterion needs a sound source, which did not exist yet — it
    was deferred to the first sound (fluidsynth demo) instead of pretending it was heard.

- [x] **1.3 — MIDI is not audio**
  - **Concept:** MIDI describes *what to play*; the sound comes from a virtual instrument (VSTi).
  - **Exercise:** draw (on paper/in your head) the chain: MiniLab → Reaper → VSTi → speaker.
  - **Criterion:** you know that changing the instrument changes the sound without changing the MIDI.
  - **Findings (verified):** in the running fluidsynth shell, `prog 0 40` (program change on channel 0)
    turned the same key press into a violin — the MIDI events stayed identical. The student judged
    the result ugly, which is itself the lesson: FluidR3 is a ~140 MB General MIDI bank, so sustained
    instruments have few seconds of looped sample and no bow/vibrato/expression (CC 11) control, while
    percussive instruments (piano) survive sampling well. Better samples, not different MIDI, is what
    fixes the sound — this is the motivation for choosing native VSTi in module 2.

### Module 2 — First sound in Reaper (2 lessons)

- [x] **2.1 — Configure Reaper**
  - **Concept:** MIDI input, audio device, buffer, and latency.
  - **Exercise:** `Options → Preferences → MIDI Devices`: enable `Minilab3: MIDI (capture)`.
    Check `Audio → Device` (ALSA/JACK) and play with comfortable latency.
  - **Criterion:** key light on the track = immediate sound, no double trigger, no annoying delay.
  - **Findings (verified):** the Reaper side of the chain works. `Minilab3: MIDI` enabled as
    `Enabled+Control`; `MCU/HUI`, `DIN THRU` and `ALV` left disabled (they cause double triggers);
    audio system **JACK** (served by `pipewire-jack`), 48 kHz / 256 samples ≈ 5.3 ms per block
    ≈ 10.6 ms round trip. Armed track with `Input: MIDI → Minilab3: MIDI → All channels` gave one
    meter pulse per key, and `aseqdump` showed one `Note on`/`Note off` pair per press.
  - **Note:** the "immediate sound" half of the criterion cannot be verified before an instrument
    exists; it was replaced by "one pulse per key, 48 kHz / 256" and the sound test moved to 2.2.
  - **Concept learned along the way:** *arm* ≠ *recording* — arming opens the input, shows the
    meter, and marks the track as the one that will record; the transport Record is what writes.

- [x] **2.2 — First recorded note**
  - **Concept:** armed track + `Record: input (audio or MIDI)` + monitoring.
  - **Exercise:** insert a VSTi (Surge XT, Vital, or Dexed), record 4 bars of loose notes,
    and see the events in the MIDI Editor.
  - **Criterion:** there is a MIDI item with your notes in the project.
  - **Findings (verified):** `surge-xt` installed; Reaper only listed the **CLAPi** build, not the
    VST3 one — it scans `/usr/lib/clap` but not `/usr/lib/vst3` (adding that path in
    `Preferences → Plug-ins → VST` is optional, CLAP works fine). Default patch is `Init Saw`
    (raw sawtooth) and was rejected as unpleasant; `Keys → Soft Suitcase` was used instead.
    Tempo set to 100 BPM via `Insert → Tempo/Time Signature Marker…` because the student did not
    know the transport panel. Recorded ~4 bars (~9.6 s) and confirmed the notes in the MIDI Editor
    sit where they were played.
  - **Note:** the student did not identify the transport bar from the name; describe UI by the
    buttons (`▶ ■ ●`) rather than by Reaper's terminology.

### Module 3 — Rhythm: pads and drums (2 lessons)

- [x] **3.1 — Drums with the pads**
  - **Concept:** General MIDI note map (kick 36, snare 38, closed hat 42…).
  - **Exercise:** record a 2-bar groove (kick/snare/hat) using the pads.
  - **Criterion:** the groove is in time, with at least 3 pieces.
  - **Findings (verified):** no drum instrument existed; `surge-xt` only ships single-hit drum patches (one sound per preset), so `avldrums.lv2` was installed instead — it carries five complete GM kits as `.sf2` (Black Pearl 4, Red Zeppelin 4, Blonde Bop, Buskman's Holiday) and exposes them as an LV2 instrument, which Reaper loads natively from `/usr/lib/lv2`.
  - **Root cause of the silence (verified with `fluidsynth`):** the Black Pearl `.sf2` has **no drum preset on bank 128** (`No preset found on channel 9 [bank=128 prog=0]`). The pads send on MIDI channel 10, which always looks for bank 128 → empty → silence; keys on channel 1 use preset 0, the kit mapped by note number, which is why keys sounded and pads did not.
  - **Fix:** remap the pad channel to 1 in front of the plugin — `IX → MIDI Tool II` inserted *before* `AVL Drums`, `Input Channel 10`, `Output Channel 1`. (Reaper's `Map input to channel` in the input menu did not take effect.)
  - **Pad feel:** pad velocity curve lives in the hardware, not in Reaper. With no MCC on Linux, it was set through the community WebMIDI configurator `minilab3.klangsoft.com` in Chrome: `Velocity Curves → Pad → Log` (light hits translate to higher velocity) or `Fixed` for finger-drumming consistency.
  - **Also learned:** pad aftertouch (polyphonic) exists on the MiniLab 3 but is irrelevant for a sampler — it only matters with instruments that use pressure as modulation.
  - **Note:** the criterion's "in time" half was met only approximately; the student judged the hi-hat slightly off the click. Rather than repeat the take, human timing inaccuracy became the motivation for 3.2.

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
| 2026-11-01 | 1.2 | Played note 48 at 20 different forces: the note never changed, velocity stayed 76–127 (curve compressed at the top); confirmed the keys send **no** aftertouch; distinguished note events from continuous CC streams (74 = knob 1, 82 = second control) | Criterion asks about a *sound* difference and the system still has no sound source; answered from the numbers, hearing deferred to the fluidsynth demo |
| 2026-11-01 | 1.2 (hearing) | Optional demo outside the roadmap: installed `soundfont-fluid`, started `fluidsynth -a pulseaudio -m alsa_seq` and wired the keyboard with `aconnect 16:0 → FLUID Synth`. Heard the same note at velocity ~76 and 127 (quiet/dull vs loud/bright) and confirmed the GM piano ignores knob 1's CC 74 | — |
| 2026-11-01 | 1.3 | Drew the chain MiniLab → REAPER → VSTi → speaker; in the fluidsynth shell ran `prog 0 40` and heard the same notes as violin (judged ugly — GM bank limits, no sustained-instrument expression); answered that the VSTi is where sound is born | — |
| 2026-11-06 | 2.1 | Configured Reaper: `Minilab3: MIDI` enabled as input (MCU/HUI, DIN THRU and ALV kept off), audio system JACK over PipeWire at 48 kHz / 256 samples (~10.6 ms round trip), armed a track with `Input: MIDI → Minilab3 → All channels`; one meter pulse per key and one `Note on`/`Note off` pair in `aseqdump`; clarified that arm ≠ recording | Criterion's "immediate sound" deferred to 2.2 (no instrument installed); 2.2 needs `paru -S surge-xt` |
| 2026-11-06 | 2.2 | Installed `surge-xt` and inserted it as **CLAPi** (Reaper listed the CLAP build only, not the VST3 one); rejected the default `Init Saw` and switched to `Keys → Soft Suitcase`; set 100 BPM through `Insert → Tempo/Time Signature Marker…`; recorded ~4 bars with input monitoring on and confirmed the notes sit where they were played | Reaper's scan paths cover `/usr/lib/clap` but not `/usr/lib/vst3` (harmless, CLAP works); the student did not recognize the transport bar by name |
| 2026-11-06 | 3.1 | Installed `avldrums.lv2` (5 GM kits as `.sf2`) and loaded `Black Pearl 4`; diagnosed silence with `fluidsynth` (no bank-128 preset → channel 10 empty) and remapped pads with `IX → MIDI Tool II` (input 10 → output 1) before the plugin; set the pad velocity curve to `Log` through the WebMIDI configurator, since MCC has no Linux build; recorded a 2-bar kick/snare/hat groove | Reaper's metronome gives no click (`Metronome output`, run-during-playback/recording all checked) — student used a phone click; groove is slightly off the grid, deferred to 3.2 |
| 2026-11-06 | — (setup) | Transport control from the MiniLab: with `Transport mode = None` the `Shift` + pads 4–7 sent `CC 105–108` (127 press / 0 release) on the `MIDI` port, and CC shortcuts fired on both (Play/pause played on press, paused on release). Removed the CC shortcuts, set `Transport mode → MCU` in the WebMIDI configurator, and added a `Mackie Control Universal` surface on `MCU/HUI`; transport now works through MCU. Next: track control (faders/knobs) via MCU | REAPER failed to open (stopped before initialising audio) while Chrome (WebMIDI) and `aseqdump` held the MiniLab ports — suspected cause, not proven |

---

## Student notes

_(free space — questions, music references, track ideas)_

- 2026-11-01 — asked how to learn chords on the controller "like a piano" (interest in module 4;
  deferred because the daily cap was already hit and recording MIDI — module 2 — is the prerequisite).

---

## Update rules for this document

1. When a lesson is completed: replace `[ ]` with `[x]`.
2. Update the **Current state** table (progress, date, next lesson, blockers).
3. Add one row to **Session log** — never edit old rows.
4. New lessons go **at the end of the matching module**; new modules go at the end of the roadmap.
5. If a lesson is skipped, justify it in the "What was done" column.
