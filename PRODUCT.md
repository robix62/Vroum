# Product

<!-- impeccable:product-schema 1 -->

## Platform

web

## Stack

Static HTML/CSS/JS in a single file (`vroum.html`), with no framework and no build step. Hosted on GitHub Pages in a dedicated repository, which provides the HTTPS that geolocation and AudioWorklet need.

## Users

One user: the owner (Chrob), driving his own Tesla Model Y. He uses it in the car's built-in browser, from the driver's seat, glancing at the screen. A phone on the car's speakers over Bluetooth is a secondary context. The product is not shared or published for anyone else.

## Product Purpose

Vroum plays a live combustion-engine sound through the car's cabin speakers. The sound follows the car's real speed. It is a personal, subscription-free replacement for paid services such as teslasound.io.

Success means the sound is convincing enough to pass for a real engine of the chosen type, and that it reacts in step with the driving.

## Positioning

Vroum is a self-owned and self-hosted tool, with no subscription, no account and no daily time limit. Its sound comes primarily from the owner's own real engine recordings, which he loads into the page. They are pitched by RPM and crossfaded between a low-RPM and a high-RPM recording. Procedural synthesis is only the fallback.

## Operating Context

- Runs in the Tesla browser while driving, and must stay in the foreground: audio stops when the browser goes to the background.
- Speed comes from browser geolocation, at about 1 Hz in the car. The page extrapolates between fixes and simulates a gearbox: RPM, gear, load from acceleration, upshifts and downshifts.
- Audio plays only inside the cabin. A web page cannot use the external Boombox speaker.
- The audio starts only after a tap, because of browser autoplay rules. The page requests a screen wake lock while running.
- A Test mode with on-screen throttle and brake pedals lets the owner audition sounds without driving.
- Recordings are stored per engine in the browser's IndexedDB, so they survive reloads in the car.

## Capabilities and Constraints

- **Cars (real recordings only, synthesis removed at the owner's request):** Ferrari 458 (V8), BMW M1 Procar (inline-six), BAC Mono (inline-four), Streiter V12 (multi-RPM set). Sounds come from MIT-licensed repositories: markeasting/engine-audio and ATG Vehicle Noise Synthesizer.
- **Synthesis model:**
  - Engine-order additive synthesis, with orders specific to each architecture, slow per-order fluctuation and intake noise.
  - Blended with a per-cylinder exhaust-pulse texture that follows the real firing order and bank split.
  - Shaped by a simulated exhaust: a resonant pipe, muffler EQ, a load-dependent low-pass filter and soft saturation.
- **Driving effects:** rev limiter, overrun pops, turbo whistle and blow-off.
- **Real recordings:** up to two per engine (low RPM and high RPM), each with the RPM it was recorded at. Playback rate equals current RPM divided by the reference RPM, clamped to 0.25–4×. The loop seam is crossfaded automatically.
- **Hard constraints:**
  - HTTPS is required for GPS and AudioWorklet. A ScriptProcessor fallback exists for non-secure contexts.
  - No backend.
  - No third-party sound assets are bundled. The owner supplies the recordings, for example CC0 recordings from Freesound or clips he extracts himself.
- **Open:** whether to ship default bundled recordings. Their sourcing and licensing are undecided.

## Brand Commitments

The name is "Vroum". The UI language is French, informal. Engines are named by architecture ("V8 américain", "V12 GT"), never by car brand or model.

## Evidence on Hand

- `vroum.html`, the working app.
- No recordings are bundled yet, and no user testing exists beyond the owner's own listening.
- The owner's listening feedback so far:
  - The first synthesis sounded "like bees" (too buzzy).
  - The V12 did not resemble a big engine. That feedback drove the move to real recordings as the primary source.

## Product Principles

1. **Realism over features.** A convincing engine beats more options. When synthesis and recordings disagree, recordings win.
2. **Glanceable and safe while driving.** Large touch targets, minimal reading and no required interaction once running. Sound must never mask sirens, so the volume stays sensible.
3. **Self-contained.** One static file, no account, no subscription, no external services beyond hosting.
4. **Honest about limits.** State plainly what the browser cannot do (Boombox, background audio, GPS lag) instead of hiding it.

## Accessibility & Inclusion

The page is used in a moving vehicle, so it needs high-contrast, large touch targets usable at arm's length, and must not depend on fine reading while driving.
