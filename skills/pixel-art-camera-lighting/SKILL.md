---
name: pixel-art-camera-lighting
description: Use when setting up the Blender camera and lights for pixel art renders, or when rendered diagonals look jagged, shading has too many colors or soft gradients, or a new character's lighting differs from the rest of the series.
---

# Camera and lighting for pixel art

## Camera

- **Orthographic**, no perspective.
- Tilted down **30°** (Blender: elevation 30° above the horizon = `rotation_euler.x` of 60°, `z` set for the base view).
- Why: diagonals on the floor should step 2 px across per 1 px down (classic 2:1 isometric), the grid pixel artists draw by hand. A wrong angle makes every diagonal jagged.
- Caveat: an exact 2:1 slope is arctan(0.5) ≈ 26.57°, not 30°. The video uses 30°; if diagonals still look off, test 26.57° and keep whichever stays clean at sprite size.
- Fix the camera once and reuse it for every character.

## Lighting: toon, three tones

- Replace realistic shading with a toon shader: **3 tones per material** (shadow, mid, highlight). Realistic shading creates hundreds of colors and soft gradients, which pixel art cannot use.
- Shadows lean cool, highlights lean warm.
- One sun, always from the **upper left**, for every character.
- No soft light, no ambient occlusion, no bloom, no anti-aliasing on the final render.
