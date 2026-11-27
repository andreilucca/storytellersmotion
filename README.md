# Storytellers Motion

### Live app: **https://andreilucca.github.io/storytellersmotion**

Repository: https://github.com/andreilucca/storytellersmotion

**Current version: v7.0-b58 (2026-11-27)**

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
- **Effects engine (WebGL)**: 23 animated background effects (Melt, Vortex,
  Ripple, Tunnel, Kaleido, Glitch, Droste, Liquid, Bloom,
  VHS, Burn, Halftone, Slices, Feedback, Twinspin, Depth 3D,
  Cymatics, Burst, Eclipse, Tendril, Orbits, Film burn, Thermal) with per-effect settings, palette editing, and
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
- **Hint**: one quiet row at the top right, built from the current state.
  Pick an effect and it says only what that effect goes best with (artwork
  mode, and Full render when it needs it). Move a slider and it says what
  that slider does in the selected effect, or that the effect does not use
  it. Format, artwork mode, text style and render mode each bring their
  own hint.
- **Entrance switches** (Artwork tab), each On or Off: "Sleeve opening"
  (record out, then text, then the effect grows from the record), "Effect
  opening" on Disc, Card and Vinyl Record (the effect starts after one
  second and grows out of the artwork, then the text), and "Text entrance".
  They act on Full render only; Off puts everything in place from the
  first frame. Saved in presets.
- **Play opening** (Artwork tab): plays the start of the video in the
  preview exactly as Full render exports it. It also plays when one of the
  entrance switches is flipped.
- **Film burn** (an effect in the grid): light leaks that flicker over the
  whole frame like the end of a film roll, your picture included, on a
  plain background. Red, orange, yellow, green and blue light comes and
  goes at random all the time, most of it faint, some of it burning hot,
  plus hairs and dust. Speed = how fast it flickers, Warp = how strong the
  light is, Scale = size of the leaks, Streak = hairs and dust, Swirl =
  colors from the Palette instead of film colors.
- **Thermal** (an effect in the grid): the artwork seen through a heat
  camera, from black and indigo through magenta and orange to yellow and
  white. It never settles and never repeats: every few seconds it glides
  to a new random reading of the picture (what counts as hot: brightness,
  warm colors or strong colors), with heat waves, hot spots that swell and
  fade, a drifting temperature field, and hot and cold swapping now and
  then. Nothing sweeps across the frame. Warp = twist and heat haze,
  Streak = hot spots, Swirl = how far it runs hotter or colder, Scale =
  size of the heat bands, Speed = how fast it moves to the next reading.
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

## Stable effects: do not alter

All 23 effects are final. The owner tested each one and accepted it exactly
as it is in v7.0-b58. This section is a rule for anyone who works on this
file, a person or an AI.

**The rule**

- Never rewrite, improve, sharpen, recolor, re-time or refactor an existing
  effect, its default look, or the shared code it passes through (helper
  functions, color grading, the finishing pass).
- An effect changes only when the owner asks for a change to that effect,
  by name. A general request ("make the effects better") is not that.
- New work means new effects or new features beside the existing ones.
- Every new build must prove it: the fingerprints below stay the same for
  every effect that was not the subject of the request.
- When a change to an effect is requested, keep the old version, and show
  old and new side by side on the same artwork and settings before it
  ships.

**Why this rule exists**

- 2026-11-13 and 14: the existing effects were "re-evaluated", and Vortex,
  Melt, Ripple, Droste and Liquid were rewritten with sharpening, extra
  contrast and painted light. They came out worse: hard edges, crushed
  tones, loud color, less depth. The judging had been done on small still
  frames, never next to the original.
- 2026-11-16: a colleague noticed on the live app. The five effects were
  put back to their original v6.9 code in b49 and checked pixel for pixel
  against the original.
- Tunnel (slower start), VHS (no black band) and Twinspin (twin galaxies)
  were changed at the owner's request and are accepted as they are now.
- Removed for good, do not bring back: Reactor (b50), Spectrum (b51), and
  earlier Datamosh, Palette flow, Nebula, CRT, Dither, ASCII, Echo, Lasers,
  Topographic, Warp, Pixel sort, Voronoi, Morphogen.
- Added and accepted: Burst, Eclipse, Tendril, Orbits, Film burn, Thermal.

**Fingerprints of v7.0-b58**

A fingerprint is the first 12 characters of the SHA-256 of a piece of
`index.html`, read as UTF-8 text. If one differs, that piece was changed.

| Piece | How it is cut out | Fingerprint |
|---|---|---|
| Main shader | text between ``const FS = ` `` and the closing `` `; `` | `991c957377cb` |
| Vertex shader | same, for `VS` | `e883cb9e218b` |
| Finishing shader | same, for `FS_COPY` | `b02464588c62` |
| Film burn logic | from the line `// ── FILM BURN` up to the line `// ── DETERMINISTIC GPU EXPORT` | `40d1c26d7add` |
| Default looks | the `MODE_DEFAULTS` object plus the two `MODE_DEFAULTS["32"]` and `["33"]` lines | `47cf18326865` |

Per effect: the block in the main shader from `if(uMode == N){` to its
matching closing brace.

