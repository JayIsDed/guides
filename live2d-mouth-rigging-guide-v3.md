# Live2D Cubism — Mouth Rigging Guide

**Prereq:** Eyes are rigged. If you haven't done that yet, start with the [Eye Rigging Guide](live2d-eye-rigging-guide-v3.md) and come back.

**Jump to:** [Layers You Need](#layers-you-need) | [Open/Close](#mouth-openclose--parammouthopeny) | [Smile/Frown](#smilefrown--parammouthform) | [Lip Sync Vowels](#lip-sync-vowel-shapes) | [Combining Parameters](#combining-parameters) | [Common Mistakes](#common-mistakes) | [Quick Reference](#quick-reference) | [Next Steps](#next-steps)

---

## Layers You Need

Before touching any parameters, make sure your PSD has the right layers separated. At minimum you need three: **upper lip**, **lower lip**, and **mouth interior** (the dark inside). That trio gets you a working mouth. Everything else is polish.

**Teeth** (upper row, lower row) add a lot of believability for very little extra work. Upper teeth peek down from behind the upper lip when the mouth opens. Lower teeth ride with the lower lip -- they move together, so you can parent them or just rig them on the same parameter with similar movement.

**Tongue** is optional but worth it if your model talks a lot. It adds life to vowel shapes, especially the "Ah" and "Oh" sounds where the tongue is visible. If your art doesn't have a tongue drawn, skip it -- you can always add one later.

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

This is the foundation of your mouth rig, and the single most important thing to get right. The parameter goes from 0 (closed) to 1 (fully open). Here's the thing that trips up almost every beginner: **the lower lip does 70-80% of the movement.**

Think about your own face for a second. When you open your mouth, your jaw drops. Your upper lip barely moves -- it might lift a tiny amount, but the jaw is doing the heavy lifting. If you move both lips equally, you get a puppet mouth. That mechanical "flap flap" look where it seems like the face is splitting in half. Not great.

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

A. **Start with the lower lip.** Select it in the Parts List, find `ParamMouthOpenY` in the Parameter List (or create it, range 0 to 1), and add keyforms at 0 and 1. At slider 0, don't touch anything -- that's your default closed state. At slider 1, grab the Deformation Tool and drag the lower lip **down**. How far depends on how dramatic you want the open to look, but a good starting point is about the height of the eye opening.

B. **Now the upper lip.** Same parameter, same keyforms. At slider 1, nudge the upper lip **slightly up**. We're talking subtle -- maybe 20-30% of the distance you moved the lower lip. If it feels like you're barely moving it, you're probably in the right range.

C. **The mouth interior** needs to appear and disappear. At slider 0 (closed), either scale the interior mesh very small or hide it entirely so it's invisible behind the closed lips. At slider 1 (open), scale and position it to fill the gap between the upper and lower lip. This is the dark space inside the mouth.

D. **Teeth**, if you have them. Rig upper teeth on the same parameter. At slider 1, they should peek down just below the upper lip -- they stay relatively still since they're anchored to the skull, not the jaw. Lower teeth move down with the lower lip. You can match the lower lip's movement roughly, or parent the lower teeth to the lower lip layer.

E. **Test it.** Scrub the slider back and forth. The mouth should open smoothly, the interior should appear gradually, and it should look like a jaw dropping -- not a face splitting. If both lips are moving the same amount, go back and reduce the upper lip movement.

---

## Smile/Frown — `ParamMouthForm`

This parameter controls **emotion**, not talking. It goes from -1 (frown) through 0 (neutral) to +1 (smile). The critical thing to understand here: smile and frown happen at the **corners** of the mouth, not the center.

Watch someone smile in real life. The middle of their lips doesn't really move up -- the corners pull up and outward. Same with a frown: the corners pull down. If you move the entire mouth uniformly, you get something that looks more like a mouth teleporting vertically than an actual expression.

A. **Select both lip layers** (or work on them one at a time -- same parameter either way). Create `ParamMouthForm` with range -1 to +1, and add keyforms at -1, 0, and +1. Leave 0 alone -- it's neutral.

B. **At +1 (smile):** Use the Deformation Tool on the **corners** of the lip meshes. Pull them up and slightly outward. The lips may also widen a bit horizontally as the corners spread. The center of the lips should barely move. If you have a mouth interior layer, it can widen slightly at +1 to match the smile.

C. **At -1 (frown):** Pull the corners **down** and slightly inward. The mouth may narrow a bit. This is usually more subtle than the smile -- most models don't need a dramatic frown unless you're going for a very expressive character.

D. **Check the overall shape.** A smile should curve the lips into an upward arc. A frown should droop them. If the whole mouth is just sliding up and down without changing shape, you're moving the center instead of the corners.

---

## Lip Sync Vowel Shapes

This is where your model goes from "mouth opens and closes" to "actually looks like it's talking." Each Japanese vowel sound (A, I, U, E, O) gets its own parameter that deforms the mouth into a specific shape. These are optional, but if you're streaming with VTube Studio, they're worth the effort.

**VTube Studio auto-detects these.** When you talk into your mic, VTS analyzes the audio, figures out which vowel you're closest to, and drives the corresponding parameter. You don't manually control them while streaming -- you just talk and the model's mouth matches. All five parameters use a 0-to-1 range where 0 is neutral/closed and 1 is the full vowel shape.

Here's what each shape looks like and why:

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

A. **ParamA -- "Ah" as in "father."** This is the biggest, widest mouth shape. The jaw drops, the mouth opens wide, and the shape is roughly oval. If you already rigged `ParamMouthOpenY`, the A shape will look similar to full-open but potentially wider. The tongue is usually visible and flat at the bottom of the mouth.

B. **ParamI -- "Ee" as in "feet."** The mouth stretches **sideways** but stays relatively flat. Teeth are often visible. Think of a forced grin. The jaw doesn't drop much -- the corners pull out. This is one of the most distinct shapes, so really exaggerate the horizontal stretch.

C. **ParamU -- "Oo" as in "food."** Small and puckered. The lips push forward and form a tiny circle. This is the smallest mouth shape -- basically the opposite of A. Make it noticeably small; beginners tend to make U too big, and then it looks like O.

D. **ParamE -- "Eh" as in "bed."** A relaxed, half-open shape. It sits between A and I -- not as wide as A, not as flat as I. This is the most "neutral talking" shape. It doesn't need to be as dramatic as the others.

E. **ParamO -- "Oh" as in "boat."** Round and open, with the lips forming a circle. Bigger than U but rounder than A. The key difference from U is that O is open -- you can see into the mouth -- while U is puckered and almost closed.

For each vowel, the process is the same: create the parameter (0 to 1), add keyforms at 0 and 1, leave 0 as neutral, and deform the lips/interior/teeth at 1 to match the target shape. Work on upper lip, lower lip, and mouth interior together so the shapes stay consistent.

---

## Combining Parameters

Mouth parameters blend together at runtime, and Cubism handles the interpolation automatically. But "automatically" doesn't mean "correctly." Some combinations produce weird artifacts if you don't check them -- the same concept as [checking eye corners](live2d-eye-rigging-guide-v3.md#step-3-check-the-corners) where extreme X+Y combos could break the iris position.

The way to test is simple: set two parameters to their extreme values at the same time and look at the result in the viewport. If the mesh distorts, clips, or looks unnatural, you need to adjust the keyforms at that specific combination.

A. **Open + Smile** (`ParamMouthOpenY` = 1, `ParamMouthForm` = +1). This is "happy talking" or laughing. The mouth might stretch too wide because both the open and the smile are pulling the corners. If it looks like the Joker, reduce the smile's corner displacement slightly, or add a combined keyform that tames the extremes.

B. **Open + Frown** (`ParamMouthOpenY` = 1, `ParamMouthForm` = -1). Yelling, crying, shock. Watch for the lower lip clipping through the chin. The frown pulls corners down while the open pulls the jaw down -- that's a lot of downward movement stacking. You may need to limit how far the lower lip drops in this combo.

C. **Open + Each Vowel.** Some vowels fight the open parameter because they define their own mouth shape. If you have both `ParamMouthOpenY` at 1 and `ParamA` at 1, the mouth might double-open or distort. Test each vowel at full value with the mouth fully open and fully closed. In VTube Studio, the open parameter and vowel parameters typically work in coordination (VTS handles this), but it's worth verifying in the editor.

D. **Smile + Vowels.** Happy talking with lip sync active. The corners from the smile plus the shape deformation from vowels can produce distortion at the edges of the mouth. The I shape (sideways stretch) combined with smile (corners up) is the most likely to look weird -- check that one first.

If a combo looks bad, you have two options: dial back one of the parameters' extremes so the blend is less aggressive, or add keyforms at the combined position so Cubism knows exactly what the mesh should look like when both parameters are active. The second approach is more work but gives you precise control.

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
| Lip sync — "Ah" | `ParamA` | 0 to 1 | [Vowel Shapes](#lip-sync-vowel-shapes) |
| Lip sync — "Ee" | `ParamI` | 0 to 1 | [Vowel Shapes](#lip-sync-vowel-shapes) |
| Lip sync — "Oo" | `ParamU` | 0 to 1 | [Vowel Shapes](#lip-sync-vowel-shapes) |
| Lip sync — "Eh" | `ParamE` | 0 to 1 | [Vowel Shapes](#lip-sync-vowel-shapes) |
| Lip sync — "Oh" | `ParamO` | 0 to 1 | [Vowel Shapes](#lip-sync-vowel-shapes) |
| Fix weird combo distortions | Set both params to extremes, check viewport | -- | [Combining](#combining-parameters) |
| Use the Deformation Tool | Top toolbar, lasso icon | -- | [Tools (Eye Guide)](live2d-eye-rigging-guide-v3.md#tools--which-one-and-when) |

---

## Next Steps

Mouth done. Eyes done. The next piece is getting the **head and body** moving -- head tilts, body sway, breathing. That's where your model stops being a face and starts being a character.

Head over to the [Head & Body Rigging Guide](live2d-head-body-rigging-guide.md) when you're ready.

---

*Your model can talk now. Go say something weird with it.*
