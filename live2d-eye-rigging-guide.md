# Live2D Cubism — Eye Rigging Guide

**For:** First model, blinking already working. Next step: eye movement + googly eyes.
**Software:** Live2D Cubism Editor
**Skill level:** Beginner — layer separation done, blink parameter working

---

## What You're Building

```
                 ┌─────────────────────┐
                 │   FULL EYE MOTION   │
                 └──────────┬──────────┘
            ┌───────────────┼───────────────┐
            ▼               ▼               ▼
      ┌───────────┐  ┌───────────┐  ┌──────────────┐
      │ Left/Right│  │  Up/Down  │  │  Googly Eyes  │
      │ EyeBallX  │  │ EyeBallY  │  │  (physics OR  │
      │           │  │           │  │  independent) │
      └───────────┘  └───────────┘  └──────────────┘
```

Each eye needs **two movement axes** (left/right + up/down). Combined, these let the eye look in any direction. Googly eyes add independent control or jiggle physics on top.

---

## Step 1: Left/Right Eye Movement

**Parameter:** `ParamEyeBallX`
**What it does:** Moves the iris/pupil left and right inside the eye

### Where Things Are in Cubism

```
┌─────────────────────────────────────────────────┐
│ Cubism Editor                                   │
│                                                 │
│  Left Panel: PARTS LIST (your layers)           │
│  ├── Find your iris/pupil layer                 │
│  └── Click it to select                         │
│                                                 │
│  Bottom Panel: PARAMETER LIST                   │
│  ├── Scroll to find "ParamEyeBallX"             │
│  ├── If it doesn't exist: right-click →         │
│  │   "Create Parameter" → name it ParamEyeBallX │
│  └── The slider goes from -1 to +1              │
│                                                 │
│  Top Toolbar: TOOLS                             │
│  ├── Arrow = Select/Move (don't use for this)   │
│  └── Lasso-looking icon = DEFORMATION TOOL ← ★ │
│       USE THIS ONE for moving the iris          │
└─────────────────────────────────────────────────┘
```

### The Process

**A. Select the iris mesh** (just the iris, not the whole eye — one eye at a time)

**B. Set up three keyforms on ParamEyeBallX:**

Click the three dots `•••` on the ParamEyeBallX parameter bar → Insert Keyform at these values:

| Keyform Value | What the Eye is Doing | What You Do |
|:---:|:---:|:---|
| **-1** | Looking LEFT | Use deformation tool, drag iris to the LEFT |
| **0** | Looking CENTER | Don't touch anything — this is the default pose |
| **+1** | Looking RIGHT | Use deformation tool, drag iris to the RIGHT |

```
     -1 (left)           0 (center)         +1 (right)

    ┌─────────┐         ┌─────────┐        ┌─────────┐
    │ ●    ·  │         │  · ● ·  │        │  ·    ● │
    │         │         │         │        │         │
    └─────────┘         └─────────┘        └─────────┘
    iris shifted         iris centered      iris shifted
    to the left                             to the right
```

**C. Repeat for the other eye** — select the OTHER iris mesh, same parameter, same process. Both eyes on the same `ParamEyeBallX` means they move together (which is normal).

> **Don't move the iris past the white of the eye.** It'll look weird. Keep it natural — a little goes a long way.

---

## Step 2: Up/Down Eye Movement

**Parameter:** `ParamEyeBallY`
**What it does:** Moves the iris/pupil up and down

Exact same process as Step 1, just vertical:

| Keyform Value | What the Eye is Doing | What You Do |
|:---:|:---:|:---|
| **-1** | Looking DOWN | Deformation tool, drag iris DOWN |
| **0** | Looking CENTER | Default — don't touch |
| **+1** | Looking UP | Deformation tool, drag iris UP |

```
       +1 (up)           0 (center)         -1 (down)

    ┌─────────┐         ┌─────────┐        ┌─────────┐
    │    ●    │         │         │        │         │
    │         │         │    ●    │        │         │
    │         │         │         │        │    ●    │
    └─────────┘         └─────────┘        └─────────┘
```

Do both eyes, same parameter.

---

## Step 3: Check the Corners

With both X and Y parameters rigged, your eye can now look in **8 directions + center**. But the corners (looking top-left, bottom-right, etc.) can sometimes look warped because Cubism blends two deformations together.

**How to check:**
1. Set `ParamEyeBallX` to **-1** AND `ParamEyeBallY` to **+1** at the same time (top-left)
2. Look at the iris — does it look natural? Or stretched/squished?
3. If it looks off: with both parameters at those positions, adjust the iris with the deformation tool
4. Repeat for all four corners: (-1,+1) (-1,-1) (+1,+1) (+1,-1)

```
    (-1,+1)         (0,+1)         (+1,+1)
    top-left        top-center     top-right

    (-1, 0)         (0, 0)         (+1, 0)
    left            CENTER         right

    (-1,-1)         (0,-1)         (+1,-1)
    bottom-left     bottom-center  bottom-right
```

---

## Step 4: Clipping Mask (Keep the Iris Inside the Eye)

This prevents the iris from visually escaping the eye boundary at extreme positions.

### Where to Find It

```
┌─────────────────────────────────────────────────┐
│ Left Panel: PARTS LIST                          │
│                                                 │
│  ├── Eye White layer                            │
│  │   └── Iris layer ← drag INSIDE the eye white│
│  │                                              │
│  When the iris is NESTED under the eye white    │
│  in the parts list, it gets clipped to it.      │
│  The iris can never visually leave the eye.      │
│                                                 │
│  Alternative method:                            │
│  Select iris → Inspector panel → Clipping →     │
│  Select the eye white as the clipping target    │
└─────────────────────────────────────────────────┘
```

