# Storytellers Motion: handoff for the next chat

Written on 2026-11-27, at build **v7.0-b58**. It replaces the handoff of 2026-11-15.
Read all of it before the first task. Sections 1 and 2 are mandatory behaviour, not background.

To start a new chat, give it three files: `index.html` (b58), `README.md` and this file.

## 1. How to work with the owner

The owner is Andrei. He set these rules himself, most of them more than once.

**Language**
- He writes Romanian. Answer in Romanian **without diacritics**, always.
- Everything in the app and the repo is in **English**: interface text, hints, comments, README.
- Never use em dashes or horizontal separator lines, in chat or in files.
- Spell out an acronym the first time it appears in a reply.

**Permission**
- Never change or generate anything on your own initiative. Propose, then wait for an explicit OK for that step.
- When he says to do nothing until you confirm you understood, do exactly that: state in two or three lines what you understood, ask if it is right, and stop. Do not run anything, not even a read-only comparison.
- One item at a time: deliver, he tests, he gives a verdict, then the next.

**Honesty**
- Verify before claiming. In every delivery say what was verified, how, and what was not.
- No fix that is a guess. State uncertainty plainly.
- Short answers, no flattery, no filler. When he points out a mistake, fix it and move on.
- He is harsh when frustrated. Stay steady, own a real mistake in one line.

**Showing before deciding**
- He cannot judge from a description. Show a build, rendered frames or a short video first.
- When copying a reference (a video of an effect he likes), measure it frame by frame. A reading taken every half second once gave the wrong rhythm.

**Deliveries**
- Every build bumps the version. The page footer holds only `Storytellers Motion · v7.0-bNN · YYYY-MM-DD`.
- What changed goes into `README.md`, as a new row at the top of the Changelog table. README is updated with every build.
- Links without a trailing slash. The project's earlier name must not appear anywhere.
- He uploads `index.html` and `README.md` to GitHub himself after testing.
- Keep the interface minimal: he rejected a hint card with labels and extra rows as overloaded.

## 2. Stable effects: do not alter

This is the most important rule of the project.

All 23 effects are final. He tested each and accepted it exactly as it is in b58.

- Never rewrite, improve, sharpen, recolor, re-time or refactor an existing effect, its default look, or shared code it passes through (helper functions, color grading, the finishing pass).
- An effect changes only when he asks for a change to that effect, by name. "Make the effects better" is not that. If he says something general, ask which effect and what exactly.
- New work means new effects or new features beside the existing ones. He brings the ideas for new effects himself. Do not propose effects.
- Every build must prove that nothing else moved: recompute the fingerprints listed in `README.md`, section "Stable effects: do not alter", and report that they are unchanged for every effect that was not the subject of the request.
- When he does ask to change an effect: keep the old code, and show old and new side by side on the same artwork and settings before it ships.

**Why**

- On 2026-11-13 and 14 the existing effects were "re-evaluated" and Vortex, Melt, Ripple, Droste and Liquid were rewritten with sharpening, extra contrast and painted light. They came out worse: hard edges, crushed tones, loud color, less depth. They had been judged on small still frames, never next to the original.
- On 2026-11-16 a colleague of his noticed on the live app. He was furious. The five were put back to their original v6.9 code in b49 and checked pixel for pixel against the original.
- The lesson he drew: what exists and works is not to be touched.

**Start of every session**: before any work, compute the fingerprints of the `index.html` you were given and compare them with the README table. If they differ, tell him before doing anything else.

## 3. Project facts

