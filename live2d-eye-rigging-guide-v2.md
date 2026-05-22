# Live2D Cubism — Eye Rigging Guide

**Starting point:** Layer separation done, blinking works. This guide covers everything needed for full eye movement + googly eyes.

**Jump to:** [Panel Map](#cubism-editor--panel-map) | [Tools](#tools--which-one-and-when) | [Left/Right](#step-1-leftright-eye-movement) | [Up/Down](#step-2-updown-eye-movement) | [Corners](#step-3-check-the-corners) | [Clipping](#step-4-clipping-mask) | [Googly Eyes](#step-5-googly-eyes) | [Common Mistakes](#common-mistakes) | [Quick Reference](#quick-reference)

---

## Eye Movement — What Exists

| Motion Type | Parameter Name | What It Controls | Required? |
|:---|:---|:---|:---:|
| **Blink** | `ParamEyeLOpen` / `ParamEyeROpen` | Eyelid open/close | Already done |
| **Left/Right look** | `ParamEyeBallX` | Iris moves horizontally | Yes — [Step 1](#step-1-leftright-eye-movement) |
| **Up/Down look** | `ParamEyeBallY` | Iris moves vertically | Yes — [Step 2](#step-2-updown-eye-movement) |
| **Independent left eye** | `ParamEyeBallXL` / `ParamEyeBallYL` | Left iris only | Optional — [Step 5](#independent-control-setup) |
| **Independent right eye** | `ParamEyeBallXR` / `ParamEyeBallYR` | Right iris only | Optional — [Step 5](#independent-control-setup) |
| **Googly wobble** | Physics system | Iris jiggles after head movement | Optional — [Step 5](#jiggle-physics-setup) |

---

## Cubism Editor — Panel Map

| Panel | Location | What's In It | When You Use It |
|:---|:---|:---|:---|
| **Parts List** | Left side | All your layers (eye white, iris, etc.) | Select which layer to work on |
| **Parameter List** | Bottom | All parameters and their sliders (-1 to +1) | Set [keyforms](#keyform-values) for each position |
| **Inspector** | Right side | Properties of selected item, [clipping](#step-4-clipping-mask) settings | Set up clipping masks |
| **Toolbar** | Top | [Tools](#tools--which-one-and-when) — arrow, deformation, draw, etc. | Switch to **Deformation Tool** |
| **Physics Panel** | Top menu > Modeling > Physics | Pendulum/[jiggle](#jiggle-physics-setup) settings | Set up [googly eye](#step-5-googly-eyes) wobble |

---

## Tools — Which One and When

| Tool | Icon Looks Like | Use For | **Don't** Use For |
|:---|:---|:---|:---|
| **Deformation Tool** | Lasso / net shape | **Moving iris for [keyforms](#keyform-values)** | -- |
| **Arrow/Select Tool** | Arrow | Selecting layers in [Parts List](#cubism-editor--panel-map) | Moving the iris — it **teleports** instead of deforming |
| **Draw Tool** | Pen | Drawing new mesh points | Eye rigging |

> **Rule: Always use the Deformation Tool when positioning the iris for a keyform. The Arrow tool will teleport the mesh and break your rig. If something jumped — you used the wrong tool. See [Common Mistakes](#common-mistakes).**

---

## Step 1: Left/Right Eye Movement

**Parameter:** `ParamEyeBallX`

### Keyform Values

| Slider Position | Eye Direction | What To Do |
|:---:|:---|:---|
| **-1** | Looking left | Deformation tool > drag iris LEFT |
| **0** | Looking center | Don't touch — this is the default |
| **+1** | Looking right | Deformation tool > drag iris RIGHT |

### Visual

```
     -1 (left)            0 (center)           +1 (right)

    .----------.         .----------.         .----------.
    | (o)      |         |   (o)    |         |      (o) |
    |          |         |          |         |          |
    '----------'         '----------'         '----------'
```

### Process

| Step | Action | Where |
|:---:|:---|:---|
| 1 | Select **iris/pupil layer** (one eye) | [Parts List](#cubism-editor--panel-map) (left panel) |
| 2 | Find or create `ParamEyeBallX` | [Parameter List](#cubism-editor--panel-map) (bottom panel) |
| 3 | Click `...` on parameter > **Insert Keyform** at -1, 0, +1 | Parameter bar |
| 4 | Set slider to **-1** | Parameter slider |
| 5 | Use [**Deformation Tool**](#tools--which-one-and-when), drag iris **LEFT** | Viewport |
| 6 | Set slider to **+1** | Parameter slider |
| 7 | Use [**Deformation Tool**](#tools--which-one-and-when), drag iris **RIGHT** | Viewport |
| 8 | Repeat steps 1-7 for the **other eye's iris** | Same parameter, other layer |

> Both eyes on the same `ParamEyeBallX` = they move together. This is normal.

---

## Step 2: Up/Down Eye Movement

**Parameter:** `ParamEyeBallY`

Same process as Step 1, just vertical.

| Slider Position | Eye Direction | What To Do |
|:---:|:---|:---|
| **-1** | Looking down | Deformation tool > drag iris DOWN |
| **0** | Looking center | Don't touch |
| **+1** | Looking up | Deformation tool > drag iris UP |

### Visual

```
      +1 (up)             0 (center)          -1 (down)

    .----------.         .----------.         .----------.
    |   (o)    |         |          |         |          |
    |          |         |   (o)    |         |          |
    |          |         |          |         |   (o)    |
    '----------'         '----------'         '----------'
```

---

## Step 3: Check the Corners

With both X and Y rigged, the eye can look in 9 directions. The **corners** sometimes look warped because Cubism blends two deformations at once.

### All 9 Positions

| | X = -1 (left) | X = 0 (center) | X = +1 (right) |
|:---|:---:|:---:|:---:|
| **Y = +1 (up)** | top-left | top-center | top-right |
| **Y = 0 (center)** | left | CENTER | right |
| **Y = -1 (down)** | bottom-left | bottom-center | bottom-right |

### How To Check

| Step | Action | If It Looks Off |
|:---:|:---|:---|
| 1 | Set X to -1 and Y to +1 (top-left) | Adjust iris with deformation tool at this position |
| 2 | Set X to +1 and Y to +1 (top-right) | Same — adjust if stretched |
| 3 | Set X to -1 and Y to -1 (bottom-left) | Same |
| 4 | Set X to +1 and Y to -1 (bottom-right) | Same |

---

## Step 4: Clipping Mask

Prevents the iris from visually leaving the eye boundary at extreme positions.

### Two Methods

| Method | How | Best When |
|:---|:---|:---|
| **Drag nesting** | In Parts list, drag iris layer UNDER eye white layer so it's nested inside | Simple setup, always works |
| **Inspector clipping** | Select iris > Inspector panel (right) > Clipping > pick eye white as target | More control, same result |

### What Clipping Does

| Without Clipping | With Clipping |
|:---|:---|
| Iris can slide outside the eye boundary | Iris is invisible outside the eye boundary |
| Looks broken at extreme positions | Looks natural at all positions |

---

## Step 5: Googly Eyes

Two approaches — pick one or combine both.

### Approach Comparison

| | Jiggle Physics | Independent Control |
|:---|:---|:---|
| **What it does** | Eyes wobble and settle after head movement | Each eye moves on its own |
| **Difficulty** | Easier — just physics settings | Harder — separate parameters per eye |
| **Parameters needed** | Shared `ParamEyeBallX/Y` (already done) | Custom per-eye: `XL`, `YL`, `XR`, `YR` |
| **Control** | Automatic — head moves, eyes react | Manual — face tracking drives each eye |
| **The vibe** | Bouncy, reactive, alive | Full chaos, look two directions at once |
| **Can combine?** | Yes | Yes — independent + physics = maximum googly |

---

### Jiggle Physics Setup

#### Physics Settings — What Each One Does

| Setting | What It Controls | Low Value | High Value |
|:---|:---|:---|:---|
| **Damping** | How fast wobble stops | 0.2 = WOBBLE WOBBLE wobble... settle | 0.9 = shift... done |
| **Length** | How far the swing goes | 3 = wide dramatic swings | 8 = subtle shifts |
| **Speed** | How fast it oscillates | 1.0 = gentle | 2.0 = snappy |
| **Reaction Force** | How much head motion triggers it | Low = lazy response | High = twitchy |

#### Presets

| Style | Damping | Length | Speed | Result |
|:---|:---:|:---:|:---:|:---|
| **Normal eyes** | 0.7-0.9 | 5-8 | 1.0 | Subtle, settles fast |
| **Googly eyes** | 0.2-0.4 | 3-5 | 1.5-2.0 | Wobble city |
| **Drunk eyes** | 0.1-0.2 | 2-3 | 0.5 | Slow, sloppy, never quite settle |
| **Anime startle** | 0.5 | 4 | 2.5 | Quick snap then settle |

```
  HIGH DAMPING (0.9)                LOW DAMPING (0.3)
  "Normal eyes"                     "Googly eyes"

  Head moves > eyes shift           Head moves > eyes shift
  > settle quickly                  > WOBBLE WOBBLE WOBBLE
  > done                            > wobble wobble
                                    > wobble
                                    > ...settle
```

#### Setup Process

| Step | Action | Where |
|:---:|:---|:---|
| 1 | Open Physics panel | Top menu > Modeling > Physics |
| 2 | Create new physics group, name it "Left Eye Jiggle" | Physics panel |
| 3 | Set **Input** to head parameters: `ParamAngleX`, `ParamAngleY`, `ParamAngleZ` | Input section |
| 4 | Set **Output** to `ParamEyeBallX` and `ParamEyeBallY` | Output section |
| 5 | Adjust Damping, Length, Speed to taste (start with Googly preset) | Settings section |
| 6 | Repeat for other eye as separate physics group | Physics panel |

> **Pro tip:** Use slightly different Damping/Speed values for each eye. Left eye at 0.3 damping, right eye at 0.35. They'll wobble out of sync for maximum googly energy.

---

### Independent Control Setup

Instead of shared parameters, each eye gets its own:

| Eye | Left/Right Parameter | Up/Down Parameter |
|:---|:---|:---|
| Left eye iris | `ParamEyeBallXL` | `ParamEyeBallYL` |
| Right eye iris | `ParamEyeBallXR` | `ParamEyeBallYR` |

| Step | Action |
|:---:|:---|
| 1 | Right-click in parameter panel > Create Parameter |
| 2 | Create all four custom parameters (XL, YL, XR, YR) |
| 3 | Rig left iris to XL/YL only (same keyform process as Steps 1-2) |
| 4 | Rig right iris to XR/YR only |
| 5 | In VTube Studio, map face tracking to each eye independently |

---

## Common Mistakes

| Problem | Why It Happens | Fix |
|:---|:---|:---|
| **Iris teleports** instead of sliding | Used **Arrow** tool instead of **[Deformation Tool](#tools--which-one-and-when)** | Switch to **Deformation Tool** |
| **Iris goes outside** the eye | No [clipping mask](#step-4-clipping-mask) | Add clipping |
| **Both eyes move** when I only select one | Both irises on same mesh | Each iris needs its own layer |
| **Corners look stretched** | X and Y deformations stacking | Manually [adjust corners](#step-3-check-the-corners) |
| **Eyes look dead/robotic** | Movement range too small | Drag iris further at -1 and +1 |
| **Jiggle never stops** | [Damping](#physics-settings--what-each-one-does) too low | Raise damping: 0.2 > 0.4 |
| **No jiggle at all** | Input not connected or [damping](#physics-settings--what-each-one-does) too high | Check physics Input links to head params |
| **One eye jiggles**, other doesn't | Only made one physics group | Create a [second group](#jiggle-physics-setup) for the other eye |

---

## What To Rig Next

| Feature | Parameters | Difficulty | Impact | Uses Same Skills As |
|:---|:---|:---:|:---|:---|
| **Mouth open/close** | `ParamMouthOpenY` | Easy | Talking, reactions | [Keyforms](#keyform-values) (same as eye movement) |
| **Smile/frown** | `ParamMouthForm` | Easy | Emotion range | [Keyforms](#keyform-values) |
| **Eyebrows** | `ParamBrowLY`, `ParamBrowRY` | Easy | Expression depth | [Keyforms](#keyform-values) |
| **Head rotation** | `ParamAngleX/Y/Z` | Medium | Makes tracking feel alive — **big impact** | [Keyforms](#keyform-values) + [corners](#step-3-check-the-corners) |
| **Hair physics** | [Physics system](#jiggle-physics-setup) | Medium | Movement, life, bounce | Same setup as [googly eyes](#jiggle-physics-setup) |
| **Body sway** | `ParamBodyAngleX/Y/Z` | Medium | Breathing, subtle motion | [Keyforms](#keyform-values) |
| **Accessory physics** | [Physics system](#jiggle-physics-setup) | Easy-Medium | Earrings, ribbons, etc. | Same setup as [googly eyes](#jiggle-physics-setup) |

---

## Quick Reference

| I Need To... | Where It Is | More Info |
|:---|:---|:---:|
| **Select my iris layer** | **Left panel** > Parts List > click the iris | [Panel Map](#cubism-editor--panel-map) |
| **Find/create a parameter** | **Bottom panel** > Parameter List > scroll or right-click to create | [Panel Map](#cubism-editor--panel-map) |
| **Add a keyform** | **`...`** on the parameter bar > Insert Keyform | [Keyforms](#keyform-values) |
| **Move the iris for a keyform** | **Top toolbar** > **Deformation Tool** (lasso icon) | [Tools](#tools--which-one-and-when) |
| **Set up clipping** | Drag iris UNDER eye white in Parts List, OR Inspector > Clipping | [Clipping](#step-4-clipping-mask) |
| **Open physics** | **Top menu** > Modeling > Physics | [Jiggle Setup](#jiggle-physics-setup) |
| **Adjust jiggle** | Physics panel > select group > tweak Damping/Speed/Length | [Physics Settings](#physics-settings--what-each-one-does) |
| **Check all directions** | Set both X and Y sliders manually, check viewport | [Corners](#step-3-check-the-corners) |
| **Test physics live** | Play button in physics panel, move head params | [Presets](#presets) |

---

*Tables over paragraphs. Options before instructions. Jiggle before settle.*
