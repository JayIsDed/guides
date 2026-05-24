# Live2D Reference Videos

Video tutorials that show the actual UI — where to click, what panels do, how to navigate Cubism.

## UI Orientation

- [Beginner Intro to Live2D Rigging - GeekCon DX 2025](https://www.youtube.com/watch?v=yeUOnzb7Y4Y) — covers UI customization, ArtMeshes, deformers, parameters. Start here if the interface feels foreign.
- [Live2D Cubism Tutorial - Basic Tools & Navigation](https://www.youtube.com/watch?v=3GNCJb3aylk) — basic tools walkthrough for beginners.

## Full Rigging Series (Start to Finish)

- [EASY beginner VTuber rig — Full tutorial series (playlist)](https://www.youtube.com/playlist?list=PLVr9R5AakwuT4wgxFr5mcv0As_OzR_WCB) — step-by-step from zero, walks through the interface visually.
- [How to Make a Half-body VTuber Model — Full tutorial series (playlist)](https://www.youtube.com/playlist?list=PLlkGCGRq_RedaKObKcybyzK8VsqH9W65X) — complete half-body model creation.

## Mouth Rigging

- [Easy mouth rigging + multiple forms — step by step](https://www.youtube.com/watch?v=2H3xgBoypqA) — shows the clicks, covers vowels and forms.
- [VTuber Model Mouth Rigging Tutorial (vowels/aeiou, frown, smile)](https://www.youtube.com/watch?v=tJdS1G5ybkE) — Kira Omori, well-known rigger, solid walkthrough.
- [Live2D Tutorial: Mouth Rigging — Full Process](https://www.youtube.com/watch?v=-9Vj6efM9BU) — full mouth rigging process.
- [HOW TO: easy mouth rigging — Quick step-by-step guide (Ep5)](https://www.youtube.com/watch?v=EjcujhDaB6U) — quick and focused.
- [Live2D Full Guide Part 8/8 — Basic Mouth Rigging & Stream Setup](https://www.youtube.com/watch?v=R1jdJ25IQXA) — mouth rigging plus VTube Studio setup.

## Head Angles

- [Step-by-Step Live2D Rigging Head Angles with High Range of Movement](https://www.youtube.com/watch?v=mDcs3WjUh80) — the hardest part of rigging, broken down step by step.

## Official Documentation

- [Live2D Cubism Editor Tutorials](https://docs.live2d.com/en/cubism-editor-tutorials/top/) — official tutorial series with screenshots.
- [Live2D Cubism Editor Manual](https://docs.live2d.com/en/cubism-editor-manual/top/) — full reference manual.
- [Paste Form Special (copy/mirror keyforms)](https://docs.live2d.com/en/cubism-editor-manual/paste-form-special/) — how to copy and flip keyforms left/right.
- [Keyforms (X, Y Movements)](https://docs.live2d.com/en/cubism-editor-manual/keyform-xydirection/) — combined keyform grids for multi-parameter rigging.

## Quick Troubleshooting Reference

| Problem | Likely Cause | Video/Doc |
|---------|-------------|-----------|
| Mouth doesn't move | Missing keyforms or wrong tool (Arrow vs Deformation) | Mouth rigging videos above |
| Vowels don't work when mouth is open | Vowel keyforms set at OpenY=0, need combined keyforms at OpenY=1 | [Keyforms doc](https://docs.live2d.com/en/cubism-editor-manual/keyform-xydirection/) |
| Mouth distorts when head turns | No combined keyforms for AngleX + mouth params | [Head angles video](https://www.youtube.com/watch?v=mDcs3WjUh80) |
| Lip sync doesn't fire in VTube Studio | Audio input or parameter name mismatch in VTS settings | [Stream setup video](https://www.youtube.com/watch?v=R1jdJ25IQXA) |
| Copy deformation to other side | Use Paste Form Special with Flip Horizontally | [Paste Form Special](https://docs.live2d.com/en/cubism-editor-manual/paste-form-special/) |
