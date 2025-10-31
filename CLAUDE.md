# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Project Overview

This is a WebAR game called "Wall Spider Smash" built as a single-file application using three.js and WebXR. Players use their phone's AR capabilities to detect surfaces (walls, floors, tables), spawn spiders on those surfaces, and tap to smash them.

## Architecture

### Single-File Application
The entire application exists in `index.html` with inline styles and JavaScript module. There is no build process, package manager, or external dependencies beyond CDN-loaded three.js.

### Key Components

**WebXR Session Management** (index.html:138-168)
- Uses WebXR Device API for immersive-ar sessions
- Requires "hit-test" feature for surface detection
- Optional "dom-overlay" feature for UI overlay
- Reference space type: "local"

**Hit Testing Flow** (index.html:170-189)
- Hit test source is created from viewer space
- `getHitTestResults()` detects surfaces in real-world
- First detected surface triggers spider spawning (once per session)
- Hit pose matrix provides surface position and orientation

**Spider Spawning System** (index.html:69-98)
- Takes a 4x4 transformation matrix from hit test result
- Extracts plane position and quaternion from matrix
- Spawns 7 spiders with random offsets within 0.6m x 0.6m patch
- Orients spiders to "cling" to detected surface using plane's rotation
- Spider geometry: 0.12m x 0.16m planes (12cm x 16cm)

**Raycasting & Tap Detection** (index.html:100-131)
- Converts screen tap coordinates to NDC space
- Raycasts through camera against alive spiders only
- On hit: squash animation (120ms), fade out, remove from scene
- Score increments per smash

**Procedural Spider Texture** (index.html:48-62)
- Canvas-based texture generation (128x128px)
- Black body + head circles, 6 legs drawn as strokes
- CanvasTexture with anisotropy=8 for quality

## Development

### Running Locally
Since this uses WebXR AR features, it must be:
1. Served over HTTPS (or localhost)
2. Accessed from an AR-capable device (Android Chrome or iOS Safari with WebXR support)

Simple local server:
```bash
python3 -m http.server 8000
# Then access via https://localhost:8000 or tunnel to device
```

### Testing
- Desktop browsers won't show AR functionality (WebXR requires AR-capable hardware)
- Use Chrome DevTools WebXR emulation for basic testing
- Real device testing required for hit-test and surface detection

## State Management

**Global State:**
- `score`: Current smash count
- `placed`: Boolean flag preventing multiple spider spawns per session
- `hitSource`: XRHitTestSource for surface detection
- `xrRefSpace`: XRReferenceSpace for pose tracking
- `spiders`: Array of all spawned spider meshes

**Spider State:**
- Each mesh has `userData.alive` boolean
- Dead spiders remain in array but filtered from raycasting
- Removed from scene after smash animation completes

## Key Implementation Details

- The game doesn't distinguish between walls/floors/tables - any detected surface works
- Spiders are 2D sprites (planes) oriented to face outward from the surface normal
- The plane quaternion from hit test defines surface orientation; spiders use this to "stick" correctly
- Random Z-rotation (±0.3 rad) and scale variance (0.9-1.3x) for visual variety
- Animation loop uses `renderer.setAnimationLoop()` for XR frame synchronization
