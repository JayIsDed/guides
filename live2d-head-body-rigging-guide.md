# Live2D Cubism — Head & Body Rigging Guide

**Starting point:** Eyes and mouth are rigged ([Eye Guide](live2d-eye-rigging-guide-v2.md) | [Mouth Guide](live2d-mouth-rigging-guide.md)). This guide covers head rotation, eyebrows, and body sway — the things that make tracking feel alive.

**Jump to:** [What Exists](#head--body--what-exists) | [Eyebrows](#step-1-eyebrows) | [Head Rotation](#step-2-head-rotation) | [Body Sway](#step-3-body-sway) | [Breathing](#step-4-breathing) | [Common Mistakes](#common-mistakes) | [Quick Reference](#quick-reference)

---

## Head & Body — What Exists

| Motion Type | Parameter Name | What It Controls | Difficulty | Impact |
|:---|:---|:---|:---:|:---|
| **Eyebrow raise/lower** | `ParamBrowLY`, `ParamBrowRY` | Each eyebrow up/down independently | Easy | Huge expression range |
| **Eyebrow form** | `ParamBrowLForm`, `ParamBrowRForm` | Eyebrow shape (angry/sad/surprised) | Easy | Emotion variety |
| **Head turn** | `ParamAngleX` | Head left/right rotation | Medium | **Biggest single impact on model feel** |
| **Head nod** | `ParamAngleY` | Head up/down tilt | Medium | Natural movement |
| **Head tilt** | `ParamAngleZ` | Head side-to-side lean | Medium | Cute/curious expressions |
| **Body rotation** | `ParamBodyAngleX` | Upper body left/right | Medium | Full-body movement |
| **Body lean** | `ParamBodyAngleZ` | Upper body side lean | Medium | Natural sway |
| **Breathing** | `ParamBreath` | Chest/shoulders rise and fall | Easy | Subtle life — always on |

> **Priority order:** Eyebrows > Head X/Y/Z > Breathing > Body. Each one adds more life. Head rotation is the single biggest upgrade after eyes and mouth.

---

## Step 1: Eyebrows

### Eyebrow Raise/Lower

**Parameters:** `ParamBrowLY` (left) and `ParamBrowRY` (right)

| Slider Position | Brow State | What To Move |
|:---:|:---|:---|
| **-1** | Lowered / furrowed | Brow mesh DOWN, slight squeeze inward |
| **0** | Neutral | Default |
| **+1** | Raised / surprised | Brow mesh UP |

> Each eyebrow has its own parameter so they can move independently. Raising one brow = skeptical face. Both up = surprised. Both down = angry.

### Eyebrow Shape (Optional)

**Parameters:** `ParamBrowLForm`, `ParamBrowRForm`

| Slider Position | Shape | Use For |
|:---:|:---|:---|
| **-1** | Sad angle (inner end up, outer end down) | Worried, concerned |
| **0** | Neutral | Default |
| **+1** | Angry angle (inner end down, outer end up) | Angry, intense |

### Expression Combos

| Left Brow | Right Brow | Result |
|:---:|:---:|:---|
| +1 | +1 | Surprised |
| -1 | -1 | Angry / focused |
| +1 | -1 | Skeptical / "really?" |
| -1 | +1 | Confused |
| 0 (neutral) | +1 | Curious |

---

## Step 2: Head Rotation

> **This is the biggest single upgrade to your model.** Without head rotation, the model feels like a flat picture. With it, the model feels 3D.

### The Three Axes

| Parameter | Axis | Motion | Analogy |
|:---|:---|:---|:---|
| **`ParamAngleX`** | Left/Right | Head turns to look left or right | Shaking head "no" |
| **`ParamAngleY`** | Up/Down | Head tilts to look up or down | Nodding "yes" |
| **`ParamAngleZ`** | Tilt/Lean | Head tilts ear-to-shoulder | Curious dog head tilt |

### What Moves (Everything)

This is the hardest part — head rotation affects **every layer on the model**. Each axis needs keyforms on multiple parts.

| Layer | ParamAngleX (left/right) | ParamAngleY (up/down) | ParamAngleZ (tilt) |
|:---|:---|:---|:---|
| **Face outline** | Jaw shifts, cheek perspective | Chin up/down, forehead shows/hides | Whole face rotates |
| **Eyes** | Shift left/right with face | Shift up/down with face | Rotate with face |
| **Eyebrows** | Follow eye shift | Follow eye shift | Rotate with face |
| **Nose** | Shifts to follow perspective | Moves up/down | Rotates with face |
| **Mouth** | Shifts to follow perspective | Moves up/down | Rotates with face |
| **Ears** | One ear closer = bigger, far ear = smaller | Shift up/down | Rotate with face |
| **Hair (front)** | Follows head + slight delay | Follows head | Follows head |
| **Hair (back/sides)** | Opposite shift for parallax | Follows head | Follows head |

### Process for Each Axis

| Step | Action | Notes |
|:---:|:---|:---|
| 1 | Create parameter (e.g., `ParamAngleX`) range -30 to +30 | Standard range for head params |
| 2 | Add keyforms at **-30**, **0**, **+30** | Three positions: left, center, right |
| 3 | At **-30**: deform EVERY face layer to show left-turn perspective | Start with face outline, then eyes, then details |
| 4 | At **+30**: deform everything for right-turn perspective | Mirror of -30 |
| 5 | Repeat for **ParamAngleY** (-30 = look down, +30 = look up) | |
| 6 | Repeat for **ParamAngleZ** (-30 = tilt left, +30 = tilt right) | |
| 7 | [Check all corner combinations](live2d-eye-rigging-guide-v2.md#step-3-check-the-corners) | Same concept — check X+Y, X+Z, Y+Z combos |

> **This is the most time-consuming part of any Live2D rig.** Take it one axis at a time. X first, then Y, then Z. Don't try to do all three at once.

### Tips for Natural Head Rotation

| Tip | Why |
|:---|:---|
| **Near side gets bigger, far side gets smaller** | Basic perspective — the ear/cheek facing the camera grows |
| **Hair has parallax** | Front hair moves WITH the head, back hair moves LESS (it's further away) |
| **Don't move everything the same amount** | Nose moves more than ears. Eyes move less than face outline. |
| **Use reference** | Turn your own head in a mirror and watch what moves how much |

---

## Step 3: Body Sway

**Parameters:** `ParamBodyAngleX` (left/right) and `ParamBodyAngleZ` (tilt)

### What Moves

| Layer | ParamBodyAngleX | ParamBodyAngleZ |
|:---|:---|:---|
| **Entire head** | Shifts left/right | Tilts with body |
| **Shoulders** | Rotate, one higher than other | Lean to one side |
| **Torso/clothes** | Shifts with perspective | Tilts |
| **Hair** | Follows with delay ([physics](live2d-eye-rigging-guide-v2.md#jiggle-physics-setup)) | Follows with delay |
| **Accessories** | Follow body + [physics bounce](live2d-eye-rigging-guide-v2.md#jiggle-physics-setup) | Same |

### Keyform Values

| Parameter | -10 | 0 | +10 |
|:---|:---|:---|:---|
| **BodyAngleX** | Leaning left | Centered | Leaning right |
| **BodyAngleZ** | Tilting left | Straight | Tilting right |

> **Body range is smaller than head** (-10 to +10 vs -30 to +30). Body movement is subtle — too much looks like the character is falling over.

---

## Step 4: Breathing

**Parameter:** `ParamBreath`

The easiest parameter to rig and it adds constant subtle life.

| Slider Position | State | What Moves |
|:---:|:---|:---|
| **0** | Exhaled | Default position |
| **1** | Inhaled | Shoulders UP slightly, chest expands slightly |

### What To Move

| Layer | At 0 (exhaled) | At 1 (inhaled) |
|:---|:---|:---|
| **Shoulders** | Default | Shift UP 2-3 pixels |
| **Chest/torso** | Default | Expand slightly outward |
| **Head** | Default | Shift UP 1-2 pixels (rides on shoulders) |
| **Hair** | Default | Follows head shift |

### Making It Automatic

| Method | How | Result |
|:---|:---|:---|
| **VTube Studio auto-breath** | VTS settings > breathing parameter > enable | Automatic sine wave, always on |
| **Physics-driven** | Physics input: `ParamBreath` as sine wave source | More organic, slightly random |

> **Breathing is the one parameter that should always be moving.** Even when the model is idle. It's the difference between "alive" and "screenshot."

---

## Common Mistakes

| Problem | Why It Happens | Fix |
|:---|:---|:---|
| **Head rotation looks flat** | Not enough perspective difference between -30 and +30 | Exaggerate: near side bigger, far side smaller |
| **Head turn warps the face** | Not enough keyform points, mesh too sparse | Add more mesh vertices on face layers |
| **Hair doesn't follow head** | Hair not rigged to head rotation parameters | Rig hair on same `ParamAngleX/Y/Z` |
| **Body movement too dramatic** | Range too large | Use -10 to +10, not -30 to +30 |
| **Breathing looks like hiccups** | Movement too large | Shoulders move 2-3 pixels MAX. Subtle. |
| **Eyebrows move with head but look frozen** | Brows rigged to head params but not brow params | Rig brows to BOTH head angle AND `ParamBrowLY/RY` |
| **One side of face looks good, other side looks wrong** | Didn't mirror the deformation properly | Check both -30 and +30, they should be symmetrical |

---

## Quick Reference

| I Need To... | Where It Is | More Info |
|:---|:---|:---:|
| **Raise/lower eyebrows** | `ParamBrowLY` / `ParamBrowRY` (-1 to +1) | [Eyebrows](#step-1-eyebrows) |
| **Change eyebrow shape** | `ParamBrowLForm` / `ParamBrowRForm` | [Eyebrow Shape](#eyebrow-shape-optional) |
| **Turn head left/right** | `ParamAngleX` (-30 to +30) | [Head Rotation](#step-2-head-rotation) |
| **Nod head up/down** | `ParamAngleY` (-30 to +30) | [Head Rotation](#step-2-head-rotation) |
| **Tilt head** | `ParamAngleZ` (-30 to +30) | [Head Rotation](#step-2-head-rotation) |
| **Add body sway** | `ParamBodyAngleX/Z` (-10 to +10) | [Body Sway](#step-3-body-sway) |
| **Add breathing** | `ParamBreath` (0 to 1) | [Breathing](#step-4-breathing) |
| **Check corner combos** | Set two params to extremes, check viewport | [Eye Guide — Corners](live2d-eye-rigging-guide-v2.md#step-3-check-the-corners) |

---

*Head rotation is the mountain. Everything after it is downhill.*
