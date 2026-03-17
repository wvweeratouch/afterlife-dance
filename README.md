# Afterlife Dance

Interactive sound + body detection engine for **Emergence: The After Party** (York Manor, LA — Mar 28, 2026).

**Live demo**: https://wvweeratouch.github.io/afterlife-dance/

## What Is This

A browser-based performance system where **body movement drives generative music**. Camera detects motion via blob tracking + MediaPipe pose estimation, triggering layered sound across 7 scenes. Built for a live performance about stellar afterlife — stars being born, colliding, dying, and redistributing matter.

No install needed. Open in Chrome, allow camera, move.

## Quick Start

1. Open [the live demo](https://wvweeratouch.github.io/afterlife-dance/) or serve locally:
   ```
   python -m http.server 8001
   ```
2. Click to start (enables audio + camera)
3. Use buttons or keyboard `0-6` to switch scenes
4. Enable Pose for body-driven sound

## Scenes

| # | Name | Energy | Sound Character |
|---|------|--------|-----------------|
| 0 | Summoning | Low | Deep drone, sparse kalimba, wind |
| 1 | Formation | High | Prophet pad, rhodey melody, bells on movement |
| 2 | Binary | Mid | Dull bell pad, pluck pairs, harp-like interaction |
| 3 | Formless | Minimal | Hollow drone, growl textures, radio signals, hi-hat |
| 4 | Supernova | Max | Supersaw wall, pose-driven drums (kick/snare/hat) |
| 5 | Collapse | Low | Pretty bell pad, piano melody, ocean noise |
| 6 | Distribute | Mid | Bell rain, cyclical chimes, multi-blob interaction |

## Sound Architecture (SC Interactive)

4 layers, independently mixable:

- **BG** — pad chords + bass + kicks + ambient FX (always playing)
- **Auto-melody** — generative melody lines A + B (fade when body detected)
- **Interactive melody** — triggered by body movement (blob velocity + pose velocity)
- **Pose drums** — scene 4 only: body joint velocity maps to kick/snare/hat

## Controls

### Music
- **Vol** — master volume
- **BG** — background layer volume
- **MelA / MelB** — two melody lines, independently DJ-able
- **BPM** — override beats per minute
- **Density** — note density multiplier
- **Trans** — scene transition crossfade time (500ms–8s)

### Detection
- **Thresh** — blob detection threshold
- **Sens** — detection sensitivity
- **Calibrate** — capture empty background for difference mode
- **Auto-Mel** — toggle auto-generated melody
- **Pose** — toggle MediaPipe body tracking
- **PoseT** — pose velocity dead zone (0.01–0.20)

### Keyboard
- `0-6` — switch scene
- `P` — toggle pose
- `A` — toggle auto-melody
- `D` — toggle debug

## Detection

- **Blob tracking**: Frame differencing on downscaled video (80x60), clustering bright/moving pixels
- **MediaPipe Pose**: 33 landmarks, joint velocity computed from key body points (shoulders, elbows, wrists, hips, knees, ankles)
- **No skeleton needed for basic interaction** — blob mode works with any bright/moving object
- Works in dark room with dancers holding lights, or in lit room after calibration

## Credits

- **Wave Pongruengkiat** — artist, concept, performance
- **Bri-yarni Oracle** — co-developer (AI)
- **Henry Tan** — collaborator
- **Peach (Pongsakorn Wechakarn)** — chanting rave
- Built for [Emergence: The After Party](https://www.fathomers.org/emergence) — Fathomers

## Tech

- [SuperCollider WASM](https://www.npmjs.com/package/supersonic-scsynth) (supersonic-scsynth)
- [MediaPipe Pose](https://google.github.io/mediapipe/solutions/pose.html)
- Pure HTML/JS, no build step, no dependencies to install
