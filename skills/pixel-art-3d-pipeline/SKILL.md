---
name: pixel-art-3d-pipeline
description: Use when turning a 3D model (Blender) into isometric pixel art sprites, planning a sprite sheet with multiple directions and animations, or asking how the 3D-to-pixel-art pipeline fits together.
---

# 3D model → isometric pixel art (pipeline)

Source: Mekai Games, "How to Transform a 3D Model Into Pixel Art" (youtube.com/watch?v=Y4uTF3aBTKo). Principle: **the pipeline produces frames; it does not judge them.** Every character is still reviewed frame by frame, and fixes go back to the model.

## Parameters (decide once per project, write in the project's art doc)

| Parameter | Video's value | Notes |
|---|---|---|
| Sprite height | 64 px | drives how much detail the model may have |
| Directions | 8 (rotate model 45° each) | camera never moves |
| Animations | idle, walk, attack, hit, death | ~10 frames each, ~400 frames/character |
| Palette | 26 colors, shared by all characters | project palette replaces this |
| Light | upper left, same for every character | never per-character |

## Order (each step is its own skill)

1. `pixel-art-modeling`: simple shapes, strong silhouette, family traits
2. `pixel-art-camera-lighting`: orthographic camera, 30° tilt, 3-tone toon shader
3. `pixel-art-render-directions`: 8 rotations × all frames at sprite size, plus a part-ID pass
4. `pixel-art-cleanup`: palette snap, stray pixels, part contours, outline, floor shadow
5. `pixel-art-design-review`: frame-by-frame check; failures go back to step 1

Steps 2–3 are set up once per project and reused by every character. Only step 1 is per-character work.

## Rules

- Do not change camera, light or palette per character; consistency across the series is the product.
- Do not drive a live Blender session from several agents at once; it is a serialized resource, one writer at a time.
- Do not fix in 2D what is wrong in the model. Go back to the model.
