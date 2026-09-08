# Blocking Spec — how to build and render a blocking the prompt can use

Written to be handed to whoever builds the blocking. Read it before you open Blender, not after.

---

## The single rule

**Every object the prompt must name has to be nameable from the blocking alone.**

Test it: point at any object in any frame and write one sentence that identifies it without
ambiguity. If that sentence needs *"the third one from the left, roughly"* — the blocking has
failed. Give the object a distinct colour and try again.

Everything below follows from that rule.

---

## Priority order

| # | Property | Why it matters |
|---|---|---|
| 1 | **Silhouette** | The model tracks outlines. An unclear outline breaks the motion itself, not just the identity. |
| 2 | **Separability** | Two touching objects of the same colour merge into one object in the model's read. |
| 3 | **Colour as ID** | The only identity signal that survives rotation, scale and motion blur. |
| 4 | **Matte material** | Exists only to protect 2 and 3. |
| 5 | Detail | Irrelevant. The prompt carries it. |

Simplicity is not the goal. Simplicity is the price paid for render speed. **Readability is the
goal.**

---

## Do

- **Flat matte shaders, saturated, clearly distinct hues.** Six objects that must be told apart
  need six hues far apart on the wheel — not six blues.
- **Maximum contrast against the background.** A flat black or flat mid-grey void is ideal: it
  reads unambiguously as a placeholder, and every object pops off it.
- **Primitives are fine.** Sphere for fruit, cube for ice, cylinder for a can, box for a person.
  The translation map converts them.
- **One unique object may be neutral.** A hero that is the only pale object in frame is
  identified by uniqueness alone. This works for exactly one object per shot.
- **Motion clearly above noise.** A move too small to read is a move the model will drop.
- **Leave black gaps where you have nothing.** They become abstract shots — underwater,
  particles, pure liquid. Better than blocking something badly.
- **Grids and tracking marks are allowed** — but tell the prompt writer they exist, so they get
  a destination in the text.
- **Keep the camera doing one clear thing per shot.** Orbit, or rise, or push in. A move that
  does three things at once reads as drift.
- **Decide which rig the camera is, and stay inside its limits.** Rotation rate is what reads as
  smooth or broken, and the acceptable rate is a property of the rig, not of taste: a 76 °/s whip
  is a defect on a gimbal or steadicam and the entire point on a motion-control arm. Handheld
  wants a slow body sway in long waves plus a small tremor, never fast jitter; a tripod wants
  none of it; an FPV drone tolerates rates that would ruin a dolly. Name the rig in the cut list
  so the prompt writer can describe the lens feel truthfully — "robo-arm snap", "steadicam
  glide", "handheld", "car mount", "probe macro" — and so nobody tries to smooth out a move that
  is meant to be violent.

## Do not

- **A colour far from the final colour on a shape that does not read as the object.** Measured
  on a real job: magenta discs became yellow lemon slices, while magenta spheres in the same
  frame, under the same prompt, stayed pink. The disc's silhouette says *slice of fruit*, so the
  model applied the translation; the sphere's says nothing, so the model kept the only signal it
  had — the colour. Text lost, three restatements notwithstanding: the prompt enters once,
  the colour is present in every frame at exact pixel coordinates.

  **The rule: colour may select a variant of an object whose shape already says what it is; it
  may not carry the identity of an object whose shape says nothing.** Six cylinders in six hues
  reading as six flavours of can works. A sphere that must become a lemon does not — render it
  near its final colour instead, and separate the objects by value and saturation rather than by
  hue (bright saturated hero, duller mid tone for the crowd, near-cream for slices).
- **Chrome, mirror, glass or heavy transparency on anything that needs an identity.** Chrome
  does not show its own colour, it shows the room. A green chrome can and a red chrome can are
  both just silver with smudges — the identity signal is dead.
