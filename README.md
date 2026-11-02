# Browser Animator

A 2D frame-by-frame animation studio in a single self-contained `index.html`. It needs no build step and no dependencies, and works offline. It's built to run well on Chromebooks.

## Run it
- **Easiest:** double-click `index.html`, or drag it into a Chrome tab.
- **Or serve it locally:** run `python3 -m http.server 8000` and open http://localhost:8000.
- **Or host it** on GitHub Pages: Settings → Pages → deploy from branch.

## Features
- **Select & move (V):** click a shape to grab it and drag it anywhere, or drag a box around part of a drawing. Drag a corner to resize (Shift keeps proportions). Arrow keys nudge, Delete removes, and Ctrl+C / Ctrl+V pastes the selection onto another frame.
- **Tools:** brush (smoothed, with pen pressure), eraser, fill bucket (tolerance, can sample all layers), line, rectangle and ellipse (filled or outline, Shift to constrain), eyedropper.
- **Brush settings:** size, opacity, color picker and a saved-colors palette that is remembered between sessions.
- **Timeline:** add, duplicate, delete and drag-to-reorder frames, with live thumbnails.
- **Onion skin:** previous frames show tinted red and next frames tinted green. You can set the opacity and show ±1–3 frames.
- **Layers:** shared across all frames. Add, delete, hide/show, drag to reorder, rename (double-click) and set per-layer opacity.
- **Music:** import an MP3, WAV, OGG or M4A file.
  - A waveform appears under every frame in the timeline, and the tempo is detected automatically.
  - Pink lines mark exactly where each beat falls, and frames that contain a beat get a pink dot.
  - Playback runs off the audio clock, so the frames stay locked to the song.
  - Stepping through frames plays a short snippet of the song.
  - Press `M` to drop markers on the beat while playing, and `J` / `K` to jump between beats.
  - The music settings suggest frame rates where every beat lands exactly on a frame, and can add frames to match the song's length.
- **Playback:** play/pause, FPS from 1–30, loop toggle.
- **Undo/redo:** covers drawing and frame/layer operations. Memory is capped.
- **Export:** video with music (MP4 or WebM, recorded in real time), animated GIF (built-in median-cut + LZW encoder, optional transparency), PNG sprite sheet, and the project as `.json`.
- **Saving:** to browser `localStorage` (auto-save on by default; restores your last project when you come back), plus `.json` import/export.

## Shortcuts
`V` select · `B` brush · `E` eraser · `G` fill · `L` line · `R` rect · `C` ellipse · `I` eyedropper · `[` `]` size ·
`,` `.` prev/next frame · `Space` play · `N` new frame · `D` duplicate · `Del` delete frame · `O` onion skin · `M` beat marker · `J` `K` prev/next beat · `Ctrl+C` / `Ctrl+V` copy/paste the selection ·
`Ctrl+Z` / `Ctrl+Y` undo/redo · `Ctrl+S` save · `+` `-` `0` zoom · `?` help

## Code layout
The `<script>` in `index.html` is split into numbered sections:

| Section | Purpose |
|---|---|
| 0 Utilities | helpers and the generic `makeSortable` drag-to-reorder |
| 1 Project model / state | `Project`, `S` (app state) and change notifications |
| 2 History | undo/redo command stack |
| 3 Renderer | compositing, live stroke preview and onion skin |
| 4 View | zoom/fit and the brush cursor |
| 5 Tools | drawing tools and flood fill |
| 5b Select | select, move, resize and copy/paste shapes |
| 6 Timeline | frame strip and frame commands |
| 7 LayersPanel | layer list and layer commands |
| 8 Playback | play/pause and frame timing |
| 8b Music | audio import, waveform, beat detection and audio-synced playback (song files are stored in IndexedDB) |
| 9 Export | `GifEncoder`, sprite sheet, video recording |
| 10 Store | localStorage, JSON, auto-save |
| 11–12 | dialogs, UI wiring and the `Actions` table |
| 13 Modes | tab registry |
| 14 App bootstrap | startup and global key handling |

**Adding 3D later:** build a module with the shape `{ el, activate(), deactivate(), onKey(e) }` and register it with `Modes.register('3d', Studio3D)`. The 3D tab and placeholder panel are already in place. three.js would be the only external script.
