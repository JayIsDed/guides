# Live2D Cubism — Mouth Rigging Guide

**Starting point:** Eyes are rigged ([Eye Guide](live2d-eye-rigging-guide-v2.md)). This guide covers mouth open/close, lip sync shapes, and smile/frown expressions.

**Jump to:** [What Exists](#mouth-movement--what-exists) | [Open/Close](#step-1-mouth-openclose) | [Smile/Frown](#step-2-smilefrown) | [Lip Sync Shapes](#step-3-lip-sync-shapes-vowels) | [Combining](#step-4-combining-parameters) | [Common Mistakes](#common-mistakes) | [Quick Reference](#quick-reference)

---

## Mouth Movement — What Exists

| Motion Type | Parameter Name | What It Controls | Priority |
|:---|:---|:---|:---:|
| **Open/Close** | `ParamMouthOpenY` | Jaw drop — how wide the mouth opens | Do first |
| **Smile/Frown** | `ParamMouthForm` | Mouth shape — corners up or down | Do second |
| **Lip Sync A** | `ParamA` | "Ah" vowel shape | Optional — for talking |
| **Lip Sync I** | `ParamI` | "Ee" vowel shape | Optional — for talking |
| **Lip Sync U** | `ParamU` | "Oo" vowel shape | Optional — for talking |
| **Lip Sync E** | `ParamE` | "Eh" vowel shape | Optional — for talking |
| **Lip Sync O** | `ParamO` | "Oh" vowel shape | Optional — for talking |

> **Minimum viable mouth:** Just `ParamMouthOpenY` gives you open/close for basic talking. Add `ParamMouthForm` for emotions. Vowel shapes are polish.

---

## Layers You Need

| Layer | What It Is | Moves During |
|:---|:---|:---|
| **Upper lip** | Top edge of the mouth | Open/close, vowel shapes |
| **Lower lip** | Bottom edge of the mouth | Open/close (moves the most) |
| **Mouth interior** | Dark inside of mouth, teeth, tongue | Visible when mouth opens |
| **Teeth (upper)** | Top teeth row | Stays mostly still, peeks during open |
| **Teeth (lower)** | Bottom teeth row | Moves down with lower lip |
| **Tongue** | Optional — adds life to vowels | Vowel shapes (especially "L", "Ah") |

> Not all models have all layers. At minimum you need **upper lip + lower lip + mouth interior**. Teeth and tongue are bonus.

---

## Step 1: Mouth Open/Close

**Parameter:** `ParamMouthOpenY`

### Keyform Values

| Slider Position | Mouth State | What To Move |
|:---:|:---|:---|
| **0** | Closed | Default — don't touch |
| **1** | Fully open | Lower lip DOWN, upper lip slightly UP, interior visible, teeth peek |

### Visual

```
        0 (closed)                    1 (open)

      .----------.                .----------.
      |          |                |  __====__ |
      |   ____   |                | /  teeth \ |
      |  '----'  |                | |  dark  | |
      |          |                | \________/ |
      '----------'                '----------'
```

### Process

| Step | Action | Notes |
|:---:|:---|:---|
| 1 | Select **lower lip** layer | This moves the most |
| 2 | Find or create `ParamMouthOpenY` (range 0 to 1) | [Parameter List](#mouth-movement--what-exists) |
| 3 | Add keyforms at **0** and **1** | `...` > Insert Keyform |
| 4 | At slider **1**: use [**Deformation Tool**](live2d-eye-rigging-guide-v2.md#tools--which-one-and-when), drag lower lip **DOWN** | How far = how dramatic the open looks |
| 5 | Select **upper lip** layer, same parameter | |
| 6 | At slider **1**: drag upper lip **slightly UP** | Subtle — upper lip barely moves in real life |
| 7 | Select **mouth interior** layer, same parameter | |
| 8 | At slider **0**: scale interior very small or hide it | Invisible when closed |
| 9 | At slider **1**: scale interior to fill the gap between lips | Visible when open |
| 10 | If you have **teeth**: rig them on same parameter, peek down from upper lip at **1** | |

> **Key:** The lower lip does 70-80% of the work. The upper lip barely moves. Most beginners move both equally — that looks unnatural.

---

## Step 2: Smile/Frown

**Parameter:** `ParamMouthForm`

### Keyform Values

| Slider Position | Expression | What To Move |
|:---:|:---|:---|
| **-1** | Frown | Mouth corners DOWN, slight droop |
| **0** | Neutral | Default — don't touch |
| **+1** | Smile | Mouth corners UP, slight widening |

### What Moves

| Layer | At -1 (frown) | At 0 (neutral) | At +1 (smile) |
|:---|:---|:---|:---|
| **Upper lip** | Flatten slightly | Default | Curve upward at corners |
| **Lower lip** | Corners pull down | Default | Corners pull up, may widen |
| **Mouth interior** | May narrow | Default | May widen slightly |

> **Tip:** Smile/frown is about the **corners** of the mouth, not the center. Move the edges of the lip meshes, not the middle.

---

## Step 3: Lip Sync Shapes (Vowels)

> **Optional but adds a lot to talking streams.** Each vowel gets its own parameter that controls a specific mouth shape.

### The Five Shapes

| Parameter | Sound | Mouth Shape | Key Feature |
|:---|:---|:---|:---|
| **`ParamA`** | "Ah" (father) | Wide open, round | Jaw drops, mouth wide |
| **`ParamI`** | "Ee" (feet) | Wide, flat, teeth showing | Mouth stretches sideways |
| **`ParamU`** | "Oo" (food) | Small, puckered, round | Lips push forward |
| **`ParamE`** | "Eh" (bed) | Half open, relaxed | Between A and I |
| **`ParamO`** | "Oh" (boat) | Round, open, O-shape | Lips form a circle |

### Visual — All Five

```
   "Ah" (A)      "Ee" (I)      "Oo" (U)      "Eh" (E)      "Oh" (O)

  .--------.    .--------.    .--------.    .--------.    .--------.
  |        |    |        |    |        |    |        |    |        |
  | (    ) |    | [----] |    |  (oo)  |    | [    ] |    |  (  )  |
  |        |    |        |    |        |    |        |    |        |
  '--------'    '--------'    '--------'    '--------'    '--------'
   wide open     wide flat     small round   half open     round open
```

### Process (Same for Each Vowel)

| Step | Action |
|:---:|:---|
| 1 | Create parameter (e.g., `ParamA`) with range 0 to 1 |
| 2 | Keyform at **0** = mouth closed/neutral |
| 3 | Keyform at **1** = that vowel's shape |
| 4 | Deform upper lip, lower lip, and interior to match the shape |
| 5 | Repeat for each vowel |

> **VTube Studio** auto-detects vowel sounds from your microphone and drives these parameters. You don't manually control them while streaming.

---

## Step 4: Combining Parameters

Mouth parameters work together. Cubism blends them automatically, but some combos need manual adjustment — same concept as [checking eye corners](live2d-eye-rigging-guide-v2.md#step-3-check-the-corners).

### Combos To Check

| Combo | What It Looks Like | Common Problem |
|:---|:---|:---|
| **Open + Smile** | Happy talking | Mouth may stretch too wide — adjust at both values |
| **Open + Frown** | Sad talking, yelling | Lower lip may clip through chin |
| **Open + each vowel** | Talking with shapes | Some vowels may fight the open parameter |
| **Smile + vowels** | Happy talking with lip sync | Corners may distort at extremes |

> Set both parameters to their extreme values at the same time and check if it looks right. If not, adjust the mesh at that combination.

---

## Common Mistakes

| Problem | Why It Happens | Fix |
|:---|:---|:---|
| **Mouth looks like a puppet** | Upper and lower lip move equally | Lower lip does **70-80%** of the movement |
| **Teeth float in space** | Teeth not rigged to same parameter | Rig teeth on `ParamMouthOpenY` |
| **Interior visible when closed** | Interior mesh too big at keyform 0 | Shrink or hide interior at closed position |
| **Smile looks like a grimace** | Moving center of mouth instead of corners | Only move the **corners** for smile/frown |
| **Vowels all look the same** | Not enough shape variation | Exaggerate each shape — A is wide, U is tiny, I is flat |
| **Lip sync doesn't trigger** | VTube Studio not configured | Check audio input + parameter mapping in VTS settings |

---

## Quick Reference

| I Need To... | Where It Is | More Info |
|:---|:---|:---:|
| **Open/close the mouth** | `ParamMouthOpenY` (0 = closed, 1 = open) | [Step 1](#step-1-mouth-openclose) |
| **Add smile/frown** | `ParamMouthForm` (-1 = frown, +1 = smile) | [Step 2](#step-2-smilefrown) |
| **Add lip sync** | `ParamA`, `ParamI`, `ParamU`, `ParamE`, `ParamO` | [Step 3](#step-3-lip-sync-shapes-vowels) |
| **Fix weird combos** | Set two parameters to extremes, check viewport | [Combining](#step-4-combining-parameters) |
| **Use Deformation Tool** | Top toolbar, lasso icon | [Tools (Eye Guide)](live2d-eye-rigging-guide-v2.md#tools--which-one-and-when) |

---

*Same tools, new layers. If you rigged the eyes, you already know the workflow.*
