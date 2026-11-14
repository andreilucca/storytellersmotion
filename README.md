# Storytellers Motion

### Live app: **https://andreilucca.github.io/storytellersmotion**

Repository: https://github.com/andreilucca/storytellersmotion

**Current version: WEB v7.0-b34 (2026-11-14)**

Turn a cover and a song into a smooth, professional promo video: a spinning
disc, a vinyl record, or a record sliding out of its sleeve, over an animated
effects engine. Everything renders 100% in your browser: no server, no upload,
no watermark, no account. Your files never leave your device.

## Features

- **Four artwork modes**
  - **Disc**: the artwork itself is the spinning record, with drop shadow,
    anti-aliased edge and centre spindle hole.
  - **Card**: the square artwork centred as a poster, no rotation.
  - **Vinyl**: a black grooved record under a light that stays fixed in the
    room, with the artwork as the spinning centre label. Two variants:
    **Record** (the record on its own) and **Sleeve** (the artwork as the
    cover, with the record pulled halfway out and spinning).
    An optional **Label image** puts a different picture on the centre label,
    for example the Side A artwork.
  - **Off**: no artwork layer, the background effect fills the whole frame.
- **Effects engine (WebGL)**: 21 animated background effects (Melt, Vortex,
  Ripple, Tunnel, Kaleido, Glitch, Droste, Liquid, Bloom, Spectrum,
  Reactor, VHS, Burn, Halftone, Slices, Feedback, Twinspin, Depth 3D,
  Cymatics, Burst, Eclipse) with per-effect settings, palette editing, and
  forward and reverse direction. Colors come from the artwork.
- **Every effect has its own look**: selecting an effect loads its full
  default look, including the finishing dials. Reset restores that look.
- **Burst follows the music**: when the audio loads, the whole track is
  analysed once and the hits are mapped. Bursts land on those hits in the
  preview and in the export. 24 compositions take turns in a shuffled deck
  (grids, rays, rings, constellations, galaxies, flower of life, mandalas,
  butterflies, mushrooms, orbits and more), and none repeats until all 24
  have played. Speed sets how often it hits. Burst always exports as a full
  render.
- **Tabbed panel**: four tabs (Media, Setup, Artwork, Effects), a thumbnail
  of every effect rendered from your artwork, and Record always in view.
- **Presets and Random**: Save keeps the whole look under a name (artwork
  mode, effect and its dials, disc speed and direction, photo size,
  background color, text style). Presets live in your own browser. Random
  picks another effect and varies its main dials.
- **Finishing dials**: Aberration, Glow, Grain and Vignette apply on top of
  any effect.
- **Rotation**: 1 to 120 RPM (default 1), clockwise or counter-clockwise.
- **Optional audio**: MP3, WAV, OGG, M4A, or export a silent clip. Full song
  of any length, or a fixed window (30s or 1m) dragged across the waveform,
  with audio preview.
- **Social formats**: Square 1080x1080, Portrait 1080x1350, Story 1080x1920,
  Wide 1920x1080 (YouTube).
- **60 fps deterministic export**: every frame is computed at its exact
  timestamp with a perfectly constant rotation step per frame. Zero dropped or
  duplicated frames, by construction. The export matches the live preview.
- **Fast full-song exports (Fast loop)**: only one seamless loop is rendered
  (whole disc revolutions, at least 12 seconds, plus a 1 second background
  crossfade baked at render time), then stream-copied to the full song length.
  **Full render** computes every frame of the song 1:1 and is required for
  FULL-tagged effects.
- **Full-song safe**: the export bitrate adapts to the song length so long
  tracks always fit in browser memory (up to 45 Mbps on short clips, around
  24 Mbps at 5 to 6 minutes, never below 8 Mbps on the GPU path), with a
  keyframe every 2 seconds for clean seeking.
- **Platform-friendly heavy effects**: Grain reseeds its noise pattern at
  10 Hz instead of every frame and VHS snow is calmed. Motion, rotation and
  frame rate are untouched; this only lowers the temporal entropy of the
  encoded pixels, so platform re-encoders (YouTube) keep the HD rungs cheap
  enough to serve on Auto quality.
- **Background color**: dark by default (`#171616`). The color you pick is
  remembered in your own browser.

## Fast loop and rotation speed

The loop is always a whole number of disc revolutions. At 1 RPM one revolution
takes 60 seconds, so:

- 30s and 1m clips are rendered in full, frame by frame.
- On a full song, Fast loop renders a 60 second loop instead of a 12 second
  one, so the export takes longer than at higher speeds.