- **Same-colour objects that overlap or touch.** They fuse into one shape.
- **A background sharing a colour with any object.** The object disappears into it.
- **Many tiny objects that must be individually named.** A mass is fine ("berries rise around
  it"); the moment you need *that specific berry*, it fails.
- **Emissive or deep shadow on identity-bearing objects.** Both crush hue.
- **A near-final look.** Counter-intuitive but real: the closer the blocking is to a finished
  render, the more the model copies its materials instead of your reference sheets. Crude and
  colourful beats polished and grey.

---

## Render settings

| Setting | Value | Why |
|---|---|---|
| **Resolution** | 1920×1080 | Standard, and unambiguous. |
| **Aspect** | **must equal the final delivery aspect** (16:9, 21:9, 9:16) | Non-negotiable — see below. |
| **Frame rate** | 24 fps | Cinematic default; 30 or 60 also fine — just state it. |
| **Renderer** | Workbench / flat viewport render | Seconds per frame instead of minutes. Cycles is wasted here. |
| **Format** | MP4, H.264 | Universally accepted by the video platforms. |
| **Lighting** | flat, unlit or minimal | Fancy lighting only tempts the model to copy it. |
| **Duration** | the exact final duration | Timecodes in the prompt come straight from this file. |
| **Motion blur** | OFF | Blur hides object identity and reads as a material choice. |
| **Overlays** | OFF | No gizmos, no armatures, no origin dots, no viewport text. |

Turn off before rendering: gizmos, selection outlines, the grid floor (unless deliberate), text
info, the N-panel, armature display. **Anything visible is something the model will try to
render.**

---

### The aspect ratio is not negotiable

**Render at the aspect you will deliver in.** This is the one setting no prompt can compensate
for.

A viewport capture at 1132×822 is 1.377:1 — close to 4:3. Ask a model for 16:9 (1.778:1) from
that source and it must reconcile a 29% difference: it crops, pads or stretches. If the subject
moves vertically, the crop lands exactly on its motion, and the framing that comes back no
longer matches the blocking. Measured on a real job: a falling subject returned reframed,
because the source was 1.377:1 while the prompt asked for 16:9.

Set Resolution X and Y before you render, not after. Verify with:

```bash
ffprobe -v error -select_streams v:0 \
        -show_entries stream=width,height,display_aspect_ratio \
        -of default=noprint_wrappers=1 blocking.mp4
```

## What actually gets read

The prompt writer extracts frames and reads them. Make that possible:

```bash
ffprobe -v error -show_entries format=duration \
        -show_entries stream=width,height,r_frame_rate,nb_frames \
        -of default=noprint_wrappers=1 blocking.mp4

# contact sheet
ffmpeg -i blocking.mp4 -vf "fps=4" -q:v 4 frames/f_%03d.jpg

# cut detection
ffmpeg -i blocking.mp4 -vf "select='gt(scene,0.25)',showinfo" -f null -
```

**Warning on cut detection:** it works by comparing consecutive frames. A blocking whose shots
all share the same grey materials produces almost no measurable change, so the detector reports
*zero cuts* even across a hard edit. Never trust a zero-cut result — confirm by eye, or hand
over a cut list with the file.

---

## Hand over more than the file

Ship three things with the render:

1. **The video file.**
2. **A cut list** — timecode, camera move, object action; one line per shot. Two minutes of your
   time, and it saves the prompt writer from guessing a timecode into the final film.
3. **A list of the placeholders** — every artefact that is process, not content: tracking grids,
   nulls, checkerboard surfaces, placeholder colours, the empty background. Each needs a
   destination in the prompt; anything unnamed gets rendered literally.
4. **A list of what you did NOT block** — fluids, smoke, particles, sparks, dust. These carry no
   structural lock, so they are the likeliest thing to drift, and the prompt writer cannot tell
   a deliberate omission from an oversight. Each needs its own rule naming source, direction and
   limit.
5. **The rig**, per shot if it changes — rotation rate reads as smooth or broken depending on it.

Example cut list:

```
CUT 1  0.0-4.0   camera: low, rises with subject      objects: sphere drops, hero rises, splits x3
CUT 2  4.0-6.0   camera: spiral up around subject     objects: small spheres rise
CUT 3  6.0-8.0   camera: 3/4 above, dives down wall   objects: lid pops, particles burst radially
GAP    8.0-10.0  black — liquid sim goes here

RIG: steadicam glide throughout, except CUT 3 (robo-arm snap)
NOT BLOCKED — needs containment rules in the prompt: the liquid in the gap, the condensation,
the particle burst in CUT 3
```

---

## The tension worth knowing

More detail in the blocking = a shorter, more accurate prompt, but a higher risk the model
copies the blocking's look. Less detail = safer, but more translation maps to write.

**Optimum: crude geometry, deliberate colour.** Cheap to render, unambiguous to address, and
visibly not a finished look — so the model does not mistake it for one.

---

## Quick audit before handing over

- [ ] Every identity-bearing object has a distinct saturated hue
- [ ] No chrome, glass or transparency on those objects
- [ ] Background contrasts with everything
- [ ] No two same-colour objects overlap or touch
- [ ] Every motion is visibly larger than noise
- [ ] Overlays, gizmos and motion blur are off
- [ ] Resolution and aspect match the final delivery
- [ ] Aspect verified with `ffprobe`, not assumed — a mismatch cannot be fixed by any prompt
- [ ] Duration is final — timecodes come from this file
- [ ] Cut points locked
- [ ] Cut list written
- [ ] Placeholder artefacts listed for the prompt writer
