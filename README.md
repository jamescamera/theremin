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

**VOICE** cycles five oscillator setups (saw, pure, chip, drone, organ) plus **DRUMS** — see
below. **SNAP** quantises
to A minor pentatonic so everything stays in key. **FLIP** switches cameras, **MUTE**
silences the tone, and **REC** captures a take — video plus sound where the browser supports
it, otherwise audio only.

Recordings are composited rather than taken from the raw camera track: each frame is redrawn
into a canvas with the reticle, readouts, hit ripples, spectrum, oscilloscope, every control
and the live sequencer grid on top, and *that* is what gets recorded. Element positions are
read from the DOM so the take matches the layout exactly, and cached rather than measured per
frame — reading rects forces layout, thirty times a second.

## Playing the drums with the camera

Set **VOICE** to **DRUMS** and the instrument stops holding a tone altogether. The oscillators
go silent and the camera strikes a kit instead:

- **Hue picks the drum**, in six arcs around the wheel — reds are the kick, yellow-greens the
  snare, greens the clap, blues the hat, violets the open hat, magentas the 808. The readout
  names whichever one you are pointed at, so you can find a drum before you hit it.
- **Movement is the stick.** A flick of motion fires the hit; holding still is silence.
- **How hard you move is velocity**, so the kit plays soft or loud.
- In the 808 arc, the note still comes from the colour, and the readout shows it.

Hits are detected as a *rise* above the scene's own motion floor rather than a fixed
threshold, so a busy or grainy picture will not machine-gun and a calm one still responds.
There is a 110 ms refractory gap between hits. **MUTE** stops the camera striking, which
leaves the sequencer running underneath — handy for dropping out of your own take.

Every drum that sounds throws a ring of its own colour from its own place on screen — kick
from low, hats from up high, the 808 in whatever colour you wrote it with — so the picture
reacts to *which* colour you activated rather than just flashing. Kicks and 808s bloom the
whole frame so they read even when you are looking elsewhere.

### Making it repeat

**CAPTURE** turns a performance into a loop. Hits you play with the camera are quantised to
the nearest sixteenth and written onto the grid, so they come back round instead of
vanishing — the cells light up as you play them, and you can then mute, clear or retune what
you caught. Because capture is meaningless without a loop turning and a camera striking
drums, switching it on selects the DRUMS voice and starts the transport rather than failing
silently.

The hits you *hear* are still not quantised — only the ones written to the grid are. Snapping
live playback forward to the next sixteenth would put up to ~160 ms between the gesture and
the sound, which reads as broken. So you play loose and the loop repeats tight.

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
