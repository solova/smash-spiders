# Wall Spider Smash

A WebAR game where you smash spiders on a marker using your phone's camera. Built with AR.js for maximum mobile browser compatibility.

## Play Now

**[Play the game](https://solova.github.io/smash-spiders/)** (works on all mobile browsers!)

## Requirements

- Mobile device or computer with a camera
- Camera permissions
- **Hiro marker** (print or display on screen)

## How to Play

1. **Get the Hiro marker**: Download and print or display this image on another screen:
   - [Download Hiro Marker](https://raw.githubusercontent.com/AR-js-org/AR.js/master/data/images/hiro.png)

2. **Open the game**: Visit the game link on your device

3. **Grant camera permission** when prompted

4. **Point your camera at the Hiro marker**: Spiders will spawn on the marker

5. **Tap spiders to smash them** and increase your score!

## Browser Support

Works on **all modern mobile browsers**:
- ✅ iOS Safari (iPhone/iPad)
- ✅ Android Chrome
- ✅ Desktop browsers with webcam (for testing)

Unlike WebXR-based solutions, this game uses AR.js marker tracking which has universal browser support.

## Technical Details

- Built with **three.js**, **A-Frame**, and **AR.js**
- Marker-based AR tracking (no WebXR required)
- Single-file application (no build process)
- Procedurally generated spider textures

## Local Development

Since camera access requires HTTPS, serve the file securely:

```bash
python3 -m http.server 8000
# Access via https://localhost:8000 or use ngrok/similar for mobile testing
```

For more details, see [CLAUDE.md](CLAUDE.md).
