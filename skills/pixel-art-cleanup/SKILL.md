---
name: pixel-art-cleanup
description: "Use when post-processing raw 3D renders into final pixel art sprites: palette limiting, removing stray pixels, outlines, contour lines between parts, floor shadow."
---

# Cleanup (post-process raw renders)

Apply in this order, as a script over all frames so every frame gets identical treatment:

1. **Palette snap**: map every pixel to the nearest color of the project's fixed palette (26 colors in the video). No new colors outside the palette.
2. **Remove stray pixels**: delete single isolated pixels; they read as dirt.
3. **Part contours**: using the part-ID mask, draw a thin contour line wherever two different parts overlap, so e.g. an arm in front of the body stays readable.
4. **Outline**: 1 px dark outline around the whole sprite.
5. **Floor shadow**: soft shadow under the sprite.

Keep the order: outline after contours, so the contours are not outlined over.

Output must match pixel for pixel across directions and frames (same palette, same rules). If a frame looks wrong, don't hand-edit it; go back to the model (`pixel-art-design-review`).