- **What**: a browser tool that turns a cover image and a song into a promo video: a spinning disc, a card, a vinyl record or a record sliding out of its sleeve, over an effects engine. Everything runs in the browser.
- **Who**: Andrei and one colleague at the Storytellers label. Not a public product.
- **Core requirement**: universal. Any cover with any song must look good. Nothing may be tuned to one artwork.
- **Repository**: <a href="https://github.com/andreilucca/storytellersmotion" target="_blank">github.com/andreilucca/storytellersmotion</a>
- **Live app**: <a href="https://andreilucca.github.io/storytellersmotion" target="_blank">andreilucca.github.io/storytellersmotion</a>
- **Files**: `index.html` (the whole app, about 5,470 lines), `vendor/ffmpeg/` (ffmpeg.wasm 0.12.6: `ffmpeg.js`, `util.js`, `814.ffmpeg.js`, `ffmpeg-core.js`, `ffmpeg-core.wasm`), `fonts/` (three fonts), `README.md`, and a local launcher (`.bat` plus `.ps1`).
- Nothing is loaded from the internet at runtime. Opened from `file://` the export fails, so it must be served over http.
- A client-side password screen guards the page. The password is defined in `index.html`.
- Browser storage: `stm.bg` (background color), `stm.presets` (saved looks).

**Current build**: v7.0-b58 · 2026-11-27, md5 of `index.html`: `2f5bc1733b4f8853bc784888edb347c1`. On 2026-11-27 the repository held b55.

## 4. Architecture of index.html

Search by name, line numbers shift.

- `S`: app state. `draw()` and `animLoop()`: the preview loop.
- `TXT`, `drawTextOverlay()`, `txtIntro(tSec, startSec)`: the text on the video, drawn in 2D over the frame.
- Effects engine `window.FX`: WebGL1. Shaders `FS` (main: the effect is chosen by `uMode`, the artwork mode by `uArt`), `VS`, `FS_COPY` (finishing pass: aberration, grain, glow, vignette, and Film burn).
- `st`: effect state. `MODE_DEFAULTS`: the full default look of each effect. `render(W, H, angleDeg, tOverride)` draws a frame for the preview and for the export alike.
- `uArt`: 1 Disc, 2 Card, 3 Vinyl Record, 4 Vinyl Sleeve, 0 Off.
- Export: `liveExport()` renders at 60 frames per second, encodes H.264 with WebCodecs (tries `avc1.64002a`, `avc1.640028`, `avc1.4d002a`, `avc1.42e02a`), muxes with ffmpeg.wasm. Fast loop: whole disc turns, at least 12 s, 1 s crossfade, stream-copied to the song length. Full render: every frame. `liveExportRealtime()` is the fallback and plays no openings.
- Openings: `openingPlan(fastLoop)`, `sleeveIntro`, `fxIntro`, `txtIntro`. Preview playback: `playOpening()`, `previewRender()`.
- Film burn: `filmEvent`, `filmLeak`, `filmState(t, P)`, pure functions of time, drawn in `FS_COPY`. It is effect 32.
- Thermal: effect 33, a block in `FS`, time based.
- Burst: the song is analysed once at load and a hit map is built.
- Hints: table `FXI`, `fxParts()` and `bestClause()` for effects, `dialHint()` for sliders, exposed as `window.SM_HINTS`.
- Thumbnails: `makeThumbs()`, one per effect, rendered by the engine from the artwork.

## 5. Features at b58

- **Tabs**: Media, Setup, Artwork, Effects. Record Video pinned at the bottom.
- **Formats**: Square 1080x1080, Portrait 1080x1350, Story 1080x1920, Wide 1920x1080.
- **Artwork**: Disc, Card, Vinyl (Record or Sleeve), Off. Label image. Text: Off, Clean, Style A, B, C, size 1x, 1.5x, 2x.
- **Switches** (Artwork tab): Sleeve opening, Effect opening, Text entrance, and the Play opening button.
- **Effects tab**: presets (Save, delete, Random), Export render (Fast loop, Full render), the effect grid, Forward and Reverse, Effect settings, Palette.
- **The 23 effects**, with their `uMode` id. FULL means Full render is needed for a whole song.

