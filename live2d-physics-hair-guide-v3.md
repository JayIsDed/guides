# Live2D Cubism — Physics, Hair & Accessories Guide

**Starting point:** Head rotation rigged ([Head & Body Guide](live2d-head-body-rigging-guide.md)). Eyes done, including googly physics ([Eye Guide — Jiggle Physics](live2d-eye-rigging-guide-v3.md#jiggle-physics-setup)). This guide covers making things flow — hair, earrings, ribbons, tails, anything that reacts to movement.

**Jump to:** [How Physics Works](#how-physics-works) | [Hair Setup](#hair-setup) | [Multi-Input](#multi-input) | [Accessories](#accessories) | [Hair Presets](#hair-presets) | [Accessory Presets](#accessory-presets) | [Tuning By Feel](#tuning-by-feel) | [Common Mistakes](#common-mistakes) | [Quick Reference](#quick-reference)

---

## How Physics Works

If you rigged [googly eyes](live2d-eye-rigging-guide-v3.md#jiggle-physics-setup), you already know this system. Every physics object in Live2D — eyes, hair, earrings, tails, ribbons — is the same thing under the hood: a **pendulum**. A weight on a string. The head moves, the weight swings. That's the entire physics engine.

```
           ANCHOR
             |
             |     L = length of the string
             |     (how far the weight CAN swing)
             |
             O  <-- WEIGHT (hair tip, earring, tail end)


    Head moves RIGHT -->

             \
              \        The weight swings LEFT
               \       (opposite the movement)
                \
                 O     Then swings back...
                            and settles.
```

The pendulum model has exactly four knobs. You already tuned these for googly eyes — the only difference here is what's hanging off the string.

**Length** controls how long the string is. A short string barely moves; a long string swings wide. **Damping** controls friction — low damping means the pendulum keeps swinging forever, high damping kills the motion fast. **Speed** is oscillation frequency — low speed feels heavy and lazy, high speed feels light and snappy. **Reaction Force** is the sensitivity threshold — how much head movement it takes before the pendulum cares enough to swing.

### Input vs Output

This is the same concept from the [eye jiggle setup](live2d-eye-rigging-guide-v3.md#jiggle-physics-setup), just applied to different parts.

**Input** is what CAUSES the movement. For googly eyes, input was head rotation. For hair, input is also head rotation. For a necklace, input might be body sway or a nod. The input is always "what body part moved."

**Output** is what GETS moved. For googly eyes, output was the eyeball position parameter. For hair, output is a hair sway parameter. For earrings, output is an earring swing parameter. The output is always "what dangly thing reacts."

Same pendulum, different weight. If you can jiggle an eyeball, you can flow a ponytail.

---

## Hair Setup

Hair physics involves three layers of work, and the order matters. You need **parameters** first (names for the motion), then **keyforms** on those parameters (how far the hair CAN move), then **physics** attached to the parameters (when the hair DOES move). Skipping straight to the physics panel is the most common mistake — physics can only drive parameters that already exist with keyforms already rigged.

### A. Create Hair Parameters

Before anything touches the physics panel, each hair section needs its own sway parameter. Think of these as the "slots" that physics will eventually plug into. Right-click in the Parameter List, create a new parameter for each hair section your model has.

The standard set is **ParamHairFront** for bangs, **ParamHairSideL** and **ParamHairSideR** for the strands framing the face, and **ParamHairBack** for the back. If your model has an ahoge (antenna hair), add **ParamHairAhoge** too. All of them use a range of -10 to +10, with 0 as the resting center.

You don't need all of these — only create parameters for hair sections that actually exist on your model. A character with short hair might only need ParamHairFront. A character with a ponytail might skip Front entirely and only use ParamHairBack.

### B. Rig Keyforms on Those Parameters

This is the part that defines the boundaries of motion — how far left and how far right the hair is allowed to sway. Without keyforms, the parameter exists but does nothing. Physics would drive the slider and nothing would visually happen.

The process is identical to how you rigged eye movement. Select the hair mesh layer in the Parts List, find your new hair parameter at the bottom, click the `...` menu on the parameter bar, and insert keyforms at -10, 0, and +10.

At 0 (center), don't touch anything — this is the resting position. At -10, use the Deformation Tool (not the Arrow tool — [same rule as eyes](live2d-eye-rigging-guide-v3.md#tools--which-one-and-when)) and drag the hair mesh to the LEFT. This is the maximum sway in one direction. At +10, drag the hair mesh to the RIGHT. This is the maximum sway the other direction.

How far you drag determines how dramatic the physics will look. Conservative drags make the physics feel subtle and grounded. Dramatic drags make the physics feel wild and flowy. Start conservative — you can always widen the keyforms later, but physics that clip through the face are painful to debug.

Repeat this for every hair section you created a parameter for. Each section gets its own keyform range tuned to how that specific chunk of hair should move.

### C. Attach Physics

Now the physics panel enters the picture. This is where the pendulum actually gets connected.

Open the Physics panel from the top menu (Modeling > Physics). Create a new physics group — name it something obvious like "Front Hair" or "Side Hair L." For each group, you need to wire up the input and the output.

Set the **Input** to head rotation parameters. At minimum, use `ParamAngleX` (head turn left/right). This is what causes the hair to react. Set the **Output** to the hair parameter you created — for example, `ParamHairFront`. This is what gets moved when the pendulum swings.

Now set the four physics values. If you're not sure where to start, grab values from the [Hair Presets](#hair-presets) table below. Hit the play button in the physics panel, move the head parameters around, and watch the hair react. Adjust until it feels right.

Repeat this process for each hair section: create a physics group, wire input to head params, wire output to the hair param, set values, test. Each section should be its own physics group with its own settings — front bangs should feel different from a heavy ponytail.

---

## Multi-Input

If your hair only reacts to `ParamAngleX` (head turning left/right), it will only sway when the head turns. That looks okay, but it looks flat — real hair reacts to everything. Tilting your head makes hair fall to one side. Leaning forward makes hair swing forward.

The fix is adding multiple input parameters to the same physics group. Instead of just `ParamAngleX`, add `ParamAngleZ` (head tilt) so the hair falls when the head tilts to the side. Add `ParamBodyAngleX` (body sway) so the hair follows whole-body movement, not just head turns.

You do this in the Input section of each physics group — just add more parameters to the input list. Each input contributes to the pendulum's swing. More inputs means more natural, reactive-feeling hair. The physics engine blends all the inputs together automatically.

The effect is subtle but unmistakable. Single-input hair looks like it's on a track. Multi-input hair looks like it's actually attached to a person who moves in three dimensions. It's the same amount of work either way — just a couple extra entries in the input list — but the quality jump is significant.

This applies to accessories too. An earring that only reacts to `ParamAngleX` will swing left/right but ignore tilting. Add `ParamAngleZ` and suddenly it dangles when the head tilts. Same principle.

---

## Accessories

Earrings, ribbons, necklaces, tails, scarves, capes — the process is identical to hair. Create a parameter, rig keyforms at -10/0/+10, open the physics panel, wire input and output, set values. If you can rig hair, you can rig any accessory. The only differences are the parameter names, which inputs make sense, and how heavy/fast the pendulum should feel.

For **earrings**, create `ParamEarringL` and `ParamEarringR` (one per ear — they should swing independently). Wire input to `ParamAngleX` and `ParamAngleZ` since earrings react to both turning and tilting. Use a short pendulum with fast settle — earrings are small and light, they shouldn't swing like a ponytail.

For **ribbons and bows**, create `ParamRibbon` (or per-ribbon if there are multiple). Wire input to `ParamAngleX` and `ParamAngleZ`. Ribbons are lighter than hair but flow more — medium length, low damping so they keep moving a bit.

For **necklaces**, create `ParamNecklace` and wire it to `ParamAngleY` (nodding) and `ParamBodyAngleX` (body lean) instead of the usual X rotation. Necklaces respond more to vertical movement than horizontal. High damping, low reaction force — a necklace barely moves unless there's significant motion.

For **tails**, create `ParamTail` and wire it to `ParamBodyAngleX` and `ParamBodyAngleZ`. Tails are attached to the body, not the head, so body movement should be the primary input rather than head rotation. Long pendulum, slow speed — tails are heavy.

For **scarves and capes**, use `ParamScarf` (or `ParamCape`) wired to `ParamAngleX` and `ParamBodyAngleX`. These are the most dramatic physics objects — long string, low damping, high reaction force. They should flow and billow.

Starting values for each are in the [Accessory Presets](#accessory-presets) table below.

---

## Hair Presets

Copy these as starting points. Every model is different — tune to taste after testing.

| Style | Length | Damping | Speed | Reaction | Result |
|:---|:---:|:---:|:---:|:---:|:---|
| **Short bangs** | 4 | 0.7 | 1.2 | Medium | Stiff, quick settle — short or thick hair |
| **Long flowing** | 8 | 0.3 | 0.8 | High | Big swings, slow settle — long straight hair |
| **Curly / bouncy** | 5 | 0.4 | 1.8 | High | Quick bounces, medium settle — wavy or curly |
| **Ponytail** | 9 | 0.3 | 0.6 | Medium | Heavy pendulum — swings wide and slow |
| **Ahoge** | 3 | 0.2 | 2.5 | High | Snappy spring — bounces on every movement |
| **Side strands** | 6 | 0.5 | 1.0 | Medium | Balanced sway — visible but not distracting |
| **Twin tails** | 7 | 0.35 | 0.9 | High | Each side independent, asymmetric swing |
| **Hime cut** | 5 | 0.6 | 1.0 | Medium | Controlled, elegant — not too wild |

---

## Accessory Presets

| Accessory | Length | Damping | Speed | Reaction | Result |
|:---|:---:|:---:|:---:|:---:|:---|
| **Earrings (light)** | 3 | 0.6 | 2.0 | High | Quick dangle, fast settle |
| **Earrings (heavy)** | 4 | 0.7 | 1.0 | Medium | Slower swing, weighted feel |
| **Ribbon / bow** | 6 | 0.3 | 1.2 | Medium | Flowing, keeps moving |
| **Necklace** | 5 | 0.8 | 0.8 | Low | Barely moves, subtle weight |
| **Scarf** | 8 | 0.2 | 0.7 | High | Dramatic flow, long settle |
| **Cape** | 10 | 0.25 | 0.5 | High | Wide sweeping, cinematic |
| **Tail (short)** | 5 | 0.5 | 1.5 | Medium | Perky, reactive |
| **Tail (long)** | 10 | 0.3 | 0.5 | Medium | Slow, heavy pendulum |
| **Antenna / ahoge** | 3 | 0.2 | 2.5 | High | Same as hair ahoge — snappy spring |

---

## Tuning By Feel

| It feels... | Adjust | Direction | Why |
|:---|:---|:---|:---|
| **Too stiff** | Length | Increase | Longer string = more room to swing |
| **Too floppy / won't stop** | Damping | Increase | More friction = faster settle |
| **Too slow / heavy** | Speed | Increase | Faster oscillation = lighter feel |
| **Doesn't react to small movements** | Reaction Force | Increase | Lower threshold = more sensitive |
| **Overreacts to everything** | Reaction Force | Decrease | Higher threshold = only big movements trigger it |
| **Moves too far** | Length | Decrease | Or reduce keyform range at -10/+10 |
| **Settles too fast** | Damping | Decrease | Less friction = longer ring-out |
| **Jitters / vibrates** | Speed + Damping | Reduce Speed, raise Damping | Over-tuned — oscillation is fighting itself |
| **Feels disconnected** | Inputs | Add more [multi-input](#multi-input) params | Single-axis motion looks robotic |

---

## Common Mistakes

| Problem | Cause | Fix |
|:---|:---|:---|
| **Hair doesn't move at all** | No physics group, or input not wired | Open Physics panel, check input has head params connected |
| **Parameter slider moves but nothing visual happens** | No keyforms rigged on the hair parameter | [Rig keyforms first](#b-rig-keyforms-on-those-parameters) — physics drives the slider, keyforms drive the mesh |
| **Hair clips through the face** | Keyform range too wide at -10/+10 | Reduce how far you dragged the mesh, or add a [clipping mask](live2d-eye-rigging-guide-v3.md#step-4-clipping-mask) |
| **All hair sections move identically** | Same physics values on every group | Give each section different Length/Damping/Speed — front bangs shouldn't feel like a ponytail |
| **Hair jitters or vibrates** | Speed too high + damping too low | Reduce speed or increase damping — see [Tuning By Feel](#tuning-by-feel) |
| **Hair only sways on one axis** | Only `ParamAngleX` as input | Add `ParamAngleZ` and `ParamBodyAngleX` — see [Multi-Input](#multi-input) |
| **Earrings swing like a ponytail** | Length too high for a small object | Earrings should be Length 3-4, not 8+ — see [Accessory Presets](#accessory-presets) |
| **Necklace doesn't react** | Input set to head turn instead of nod/body | Wire necklace input to `ParamAngleY` and `ParamBodyAngleX` — see [Accessories](#accessories) |
| **Accessory feels detached from the model** | Wrong input parameter for that body part | Match input to what the accessory is physically attached to — head accessories get head params, body accessories get body params |
| **Went straight to physics, skipped params/keyforms** | Assumed physics creates motion directly | Physics drives parameters. [Parameters need keyforms](#a-create-hair-parameters). Keyforms are the actual mesh deformation. All three layers required. |

---

## Quick Reference

| I need to... | Go to | More info |
|:---|:---|:---|
| **Create a hair parameter** | Parameter List > right-click > Create Parameter | [Create Hair Parameters](#a-create-hair-parameters) |
| **Rig hair keyforms** | Same as eye keyforms — Deformation Tool at -10, 0, +10 | [Rig Keyforms](#b-rig-keyforms-on-those-parameters) |
| **Open the physics panel** | Top menu > Modeling > Physics | [Attach Physics](#c-attach-physics) |
| **Pick starting values for hair** | Copy from preset table | [Hair Presets](#hair-presets) |
| **Pick starting values for accessories** | Copy from preset table | [Accessory Presets](#accessory-presets) |
| **Make movement more natural** | Add multiple inputs to one physics group | [Multi-Input](#multi-input) |
| **Fix something that feels wrong** | Check the tuning table | [Tuning By Feel](#tuning-by-feel) |
| **Review how the pendulum works** | Same as googly eye physics | [How Physics Works](#how-physics-works) |
| **Compare to googly eye physics** | Same system, different values | [Eye Guide — Jiggle Physics](live2d-eye-rigging-guide-v3.md#jiggle-physics-setup) |
| **Head rotation setup** | Prereq for physics — head must be rigged first | [Head & Body Guide](live2d-head-body-rigging-guide.md) |

---

*Same engine, different costume. The pendulum doesn't care if it's an eyeball or a ponytail — it just swings.*
