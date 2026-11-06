# GLOSSARY.md — Terms in plain language

Short definitions for the terms that show up in lessons. One entry per term, added when the term
first appears. Portuguese in parentheses, because the conversation is in Portuguese.

---

## MIDI

Protocol of short messages describing *what to play* — never sound. (MIDI = protocolo de mensagens;
não é áudio.)

## Client / Port

How ALSA exposes a device: the **client** is the device (the MiniLab 3 is client 16), each **port** is
one channel of communication inside it. (`cliente` = o aparelho; `porta` = um canal dentro dele.)

## Controller (controlador)

A device that only sends messages. The MiniLab 3 has no built-in sound engine.
(`controlador` = aparelho que só manda mensagens, não gera som.)

## Note on / Note off

Message that starts and ends a note. Carries the note number (60 = middle C) and velocity.
(`note on` = nota começou; `note off` = nota terminou.)

## Velocity

Force of the key press, 1–127. It is a property of the single event, not a continuous value.
(`velocity` = intensidade da tecla naquele toque.)

## CC (Control Change)

Continuous control message: controller number + value 0–127, sent repeatedly while the control moves.
`CC 74` = first knob (brightness/timbre), `CC 1` = mod strip.
(`CC` = mensagem de controle contínuo; um valor por passo do movimento.)

## Aftertouch

Pressure applied *after* the key is already down; a continuous message while you lean on the key.
The MiniLab 3's **keys send none**, but its **pads send polyphonic aftertouch** (each pad reports its
own pressure). A sampler ignores it; it matters only when an instrument maps pressure to modulation.
(`aftertouch` = pressão depois de apertar; as teclas não mandam, os pads mandam — um por pad.)

## Velocity curve

How physical force is translated into MIDI velocity 1–127: `linear` (1:1), `exponential` (harder in the
middle), `logarithmic` (light touch already sends high velocity), `fixed` (always the same value).
On the MiniLab 3 it lives in the hardware, per group (keys / pads), not in the DAW.
(`curva de velocity` = relação entre a força da batida e o número MIDI enviado.)

## GM drum map

Fixed note numbers for drum pieces on MIDI channel 10: 36 kick, 38 snare, 42 closed hat, 46 open hat,
49 crash, 51 ride. It is why pad 1 plays a kick without any configuration.
(`mapa GM de bateria` = números de nota fixos para cada peça, no canal 10.)

## VSTi (instrumento virtual)

Plugin que gera som a partir de MIDI (`.vst3`/`.so` no Linux, ou um SoundFont tocado pelo fluidsynth).
É a caixa onde o som nasce. (`VSTi` = instrumento virtual; quem transforma receita em som.)

## Program change

Mensagem "troque o instrumento do canal X pelo número Y". No mapa General MIDI, `40` = violino.
No shell do fluidsynth: `prog 0 40` = canal 0, instrumento 40. (`program change` = troca de instrumento.)

## SoundFont (.sf2)

A file that packages recordings (samples) of real instruments plus the rules for playing them:
which sample answers each note, which sample layer answers each velocity range, loop points,
envelopes. It is the instrument, stored as data. `fluidsynth` plays `.sf2` files.
(`SoundFont` = arquivo com amostras de instrumentos + regras de disparo; é o instrumento em forma de dado.)

## General MIDI (GM)

Standard that fixes which instrument sits at each program number (0 = acoustic piano, 33 = finger
bass, …), so the same MIDI file sounds roughly the same on any GM device.
(`General MIDI` = padrão que fixa o instrumento de cada número, para o mesmo arquivo soar parecido em qualquer aparelho.)

## Sample (amostra)

A short recording of a real sound, replayed at different pitches. A piano note at C2 and at C6 are
usually two different samples, not one stretched.
(`sample` = gravação curta de um som real, reproduzida em alturas diferentes.)

## Pitch bend

The pitch strip message. Signed value from -8192 to +8191, centered on **0** (as shown by `aseqdump`).
(`pitch bend` = fita de afinação; no centro vale 0.)

## Bank (banco)

A set of assignments for the same physical controls. The 8 pads have 2 banks: bank A sends notes
36–43, bank B sends 44–51. Switching banks changes the note number, not the hardware.
(`banco` = página de atribuições; o mesmo pad manda outra nota.)

## Channel (canal)

MIDI channel number (1–16). Keys here send on channel 1, pads send on channel 10 (drums) — in
`aseqdump` this appears as `Ch 0` and `Ch 9`, counting from zero.
(`canal` = via de transmissão; teclas no canal 1, pads no canal 10.)

## SysEx (System Exclusive)

Manufacturer-specific message, starting with `F0`. Arturia's are prefixed `F0 00 20 6B`, used by the
keyboard to report its own controls. DAWs can ignore them safely.
(`SysEx` = mensagem exclusiva do fabricante.)

## Arpeggiator (arp)

A feature that, when you hold a chord, plays its notes one at a time, in a repeating pattern,
instead of all at once. (`arpejador` = toca as notas do acorde seguradas, uma por vez, em repetição.)

## Discrete vs continuous

A key press is **discrete**: two events (on, off), each with one value. A knob is **continuous**: it
emits a stream of messages, one per increment of movement. Both use the same 0–127 range.
(`discreto` = um evento por gesto; `contínuo` = fluxo de mensagens durante o gesto.)

## DAW

Digital Audio Workstation — the program where you record, arrange, and mix. Here: REAPER.
(`DAW` = estação de áudio digital, o programa onde se grava e mixa.)

## VSTi

Virtual instrument: the plugin that turns MIDI into sound. (`VSTi` = instrumento virtual.)

## Latency

Delay between pressing a key and hearing the sound. (`latência` = atraso entre tocar e ouvir.)
