# soemdsp-musical

Algorithmic melody, rhythm, and pitch tools -- the parts of a patch that
decide *what* to play, not just how it sounds.

## What's here now

- **Chord Sequencer** (`native_modules/chord_sequencer/`) — steps
  through a held/sequenced chord, WebAssembly-compiled.
- **Pitch Quantizer** (`native_modules/pitch_quantizer/`) — snaps a
  0.1V/Oct-style pitch signal to a musical scale, WebAssembly-compiled.
- **Turing Machine** (`public/node-graph-turing-machine.js`) — the
  classic looping-random-sequence generator: a shift register seeded
  with random bits, with a probability knob for how often a new random
  bit overwrites the loop instead of repeating it. JS only for now, not
  yet ported to WebAssembly.
- **Chord Memory** (`public/node-graph-chord-memory.js`) — holds and
  advances through a latched chord. JS only for now.

More algorithmic melody/rhythm tools are staged for this line and will
land here as the lineup gets finalized.