| Effect | Id | Full render | Fingerprint |
|---|---|---|---|
| Melt | 0 |  | `f06371af7390` |
| Vortex | 2 | FULL | `9ba80451ba3b` |
| Ripple | 3 | FULL | `b5ab4fb38f97` |
| Tunnel | 6 | FULL | `c2ab1bb1dfd7` |
| Kaleido | 7 | FULL | `fbe97f2a81d6` |
| Glitch | 8 |  | `53b23f583149` |
| Droste | 10 |  | `e9cbc665a21a` |
| Liquid | 12 |  | `75e2c2091a1d` |
| Bloom | 4 |  | `74400b66b0ab` |
| VHS | 17 |  | `c3721c3f167c` |
| Burn | 18 |  | `677f2bd2dd55` |
| Halftone | 19 |  | `cf50a3c7c4db` |
| Slices | 20 |  | `2a42f671c781` |
| Feedback | 16 | FULL | `19dc3c4107cd` |
| Twinspin | 23 |  | `c5e157e149fa` |
| Depth 3D | 24 | FULL | `a0828684e9bd` |
| Cymatics | 25 |  | `34605028019a` |
| Burst | 28 | FULL | `4ac33c4bb739` |
| Eclipse | 29 |  | `295e40c4016c` |
| Tendril | 30 |  | `af37ce9e6b62` |
| Orbits | 31 |  | `5a6ff40bcbf2` |
| Film burn | 32 |  | `see FILM` |
| Thermal | 33 | FULL | `9760a68500f6` |

Film burn has no block in the main shader. It lives in the finishing shader
and in the Film burn logic.

## Fast loop and rotation speed

The loop is always a whole number of disc revolutions. At 1 RPM one revolution
takes 60 seconds, so:

- 30s and 1m clips are rendered in full, frame by frame.
- On a full song, Fast loop renders a 60 second loop instead of a 12 second
  one, so the export takes longer than at higher speeds.

Effects marked **FULL** (Vortex, Ripple, Tunnel, Kaleido, Feedback,
Depth 3D, Thermal) rotate or evolve continuously and need Export render:
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
  `Storytellers Motion · v7.0-b58 · 2026-11-27`.
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

### Tested and confirmed: keep as it is

These were tested by the owner and accepted. Do not change them unless he
asks for that item.

- **Export**: Fast loop and Full render both give a valid MP4 with sound.
  File name `storytellersmotion-<ratio>-<artwork>-<effect>.mp4`.
- **Sleeve opening** (Full render): record out from 0.5 to 4.5 s, text
  from 5.0 s, effect from 8.0 s. He asked for this slow pace. Do not
  shorten it.
- **Effect growth in the opening**: Tendril from the rim at its own pace,
  Orbits one orbit after another, Eclipse thickening from the rim. Every
  other effect opens as a soft circle out of the record.
- **Effect opening** on Disc, Card and Vinyl Record: effect from 1.0 s,
  text from 3.5 s. Switch on by default.
- **Play opening**: the preview runs on the same plan as the export.
- **Film burn**: light on in about half of the frames, at random, with no
  build-up and no calm. Longest dark gap about half a second. It lies over
  the artwork, on a plain background.
- **Thermal**: no line sweeping across the frame. It glides between random
  readings and never repeats, so it is marked FULL.
- **Hint**: one quiet row at the top right, plain interface font. An effect
  says only what it goes best with. No labels, no extra rows.
- **Footer**: name, version and date only. Change notes go in the
  Changelog.
- **Interface**: no bright colors, short labels, nothing hidden.
- **Local use**: the page must be served over http (the launcher does
  this). Opened straight from the file, the export fails.

## Changelog

The page footer shows only the name, the version and the date. What changed
in each build is listed here, newest first.

| Build | Date | What changed |
|---|---|---|
| v7.0-b58 | 2026-11-27 | The footer shows only name, version and date. Change notes moved to this changelog. README gained the "Stable effects: do not alter" rule with fingerprints, and the list of tested things to keep. |
| v7.0-b57 | 2026-11-27 | Hint reduced to one quiet row in the plain interface font. An effect says only what it goes best with. |
| v7.0-b56 | 2026-11-27 | Thermal marked FULL (it no longer loops with the disc). A larger hint card was tried and withdrawn in b57. |
| v7.0-b55 | 2026-11-26 | Thermal: scan line removed. It glides between random readings of the picture and never repeats. |
| v7.0-b54 | 2026-11-26 | Film burn and Thermal became effects of their own in the grid, steered by Effect settings. The two layer switches were removed. |
| v7.0-b53 | 2026-11-26 | Film burn flickers at random all the time, on a rhythm measured on reference footage. |
| v7.0-b52 | 2026-11-26 | Film burn added: light leaks flashing over the whole frame. |
| v7.0-b51 | 2026-11-26 | Spectrum removed. |
| v7.0-b50 | 2026-11-16 | Reactor removed. |
| v7.0-b49 | 2026-11-16 | Vortex, Ripple, Melt, Droste and Liquid restored to their original code and look. |
| v7.0-b48 | 2026-11-14 | Play opening: the openings can be watched in the preview. |
| v7.0-b47 | 2026-11-14 | Effect opening on Disc, Card and Vinyl Record, with its own switch. |
| v7.0-b46 | 2026-11-14 | Switches for the Text entrance and the Sleeve opening. |
| v7.0-b45 | 2026-11-14 | In the Sleeve opening, Orbits unfold from the record and Eclipse grows from the rim. |
| v7.0-b42 | 2026-11-14 | In the Sleeve opening, Tendril starts from nothing at the rim and grows. |
| v7.0-b41 | 2026-11-14 | In the Sleeve opening, the effect grows out of the record. |
| v7.0-b40 | 2026-11-14 | Slow Sleeve opening in three steps: record, then text, then effect. |