| Id | Name | Id | Name | Id | Name |
|---|---|---|---|---|---|
| 0 | Melt | 17 | VHS | 25 | Cymatics |
| 2 | Vortex (FULL) | 18 | Burn | 28 | Burst (always Full render) |
| 3 | Ripple (FULL) | 19 | Halftone | 29 | Eclipse |
| 6 | Tunnel (FULL) | 20 | Slices | 30 | Tendril |
| 7 | Kaleido (FULL) | 16 | Feedback (FULL) | 31 | Orbits |
| 8 | Glitch | 23 | Twinspin | 32 | Film burn |
| 10 | Droste | 24 | Depth 3D (FULL) | 33 | Thermal (FULL) |
| 12 | Liquid | 4 | Bloom | | |

- **Film burn**: light leaks flickering over the whole frame, artwork included, on a plain background. Dials: Speed flicker speed, Warp strength of the light, Scale size of the leaks, Streak hairs and dust, Swirl colors from the Palette instead of film colors. Trails has no role.
- **Thermal**: the artwork through a heat camera. It glides every six seconds to a new random reading and never repeats. Dials: Warp twist and haze, Streak hot spots, Swirl hotter or colder, Scale band size, Speed pace of change.
- **Openings on Full render**: Sleeve: record out 0.5 to 4.5 s, text from 5.0 s, effect from 8.0 s. Disc, Card, Vinyl Record: effect from 1.0 s, text from 3.5 s. He asked for the slow pace. He uses the Sleeve opening on 1 minute clips.
- **Hint**: one quiet row at the top right. An effect says only what it goes best with. A slider says what it does in the selected effect.
- **Interface rules**: no bright colors, soft text, accent green `#2f9a5c`, short labels, nothing hidden.

The full list of what he tested and accepted is in `README.md`, section "Tested and confirmed: keep as it is".

## 6. Decisions already taken. Do not reopen

- **Removed effects**: Reactor, Spectrum, Datamosh, Palette flow, Nebula, CRT, Dither, ASCII, Echo, Lasers, Topographic, Warp, Stardust, Tornado, Fluid ink, Reaction, Chrome, Holofoil, Slitscan, Shatter, Fisheye orbit, Pixel sort, Voronoi, Morphogen.
- **Dropped features**: 3D geometry from a depth model, still image and animated loop export, the style layers Dots, Contour and Threshold, a 1440p or 4K export, shortening the Sleeve opening for 30 second clips, the two alternative panel layouts.
- **Thermal and Film burn as layers over other effects**: tried, then turned into effects of their own at his request. Do not bring the layer switches back.
- **A hint card with a label, a Best with row and a Dials row**: rejected as overloaded.
- **Live beat detection, an envelope follower, a Pulse dial**: rejected earlier. Only Cymatics and Burst follow the music.

## 7. Verification

How builds were checked in the web chat, where there is no real graphics card and no Windows:
- JavaScript syntax check and shader compile for every build.
- Frames rendered with the real shader in a software renderer.
- The real app driven in headless Chromium, with a settable clock when timing mattered.
- For effects: a comparison of each effect's code against the previous build, to prove nothing else moved.

The working folder of the web chat does not survive between days. The delivered `index.html` is the only source of truth. The repository can be cloned for `vendor/` and `fonts/`.

**Not verified by anyone at b58**: a full export of a build later than b55 (b56 to b58 changed only hints, the FULL mark of Thermal and the footer).

## 8. Open work

- **A single local Windows program**: he took this to Claude Code on 2026-11-15 with the earlier handoff. Its state is not known here. Ask him.
- **Tempo grid with automatic beat tracking**: on the todo list, postponed on 2026-11-14. A test on seven of his tracks found the tempo with a simple method on all seven, but beat positions needed the madmom neural tracker on tracks with rolling bass or broken rhythm (he confirmed three by ear). The trained madmom models are licensed for non-commercial use only. He considers his use non-commercial. Ask before reopening.
- **New effects**: only when he brings one, as he did with Film burn (a Filmora effect he showed in a demo video).
