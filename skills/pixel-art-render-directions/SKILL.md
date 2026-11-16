---
name: pixel-art-render-directions
description: Use when rendering sprite frames from Blender for all directions and animations, exporting sprite sheets, or needing a per-part ID mask for pixel art contour lines.
---

# Rendering the directions

- **Rotate the model, not the camera**: 8 steps of 45° around the vertical axis (camera and light stay fixed so the light is always upper left).
- For every direction, render every animation frame (idle, walk, attack, hit, death; ~10 each) at the final sprite size (64 px tall in the video).
- Render a **second, hidden pass** with a flat unique color per part/material (part-ID mask), same frames, same size. Cleanup uses it to draw contours where parts overlap.
- Name outputs predictably, e.g. `<char>/<anim>/<dir>_<frame>.png` and `<char>/<anim>/<dir>_<frame>_id.png`.
- Output is a raw render: close, but not pixel art yet. Do not ship it; run `pixel-art-cleanup`.

## Scale check

frames = 8 directions × animations × frames each (≈400 per character). Script the loop; never render by hand.
