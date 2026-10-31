# JOURNAL.md — Learning diary

A **personal, subjective** record of the journey with the MiniLab 3. Here you write what you felt,
what confused you, what made you proud — things that don't fit the objective table in `GUIDE.md`.

**`GUIDE.md` answers "what step am I on?"; `JOURNAL.md` answers "how am I living this?"**

## How to write

- **Newest entries at the top.**
- One entry per study session (or per day, if there is more than one).
- Always dated in the format `## YYYY-MM-DD — short title`.
- Bullet points are fine. It doesn't need to be pretty, it needs to be honest.
- Never rewrite old entries. If you changed your mind, write a new entry referring to the previous one.
- **The student writes the content.** The agent may help format it, but never invents feelings.

### Entry template

```markdown
## YYYY-MM-DD — short title

**Lesson:** 0.1
**Time:** ~20 min
**What I did:** ...
**What I understood:** ...
**What blocked / stayed confusing:** ...
**Feeling:** (excited / lost / stuck / fun)
**Wish for next time:** ...
```

---

## 2026-10-31 — Day zero: the keyboard already speaks, I don't yet

**Lesson:** none yet (course created today)
**Time:** ~1 h setting up the environment

**What I did:**
- Plugged the MiniLab 3 into the ThinkPad and found out Linux recognizes it with nothing installed.
- Saw the four ports the keyboard exposes (`MIDI`, `DIN THRU`, `MCU/HUI`, `ALV`) and didn't understand
  half the names.
- Tried to enable the control surface in Reaper and got a
  `Control Surface: Error opening MIDI inputs`.
- We cleared Reaper's MIDI list, which had dozens of phantom entries.

**What I understood:**
- The keyboard makes no sound: it only sends messages. The sound comes from the virtual instrument inside the DAW.
- "Midi-Bridge" is PipeWire bridging the keyboard and the programs.
- MCU/HUI is a protocol for controlling the DAW (faders, transport), separate from note MIDI.

**What blocked / stayed confusing:**
- The sheer number of similar-looking names in the MIDI lists.
- I still don't know what `ALV` and `DIN THRU` do in practice.
- I don't know whether I'll use the faders to mix or whether that's too much complexity for the start.

**Feeling:** excited, but with the impression that there are a lot of acronyms between me and the sound.

**Wish for next time:** press a key and see it turn into sound inside Reaper, with no configuration
in the way.
