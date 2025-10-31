# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Project Overview

This is a WebAR game called "Wall Spider Smash" built as a single-file application using three.js, A-Frame, and AR.js. Players point their phone camera at a Hiro marker to spawn spiders on it, then tap to smash them.

## Architecture

### Single-File Application
The entire application exists in `index.html` with inline styles and JavaScript module. There is no build process, package manager, or external dependencies beyond CDN-loaded libraries (A-Frame, AR.js, three.js).

### Key Components

**AR.js Marker Tracking** (index.html:163-175)
- Uses A-Frame with AR.js for marker-based AR
- Preset "hiro" marker is used (standard AR.js marker)
- `markerFound` event triggers spider spawning (once per session)
- `markerLost` event updates UI hint
- Works on all mobile browsers (iOS Safari and Android Chrome)

**A-Frame Scene Setup** (index.html:163-175)
- Embedded A-Frame scene with webcam as source
- Detection mode: mono_and_matrix with 3x3 matrix code
- VR mode disabled (AR only)
- Camera entity automatically handles AR camera

**Spider Spawning System** (index.html:97-119)
- Spawns 10 spiders on marker detection
- Random offsets within 15cm x 15cm area around marker
- Spiders are parented to marker object (move with marker)
- Spider geometry: 0.12m x 0.16m planes (12cm x 16cm)
- Random Z-rotation (full 360°) and scale variance (0.8-1.3x)

**Raycasting & Tap Detection** (index.html:121-160)
- Listens for both `click` and `touchend` events
- Converts tap coordinates to NDC space
- Raycasts through camera against alive spiders only
- On hit: squash animation (120ms), fade out, remove from marker
- Score increments per smash

**Procedural Spider Texture** (index.html:36-49)
- Canvas-based texture generation (128x128px)
- Black body + head circles, 6 legs drawn as strokes
- CanvasTexture with anisotropy=8 for quality

## Development

### Running Locally
Since this uses camera access, it must be served over HTTPS (or localhost):

```bash
python3 -m http.server 8000
# Then access via https://localhost:8000 or tunnel to device
```

### Testing
- Requires printing or displaying the Hiro marker (https://raw.githubusercontent.com/AR-js-org/AR.js/master/data/images/hiro.png)
- Works on desktop browsers with webcam (for testing)
- Works on all mobile browsers (iOS Safari, Android Chrome)
- Camera permissions must be granted

### Marker Setup
The game uses the standard "Hiro" marker pattern. Users can:
1. Print the marker from: https://raw.githubusercontent.com/AR-js-org/AR.js/master/data/images/hiro.png
2. Display the marker on another screen
3. Point their device camera at the marker to spawn spiders

## State Management

**Global State:**
- `score`: Current smash count
- `spidersSpawned`: Boolean flag preventing multiple spider spawns per marker detection
- `markerObject`: three.js Object3D reference to the marker (parent of all spiders)
- `scene`: A-Frame scene's three.js object3D
- `camera`: A-Frame camera entity
- `raycaster`: three.js Raycaster for tap detection
- `spiders`: Array of all spawned spider meshes

**Spider State:**
- Each mesh has `userData.alive` boolean
- Dead spiders remain in array but filtered from raycasting
- Removed from marker after smash animation completes

## Key Implementation Details

- Spiders are 2D sprites (planes) that lie flat on the marker surface
- Spiders are parented to the marker, so they move/rotate with it naturally
- Random rotation and scale variance for visual variety
- Animation uses requestAnimationFrame for smooth squash effect
- Works without WebXR (AR.js uses computer vision for marker tracking)
- Compatible with iOS Safari (unlike WebXR which isn't supported on iOS)