Effects marked **FULL** (Vortex, Ripple, Tunnel, Kaleido, Spectrum, Feedback,
Depth 3D) rotate or evolve continuously and need Export render:
Full render on full songs. The app reminds you with a toast.

## How the export works

The exporter picks the best available strategy at record time. The chosen
strategy is logged to the browser console as `[storytellersmotion export] strategy:`.

**1. Deterministic GPU export (primary, WebCodecs).** Frames are rendered by
the WebGL engine one at a time at exact timestamps and handed straight to the
GPU hardware H.264 encoder in quality mode. No real-time capture and no
intermediate encode generation. Output is 60 fps for both fast loop and full
render. The loop crossfade is baked during rendering, so the H.264 stream is
encoded exactly once; ffmpeg.wasm only remuxes it and muxes the original
audio (AAC 320k, faststart).

**2. Real-time capture (fallback).** On browsers without WebCodecs H.264
support, the scene is captured live (MediaRecorder at up to 60 Mbps), then
ffmpeg.wasm bakes the loop crossfade (x264 superfast, crf 16, adaptive
maxrate, keyframe every 2 seconds) and muxes the audio.

**Classic exporter (Disc with effects off).** The disc rotation is perfectly
periodic, so a single revolution is rendered once with sub-frame motion blur,
encoded, then stream-copy-looped to the full duration with the original audio
muxed in.

ffmpeg.wasm is vendored in `vendor/ffmpeg` (nothing fetched from a CDN) and is
preloaded in the background when the page opens, so recording starts
instantly. Browser memory is limited to roughly 2 GB, which is why the
bitrate adapts to duration; very long mixes (20+ minutes) export at more
modest bitrates.

## Project structure

```
index.html          the whole app (UI, WebGL engine, exporters)
Start Storytellers Motion.bat      double-click launcher for Windows
Start-StorytellersMotion.ps1       the local server the launcher runs
vendor/ffmpeg/      vendored ffmpeg.wasm (works offline, no CDN)
fonts/              embedded fonts
```

## Usage

1. Open the live app and pass the access screen.
2. Upload the artwork, optionally the song, pick a format.
3. Pick an artwork mode (Disc, Card, Vinyl, Off) and optionally an effect;
   the preview is what you get.
4. Record Video, then Download MP4.

## Running it locally

The app must be served over http. Opening `index.html` directly from disk
(`file://`) shows the preview, but the export cannot load its engine and
fails with "Failed to fetch".

On Windows, double-click **Start Storytellers Motion.bat**. It starts a small
local server (`Start-StorytellersMotion.ps1`, PowerShell only, no install, no
administrator rights) and opens the app in the default browser. Keep the
window open while you work; close it to stop.

The folder must hold `index.html`, `vendor`, `fonts` and the two launcher
files. Alternative, with Python installed:

```
python -m http.server 8080
```

then open http://localhost:8080.

## Uploading to YouTube

The exported MP4 is always complete 1080p/60. What YouTube serves on **Auto**
quality is decided per upload by its server-side adaptive bitrate (SABR),
and heavy content matters:

- Smooth effects (Bloom, Liquid, Eclipse...) are cheap to re-encode and usually
  get HD on Auto right away.
- Detail-dense effects (Depth 3D, VHS, Burst...) produce expensive 1080p
  renditions; a fresh upload can sit on Auto 360p even though 1080p60 is
  already processed and works when selected manually.
- **If a fresh upload stays on Auto 360p, delete the video and upload the same
  file again.** The serving decision is made per upload, not per file, and a
  re-upload of the identical MP4 typically lands directly on HD. Waiting has
  no guaranteed deadline; re-uploading takes 10 minutes.
- There is no uploader-side setting to force playback quality for viewers;
  the only upload-side levers are re-uploading and higher-resolution masters.

## Troubleshooting

- After an update, check the version tag in the footer; hard refresh with
  Ctrl+F5 if it shows an older version. Current:
  `Storytellers Motion · WEB v7.0-b34 · 2026-11-14`.
- The console line `[storytellersmotion export] strategy:` tells you which pipeline
  ran: `webcodecs avc1...` means the GPU encoder (the progress label also
  shows `GPU encode`), anything else means the real-time fallback.
- Playback stutter on YouTube while the app is exporting is expected: the GPU
  encode and the browser's 1080p60 decode compete for the same GPU. Check
  Stats for nerds ("dropped of" counter) and retest with the app tab closed.
- FULL-tagged effects on a full song need Export render: Full render;
  Fast loop would reset their continuous rotation at every loop.
- The background color looks different on another computer: the chosen color
  is stored per browser. Pick it once on each machine.
