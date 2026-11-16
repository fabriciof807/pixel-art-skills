---
name: pixel-art-modeling
description: Use when modeling a character or object in Blender that will be rendered as small pixel art sprites, or when a sprite reads as noise or has an unclear silhouette.
---

# Modeling for pixel art

At 64 px tall, detail becomes noise. **Silhouette is what matters.**

- Build from boxes, cylinders, spheres. No sculpting, no fine detail.
- Give each character one strong, instantly readable shape (treads, drill, six legs, giant cannon).
- Keep the family traits shared by the whole series (the video: big round eye, pink antenna light, brass exhaust pipe). Take the project's list from its art direction doc.
- Keep parts as separate objects/materials: the part-ID pass in `pixel-art-render-directions` and the contours in `pixel-art-cleanup` depend on it.

## Check before leaving this step

- Fill the model with a flat color and view it at sprite size: is it still identifiable?
- Is it distinguishable from every existing character's silhouette? If not, redesign (see `pixel-art-design-review`).
