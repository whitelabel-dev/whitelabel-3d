# viewer/

The v0.2 Three.js viewer for whitelabel-3d. Single-file static HTML — no build step.

## Run locally

```sh
# Option 1: just open in a browser
open viewer/index.html

# Option 2: serve from a local HTTP server (recommended — file:// has CORS quirks for some GLBs)
cd viewer && python3 -m http.server 8000
# then visit http://localhost:8000
```

## Deploy to 3d.whitelabel.dev

```sh
# Once a Vercel project is wired up for this repo:
cd /Users/a33/Documents/whitelabel-3d
vercel --prod   # serves viewer/index.html as the root if configured
```

Alternative: drop `viewer/` into any static host (Cloudflare Pages, Netlify, GitHub Pages).

## What it does today (v0.2)

- Three.js scene with PBR lighting + room environment
- Drag-and-drop `.glb` / `.gltf` files anywhere on the page
- File picker as fallback
- Orbit / pan / zoom (matches Blender / SketchUp muscle memory)
- Auto-frame on load (camera positions itself to fit the bounds)
- Model stats: vertex count, triangle count, materials, bounds
- Wireframe toggle (useful for inspecting scan topology)
- Reset camera

## What it doesn't do yet (v0.3+)

- Object placement metadata layer (the *items* table from `schema/migrations/0001_initial.sql` won't render until v0.3)
- Sensor overlay (HVAC vents pulsing, leak detector flashing) — v0.5
- Spatial query API (`world.find("printer near conference room")`) — v0.6
- Voice resolve (`whitelabel-flow` integration) — v0.7
- AR (`whitelabel-goggles` integration) — v0.9

This is the substrate. Render-only today. Reactive surfaces layer on next.

## How to test with your iPhone scan

1. Scan a room with [3D Scanner App](https://apps.apple.com/us/app/3d-scanner-app/id1419913995)
2. Export as **glb**, AirDrop to Mac
3. Open `viewer/index.html` in Chrome / Safari
4. Drag the `.glb` onto the page → scene loads, camera auto-frames

If the model looks dark, that's lighting baked into the scan. Toggle wireframe to see topology. v0.3 will add lighting correction + scan cleanup helpers.
