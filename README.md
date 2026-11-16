# pixel-art-skills

Six skills for turning a 3D model (Blender) into isometric pixel art sprites:
8 directions, several animations, one consistent style across a whole series.

Based on the pipeline from Mekai Games, "How to Transform a 3D Model Into Pixel Art"
(https://www.youtube.com/watch?v=Y4uTF3aBTKo).

## Install

With [`skills`](https://github.com/vercel-labs/skills), on any machine:

```bash
# all six skills, globally (every project)
npx skills add fabriciof807/pixel-art-skills -g -y

# only one skill
npx skills add fabriciof807/pixel-art-skills -g -y --skill pixel-art-cleanup

# list what the repo contains
npx skills add fabriciof807/pixel-art-skills --list
```

Drop `-g` to install into the current project only. Or copy the `skills/` folders into
`~/.claude/skills/` by hand.

## Skills

### `pixel-art-3d-pipeline`: overview

Entry point. Explains the whole pipeline, the order of the other five skills, and the
project parameters that are decided once and written into the project's art document:
sprite height (64 px in the video), directions (8), animations (idle, walk, attack, hit,
death), palette (26 colors in the video), and light direction (upper left).

Its core principle: **the pipeline produces frames; it does not judge them.** Every
character is still reviewed frame by frame, and fixes go back to the model.

### `pixel-art-modeling`: step 1, per character

Builds the character from simple shapes (boxes, cylinders, spheres). At 64 px tall,
detail becomes noise, so the **silhouette** is what matters: one strong, instantly
readable shape per character, plus the family traits shared by the whole series.

Keeps parts as separate objects and materials, because the part-ID pass and the contour
lines depend on them.

### `pixel-art-camera-lighting`: step 2, once per project

Sets up the camera and light that every character shares:

- Orthographic camera, no perspective, tilted down 30°. Diagonals then step 2 px across
  per 1 px down, the classic 2:1 isometric grid. The skill notes that the exact 2:1 slope
  is about 26.57°; if diagonals still look off at sprite size, test that value.
- Toon shader with three tones per material (shadow, mid, highlight). Shadows lean cool,
  highlights lean warm.
- One sun from the upper left, for every character. No soft light, ambient occlusion,
  bloom, or anti-aliasing on the final render.

### `pixel-art-render-directions`: step 3, per character

Rotates the **model**, not the camera: eight steps of 45° around the vertical axis, so the
light stays fixed. For each direction it renders every animation frame at the final sprite
size, plus a second hidden pass with a flat unique color per part (the part-ID mask).

The output is a raw render: close, but not pixel art yet. It is not shipped as-is. The
skill suggests a predictable naming scheme (`<char>/<anim>/<dir>_<frame>.png` and
`..._id.png`) and a script for the loop, since one character is around 400 frames.

### `pixel-art-cleanup`: step 4, per character

A script applied to every frame with identical rules, in this order:

1. Palette snap: every pixel goes to the nearest color of the project's fixed palette.
2. Remove stray single pixels, which read as dirt.
3. Part contours: thin lines where two parts overlap, using the part-ID mask.
4. One-pixel dark outline around the whole sprite (after the contours, so they are not
   outlined over).
5. Soft floor shadow.

Frames must match pixel for pixel across directions and animations. If a frame looks
wrong, the skill says not to hand-edit it.

### `pixel-art-design-review`: step 5, per character

Frame-by-frame check of every direction of every animation. When something reads wrong,
fix the model and re-render. Lessons from the video:

| Problem | Fix |
|---|---|
| Enemies in the heroes' color read as heroes | Dark base for enemies; hero color only as trim |
| A ring around a helmet reads as a target | Avoid ring shapes on the head |
| A weapon pointing at the camera vanishes in the front view | Aim weapons slightly to the side |
| A design too similar to existing characters | Discard it and start over |

Also check: silhouette readable at sprite size, family traits present, palette and light
unchanged from the rest of the series.

## Recommended workflow

The work splits into two parts: a **project setup** done once, and a **per-character loop**
repeated for every character in the series.

### 1. Project setup (once)

1. Read `pixel-art-3d-pipeline` and write the project parameters into the art document:
   sprite height, directions, animations, palette, light direction, and the family traits.
2. Apply `pixel-art-camera-lighting`: set the orthographic camera and the three-tone
   shader, and fix them. Every character will be rendered with this same setup.

### 2. Per character (repeat for each one)

1. **Model** with `pixel-art-modeling`. Check the silhouette: fill the model with a flat
   color and look at it at sprite size.
2. **Render** with `pixel-art-render-directions`: 8 directions times every animation frame,
   plus the part-ID pass.
3. **Clean up** with `pixel-art-cleanup`, as one script over all frames.
4. **Review** with `pixel-art-design-review`, frame by frame.
5. If the review fails, **go back to step 1** (the model). Do not fix the sprite in 2D.
   Re-render and clean up again.

### Rules that hold across the series

- Do not change camera, light, or palette per character. Consistency is the product.
- Do not fix in 2D what is wrong in the model.
- Do not hand-render frames; script the loop.
- Blender is a serialized resource in practice: one writer at a time, even with several
  agents.

## Layout

```
pixel-art-skills/
├── README.md
├── LICENSE
└── skills/
    ├── pixel-art-3d-pipeline/SKILL.md
    ├── pixel-art-modeling/SKILL.md
    ├── pixel-art-camera-lighting/SKILL.md
    ├── pixel-art-render-directions/SKILL.md
    ├── pixel-art-cleanup/SKILL.md
    └── pixel-art-design-review/SKILL.md
```

## Notes

- The skills are generic: they carry the video's values (64 px sprites, 26-color palette,
  30° camera) as defaults. A project should write its own palette and family traits in its
  art direction document.
- The skills assume Blender for rendering and a script for the post-process.
