# Afterlife Dance — Technical Architecture

## Stack
- Pure HTML/JS — no dependencies, no build step
- Web Audio API — all sound synthesis
- Canvas 2D — particle rendering + camera overlay
- MediaPipe Pose (CDN) — 33 body landmarks, toggleable
- getUserMedia — webcam for both LED detection and MediaPipe

## Sound Engine

### Layers (per scene, always active)
1. **Ambient** — generative, continuous, never stops even without dancer
2. **Beat** — rhythm layer, BPM adjustable via slider
3. **Noise** — texture (breath, rumble, white noise, etc.)
4. **Melody #1** — primary melodic content
5. **Melody #2** — secondary / shadow melody
6. **Sensor Notes** — triggered by LED detection or MediaPipe

### Parameters (per layer, per scene)
- volume (0-1)
- filter frequency + resonance
- pitch / detune
- pan (-1 to 1)
- reverb send (0-1)
- delay send (0-1)
- delay time (ms)
- delay feedback (0-1)

### Global Controls
- BPM (slider + number input, 40-200)
- Sensitivity (slider, affects detection threshold)
- Echo Level (slider, affects MediaPipe delay+echo wet/dry)
- Master Volume

### Transitions
All parameters lerp between scene states. Duration per transition configurable. Easing curves: ease-in, ease-out, ease-in-out, exponential.

## Input System

### LED Blob Detection
- Frame differencing on camera feed
- Detects bright points in dark
- Output: array of {x, y, brightness, area}
- Fine, complex, reliable in dark

### MediaPipe Pose
- 33 pose landmarks when enabled
- On/off toggle (unreliable in dark)
- When detected → triggers notes with delay+echo effect
- Echo level adjustable

### 4 Khwan Dance Principles (modulate sound)
1. **Focal Point (จอมขวัญ)** — stillest detected point → panning
2. **Energy Flow (ขวัญไหลเวียน)** — velocity entropy → filter openness
3. **Lines→Circles (ลายวงก์ขวัญ)** — trajectory curvature → pitch bend
4. **Awareness (สิง)** — jerk smoothness → reverb wet/dry

## Scene Sound Specs

### Scene 0: Summoning Chanting
- Ambient: breath noise (inhale/exhale cycle ~4s)
- Beat: heartbeat sub-bass thump (~60bpm, ~1Hz)
- Noise: bandpassed white noise (breath)
- Melody: none
- Sensor: none (untracked)

### Scene 1: Star Formation
- Ambient: แคน drone building, warm pad layers entering
- Beat: กลอง pulse entering, building
- Noise: granular — particles of sound coalescing
- Melody: pentatonic fragments, scattered
- Sensor: LED blobs → bell-like tones per blob

### Scene 2: Binary (Quantum Entanglement)
- Ambient: two oscillator groups PHASE-LOCKED. One changes → other mirrors instantly but inverted. Spooky harmony.
- Beat: phase-locked dual rhythm
- Noise: beating interference patterns
- Melody: two melodies shadowing each other
- Sensor: LED pair → entangled note pairs

### Scene 3: Formless Bodies
- Ambient: generated voice — granulated speech, gibberish, phonemes without meaning, formant synthesis drifting
- Beat: rhythm dissolves, loses grid
- Noise: sub-bass rumble, earth drone
- Melody: fragments of Scene 1 reversed/pitched
- Sensor: NO detection — autonomous generation

### Scene 4: Supernova
- Ambient: MAX CHAOS. แคน out of tune, clusters. Everything.
- Beat: HARD HITS. Kick+snare distorted, irregular, accelerating
- Noise: white/pink noise FULL
- Melody: dissonant stabs, every semitone
- Sensor: full body MediaPipe → every joint triggers explosion note

### Scene 5: Collapse
- Ambient: แคน high+thin. VIOLIN SCREECH — sustained bow pressure harmonics
- Beat: retrigger delay on last Scene 4 beat, stuttering, decaying
- Noise: reverb freeze tail from Scene 4
- Melody: violin melody — slow, high, breaking
- Sensor: plate light blobs → ascending tones + retrigger delay

### Scene 6: Distribute
- Ambient: แคน gentle cyclical. Warm pad. No end.
- Beat: soft pulse, heartbeat returns (callback to Scene 0)
- Noise: room tone, soft
- Melody: pentatonic — Scene 1 returns transformed
- Sensor: audience tokens → each glow = gentle chime. More = richer.

## Environment Modes

Toggle between two modes (button or keyboard shortcut):

### Light Mode (testing/rehearsal)
- Room is bright — camera sees everything
- Blob detection: higher threshold (ignore ambient light, only track bright LEDs or phone flashlights)
- MediaPipe: works well — likely always detects full body
- Use for: testing sound, tuning sensitivity, rehearsing with dancers
- Visual: show camera feed as background, overlay blobs + skeleton

### Dark Mode (performance)
- Room is dark — camera sees only light sources
- Blob detection: lower threshold (sensitive to any bright point)
- MediaPipe: unreliable — on/off toggle, works only during strobe or UV
- Use for: actual performance
- Visual: black background, only tracked points + particles

### Settings per mode
| Parameter | Light Mode | Dark Mode |
|-----------|-----------|-----------|
| Blob threshold | high (150+) | low (30-50) |
| MediaPipe | always on | toggle on/off |
| Camera overlay | visible (debug) | hidden |
| Background | camera feed | black |

## Placeholder Visuals (Debug Overlay)

Toggle with `D` key or button. Shows:
- Camera feed (small, corner)
- Blob positions — colored circles at detected positions, size = brightness
- MediaPipe skeleton — lines + dots for 33 landmarks when detected
- 4 Khwan Principles readout:
  - Focal Point: crosshair at position
  - Energy Flow: bar (low entropy = red, high = green)
  - Curvature: bar (linear = straight line icon, circular = spiral icon)
  - Awareness: bar (low jerk = solid, high jerk = flickering)
- FPS counter
- Current scene + transition progress bar
- BPM + sensitivity + echo level values

## Build Order

1. Sound engine core (oscillators, noise, filters, reverb, delay, retrigger)
2. Scene system (7 states + transition lerp)
3. Scene 0 + Scene 4 (test extremes)
4. UI controls (BPM, sensitivity, echo, scene buttons 0-6)
5. All 7 scenes
6. Camera + LED blob detection
7. MediaPipe (toggleable)
8. 4 Principles analysis → sound modulation
9. Polish + rehearsal tuning
