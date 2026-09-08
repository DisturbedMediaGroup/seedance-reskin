---
name: seedance-reskin
description: Turn a grey 3D blocking animation into a finished video-generation prompt that locks motion one-to-one and re-skins everything else. Use when the user hands over a blocking/previz/animatic render plus reference images and wants the prompt that drives an AI video model (Seedance, Veo, Kling, Runway, Sora, Hailuo) to reproduce that exact camera work and object choreography with real materials, products, characters and environments. Triggers - "seedance", "blocking", "previz", "animatic", "re-skin this", "make a prompt from this video", "motion reference", "grey render to final", "napravi prompt od blockinga", "od ovog blockinga", "prompt za video".
---

# Blocking Re-skin

A blocking render carries the one thing video models are worst at: **coherent camera motion
and object choreography that survives 30 seconds and multiple cuts**. It carries the one thing
they are best at badly: materials, colour, brand, world.

So the prompt has exactly one job — **keep the motion, replace everything else**.

Everything below serves that split. The blocking video is the motion authority. The text is the
look authority. Neither may leak into the other's territory.

## The three failure modes

| Failure | Cause | Fix |
|---|---|---|
| Model reproduces the grey look | Blocking treated as a look reference | Explicit "do NOT copy its placeholder materials", plus per-object translation maps |
| Model invents its own camera / re-times cuts | Motion authority stated once, weakly | State MOTION LOCK three times: in the video's own definition, in the technical block, at the end of the prompt |
| Model multiplies, loops, freezes or misreads a shape | Prose alone cannot bound these | The hardening pass — `references/hardening-rules.md` |

The first two share a cause: the model does not know which reference owns which property.
**Every reference must be labelled with what it governs and what it does NOT.**

The third is different — it is what the model does *inside* a correct structure. It needs
counts, locks and refusals, not labels.

## The tie-breaker

Before anything else, know the one sentence a long prompt cannot ship without:

> If any text in this prompt appears to disagree with `<<<video>>>` on camera, framing,
> direction, motion, timing or object placement, `<<<video>>>` wins.

A 900-word prompt will contradict the blocking somewhere. Without an explicit precedence rule
the model resolves that conflict on its own — usually by believing the text and drifting off
the motion lock. Put it in the technical block, next to the motion lock, every time.

## Workflow

<checklist>
1. Watch the blocking. Write a cut list with timecodes before writing any prose.
2. Inventory every distinct object and how the blocking encodes its identity (colour, size, position).
3. Ask the user for the translation maps you cannot infer — **and for the exact reference token
   strings**, echoed back in text for confirmation (`#Video1`, not `#Video 1`). A wrong token
   unbinds the reference silently; see `references/hardening-rules.md` rule 12.
4. Assign each reference asset a role: MOTION / ENVIRONMENT / PRODUCT / CHARACTER / ATMOSPHERE.
5. Write the five blocks in order (see structure below).
6. Run the hardening pass — `references/hardening-rules.md`.
7. Read it back against the checks at the end. Fix what fails.
</checklist>

### Step 1 — cut list first

Never write the prompt in one pass. First produce a bare table, timecodes from the actual video:

```
CUT 1  0.0-4.0   camera: low, rockets up with subject     objects: sphere drops, hero rises, splits x3
CUT 2  4.0-6.0   camera: spiral up around subject          objects: small spheres rise
CUT 3  6.0-8.0   camera: 3/4 above, dives down the wall    objects: lid pops, particles burst radially
```

Show this to the user before prose. Mistimed cuts are the most expensive error to fix later,
and the cheapest to catch here.

Reading the video: `ffprobe` for duration and fps; `ffmpeg -vf fps=4` for a contact sheet;
`ffmpeg -vf "select='gt(scene,0.25)',showinfo"` for cut detection. Verify by eye — a blocking
whose shots share the same grey materials defeats scene detection, so a zero-cut result means
"one continuous take" only after you have actually looked at the frames.

