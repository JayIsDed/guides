# Live2D Cubism — Mouth Rigging Guide

**Prereq:** Eyes are rigged. If you haven't done that yet, start with the [Eye Rigging Guide](live2d-eye-rigging-guide-v4.md) and come back.

**Jump to:** [Layers You Need](#layers-you-need) | [Open/Close](#mouth-openclose--parammouthopeny) | [Smile/Frown](#smilefrown--parammouthform) | [Lip Sync Vowels](#lip-sync-vowel-shapes) | [Combining Parameters](#combining-parameters) | [Common Mistakes](#common-mistakes) | [Quick Reference](#quick-reference) | [Next Steps](#next-steps)

---

## Layers You Need

before you touch a single parameter, check your PSD. you need three layers separated at minimum: **upper lip**, **lower lip**, and **mouth interior** (the dark inside-of-mouth shape). that trio gets you a working mouth. everything else is polish.

**Teeth** (upper row, lower row) add a lot of believability for barely any extra work. upper teeth peek down from behind the upper lip when the mouth opens. lower teeth ride with the lower lip -- they move together, so you can parent them or just rig them on the same parameter with similar movement.

**Tongue** is optional but worth it if your model talks a lot. it adds life to vowel shapes, especially the "Ah" and "Oh" sounds where the tongue is visible. if your art doesn't have a tongue drawn, skip it -- you can always add one later.

| Layer | Required? | What It Does |
|:---|:---:|:---|
| Upper lip | Yes | Top edge of the mouth. Barely moves during open/close. |
| Lower lip | Yes | Bottom edge. Does most of the work. |
| Mouth interior | Yes | Dark shape visible when mouth opens. Hidden when closed. |
| Teeth (upper) | No | Peeks down from behind upper lip when open. |
| Teeth (lower) | No | Rides down with the lower lip. |
| Tongue | No | Adds life to vowel shapes. Visible during A and O. |

---

## Mouth Open/Close — `ParamMouthOpenY`

this is the foundation of your mouth rig, and the single most important thing to get right. parameter goes from 0 (closed) to 1 (fully open). here's the thing that trips up almost every beginner: **the lower lip does 70-80% of the movement.**

think about your own face for a second. when you open your mouth, your jaw drops. your upper lip barely moves -- it might lift a tiny amount, but the jaw is doing the heavy lifting. if you move both lips equally, you get a puppet mouth. that mechanical "flap flap" look where it seems like the face is splitting in half. not great.

```
     Closed (0)                    Open (1)
                                      _____
     __________                      / top  \  <-- barely moves
    /          \                    /  teeth  \
    \__________/                    |  dark   |
                                    \ space  /
         ^                           \_____/  <-- lower lip drops
    both lips                            ^
    touching                        jaw does the work
```

**A. Start with the lower lip.** select it in the Parts List, find `ParamMouthOpenY` in the Parameter List (or create it, range 0 to 1), and add keyforms at 0 and 1. remember the keyform thing from eyes? same deal here -- click the three dots on the parameter bar, Insert Keyform. at slider 0, don't touch anything -- that's your default closed state. at slider 1, grab the Deformation Tool and drag the lower lip **down**. how far depends on how dramatic you want the open to look, but a good starting point is about the height of the eye opening.

**B. Now the upper lip.** same parameter, same keyforms. at slider 1, nudge the upper lip **slightly up**. we're talking subtle -- maybe 20-30% of the distance you moved the lower lip. if it feels like you're barely moving it, you're probably in the right range.

**C. The mouth interior** needs to appear and disappear. at slider 0 (closed), either scale the interior mesh very small or hide it entirely so it's invisible behind the closed lips. at slider 1 (open), scale and position it to fill the gap between the upper and lower lip. this is the dark space inside the mouth.

**D. Teeth**, if you have them. rig upper teeth on the same parameter. at slider 1, they should peek down just below the upper lip -- they stay relatively still since they're anchored to the skull, not the jaw. lower teeth move down with the lower lip. you can match the lower lip's movement roughly, or parent the lower teeth to the lower lip layer.

**E. Test it.** scrub the slider back and forth. the mouth should open smoothly, the interior should appear gradually, and it should look like a jaw dropping -- not a face splitting. if both lips are moving the same amount, go back and reduce the upper lip movement.

---

## Smile/Frown — `ParamMouthForm`

this parameter controls **emotion**, not talking. it goes from -1 (frown) through 0 (neutral) to +1 (smile). the critical thing to understand: smile and frown happen at the **corners** of the mouth, not the center.

watch someone smile in real life. the middle of their lips doesn't really move up -- the corners pull up and outward. same with a frown: the corners pull down. if you move the entire mouth uniformly, you get something that looks more like a mouth teleporting vertically than an actual expression.

**A. Select both lip layers** (or work on them one at a time -- same parameter either way). create `ParamMouthForm` with range -1 to +1, and add keyforms at -1, 0, and +1. leave 0 alone -- it's neutral.

**B. At +1 (smile):** use the Deformation Tool on the **corners** of the lip meshes. pull them up and slightly outward. the lips may also widen a bit horizontally as the corners spread. the center of the lips should barely move. if you have a mouth interior layer, it can widen slightly at +1 to match the smile.

**C. At -1 (frown):** pull the corners **down** and slightly inward. the mouth may narrow a bit. this is usually more subtle than the smile -- most models don't need a dramatic frown unless you're going for a very expressive character.

**D. Check the overall shape.** a smile should curve the lips into an upward arc. a frown should droop them. if the whole mouth is just sliding up and down without changing shape, you're moving the center instead of the corners.

---

## Lip Sync Vowel Shapes

this is where your model goes from "mouth opens and closes" to "actually looks like it's talking." each Japanese vowel sound (A, I, U, E, O) gets its own parameter that deforms the mouth into a specific shape. these are optional, but if you're streaming with VTube Studio, they're worth the effort.

**VTube Studio auto-detects these.** when you talk into your mic, VTS analyzes the audio, figures out which vowel you're closest to, and drives the corresponding parameter. you don't manually control them while streaming -- you just talk and the model's mouth matches. all five parameters use a 0-to-1 range where 0 is neutral/closed and 1 is the full vowel shape.

here's what each shape looks like and why:

```
  "Ah" (ParamA)     "Ee" (ParamI)     "Oo" (ParamU)

    ___________        ___________        ___
   /           \      /           \      (   )
  |    wide     |     |___________/      (   )
  |    open     |      teeth show         \_/
   \___________/       stretched         small
                       sideways          puckered
   jaw drops,         mouth wide         lips push
   biggest shape      but flat           forward


  "Eh" (ParamE)     "Oh" (ParamO)

    _________          _______
   /         \        /       \
  |  halfway  |      |         |
   \_________/        \_______/
   relaxed,           round,
   between A          open circle
   and I              lips form O
```

**A. ParamA -- "Ah" as in "father."** this is the biggest, widest mouth shape. the jaw drops, the mouth opens wide, and the shape is roughly oval. if you already rigged `ParamMouthOpenY`, the A shape will look similar to full-open but potentially wider. the tongue is usually visible and flat at the bottom of the mouth.

**B. ParamI -- "Ee" as in "feet."** the mouth stretches **sideways** but stays relatively flat. teeth are often visible. think of a forced grin. the jaw doesn't drop much -- the corners pull out. this is one of the most distinct shapes, so really exaggerate the horizontal stretch.

**C. ParamU -- "Oo" as in "food."** small and puckered. the lips push forward and form a tiny circle. this is the smallest mouth shape -- basically the opposite of A. make it noticeably small; beginners tend to make U too big, and then it looks like O.

**D. ParamE -- "Eh" as in "bed."** a relaxed, half-open shape. it sits between A and I -- not as wide as A, not as flat as I. this is the most "neutral talking" shape. it doesn't need to be as dramatic as the others.

**E. ParamO -- "Oh" as in "boat."** round and open, with the lips forming a circle. bigger than U but rounder than A. the key difference from U is that O is open -- you can see into the mouth -- while U is puckered and almost closed.

for each vowel, the process is the same: create the parameter (0 to 1), add keyforms at 0 and 1, leave 0 as neutral, and deform the lips/interior/teeth at 1 to match the target shape. you already know how this works from eyes -- same workflow, just different shapes. work on upper lip, lower lip, and mouth interior together so the shapes stay consistent.

---

## Combining Parameters

mouth parameters blend together at runtime, and Cubism handles the interpolation automatically. but "automatically" doesn't mean "correctly." some combinations produce weird artifacts if you don't check them -- same concept as [checking eye corners](live2d-eye-rigging-guide-v4.md#step-3-check-the-corners) where extreme X+Y combos could break the iris position. same energy here.

the way to test is simple: set two parameters to their extreme values at the same time and look at the result in the viewport. if the mesh distorts, clips, or looks unnatural, you need to adjust the keyforms at that specific combination.

**A. Open + Smile** (`ParamMouthOpenY` = 1, `ParamMouthForm` = +1). this is "happy talking" or laughing. the mouth might stretch too wide because both the open and the smile are pulling the corners. if it looks like the Joker, reduce the smile's corner displacement slightly, or add a combined keyform that tames the extremes.

**B. Open + Frown** (`ParamMouthOpenY` = 1, `ParamMouthForm` = -1). yelling, crying, shock. watch for the lower lip clipping through the chin. the frown pulls corners down while the open pulls the jaw down -- that's a lot of downward movement stacking. you may need to limit how far the lower lip drops in this combo.

**C. Open + Each Vowel.** some vowels fight the open parameter because they define their own mouth shape. if you have both `ParamMouthOpenY` at 1 and `ParamA` at 1, the mouth might double-open or distort. test each vowel at full value with the mouth fully open and fully closed. in VTube Studio, the open parameter and vowel parameters typically work in coordination (VTS handles this), but it's worth verifying in the editor.

**D. Smile + Vowels.** happy talking with lip sync active. the corners from the smile plus the shape deformation from vowels can produce distortion at the edges of the mouth. the I shape (sideways stretch) combined with smile (corners up) is the most likely to look weird -- check that one first.

if a combo looks bad, you have two options: dial back one of the parameters' extremes so the blend is less aggressive, or add keyforms at the combined position so Cubism knows exactly what the mesh should look like when both parameters are active. the second approach is more work but gives you precise control. see? you already dealt with this in eyes -- corner corrections are the same concept.

---

## Common Mistakes

| Problem | Why It Happens | Fix |
|:---|:---|:---|
| Puppet mouth (mechanical flapping) | Upper and lower lip move equal amounts | Lower lip does **70-80%** of the work. Reduce upper lip movement. |
| Floating teeth | Teeth not rigged to `ParamMouthOpenY` | Rig teeth on the same parameter. Upper teeth stay mostly still; lower teeth follow the lower lip. |
| Interior visible when closed | Mouth interior mesh too large at keyform 0 | Shrink or hide the interior layer at the closed position. |
| Smile looks like a grimace | Moving the center of the mouth instead of corners | Only deform the **corners** for [smile/frown](#smilefrown--parammouthform). Center stays put. |
| All vowels look the same | Not enough shape variation between A/I/U/E/O | Exaggerate each shape. A = wide open, I = flat stretch, U = tiny pucker, E = relaxed half, O = round circle. |
| Lip sync doesn't fire in VTube Studio | Audio input or parameter mapping not configured | Check VTS settings: audio input device, lip sync sensitivity, and that your parameter names match what VTS expects. |
| Mouth distorts at combined extremes | Two parameters fighting each other | [Test combos](#combining-parameters) at extreme values. Add combined keyforms or dial back extremes. |
| U and O look identical | Both made "roundish" without enough size difference | U is **small and puckered** (lips pushed forward). O is **open and round** (you can see inside). Make U noticeably smaller. |

---

## Quick Reference

| I Need To... | Parameter | Range | More Info |
|:---|:---|:---|:---:|
| Open/close the mouth | `ParamMouthOpenY` | 0 (closed) to 1 (open) | [Open/Close](#mouth-openclose--parammouthopeny) |
| Add smile or frown | `ParamMouthForm` | -1 (frown) to +1 (smile) | [Smile/Frown](#smilefrown--parammouthform) |
| Lip sync -- "Ah" | `ParamA` | 0 to 1 | [Vowel Shapes](#lip-sync-vowel-shapes) |
| Lip sync -- "Ee" | `ParamI` | 0 to 1 | [Vowel Shapes](#lip-sync-vowel-shapes) |
| Lip sync -- "Oo" | `ParamU` | 0 to 1 | [Vowel Shapes](#lip-sync-vowel-shapes) |
| Lip sync -- "Eh" | `ParamE` | 0 to 1 | [Vowel Shapes](#lip-sync-vowel-shapes) |
| Lip sync -- "Oh" | `ParamO` | 0 to 1 | [Vowel Shapes](#lip-sync-vowel-shapes) |
| Fix weird combo distortions | Set both params to extremes, check viewport | -- | [Combining](#combining-parameters) |
| Use the Deformation Tool | Top toolbar, lasso icon | -- | [Tools (Eye Guide)](live2d-eye-rigging-guide-v4.md#where-things-are-in-cubism) |

---

## Next Steps

mouth done. eyes done. you already did the hard part -- the rest is head and body movement. head tilts, body sway, breathing. that's where your model stops being a face and starts being a character.

head over to the [Head & Body Rigging Guide](live2d-head-body-rigging-guide-v4.md) when you're ready.

after that, [Physics, Hair & Accessories](live2d-physics-hair-guide-v4.md) -- and if you set up googly eyes already, you've got a head start on how physics works in Cubism.

---

*your model can talk now. go say something weird with it.*
