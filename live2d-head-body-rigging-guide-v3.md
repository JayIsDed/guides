# Live2D Cubism — Head & Body Rigging Guide

**Prereqs:** Eyes and mouth are rigged. If not, start with the [Eye Guide](live2d-eye-rigging-guide-v3.md) and [Mouth Guide](live2d-mouth-rigging-guide-v3.md) first — you'll need those skills for everything here.

**Jump to:** [Eyebrows](#eyebrows) | [Head Rotation](#head-rotation) | [Body Sway](#body-sway) | [Breathing](#breathing) | [Common Mistakes](#common-mistakes) | [Quick Reference](#quick-reference) | [Next Steps](#next-steps)

---

## Eyebrows

Eyebrows are the easiest thing in this guide and one of the most expressive. You already know how keyforms work from rigging eyes — this is the exact same process, just applied to a different layer.

### Raise and Lower

Each eyebrow gets its own up/down parameter: **`ParamBrowLY`** for the left brow and **`ParamBrowRY`** for the right. The range is -1 to +1, with 0 as neutral. At -1, drag the brow mesh downward and squeeze it slightly inward toward the nose — that furrowed, focused look. At +1, pull it upward, maybe even exaggerate it a little. The eyebrow doesn't just translate up, it arcs — the inner end lifts more than the outer end when someone is genuinely surprised.

The reason these are separate parameters instead of one shared one is independence. If both brows were locked together, you'd lose half your expression range. Independent brows are what let your model do "the eyebrow" — one up, one down, instant personality.

### Eyebrow Shape

Beyond just up/down, you can control the actual shape of each brow with **`ParamBrowLForm`** and **`ParamBrowRForm`**. At -1, angle the brow into a sad shape — inner end tilted up, outer end drooping down. At +1, flip it for an angry angle — inner end down, outer end up. Neutral at 0. This is optional but it adds a second emotional axis on top of the raise/lower, so "raised + sad angle" reads as worried while "raised + angry angle" reads as intense surprise.

### Expression Combos

The real power is in combinations. Since each brow moves independently, here's what the pairings look like in practice:

| Left Brow | Right Brow | Expression |
|:---:|:---:|:---|
| +1 (raised) | +1 (raised) | Surprised, shocked |
| -1 (lowered) | -1 (lowered) | Angry, focused, determined |
| +1 (raised) | -1 (lowered) | Skeptical, "really?" |
| -1 (lowered) | +1 (raised) | Confused, uncertain |
| 0 (neutral) | +1 (raised) | Curious, mildly interested |
| -1 (lowered) | 0 (neutral) | Suspicious, side-eye energy |

If you also rig `ParamBrowLForm`/`ParamBrowRForm`, multiply those combos by three shape states each. That's a lot of expression range from four simple parameters.

---

## Head Rotation

This is it. The mountain. Everything you've done so far — [eyes](live2d-eye-rigging-guide-v3.md), [mouth](live2d-mouth-rigging-guide-v3.md), eyebrows — was preparation for this moment. Head rotation is the single biggest upgrade you can make to a Live2D model. Without it, the model is a flat picture that blinks and talks. With it, the model has depth, presence, and feels three-dimensional. This is also the hardest thing you'll rig, because it touches *every single layer* on your model. But everything after it — body sway, breathing — is significantly easier. Climb this one and the rest is downhill.

### The Three Axes

Head rotation uses three parameters, one for each axis of movement:

**`ParamAngleX`** is the left/right turn — shaking your head "no." This is usually the first axis people rig because it's the most visually dramatic. The range is -30 to +30, where -30 is turned fully left and +30 is turned fully right.

**`ParamAngleY`** is the up/down nod — nodding "yes." At -30, the head tilts down and you see more forehead, less chin. At +30, the head tilts up and the chin becomes more prominent. This one is subtler than X but critical for natural tracking.

**`ParamAngleZ`** is the side-to-side tilt — the curious dog head tilt, ear toward shoulder. At -30 the head leans left, at +30 it leans right. This is the easiest of the three because the face doesn't change perspective, it just rotates.

```
    ParamAngleX              ParamAngleY              ParamAngleZ
    (turn left/right)        (nod up/down)            (tilt/lean)

       -30    +30              +30                    -30    +30
        |      |                |                      |      |
       .---.  .---.           .---.                  .---.  .---.
      /  o  \/  o  \         | o o |                / o o \/ o o \
      | --- || --- |         | --- |               |  ---  ||  ---  |
       \   /  \   /           \   /                 \ /  \  /\ /  \
        '-'    '-'             '-'                   '-'  '-'  '-'
     looking  looking        looking                leaning  leaning
      left    right            up                    left    right
```

### Why It's Hard

Here's the thing that makes head rotation the hardest part of any Live2D rig: it's not one deformation, it's dozens. Every layer on your model needs keyforms for each axis. The face outline, both eyes, both eyebrows, the nose, the mouth, the ears, front hair, back hair, side hair — all of them need to move convincingly when the head turns. And they don't all move the same amount or in the same direction.

When you turn your head to the right, the right side of your face compresses and the left side stretches. Your right ear gets smaller (it's further away) and your left ear gets bigger (it's closer). Your nose shifts to the right because it's on the near side. Your hair on the left sweeps out more while hair on the right tucks in. Every layer has its own behavior, and getting all of them to play together is what takes the time.

### Walking Through It

Start with **ParamAngleX** — the left/right turn. Create the parameter with a range of -30 to +30 and add keyforms at -30, 0, and +30. Some riggers add intermediate keyforms at -10 and +10 for smoother blending, but start with three and add more if the interpolation looks rough.

(a) Begin with the **face outline**. This is the anchor everything else follows. At +30 (turning right), the left side of the jaw should push out slightly and the right side should compress inward. The chin shifts right. The overall silhouette changes to suggest a three-quarter view. Don't be timid with this — if the perspective shift is too subtle, the whole head turn will look flat.

(b) Move to the **eyes**. Both eyes shift in the direction of the turn, but not equally. The eye on the near side (right eye when turning right) moves less because it's closer to center. The far eye moves more. Both eyes also change shape slightly — the near eye may widen a touch while the far eye narrows. This is subtle but it sells the 3D illusion.

(c) **Eyebrows** follow the eyes. They shift the same direction and by roughly the same amount as the eyes they sit above. Easy.

(d) The **nose** moves the most of any facial feature because it sticks out. When turning right, the nose shifts noticeably to the right. It might also change shape slightly — one nostril becoming more visible than the other.

(e) The **mouth** follows the nose. It shifts in the same direction but slightly less. The mouth also changes shape subtly at extreme angles — the near corner becomes more defined while the far corner fades.

(f) **Ears** are pure perspective. The near ear gets bigger and possibly shifts backward. The far ear gets smaller, maybe partially hidden behind the head. If your character has visible ears, this is an important sell for depth.

(g) **Front hair** moves with the head, roughly matching the face outline's movement. But **back hair and side hair** are where parallax comes in. Back hair is further from the camera than the face, so it moves *less* than the face during a turn. This layer separation is what creates the sense of depth. If every layer moves the same amount, the head looks like a flat cardboard cutout rotating. The difference in movement distances is what makes it feel three-dimensional.

(h) After you finish all layers for +30, do the same for -30. This should be roughly a mirror of what you did for +30, which helps — you're not designing from scratch, just flipping your perspective logic.

Once ParamAngleX is done, move on to **ParamAngleY** (up/down nod). Same process, every layer, but now the shifts are vertical. When looking up (+30), you see more of the underside of the chin and jaw. The eyes shift upward, forehead compresses, hair lifts. When looking down (-30), forehead expands, chin tucks, eyes shift down. The nose is particularly important here — looking up, you see more of the nostrils; looking down, the nose tip drops.

Finally, **ParamAngleZ** (tilt). This is the easiest axis because there's no perspective change — everything just rotates around the center of the head. Select all face layers, rotate them together by the same angle, and you're most of the way there. Hair might lag slightly behind the rotation (physics will handle this later), but the face itself is a straightforward rotation.

### After All Three Axes

Once X, Y, and Z are all rigged individually, you need to check the combinations. This is the same concept as [checking eye corners](live2d-eye-rigging-guide-v3.md#step-3-check-the-corners) — set two parameters to their extremes at the same time and see if the blending looks right. Check X+Y (turned and nodding), X+Z (turned and tilting), Y+Z (nodding and tilting), and the triple combo. Cubism blends keyforms automatically, but the auto-blend sometimes produces warped results at the corners. When it does, you adjust the mesh at that specific combination.

### Tips for Natural Rotation

The single most useful thing you can do while rigging head rotation is to sit in front of a mirror and slowly turn your own head. Watch what actually moves, how much, and in what order. Your brain has a lifetime of experience seeing faces turn — use that intuition. When something looks "off" in Cubism, it's usually because a layer is moving too much, too little, or in a slightly wrong direction relative to the others.

The **near side gets bigger, far side gets smaller** — this is the core perspective rule. Burn it in. The near cheek puffs out, the far cheek recedes. The near ear grows, the far ear shrinks. The nose shifts toward the near side. Every layer should respect this principle.

**Parallax on hair** is the depth trick. Front layers move more, back layers move less. If your model has three layers of hair (bangs, sides, back), they should each move slightly different amounts during a turn. The bangs move almost as much as the face. The back hair moves noticeably less. This offset is what makes the head feel like a 3D object instead of a sticker.

**Don't move everything the same distance.** The nose moves more than the ears. The eyes move less than the face outline. Features at different depths from the camera should travel different distances. This is the same parallax principle as the hair, applied to facial features.

---

## Body Sway

After head rotation, body sway is a relief. Two parameters, smaller range, fewer layers to worry about.

**`ParamBodyAngleX`** handles left/right sway and **`ParamBodyAngleZ`** handles side-to-side lean. There's also `ParamBodyAngleY` for forward/back if you want it, but most VTuber models skip it — it's rarely triggered by face tracking.

The critical difference from head rotation: the range is **-10 to +10**, not -30 to +30. Body movement should be subtle. Your torso doesn't swing 30 degrees when you're sitting at a desk talking to a stream — it shifts a few degrees at most. If you use the same range as head rotation, the model will look like it's about to fall out of its chair.

At the keyforms, think about what actually moves when your upper body leans to one side. The shoulders rotate — one goes up, the other goes down. The torso shifts with perspective, similar to the face during head turns but gentler. The entire head rides on top of the body, so it shifts too, and hair follows with it (physics will add the delay/bounce on hair later). Clothes shift and fold. If your model has accessories — necklaces, ties, scarves — they respond to body movement as well, usually through physics rather than keyforms.

When rigging `ParamBodyAngleX` at +10 (leaning right), shift the shoulders so the right shoulder drops slightly and the left rises. Move the torso to suggest a rightward lean. The head shifts right to stay on top of the body. Reverse everything for -10. `ParamBodyAngleZ` is similar but for a more pronounced tilt — think leaning to look at something versus a casual sway.

Keep it restrained. The whole point of body sway is background life — it fills the space between head movements with gentle, almost unconscious shifting. If you can consciously notice the body moving during a stream, it's probably too much.

---

## Breathing

The easiest parameter in all of Live2D rigging. One parameter, one axis, minimal deformation, and it adds constant subtle life to any model. If head rotation is the mountain, breathing is the park bench at the bottom where you sit and feel good about yourself.

**`ParamBreath`** goes from 0 (exhaled, default) to 1 (inhaled). That's it — no negative values, no complex blending. At keyform 1, shift the shoulders upward by **2-3 pixels**. That's all. Maybe expand the chest area outward by a pixel or two if your model has a visible torso. The head rides up slightly with the shoulders — maybe 1-2 pixels. Hair follows the head.

The reason this parameter matters despite being so simple: it should always be running. Every other parameter on your model is reactive — it responds to your face, your voice, your movement. Breathing is the one that runs on its own, a constant slow sine wave that keeps the model alive even when you're silently reading chat. The difference between a model with breathing and one without is the difference between "alive" and "screenshot."

**VTube Studio** has a built-in auto-breath option that drives `ParamBreath` with a sine wave automatically. Turn it on and forget about it. You can also set up breathing through Cubism's physics system for a slightly more organic feel — the physics version introduces tiny random variations that make the breathing feel less mechanical. Either approach works. The auto-breath in VTube Studio is the path of least resistance.

The one thing to avoid: making the breath movement too large. If the shoulders jump 8-10 pixels, it looks like the character is hyperventilating. 2-3 pixels on the shoulders is genuinely all you need. Subtlety is the whole point.

---

## Common Mistakes

| Problem | What's Happening | Fix |
|:---|:---|:---|
| Head turn looks flat, like a cardboard cutout rotating | Not enough perspective difference between -30 and +30 — all layers moving the same amount | Exaggerate the near-side-bigger/far-side-smaller principle. Vary movement distance per layer for [parallax](#tips-for-natural-rotation). |
| Face warps or collapses during head turn | Mesh is too sparse on face layers — not enough vertices for Cubism to deform smoothly | Add more mesh points on the face outline and features before rigging keyforms. |
| Hair doesn't follow head rotation at all | Hair layers aren't rigged to `ParamAngleX/Y/Z` | Rig hair on the same head rotation parameters. Back hair should move less than front hair ([parallax](#tips-for-natural-rotation)). |
| Body sway looks like the model is falling over | Range is too large or deformation is too aggressive | Use -10 to +10, not -30 to +30. [Body sway](#body-sway) is subtle by nature. |
| Breathing looks like hiccups or gasping | Shoulder movement is way too large | Shoulders move **2-3 pixels max**. If you can obviously see it, it's too much. |
| Eyebrows move with head but feel frozen/dead | Brows are rigged to head rotation but not to their own `ParamBrowLY/RY` | Rig brows on **both** head angle parameters AND their own raise/lower parameters. |
| One side of the face looks great, the other looks wrong | Asymmetric deformation — didn't properly mirror the keyform work | Check both -30 and +30 for each axis. They should be roughly symmetrical. Use a mirror for reference. |
| Head turn corners look distorted | X+Y or X+Z combined values produce warped blending | [Check all corner combinations](#after-all-three-axes) and adjust mesh at those specific paired values. Same concept as [eye corners](live2d-eye-rigging-guide-v3.md#step-3-check-the-corners). |
| Model feels alive but stiff during idle | No breathing parameter running | Rig [ParamBreath](#breathing) and enable auto-breath in VTube Studio. Always-on. |

---

## Quick Reference

| I Need To... | Parameter | Range | More Info |
|:---|:---|:---|:---:|
| Raise/lower left eyebrow | `ParamBrowLY` | -1 to +1 | [Eyebrows](#eyebrows) |
| Raise/lower right eyebrow | `ParamBrowRY` | -1 to +1 | [Eyebrows](#eyebrows) |
| Change left eyebrow shape | `ParamBrowLForm` | -1 to +1 | [Eyebrow Shape](#eyebrow-shape) |
| Change right eyebrow shape | `ParamBrowRForm` | -1 to +1 | [Eyebrow Shape](#eyebrow-shape) |
| Turn head left/right | `ParamAngleX` | -30 to +30 | [Head Rotation](#head-rotation) |
| Nod head up/down | `ParamAngleY` | -30 to +30 | [Head Rotation](#head-rotation) |
| Tilt head side-to-side | `ParamAngleZ` | -30 to +30 | [Head Rotation](#head-rotation) |
| Sway body left/right | `ParamBodyAngleX` | -10 to +10 | [Body Sway](#body-sway) |
| Lean body side-to-side | `ParamBodyAngleZ` | -10 to +10 | [Body Sway](#body-sway) |
| Add breathing | `ParamBreath` | 0 to 1 | [Breathing](#breathing) |
| Rig eyes | See [Eye Guide](live2d-eye-rigging-guide-v3.md) | --- | Prereq |
| Rig mouth | See [Mouth Guide](live2d-mouth-rigging-guide-v3.md) | --- | Prereq |

---

## Next Steps

With head rotation, body sway, and breathing done, your model has real presence. It turns, it sways, it breathes — it feels alive in three dimensions instead of a flat drawing that blinks.

The next thing to tackle is **physics and hair**. Physics is what makes hair swing when the head turns, earrings bounce, ribbons trail behind movement. It takes the static keyforms you've set up across this guide and the [eye guide](live2d-eye-rigging-guide-v3.md) and adds reactive, dynamic motion on top of them. If you've already set up [googly eye physics](live2d-eye-rigging-guide-v3.md#step-5-googly-eyes), you know the basics — hair physics uses the same pendulum system, just applied to more layers with longer swing times.

The physics/hair guide is next: `live2d-physics-hair-guide-v3.md` (coming soon).

---

*You climbed the mountain. Everything from here has a view.*
