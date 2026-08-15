# COLOUR THEREMIN

Point your camera at the world and play it. A single-file web instrument — no build step,
no dependencies, no samples. Open `index.html` over HTTPS (or localhost) and press START.

## The instrument

The circle in the middle of the screen samples whatever you aim it at, and the colour drives
the synth:

| What it sees | What you hear |
| --- | --- |
| Hue | Pitch, across 2.5 octaves — red is low, violet is high |
| Brightness | Volume — black is silence |
| Saturation | Timbre, opening a lowpass filter — grey scenes sound dull |
| Motion | Vibrato depth |

**VOICE** cycles five oscillator setups (saw, pure, chip, drone, organ). **SNAP** quantises
to A minor pentatonic so everything stays in key. **FLIP** switches cameras, **MUTE**
silences the tone, and **REC** captures a take — video plus sound where the browser supports
it, otherwise audio only.

## The beat machine

**BEAT** opens a 16-step sequencer with six tracks of synthesised 808: kick, snare, clap,
closed hat, open hat, and the 808 sub itself. The hats are built the original way, from six
detuned square waves through a bandpass/highpass pair; the sub runs through soft saturation
for the driven tail.

The 808 row is the part that ties back to the camera. Tapping one of its steps writes *the
note you are currently aiming at* into that step, and the cell takes on that colour — so a
bassline reads as a row of swatches you collected from the world. Tap a lit step to clear it,
then tap again on a different colour to retune it. Notes are folded down into sub range,
which preserves the pitch class, so SNAP keeps the bass in key with the lead.

Everything else: PLAY/STOP (or the space bar), tempo from 60 to 180 (press and hold to run
it), STRAIGHT/SWING/SHUFFLE, five starter patterns, and CLEAR. Tap a track name to mute it.
The beat is mixed into the recorder, so takes capture the whole performance.

## Notes

Camera access requires a secure context — HTTPS or `localhost`. Timing uses a lookahead
scheduler against the Web Audio clock rather than `setInterval` alone, so the beat stays
tight; if the tab is backgrounded, past-due steps are skipped rather than fired as a backlog.
