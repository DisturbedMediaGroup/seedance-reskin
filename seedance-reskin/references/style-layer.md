# Style layer — one blocking, many films

The largest payoff of the whole technique.

A blocking render is expensive to produce and carries no look. So it can be re-dressed
indefinitely: **the structural lock never changes, the style layer changes completely.**

One 19-shot car-commercial blocking was shipped as four finished films — photoreal night chase,
hand-painted 2.5D, black-and-white manga, and toy stop-motion western. Same cuts, same camera,
same timing, same character blocking. Nothing else in common.

## The split

| Layer | Contents | Changes between styles |
|---|---|---|
| **Structural lock** | cut points, cut count, camera position / angle / move / framing, object trajectories, motion states, timing, world geometry (every mass the blocking stages) | never |
| **Style layer** | medium, materials, palette, lighting regime, world dressing, character design, wardrobe, era, atmosphere | entirely |

Write the structural lock once. Reuse it verbatim in every style.

## What stays identical across styles

Copy this block between styles with only the token renamed:

> `<<<video>>>` is the single source of truth for action, editing, motion state and world
> geometry — frame by frame, the previz re-dressed, never re-imagined. Every shot replicates its
> `<<<video>>>` counterpart one-to-one — no added events, no removed events, no invented camera
> moves, no invented shots. The cut count and cut points match `<<<video>>>` exactly. Every mass
> the previz shows is dressed and kept in place, at the same positions and silhouettes; nothing
> is deleted, and no structure is invented where the previz shows none. If any text in this
> prompt appears to disagree with `<<<video>>>` on camera, framing, direction, motion, timing or
> object placement, `<<<video>>>` wins.

Also identical: the shot list with its timecodes, the counts, the linear locks, the anti-freeze
rule. Only the nouns inside each shot line change.

## What the style layer replaces

Everything a grey proxy stands for:

> Where the previz uses stand-in proxy shapes (the car body's front/rear, the blocky human
> figures and limbs), this prompt's descriptions define the real object — **proxies give
> position, angle, scale and motion only, never surface, shape detail or design.**

> The reference's untextured grey-box surfaces, flat colors and viewport grid are NOT inherited
> — every grey proxy is dressed into a real object in the exact position the previz puts it.

The same blocking mass becomes:

| Blocking | Photoreal | 2D ink | Toy stop-motion |
|---|---|---|---|
| tall block flanking the road | dark-brick warehouse facade | inked storefront with shutters | brick-built canyon wall |
| grey box figure | a man in a leather jacket | a manga-drawn character in flat ink | a plastic minifigure |
| car proxy | widebody sport sedan, black paint | pink 1960s saloon in ink outline | toy car with visible studs |

## Writing a new style onto an existing blocking

1. Copy the structural lock verbatim
2. Copy the shot list, keeping every timecode
3. Replace `STYLE`, `LIGHTING`, `WORLD` and every character/object definition
4. Rewrite each shot line's nouns and adjectives — never its camera, timing or action
5. Re-check the counts: they are structural, so they carry over unchanged

## A style block that commits

Weak style blocks produce hedged output. State the medium, then state what it is never:

> High-contrast BLACK-AND-WHITE INK ILLUSTRATION brought to motion — a moving manga page. ONE
> PEN DRAWS EVERYTHING: the people, the car, the streets and the sky are all made of the same
> rough brush linework with thick-to-thin strokes, deep solid black fills, crisp white negative
> space and fine film grain — every frame reads as drawn on paper, purely hand-drawn 2D
> animation, flat as a printed page, **never 3D, never photoreal, never live-action.**
>
> THE PEOPLE ARE DRAWINGS, the same medium as the world: skin is blank flat paper tone with ZERO
> texture, shaded ONLY by cross-hatching, screentone dots and solid black shadow shapes; eyes are
> graphic ink shapes with a white catchlight dot. **A drawing, never a photograph.**

Note the extra work spent on **people**: a stylised world with photoreal faces is the standard
failure, so faces get their own paragraph insisting on the medium.

## Selective colour

A strong style device, and it needs a count:

> SELECTIVE COLOR — **EXACTLY TWO COLORED OBJECTS IN THE FILM**: the girl's rich RED dress and
> lips, and the car's PINK body — both laid as flat printed ink over the drawing. Everything
> else, including the police cars and their beacons, is black, white and grey.

## Locations as named constants

When several shots share a location, name it once and assert its identity:

> LOC-B, THE ELEVATED EXPRESSWAY (shots 6-13): one curved two-lane elevated road THROUGH the
> city — dashed center line, steel guardrails both sides, evenly spaced steel lampposts of one
> design, and a skyline of inked rooftops behind the rails. **Identical in all these shots.**

`Identical in all these shots` is the countable constraint applied to place — it stops the model
inventing a new street each cut.

## Practical consequence

Block once, sell many times. A client rejecting a look costs a style-layer rewrite, not a
re-shoot. Pitch three directions from one blocking. The expensive artefact is the motion; the
look is cheap.