**Black frames and gaps in the blocking are slots, not errors.** They exist because the artist
had nothing to block there. Fill them with shots that need no blocking — underwater, particle
fields, abstract texture, pure liquid. Say so explicitly: `(black gap in <<<video>>>)`.

### Step 2 — inventory the identity signals

For each object, ask: **what makes it unambiguously nameable in the blocking?** Only three
things work, in this priority:

1. **Colour** — the only identity signal invariant under motion. Best, **but bounded**: colour
   can select a *variant* of an object whose shape already reads as that object (six cylinders,
   six flavours of can). It cannot carry the *identity* of an object whose shape reads as
   nothing — a magenta sphere that must become a lemon stays magenta, no matter how many times
   the prompt says otherwise, because the colour is present in every frame while the text
   enters once. When a shape is ambiguous, the fix is a blocking re-render near the final
   colour, not more prose.
2. **Silhouette / size** — works when shapes differ enough (cube vs sphere vs cylinder).
3. **Position** — weakest. Valid only for an object that is unique in frame ("the one hero
   can", "the tumbler"). Never for one of six identical clones.

If an object has none of these, the prompt cannot address it. Send the user back to the
blocking — see `references/blocking-spec.md`.

### Step 3 — the translation maps

This is the part that makes the whole thing work. Blocking is abstract; the map is the
dictionary from abstract to final. Write one map per axis of change:

```
LOCATION MAP    — flat black background is a PLACEHOLDER; replace per cut:
                  CUT 1 -> <<<image_A>>>, CUT 2-4 -> <<<image_B>>>, ...
OBJECT MAP      — the falling sphere is a whole LIME; small red spheres are berries;
                  cubes are crystal-clear ice
COLOR/ID MAP    — blue clone = <<<image_17>>>, red = <<<image_20>>>, green = <<<image_16>>>
MATERIAL MAP    — the pale mottled floor is calm matte water reflecting the active location;
                  the thin dark grid in CUT 8 is a tracking aid — render as caustic ripple
                  lines, never as wireframe or tiles
```

**Rules for maps**

- Every visible artefact of the blocking process (grids, nulls, tracking marks, placeholder
  colours, empty backgrounds) must be named and given a destination. Anything unnamed gets
  rendered literally.
- Write the identity map **twice** — once in the reference definitions, once inline in every
  cut where it is used. Repetition is weight for video models, not redundancy.
- Ask the user for anything you cannot infer. Do not guess which sphere is which flavour.

### Step 4 — reference roles

| Role | Governs | Explicitly does NOT govern |
|---|---|---|
| MOTION | camera, cuts, timing, framing, object trajectories | materials, colour, background, lighting |
| ENVIRONMENT | world look, sky, terrain, palette, atmosphere | camera, composition, any motion |
| PRODUCT | one object's appearance, label, geometry | scale, position, timing |
| CHARACTER | face, wardrobe, body | performance, blocking |
| ATMOSPHERE | light, weather, palette, grade | geometry, location, framing — never appears on screen |

A reference that governs only part of the timeline says so in its own definition:
`Active for 00:00–00:03.3 only.` Without a window, an environment reference bleeds into every
shot. Write each token with its duration — `<<<video_8>>> · 30s`, not `<<<video_8>>>` — so it
carries the timeline length wherever it appears.

Write the negative half. `Environment look only.` and `Do NOT copy its flat placeholder
materials.` are load-bearing sentences, not decoration.

## Output structure

Five blocks, in this order. Full worked example: `references/worked-example.md`.

### 1 — REFERENCE DEFINITIONS

One paragraph per asset token. Each paragraph states: what it depicts, its role, its scope
limit. The motion reference's paragraph carries all the translation maps and ends with
`MANDATORY MOTION SOURCE — replicate every camera move, camera angle, framing, cut point, shot
duration and object trajectory from this video one-to-one, frame-accurate.`

### 2 — TECHNICAL BLOCK

Global rules only, nothing per-cut except the location map:

- Style sentence (render aesthetic in five to eight words)
- Audio policy (`SFX only, no music`) — this genuinely steers generation
- **Not** resolution, aspect or output size. Those are platform settings, chosen in the UI, and
  the prompt cannot change them. Stating them wastes budget at best; at worst an aspect in the
  text contradicts the blocking's real aspect and the model crops to reconcile the two. Duration
  is the one exception, and only when the prompt carries timecodes — it gives them a denominator.
- LOCATION MAP, restated as a flat per-cut list
- LENS REALISM — chromatic aberration, motion blur, depth of field with rack focus, barrel
  distortion on wides, bloom on speculars, photographic grain. This is what separates "3D
  render" from "filmed"; name the artefacts individually.
- STRICT MOTION LOCK and OBJECT LOCK, spelled out as prohibitions: *do not invent new camera
  moves, do not reframe, do not add or drop cuts, do not improvise extra elements.*
- IP status (`NON-IP (original brand from the design sheets)`) when brand-adjacent.

### 3 — PROMPT

One sentence-block per cut, always the same shape:

```
CUT n (start-end) — [world] : [camera behaviour] + [object action] + [material / liquid detail]
```

Rules:

- Timecodes on every cut, taken from the blocking, never invented.
- Name the world every single cut. Do not write "same as before" and rely on it — restate.
- Camera verb first. The model weights early tokens.
- Reference tokens inline where the object appears, not only in the map.
- Anything a cut must NOT do goes in that cut, not in a global note: `no defocus in this shot`,
  `nothing ever lands`, `no collisions, no pass-through`.
- Close the block by repeating the motion lock a third time.

### 4 — SFX / AUDIO

One chronological stream, mapped cut by cut. State the music policy up front (`SFX only, no
music`). Each sound names a visible event, so the model can align it.

### 5 — GUARDRAILS

Not a separate block — distributed. The prohibitions live where they apply. But verify all of
these appear somewhere:

- do not copy placeholder materials
- do not invent camera moves
- do not reframe
- do not add or drop cuts
- do not re-time
- do not improvise elements

## The hardening pass

The five blocks describe a correct film. They do not stop the model from wrecking one. Video
models multiply objects, loop actions, freeze anything not explicitly animated, and misread
abstract shapes. Prose cannot bound these — counts, locks and refusals can.

Run `references/hardening-rules.md` over the finished draft. The four that apply almost always:

**Countable constraints.** Write the number, enumerate it, deny one more:
`SIX ninjas and ONLY six — never a seventh. At every second: standing + fallen + on the lintel = 6.`
`EXACTLY THREE orbit passes in the whole video. No fourth orbit exists anywhere.`
Apply to characters, objects, cuts, camera moves, locations, discrete events.

**Linear progression lock.** Any multi-step action loops unless forbidden:
`strictly linear, each step happening EXACTLY ONCE and never repeating; once a step is done it
is done for good, and it only ever moves FORWARD, never backward.`

**Anti-freeze.** The blocking gives positions, not life. Everything it does not animate arrives
frozen: `performances fully alive — weight shifting, breathing, small continuous movement at all
times; nobody freezes, nobody stands like a mannequin.` Give each figure a concrete continuous
action so "alive" has something to be.

**Negative shape assertion.** When an abstract blocking shape resembles the wrong thing, the
model renders the wrong thing. Name the wrong reading and refuse it: `the lamp is a man-made
steel structure — NOT an animal, NOT a snake, NOT any organic shape.` For every blocking shape
ask what else it could be read as; each plausible wrong answer gets a NOT.

Close the prompt with a condensed recap of the tie-breaker, the counts and the identity locks.
Late tokens carry weight — the recap is the third mention of what matters most.

## One blocking, many styles

The structural lock — cuts, camera, timing, object trajectories, world geometry — never changes.
The style layer — medium, materials, palette, lighting, character design — changes completely.
One car-commercial blocking shipped as four finished films: photoreal night chase, hand-painted
2.5D, black-and-white manga, toy stop-motion western.

When the user wants a second look on the same blocking, do not rewrite the prompt — copy the
structural lock and the shot list verbatim, replace only the nouns. `references/style-layer.md`.

## Self-check before delivering

Three checks come **before** the prompt exists, not after — they gate the writing:

- [ ] Was the **reference token string confirmed verbatim by the user** (echoed back in text,
      never read off a screenshot), and does it appear identically in every place — definitions,
      technical block, beats, recap? A wrong token unbinds the reference silently.
- [ ] Does every **multi-view reference sheet** disown its layout and state how many of the
      subject actually exist ("It is one lemon, not six")?
- [ ] Does every element that is **not in the blocking** (fluid, smoke, particles, sparks) carry
      a containment rule naming its **source, direction and limit** — not a limit alone?

Run every line. A failure means rewrite, not a caveat.

- [ ] Can I write a sentence that unambiguously points to **every** object in **every** cut?
- [ ] Does every reference token state both what it governs and what it does not?
- [ ] Is every blocking artefact (grid, null, placeholder colour, empty background) given a
      destination?
- [ ] Do the timecodes sum to the stated duration, with no gaps or overlaps?
- [ ] Does the motion lock appear **three** times?
- [ ] Is the identity map written at least twice?
- [ ] Are the black gaps handled as deliberate shots?
- [ ] Are per-cut prohibitions attached to their cut rather than pooled globally?

- [ ] Is the **tie-breaker** present — text vs blocking, blocking wins?
- [ ] Does every set of repeatable things carry a **count** and a denial of one more — and was
      each count **read off an enlarged frame in this session**, never carried over from an
      earlier prompt? A wrong count instructs the model to drop or invent an element.
- [ ] Does every multi-step action carry a **forward-only lock**?
- [ ] Is there an **anti-freeze** clause, and does every figure have a continuous action?
- [ ] Has every ambiguous blocking shape been given its **NOT** list?
- [ ] Does every **verb have a subject that exists in the blocking**? A named off-screen cause
      ("six blades pass through", "a hand twists the cap") gets rendered — rewrite it as the
      visible result, and state that the cause is never seen. Check the SFX block too.
- [ ] If an **empty background is kept**: is every surface word ("studio", "seamless",
      "backdrop") a denial rather than a claim, is the light confined to the subject, and is
      the black locked as a **number** (`#000000`) that is identical in the first and last
      frame? A surface the words invent is a surface the light will reveal.
- [ ] Is every **glow word** ("bloom", "flare", "glow", "halation", "sparkle", "rim light",
      "crisp speculars") a denial rather than a request? These spread past the silhouette and
      paint the void grey. Grep the whole prompt: each hit must sit inside a NOT.
- [ ] Does the **blocking's measured aspect** match the aspect the prompt asks for? Compute
      it (`ffprobe` → width/height) rather than assuming. A mismatch is a re-render, not a
      wording fix — say so, and name the axis the subject moves along.
- [ ] Does every **effect a model renders generously** (water, smoke, sparks, dust, debris)
      carry a **count, or a reach measured against something in frame**, rather than an
      intensity word? "A splash" has no upper bound; "ten to fifteen countable droplets" or
      "reaches at most one lemon-width" does. A count removes the effect, a reach keeps it and
      sizes it — pick by whether the user wants it gone or restrained.
- [ ] Does a **closing recap** restate the tie-breaker, the counts and the identity locks?
- [ ] Have you **counted the words** (`wc -w`, not an estimate) against the target model's
      limit, and reported the count to the user? Seedance 2.5: ~1000 words.

## Model-specific notes

Length limits and token syntax vary. See `references/model-notes.md`. The structure above is
portable; only the reference-token syntax and the length budget change.

## When the blocking is not usable

If step 2 leaves objects unaddressable, stop and tell the user what to re-render, with the
specific fix. `references/blocking-spec.md` is written to be handed to the 3D artist as-is.
Do not paper over an ambiguous blocking with vague prose — that is exactly the input that
produces melted, flavour-swapped output.