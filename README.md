# soemdsp-musical

Algorithmic pitch/rhythm modules: native WebAssembly (Chord Sequencer,
Pitch Quantizer) and JS reference implementations not yet ported
(Turing Machine, Chord Memory).

## Build target (native modules)

`--target=wasm32 -O3 -nostdlib -fno-exceptions -fno-rtti`. Compiled
with clang++.

## Chord Sequencer

4-step, 6-progression chord sequencer. Encodes triads as 8-bit masks
relative to root (bit 0 = root): major = `0x91` (bits 0, 4, 7), minor =
`0x89` (bits 0, 3, 7).

Export prefix: `soemdsp_chord_sequencer`

```
int    soemdsp_chord_sequencer_create()
void   soemdsp_chord_sequencer_destroy(int handle)
void   soemdsp_chord_sequencer_sample(int handle, double clock, double reset, double progression)
int    soemdsp_chord_sequencer_scale(int handle, double progression)
double soemdsp_chord_sequencer_root(int handle, double progression)
int    soemdsp_chord_sequencer_step(int handle)
int    soemdsp_chord_sequencer_version()
```

`kStepsPerProgression = 4`, `kProgressionCount = 6`, `kMaxInstances = 32`.

Source: `native_modules/chord_sequencer/chord_sequencer.cpp`.

## Pitch Quantizer

Snaps a pitch value to the nearest active pitch class in a 12-bit scale
mask. Pitch convention: 0.1V/Oct, `semitone = pitch * 120`.

Export prefix: `soemdsp_pitch_quantizer`

```
int    soemdsp_pitch_quantizer_create()
void   soemdsp_pitch_quantizer_destroy(int handle)
double soemdsp_pitch_quantizer_sample(int handle, double pitch, int scaleMask)
int    soemdsp_pitch_quantizer_version()
```

`scaleMask`: 12-bit mask, bit *i* set = pitch class *i* (0 = C, 1 = C#,
... 11 = B) is in-scale. `kMaxInstances = 32`.

Source: `native_modules/pitch_quantizer/pitch_quantizer.cpp`.

## Turing Machine (JS only)

Shift-register sequencer: each clock edge shifts the register left one
bit; the bit shifted off the top becomes the candidate new bottom bit,
flipped with probability `probability` instead of kept. `length` bounds
the loop to 1-16 bits. `scale` exposes the low 12 bits as a pitch-class
bitmask (same convention as Pitch Quantizer's `scaleMask`).

Source: `public/node-graph-turing-machine.js`, function
`nodeGraphTuringMachineSample(state, options)`.

## Chord Memory (JS only)

Records a chord one note at a time from a mono pitch input: each Latch
trigger stores the current pitch into the next of 4 slots (round-robin),
Clear resets all slots, Advance steps an arpeggiator index across active
slots.

Source: `public/node-graph-chord-memory.js`, function
`nodeGraphChordMemorySample(state, options)`.
