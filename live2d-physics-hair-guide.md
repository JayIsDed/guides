# Live2D Cubism — Physics, Hair & Accessories Guide

**Starting point:** Eyes, mouth, and head rotation are rigged. This guide covers making things bounce, sway, and flow — hair, earrings, ribbons, tails, anything that moves on its own.

**Jump to:** [What Gets Physics](#what-gets-physics) | [How Physics Works](#how-physics-works) | [Hair Setup](#step-1-hair-physics) | [Accessories](#step-2-accessory-physics) | [Presets](#preset-library) | [Common Mistakes](#common-mistakes) | [Quick Reference](#quick-reference)

---

## What Gets Physics

| Part | Physics Type | Priority | Notes |
|:---|:---|:---:|:---|
| **Front hair / bangs** | Sway + bounce | High | First thing people notice moving |
| **Side hair** | Sway | High | Frames the face, very visible |
| **Back hair / ponytail** | Swing + gravity | Medium | Less visible but adds depth |
| **Ahoge (antenna hair)** | Bouncy spring | Medium | Character signature piece |
| **Earrings** | Pendulum swing | Low | Small but polished |
| **Ribbons / bows** | Flowing sway | Low | Adds personality |
| **Tail** | Pendulum + sway | Medium | If your model has one |
| **Clothing edges** | Subtle sway | Low | Skirt hem, scarf, loose fabric |
| **Googly eyes** | Wobble | Already done | [See Eye Guide](live2d-eye-rigging-guide-v2.md#jiggle-physics-setup) |

> **Do hair first.** It's the most visible physics on any model. Accessories are polish.

---

## How Physics Works

> If you rigged [googly eyes](live2d-eye-rigging-guide-v2.md#jiggle-physics-setup), you already know this system. Hair uses the exact same physics panel with different settings.

### The Pendulum Model

Every physics object in Live2D is a **pendulum** — like a weight on a string.

```
  Anchor point (where the hair attaches to the head)
       |
       |  <-- Length (how long the string is)
       |
       O  <-- The "weight" (the hair tip / accessory)

  Head moves RIGHT -->

       \
        \  <-- Hair swings LEFT (opposite to movement)
         \
          O  <-- Then swings back and settles
```

### The Four Settings

| Setting | What It Controls | Short String (low) | Long String (high) |
|:---|:---|:---|:---|
| **Length** | How long the pendulum string is | Short = stiff, barely moves | Long = loose, big swings |
| **Damping** | How fast the swing dies down | Low = keeps swinging forever | High = stops quickly |
| **Speed** | How fast the pendulum swings back and forth | Slow = heavy, lazy | Fast = light, snappy |
| **Reaction Force** | How much head movement triggers the swing | Low = ignores small movements | High = reacts to everything |

### The Two Connections

| Connection | What It Means | Example |
|:---|:---|:---|
| **Input** | What CAUSES the movement | Head rotation (`ParamAngleX/Y/Z`), body sway |
| **Output** | What GETS moved | Hair sway parameter, earring swing parameter |

> **Input = cause, Output = effect.** Head turns (input) makes hair swing (output).

---

## Step 1: Hair Physics

### Before Physics — Create Hair Parameters

Each hair section needs its own sway parameter. Physics drives these parameters automatically.

| Hair Section | Parameter Name | Range | What It Does |
|:---|:---|:---:|:---|
| **Front hair / bangs** | `ParamHairFront` | -10 to +10 | Bangs sway left/right |
| **Left side hair** | `ParamHairSideL` | -10 to +10 | Left strands sway |
| **Right side hair** | `ParamHairSideR` | -10 to +10 | Right strands sway |
| **Back hair** | `ParamHairBack` | -10 to +10 | Back hair sways |
| **Ahoge** | `ParamHairAhoge` | -10 to +10 | Antenna bounces |

### Rigging Hair Keyforms (Before Physics)

Same process as eye rigging — create the parameter, add keyforms, deform the mesh.

| Step | Action |
|:---:|:---|
| 1 | Create parameter (e.g., `ParamHairFront`) |
| 2 | Add keyforms at **-10**, **0**, **+10** |
| 3 | At **-10**: use Deformation Tool, sway hair to the LEFT |
| 4 | At **0**: default position |
| 5 | At **+10**: sway hair to the RIGHT |
| 6 | Repeat for each hair section |

> **The keyforms define HOW FAR the hair can move. Physics decides WHEN it moves.**

### Adding Physics to Hair

| Step | Action | Where |
|:---:|:---|:---|
| 1 | Open Physics panel | Top menu > Modeling > Physics |
| 2 | Create new physics group: "Front Hair" | Physics panel |
| 3 | **Input:** `ParamAngleX` (head turn) | Input section |
| 4 | **Output:** `ParamHairFront` | Output section |
| 5 | Set values from [preset table](#preset-library) | Settings section |
| 6 | Test — move head params, watch hair react | Play button |
| 7 | Repeat for each hair section with its own physics group | |

### Multi-Input (Advanced)

Hair looks better when it reacts to multiple inputs, not just head turn:

| Input Parameter | What It Adds |
|:---|:---|
| `ParamAngleX` (head turn) | Hair sways when you look left/right |
| `ParamAngleZ` (head tilt) | Hair falls to the tilted side |
| `ParamBodyAngleX` (body sway) | Hair follows body movement |

> Add all three as inputs to the same physics group. Each one contributes to the hair movement. More inputs = more natural.

---

## Step 2: Accessory Physics

Exact same system as hair. Create a parameter, rig keyforms, attach physics.

### Common Accessories

| Accessory | Parameter Name | Input | Physics Style |
|:---|:---|:---|:---|
| **Earrings** | `ParamEarringL/R` | `ParamAngleX`, `ParamAngleZ` | Short pendulum, fast settle |
| **Ribbon / bow** | `ParamRibbon` | `ParamAngleX`, `ParamAngleZ` | Medium sway, slow settle |
| **Necklace** | `ParamNecklace` | `ParamAngleY`, `ParamBodyAngleX` | Gravity-heavy, responds to nod |
| **Scarf / cape** | `ParamScarf` | `ParamAngleX`, `ParamBodyAngleX` | Long pendulum, flowing |
| **Tail** | `ParamTail` | `ParamBodyAngleX`, `ParamBodyAngleZ` | Long, slow, heavy |
| **Antenna / ahoge** | `ParamHairAhoge` | `ParamAngleX`, `ParamAngleY` | Short, snappy, bouncy |

---

## Preset Library

Copy these values as starting points. Adjust to taste.

### Hair Presets

| Style | Length | Damping | Speed | Reaction | Result |
|:---|:---:|:---:|:---:|:---:|:---|
| **Short bangs** | 4 | 0.7 | 1.2 | Medium | Stiff, quick settle — for short/thick hair |
| **Long flowing** | 8 | 0.3 | 0.8 | High | Big swings, slow settle — for long straight hair |
| **Curly/bouncy** | 5 | 0.4 | 1.8 | High | Quick bounces, medium settle — for curly/wavy |
| **Ponytail** | 9 | 0.3 | 0.6 | Medium | Heavy pendulum — swings wide and slow |
| **Ahoge** | 3 | 0.2 | 2.5 | High | Snappy spring — bounces on every movement |
| **Side strands** | 6 | 0.5 | 1.0 | Medium | Balanced sway — visible but not distracting |

### Accessory Presets

| Accessory | Length | Damping | Speed | Reaction | Result |
|:---|:---:|:---:|:---:|:---:|:---|
| **Earrings (light)** | 3 | 0.6 | 2.0 | High | Quick dangle, fast settle |
| **Earrings (heavy)** | 4 | 0.7 | 1.0 | Medium | Slower swing, weighted feel |
| **Ribbon** | 6 | 0.3 | 1.2 | Medium | Flowing, keeps moving |
| **Necklace** | 5 | 0.8 | 0.8 | Low | Barely moves, subtle weight |
| **Scarf** | 8 | 0.2 | 0.7 | High | Dramatic flow, long settle |
| **Tail (short)** | 5 | 0.5 | 1.5 | Medium | Perky, reactive |
| **Tail (long)** | 10 | 0.3 | 0.5 | Medium | Slow, heavy, pendulum |

### Tuning By Feel

| It Feels... | Adjust This | Direction |
|:---|:---|:---|
| **Too stiff** | Length | Increase (longer string = more movement) |
| **Too floppy / won't stop** | Damping | Increase (more friction = faster settle) |
| **Too slow / heavy** | Speed | Increase (faster oscillation) |
| **Doesn't react to small movements** | Reaction Force | Increase |
| **Overreacts to everything** | Reaction Force | Decrease |
| **Moves too far** | Length | Decrease, or reduce keyform range |
| **Settles too fast** | Damping | Decrease |

---

## Common Mistakes

| Problem | Why It Happens | Fix |
|:---|:---|:---|
| **Hair doesn't move at all** | No physics group created, or input not connected | Check Physics panel — input needs head params |
| **Hair moves but clips through face** | Keyform range too large, no clipping | Reduce range at -10/+10, or add [clipping mask](live2d-eye-rigging-guide-v2.md#step-4-clipping-mask) |
| **All hair sections move identically** | Same physics values for everything | Give each section different Length/Damping/Speed |
| **Hair jitters / vibrates** | Speed too high + damping too low | Reduce speed or increase damping |
| **Physics only works on one axis** | Only `ParamAngleX` as input | Add `ParamAngleZ` and `ParamBodyAngleX` as additional inputs |
| **Earrings swing wildly** | Length too high for a small object | Earrings should be Length 3-4, not 8+ |
| **Accessory feels disconnected from body** | Wrong input parameter | Make sure input matches the body part the accessory is attached to |

---

## Quick Reference

| I Need To... | Where It Is | More Info |
|:---|:---|:---:|
| **Create a hair sway parameter** | [Parameter List](live2d-eye-rigging-guide-v2.md#cubism-editor--panel-map) > right-click > Create | [Hair Parameters](#before-physics--create-hair-parameters) |
| **Rig hair keyforms** | Same as eye keyforms — [Deformation Tool](live2d-eye-rigging-guide-v2.md#tools--which-one-and-when) | [Rigging Hair Keyforms](#rigging-hair-keyforms-before-physics) |
| **Open Physics panel** | Top menu > Modeling > Physics | [How Physics Works](#how-physics-works) |
| **Pick starting values** | Copy from preset tables | [Hair Presets](#hair-presets) / [Accessory Presets](#accessory-presets) |
| **Fix jittery physics** | Reduce Speed, increase Damping | [Tuning By Feel](#tuning-by-feel) |
| **Add more natural movement** | Add multiple inputs to one physics group | [Multi-Input](#multi-input-advanced) |
| **Compare googly eye physics** | Same system, different values | [Eye Guide — Jiggle](live2d-eye-rigging-guide-v2.md#jiggle-physics-setup) |

---

*Same pendulum, different weight. If you can jiggle an eyeball, you can flow a ponytail.*
