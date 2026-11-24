# Render checks

## Screenshot with the real GPU
The SwiftShader fallback renders some models wrong. Use Vulkan:

```bash
timeout 280 google-chrome --headless=new --no-sandbox --enable-gpu --use-angle=vulkan --ignore-gpu-blocklist \
  --hide-scrollbars --window-size=1100,800 --virtual-time-budget=10000 \
  --screenshot="$OUT/shot.png" "http://127.0.0.1:8768/index.html?p=1&view=top"
```

Serve the deploy copy with `python3 -m http.server 8768 --bind 127.0.0.1` and stop it afterwards (`fuser -k 8768/tcp`). Use a new file name per screenshot. Add `?v=<timestamp>` to script and model URLs after each edit so the browser does not show a cached copy.

## Debug views (query string)
- `p=1`: force the chase camera.
- `view=top` / `view=under`: straight down / straight up at the robot; check rails, holes, bar orientation and that sensors face the mat.
- `view=board`: close view of the main board; check colour, print and connectors.
- `flat=1`: hide the 3D hero to screenshot the sections below it.

## Things that went wrong before
- Camera shaking: follow a smoothed heading (`1 - exp(-dt * k)`), not the raw steering angle.
- Labels shaking or pointing at the wrong part: anchor them to measured part positions and ease their screen position.
- Mobile: hero text slid under the nav; tags left the screen; cards spun end-on. Fade the hero text on scroll, clamp tags to the viewport, sway cards around the front view.
- A colour or part looked fine in code but the render showed the opposite (white rim, missing PCB, upside-down bar). Only a render proves it.