---

## Step 5: Googly Eyes — The Jiggle Physics Route

> **Two ways to do googly eyes.** This section covers the fun one — physics-based wobble where the eyes jiggle and settle after head movements. Skip to Step 6 for independent eye control instead.

### Where to Find Physics

```
┌─────────────────────────────────────────────────┐
│ Top Menu Bar:                                   │
│  Modeling → Physics / Scene Blend               │
│                                                 │
│  OR the Physics icon in the toolbar             │
│  (looks like a pendulum / swinging weight)      │
└─────────────────────────────────────────────────┘
```

### Setting Up Jiggle on the Iris

**A.** Open the Physics panel

**B.** Create a new physics group — name it something like "Left Eye Jiggle"

**C.** Set the **Input** to head movement parameters:
   - `ParamAngleX` (head turn left/right)
   - `ParamAngleY` (head tilt up/down)
   - `ParamAngleZ` (head tilt side to side)

**D.** Set the **Output** to your eye parameters:
   - `ParamEyeBallX` and `ParamEyeBallY`

**E.** Now the fun part — the jiggle settings:

| Setting | What It Does | Googly Value | Normal Value |
|---------|-------------|:---:|:---:|
| **Length** | How far the pendulum swings | 3-5 | 5-8 |
| **Damping** | How fast the wobble stops | 0.2-0.4 | 0.7-0.9 |
| **Speed** | How fast the pendulum moves | 1.5-2.0 | 1.0 |
| **Reaction Force** | How much head motion triggers it | Higher = more reactive | — |

```
   HIGH DAMPING (0.9)              LOW DAMPING (0.3)
   "Normal eyes"                   "Googly eyes"

   Head moves → eyes shift         Head moves → eyes shift
   → settle quickly                → WOBBLE WOBBLE WOBBLE
   → done                          → wobble wobble
                                   → wobble
                                   → ...settle
```

**Lower damping = more googly.** Start at 0.3 and adjust until the jiggle feels right.

**F.** Repeat for the other eye — make a second physics group. You can use slightly different damping/speed values for each eye to make them wobble *out of sync* (maximum googly energy).

---

## Step 6: Googly Eyes — Independent Control Route

> **This method lets each eye move separately.** More complex to set up but gives full control. Use this if you want one eye looking left while the other looks right.

Instead of both eyes sharing `ParamEyeBallX`:

| Eye | Left/Right Parameter | Up/Down Parameter |
|-----|---------------------|-------------------|
| Left eye iris | `ParamEyeBallXL` | `ParamEyeBallYL` |
| Right eye iris | `ParamEyeBallXR` | `ParamEyeBallYR` |

Create these as custom parameters (right-click in parameter panel → Create Parameter). Rig each iris to its OWN pair of parameters instead of the shared ones.

In VTube Studio later, you can map face tracking to control each eye independently.

> **Tip:** You can combine this with the jiggle physics from Step 5 — independent parameters + physics = each eye jiggles on its own pendulum. Maximum chaos.

---

## Quick Reference — Where Is Everything?

| I Need To... | Where It Is |
|:---|:---|
| Select my iris layer | **Left panel** → Parts list → click the iris |
| Find/create a parameter | **Bottom panel** → Parameter list → scroll or right-click to create |
| Add a keyform | **Three dots `•••`** on the parameter bar → Insert Keyform |
| Move the iris for a keyform | **Top toolbar** → Deformation tool (lasso icon) ← **USE THIS, not the arrow** |
| Set up clipping | Drag iris layer UNDER eye white in Parts list, OR Inspector → Clipping |
| Open physics | **Top menu** → Modeling → Physics, OR pendulum icon in toolbar |
| Adjust jiggle | Physics panel → select group → change Damping/Speed/Length |
| Check all directions | Set both X and Y parameters manually, look at result in viewport |

---

## Common Mistakes

| Problem | Why It Happens | Fix |
|:---|:---|:---|
| Iris teleports instead of sliding | Used Move tool instead of Deformation tool | Switch to deformation tool |
| Iris goes outside the eye | No clipping mask set up | Add clipping (Step 4) |
| Both eyes move even when I only select one | Both irises on same mesh | Make sure each iris is a separate layer/mesh |
| Corners look stretched | X and Y deformations stacking badly | Manually adjust the corner keyforms (Step 3) |
| Eyes look dead/robotic | Not enough movement range | Increase how far you shift the iris at -1 and +1 |
| Jiggle never stops | Damping too low | Raise damping from 0.2 → 0.4 |
| No jiggle at all | Damping too high, or input not connected | Lower damping, check physics input is linked to head params |

---

## Next Steps After Eyes

Once eyes are moving and jiggling:
1. **Mouth shapes** — `ParamMouthOpenY`, `ParamMouthForm` (smile/frown)
2. **Eyebrow movement** — `ParamBrowLY`, `ParamBrowRY` (raise/lower)
3. **Head rotation** — `ParamAngleX/Y/Z` (the big one — makes tracking feel alive)
4. **Hair/accessory physics** — same jiggle setup as googly eyes, just on hair layers
5. **Body sway** — `ParamBodyAngleX/Y/Z` for subtle breathing and movement

---

*Made for visual learners who want jiggle physics. Low damping, high chaos.*
*`const plant = alive` — even in 2D*
