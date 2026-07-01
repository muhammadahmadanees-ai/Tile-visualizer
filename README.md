# Tile Visualizer & Estimator | Interactive Room Tiling Preview

Upload a room photo, mark its floor and walls, and preview different tile photos on each surface with realistic perspective and lighting — then estimate material quantities and cost from your own room measurements. **Tile Visualizer & Estimator** is a single-file, dependency-free web app built entirely with the Canvas 2D API — no framework, no backend, no build step.

**🚀 Deployment:** https://tile-visualizer-theta.vercel.app/

## Features

- **Multi-Surface Marking**: Click 3–6 points to trace any floor, wall, or alcove outline directly on your room photo — supports both simple rectangles and irregular shapes
- **Multi-Tile Support**: Upload 2–4 independent tile photos and assign a different tile to each surface (e.g. one for the floor, another for a feature wall)
- **Perspective-Correct Rendering**: Custom homography (DLT) and piecewise-affine mapping keep tile rows and grout lines perfectly straight, even on angled or irregular surfaces
- **Tile Straightening**: Mark a tile photo's 4 corners to correct for an angled shot before it's applied
- **Anti-Aliased Texturing**: Custom mipmap chain with trilinear sampling eliminates moiré/aliasing where perspective compresses many tile repeats into a few pixels
- **Lighting-Aware Shading**: Per-pixel luminance matching blends the tile texture into the room photo's existing light and shadow
- **Independent Cost Estimator**: Manual-input calculator for floor/wall area, tiles needed, boxes needed, and cost by price per m² — decoupled from the visual preview

## Tech Stack

| Layer | Technology |
|-------|-----------|
| **Core** | Vanilla HTML, CSS, JavaScript (ES6+) |
| **Rendering** | HTML5 Canvas 2D API, raw `ImageData` pixel manipulation |
| **Geometry & Math** | Custom homography solver (DLT), 3×3 matrix inversion, bilinear/trilinear sampling |
| **Styling** | Vanilla CSS (variables, grid, flexbox) |
| **Fonts** | Google Fonts (Fraunces, Inter, IBM Plex Mono) |
| **Dependencies** | None — no npm packages, no build tooling |
| **Hosting** | Vercel (static deployment) |

## Architecture

```
User → Room Photo Upload → Click points → Surface (affine or homography transform)
                                                    ↓
                    Tile Photo(s) → Optional straighten → Texture → Mipmap chain
                                                    ↓
                                  Per-surface tile assignment
                                                    ↓
                             Render loop (UV lookup + shading + AA)
                                                    ↓
                                    Coverage Preview (Canvas)

Room/Tile Dimensions → Estimator (independent) → Area, Tiles, Boxes, Cost
```

## Project Structure

```text
├── index.html                 # Entire app: markup, styles, and logic
│   ├── <style>                # Design tokens (CSS variables) & component styles
│   ├── <body>
│   │   ├── Room photo panel       # Upload, canvas, surface marking, surface chips
│   │   ├── Tile photos panel      # Dynamic per-tile "slots": upload, straighten, ready preview
│   │   ├── Coverage preview panel # Render / reset buttons
│   │   └── Estimator panel        # Dimension inputs, live summary
│   └── <script>
│       ├── Homography math        # computeHomography, invert3x3, applyH
│       ├── Piecewise-affine map   # UV templates, per-triangle affine solve
│       ├── State                  # roomImg, surfaces[], tiles[]
│       ├── Tile slot factory      # addTileSlot: upload, straighten, finalize per tile
│       ├── Mipmap generation      # buildMipChain, trilinear sampling
│       ├── Render loop            # per-surface UV lookup, mip level, shading
│       └── Estimator              # calc(): areas, tile/box counts, cost
├── PROJECT_REPORT.md          # Detailed feature & architecture write-up
└── README.md                  # Project overview & usage guide
```

## Local Development

Clone it:

```bash
git clone https://github.com/muhammadahmadanees-ai/Tile-visualizer.git
cd Tile-visualizer
```

No install step needed — just open the file:

```bash
open index.html
# or double-click it in your file explorer
```

For a local dev server with live reload (optional):

```bash
npx serve .
```

## Environment Variables

None required — the app has no backend, API keys, or external services. All processing (image uploads, canvas rendering, cost calculation) happens client-side in the browser.

## Usage

1. Visit the deployment link or open `index.html` locally.
2. Upload a room photo, then click 3–6 points to trace a surface and click **Add surface**. Repeat for each floor/wall.
3. Upload one tile photo per surface type (add more slots as needed) and straighten if the shot was taken at an angle.
4. Assign each surface its tile via the dropdown next to its chip.
5. Click **Apply tiles to all surfaces** to render the preview.
6. Enter room/tile dimensions on the right for an independent cost estimate (now priced per m²).

## Deployment

Deployed on **Vercel** as a static site with automatic deployments from GitHub. Since the project is a single `index.html` with no build step, the Framework Preset is set to **Other** — Vercel serves the file directly at the root with no build command or output directory required.

**Note:** the app was initially deployed under the filename `tile-visualizer.html`, which caused a `404: NOT_FOUND` error because Vercel's static build looks for `index.html` at the root. Renaming the file resolved the issue and the site now deploys cleanly on every push.

## Repository

https://github.com/muhammadahmadanees-ai/Tile-visualizer

## License

Proprietary. All rights reserved.
