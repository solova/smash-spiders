# Wall Spider Smash

A WebAR game where you smash spiders on real-world surfaces using your phone's AR capabilities.

## Play Now

**[Play the game](https://solova.github.io/smash-spiders/)** (requires AR-capable mobile device)

## Requirements

- AR-capable mobile device (Android Chrome or iOS Safari with WebXR support)
- HTTPS connection (required for WebXR)

## How to Play

1. Open the game link on your AR-capable mobile device
2. Grant camera permissions when prompted
3. Point your camera at a surface (wall, floor, or table)
4. Wait for spiders to spawn on the detected surface
5. Tap spiders to smash them and increase your score

## Technical Details

- Built with three.js and WebXR Device API
- Single-file application (no build process required)
- Uses WebXR hit-test for surface detection
- Procedurally generated spider textures

## Local Development

Since this uses WebXR AR features, it must be served over HTTPS:

```bash
python3 -m http.server 8000
# Then access via https://localhost:8000 or tunnel to device
```

For more details, see [CLAUDE.md](CLAUDE.md).
