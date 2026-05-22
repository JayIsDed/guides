# Live2D Cubism — Physics, Hair & Accessories Guide

**Starting point:** Head rotation rigged ([Head & Body Guide](live2d-head-body-rigging-guide-v4.md)). Eyes done, including googly physics ([Eye Guide — Jiggle Physics](live2d-eye-rigging-guide-v4.md#step-5-googly-eyes--the-jiggle-physics-route)). This guide covers making things flow — hair, earrings, ribbons, tails, anything that reacts to movement.

**Jump to:** [How Physics Works](#how-physics-works) | [Hair Setup](#hair-setup) | [Multi-Input](#multi-input) | [Accessories](#accessories) | [Hair Presets](#hair-presets) | [Accessory Presets](#accessory-presets) | [Tuning By Feel](#tuning-by-feel) | [Common Mistakes](#common-mistakes) | [Quick Reference](#quick-reference)

---

## How Physics Works

you literally already did this with googly eyes. same panel, same pendulum, different thing swinging.

every physics object in Live2D — eyes, hair, earrings, tails, ribbons — is the exact same thing under the hood: a **pendulum**. a weight on a string. head moves, weight swings. that's the entire physics engine. not exaggerating. the whole thing is just pendulums all the way down.

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

the pendulum has exactly four knobs. you already tuned these for googly eyes — the only thing that changed is what's hanging off the string. an eyeball, a ponytail, an earring — the engine doesn't care. it's all just "thing on a string."

**Length** is how long the string is. short string barely moves. long string swings wide. you felt this one when you were dialing in how far the iris drifted.

**Damping** is friction. you already know this one intimately — it's what made the difference between "normal eyes" and "unhinged googly chaos." low damping = keeps swinging forever. high damping = stops fast.

**Speed** is oscillation frequency. low speed feels heavy and lazy, like a thick braid. high speed feels light and snappy, like an ahoge spring. think about how different the googly eye jiggle felt when you cranked this up vs down.

**Reaction Force** is the sensitivity threshold — how much movement it takes before the pendulum even bothers to swing. crank it up and it reacts to everything. dial it down and it only cares about big movements.

### Input vs Output

same concept from the [eye jiggle setup](live2d-eye-rigging-guide-v4.md#step-5-googly-eyes--the-jiggle-physics-route), just pointed at different parts.

**Input** is what CAUSES the movement. for googly eyes, input was head rotation. for hair? also head rotation. for a necklace, maybe body sway or a nod. input is always "what body part moved that should make this dangly thing react."

**Output** is what GETS moved. for googly eyes, output was the eyeball position parameter. for hair, output is a hair sway parameter. for earrings, an earring swing parameter. output is always "the dangly thing."

same pendulum, different weight. if you can jiggle an eyeball, you can flow a ponytail. genuinely the same system.

---

## Hair Setup

hair physics is three layers of work, and the order matters. you need **parameters** first (names for the motion), then **keyforms** on those parameters (how far the hair CAN move), then **physics** attached to the parameters (when the hair DOES move).

skipping straight to the physics panel is the most common mistake in this entire guide. physics can only drive parameters that already exist with keyforms already rigged. it's like plugging a guitar into an amp that isn't connected to a speaker — the signal goes in but nothing comes out.

### A. Create Hair Parameters

before anything touches the physics panel, each hair section needs its own sway parameter. think of these as the "slots" that physics will eventually plug into. right-click in the Parameter List, create a new parameter for each hair section your model has.

the standard set is **ParamHairFront** for bangs, **ParamHairSideL** and **ParamHairSideR** for the strands framing the face, and **ParamHairBack** for the back. if your model has an ahoge (antenna hair), add **ParamHairAhoge** too. all of them use a range of -10 to +10, with 0 as the resting center.

you don't need all of these — only create parameters for hair sections that actually exist on your model. short hair character? maybe just ParamHairFront. ponytail character? might skip Front entirely and only use ParamHairBack. match the model, not the template.

### B. Rig Keyforms on Those Parameters

this defines the boundaries of motion — how far left and how far right the hair is allowed to sway. without keyforms, the parameter exists but does nothing. physics would slide the number and nothing would visually happen. that's the "plugged into an amp with no speaker" situation.

the process is identical to how you rigged eye movement. select the hair mesh layer in the Parts List, find your new hair parameter at the bottom, click the `...` menu on the parameter bar, and insert keyforms at -10, 0, and +10.

at 0 (center), don't touch anything — this is the resting position. at -10, use the Deformation Tool (not the Arrow tool — [same rule as eyes](live2d-eye-rigging-guide-v4.md#where-things-are-in-cubism)) and drag the hair mesh to the LEFT. that's the maximum sway in one direction. at +10, drag it to the RIGHT. maximum sway the other way.

how far you drag determines how dramatic the physics will look. conservative drags = subtle and grounded. dramatic drags = wild and flowy. start conservative — you can always widen the keyforms later, but physics that clip through the face are painful to debug. ask me how I know.

repeat for every hair section you created a parameter for. each section gets its own keyform range tuned to how that specific chunk of hair should move.

### C. Attach Physics

now the physics panel enters the picture. this is where the pendulum actually gets connected — and it's the same panel you used for googly eyes.

open the Physics panel from the top menu (Modeling > Physics). create a new physics group — name it something obvious like "Front Hair" or "Side Hair L." for each group, you need to wire up the input and the output.

set the **Input** to head rotation parameters. at minimum, use `ParamAngleX` (head turn left/right). this is what causes the hair to react. set the **Output** to the hair parameter you created — for example, `ParamHairFront`. this is what gets moved when the pendulum swings.

now set the four physics values. if you're not sure where to start, grab values from the [Hair Presets](#hair-presets) table below — that's literally what they're there for. hit the play button in the physics panel, move the head parameters around, and watch the hair react. adjust until it feels right.

repeat for each hair section: create a physics group, wire input to head params, wire output to the hair param, set values, test. each section should be its own physics group with its own settings — front bangs should feel different from a heavy ponytail. if they all move identically, your audience will notice even if they can't articulate why.

---

## Multi-Input

if your hair only reacts to `ParamAngleX` (head turning left/right), it will only sway when the head turns. that looks... okay. but it looks flat. real hair reacts to everything. tilt your head and hair falls to one side. lean forward and it swings. your physics should do the same.

the fix is adding multiple input parameters to the same physics group. instead of just `ParamAngleX`, add `ParamAngleZ` (head tilt) so the hair falls when the head tilts to the side. add `ParamBodyAngleX` (body sway) so the hair follows whole-body movement, not just head turns.

you do this in the Input section of each physics group — just add more parameters to the input list. each input contributes to the pendulum's swing. more inputs = more natural, reactive-feeling hair. the physics engine blends all the inputs together automatically. you don't have to manage the math.

the effect is subtle but unmistakable. single-input hair looks like it's on a track. multi-input hair looks like it's actually attached to a person who moves in three dimensions. it's the same amount of work either way — just a couple extra entries in the input list — but the quality jump is significant. this is one of those "five minutes of work, massive visual payoff" situations.

this applies to accessories too. an earring that only reacts to `ParamAngleX` will swing left/right but completely ignore tilting. add `ParamAngleZ` and suddenly it dangles when the head tilts. same principle.

---

## Accessories

earrings, ribbons, necklaces, tails, scarves, capes — the process is identical to hair. create a parameter, rig keyforms at -10/0/+10, open the physics panel, wire input and output, set values. if you can rig hair, you can rig any accessory. the only differences are the parameter names, which inputs make sense, and how heavy/fast the pendulum should feel.

for **earrings**, create `ParamEarringL` and `ParamEarringR` (one per ear — they should swing independently, like how you gave each googly eye its own physics group). wire input to `ParamAngleX` and `ParamAngleZ` since earrings react to both turning and tilting. use a short pendulum with fast settle — earrings are small and light, they shouldn't swing like a ponytail.

for **ribbons and bows**, create `ParamRibbon` (or per-ribbon if there are multiple). wire input to `ParamAngleX` and `ParamAngleZ`. ribbons are lighter than hair but flow more — medium length, low damping so they keep moving a bit. think "tissue paper in a breeze."

for **necklaces**, create `ParamNecklace` and wire it to `ParamAngleY` (nodding) and `ParamBodyAngleX` (body lean) instead of the usual X rotation. necklaces respond more to vertical movement than horizontal — they don't swing when you turn your head, they swing when you lean forward. high damping, low reaction force — a necklace barely moves unless there's significant motion.

for **tails**, create `ParamTail` and wire it to `ParamBodyAngleX` and `ParamBodyAngleZ`. tails are attached to the body, not the head, so body movement should be the primary input rather than head rotation. long pendulum, slow speed — tails are heavy. think "lazy cat tail, not excited puppy tail." unless you want excited puppy tail, in which case crank the speed.

for **scarves and capes**, use `ParamScarf` (or `ParamCape`) wired to `ParamAngleX` and `ParamBodyAngleX`. these are the most dramatic physics objects — long string, low damping, high reaction force. they should flow and billow. this is where you get to be cinematic about it.

starting values for each are in the [Accessory Presets](#accessory-presets) table below.

---

## Hair Presets

copy these as starting points. every model is different — tune to taste after testing. these are starting lines, not finish lines.

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

the ahoge preset is basically "maximum chaos on a tiny part." speed 2.5 with damping 0.2 means it just never stops bouncing. it's the ahoge spring — it reacts to everything and it's delightful. if your model has one, absolutely rig it.

the ponytail is the opposite end — it's the "big heavy thing" preset. length 9, speed 0.6. this thing swings like a real ponytail. it takes a beat to get going and a beat to stop. the weight is the point.

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

notice how everything is just different tunings of the same four knobs. cape at length 10, speed 0.5, damping 0.25 — that's the "dramatic anime wind scene" preset. earrings at length 3, speed 2.0 — that's "small light thing that reacts fast." the pendulum doesn't care what it's simulating. you're just telling it how heavy, how long, and how much friction.

---

## Tuning By Feel

when something doesn't feel right and you can't articulate why, start here. this is the "it vibes wrong" diagnostic table.

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

if the hair jitters, that's almost always "speed too high, damping too low." the pendulum is oscillating faster than it can settle, so it fights itself. lower the speed first, then bump damping if it's still twitchy. same thing that happened if you went too wild with the googly eye settings — drunk eyes and jittering hair are the same bug, different parameter.

---

## Common Mistakes

| Problem | Cause | Fix |
|:---|:---|:---|
| **Hair doesn't move at all** | No physics group, or input not wired | Open Physics panel, check input has head params connected |
| **Parameter slider moves but nothing visual happens** | No keyforms rigged on the hair parameter | [Rig keyforms first](#b-rig-keyforms-on-those-parameters) — physics drives the slider, keyforms drive the mesh |
| **Hair clips through the face** | Keyform range too wide at -10/+10 | Reduce how far you dragged the mesh, or add a [clipping mask](live2d-eye-rigging-guide-v4.md#step-4-clipping-mask-keep-the-iris-inside-the-eye) |
| **All hair sections move identically** | Same physics values on every group | Give each section different Length/Damping/Speed — front bangs shouldn't feel like a ponytail |
| **Hair jitters or vibrates** | Speed too high + damping too low | Reduce speed or increase damping — see [Tuning By Feel](#tuning-by-feel) |
| **Hair only sways on one axis** | Only `ParamAngleX` as input | Add `ParamAngleZ` and `ParamBodyAngleX` — see [Multi-Input](#multi-input) |
| **Earrings swing like a ponytail** | Length too high for a small object | Earrings should be Length 3-4, not 8+ — see [Accessory Presets](#accessory-presets) |
| **Necklace doesn't react** | Input set to head turn instead of nod/body | Wire necklace input to `ParamAngleY` and `ParamBodyAngleX` — see [Accessories](#accessories) |
| **Accessory feels detached from the model** | Wrong input parameter for that body part | Match input to what the accessory is physically attached to — head accessories get head params, body accessories get body params |
| **Went straight to physics, skipped params/keyforms** | Assumed physics creates motion directly | Physics drives parameters. [Parameters need keyforms](#a-create-hair-parameters). Keyforms are the actual mesh deformation. All three layers required. |

the "slider moves but nothing happens" one is the classic. you'll do it at least once. everyone does. you get excited, go straight to the physics panel, wire everything up, hit play, and... the slider moves but the model just sits there. because there are no keyforms. the parameter has nowhere to go. it's like a light switch wired to nothing.

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
| **Compare to googly eye physics** | Same system, different values | [Eye Guide — Jiggle Physics](live2d-eye-rigging-guide-v4.md#step-5-googly-eyes--the-jiggle-physics-route) |
| **Head rotation setup** | Prereq for physics — head must be rigged first | [Head & Body Guide](live2d-head-body-rigging-guide-v4.md) |
| **Mouth rigging** | Next guide in the series | [Mouth Guide](live2d-mouth-rigging-guide-v4.md) |

---

*same engine, different costume. the pendulum doesn't care if it's an eyeball or a ponytail — it just swings.*
