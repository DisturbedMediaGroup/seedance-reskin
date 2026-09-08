# Hardening rules

The five-block structure gets the prompt written. These rules keep the model from breaking it.

Each one exists because video models fail in a specific, repeatable way. Apply the ones whose
failure mode your shot can hit; skip the rest.

Source: eight production prompts on the Higgsfield/Blender workflow (one-take fight, six-person
dialogue, seven-location camera piece, hypermotion soda ad, car commercial in four styles).

## 1 — Tie-breaker

The single most load-bearing sentence in a long prompt.

> If any text in this prompt appears to disagree with `<<<video>>>` on camera, framing,
> direction, motion, timing or object placement, `<<<video>>>` wins.

A prompt of 900+ words will contradict the blocking somewhere — a phrase implying a framing the
blocking does not have, an adjective implying a different speed. Without an explicit precedence
rule the model resolves the conflict however it likes, usually by trusting the text and drifting
off the motion lock.

Variants worth stating alongside it:

> Where a location still and `<<<video>>>` disagree about the angle, `<<<video>>>` always wins.

Put it in the technical block, near the motion lock.

## 2 — Countable constraints

Video models **duplicate and multiply**. Six attackers become nine; three orbits become five; one
reload becomes three. Prose cannot stop this. A number the model can check against every frame
can.

Write the count, then write the invariant that proves it:

> SIX ninjas and ONLY six — Ninja 1 to Ninja 6. Never a seventh figure: no extras, no
> duplicates, no distant silhouettes, no half-bodies at frame edges. **At every second:
> standing + fallen + on the lintel = 6.**

> Bodies on the ground count exactly: 0 before 12s → 1 → 2 → 3 → 4 at 12/13/14/16s → 5 at 20s
> → 6 at 26s. **Never more.**

> EXACTLY THREE vertical orbit passes in the whole video, one per street location, each a single
> revolution; after 00:10 the camera NEVER orbits again. **No fourth orbit exists anywhere.**

> Count of weapon events in the entire video: one draw, one magazine insertion, one slide rack,
> one shot.

> SEVEN locations, each appearing EXACTLY ONCE, in this exact order: ... **No location ever
> appears twice.**

The pattern is always: **the number → the enumeration → the explicit denial of one more.**

**Count from the frames, every time — never carry a number over from a previous prompt.** A
countable constraint with the wrong number is worse than none: it actively instructs the model
to drop or invent an element. Crop and enlarge the frame where the objects are most separated
and count there, not where they overlap. Measured failure: a count of "six cuts, seven pieces"
was copied from an earlier job onto a new blocking that actually had seven cuts and eight
pieces, and the prompt then told the model to omit a slice. Re-verify after any blocking change,
however small it looks.

Apply to: characters, objects, cuts, camera moves, locations, discrete events, repetitions.

## 3 — Linear progression lock

A multi-step action loops unless forbidden. The model reaches step 4, then plays step 2 again
because it looked good.

> He performs ONE weapon ritual — strictly linear, six steps in fixed order, each step happening
> EXACTLY ONCE and never repeating; once a step is done it is done for good, and the gun only
> ever moves FORWARD through the sequence, never backward.

Then make each step's finality explicit inside the step:

> HOLD 1 — he flicks the cigarette away; **the cigarette is gone and never comes back.**
> HOLD 2 — he draws the pistol; **from this moment it stays in his right hand — never holstered,
> lowered away or swapped hands.**
> HOLD 4 — he seats the magazine, ONE click; **the magazine is now inside the gun permanently —
> NEVER ejected, re-seated, tapped again or touched again.**
> HOLD 5 — he racks the slide ONE time; **the gun is now loaded and stays loaded — never
> charged, checked, cocked or reloaded again.**

Apply to: any assembly, any transformation, any ritual, any progressive reveal, any
opening/closing/pouring/slicing that must happen once.

## 4 — Anti-freeze

**The blocking gives positions. The blocking does not give life.** Proxies are static boxes, so
everything the blocking does not explicitly animate arrives frozen — background people stand
like mannequins, secondary objects sit dead.

> Inside their blocked positions the performances must be fully alive — weight shifting,
> breathing, gesturing, talking, reacting, small continuous movement at all times; nobody
> freezes, nobody stands like a mannequin, and nobody looks at the lens.

For faces, add the safety line to each character:

> (Safety: gaze always engaged in the task — never a frozen, glassy, unfocused stare; natural
> blink cadence.)

And give each figure a concrete continuous action, so "alive" has something to be:

> ONGOING BUSINESS — keep every man alive whenever he is in frame, foreground or background;
> nobody freezes during other men's lines:
> — turns a brass lighter over and over in his fingers; when he speaks, the lighter stops
> — stirs his coffee without hurry, drinks it down across the scene
> — worries the key ring, spins it once around a finger and catches it

Apply to: any shot with people, any environment that should read as inhabited, any secondary
object that would look dead held still.

## 5 — Negative shape assertion

When an abstract blocking shape resembles the wrong thing, the model renders the wrong thing.
Name the wrong reading and refuse it.

> The lamp is a man-made steel structure — **NOT an animal, NOT a snake, NOT any living or
> organic shape**; nothing in this shot moves except the camera.

> dressed as OLD DARK-BRICK WAREHOUSE FACADES — **NOT modern buildings, NOT storefronts with
> signage.**

> the front end faces the camera. **The rear and tail lights are NOT the face of this shot — the
> front is.**

> that grid is a motion-tracking aid, render it as caustic ripple lines on the water surface,
> **never as wireframe or tiles.**

> the pale blue mottled floor is calm matte water — the flat black background is a PLACEHOLDER,
> **never a night sky, never a dark studio.**

Look at each blocking shape and ask: *what else could this be read as?* Every plausible wrong
answer gets a NOT.

## 6 — Never name an off-screen cause

The counterpart to rule 5, and the easier mistake to make yourself.

The blocking shows a **result**: cuts appear on a surface, an object splits, a lid pops. It is
natural to write the cause — "six blades pass through", "a hand twists the cap", "a hammer
strikes" — but nothing in the blocking shows that cause. **The model reads a named object as an
object to render**, and puts six blades in frame.

Wrong:

> SIX blades pass through the lemon at once.

Right — the result, plus the cause explicitly refused:

> The lemon slices ITSELF: SIX clean wet cuts open across the peel simultaneously, as if by
> invisible blades. **No knife, blade, hand or tool is ever visible in any frame.**

Check every verb in the prompt: if its subject is not in the blocking, either rewrite it so the
visible object is the subject, or say the subject is never seen. Then carry the refusal into
the closing recap — this is the class of thing the model reintroduces late.

Also applies to sound: "six wet knife passes" in the SFX block reintroduces the knife. Describe
the sound of the event, not of the absent tool.

## 7 — A void is not a studio, and light cannot touch it

When the blocking's empty background is **kept** rather than replaced, two words destroy it:
`studio` and `seamless`. Both name a physical surface. A surface can be lit, so the model lights
it — and the background drifts from black to grey, worst on a push-in, as the fill spills onto
the wall the words invented.

Wrong:

> shot on a black seamless — dramatic studio lighting on pure black, a hard rim from behind, a
> soft fill from the front, deep clean blacks with no lift

Every clause there implies geometry. `deep clean blacks` is worse than useless: it describes a
**grade**, which the model reads as "dark", not as "nothing is there".

Right — three moves:

1. **Deny the surface, exhaustively.** `There is NO background surface: no backdrop, no
   seamless, no studio wall, no cyclorama, no floor, no fog, no set.`
2. **Confine the light.** `The light falls ONLY on the subject. ZERO ambient, bounce,
   atmospheric scatter or spill behind or beside it — nothing back there exists to catch light,
   so no glow, halo or falloff gradient ever appears in the void.`
3. **Lock the black as a number, across time.** `pure #000000, uniform` beats any adjective —
   a hex value has no range, "deep black" does. And because this failure is progressive, state
   the invariant: `Frame 0.0s and frame 9.9s have the same background value.`

Also: end on `an empty void, not a fade to black` — "goes to black" reads as a transition.

And check the product reference: a design sheet or photo has its own background. Add `never
take its background` to its definition.

## 8 — Glow words paint the background

If a void keeps drifting to grey after rule 5c, the leak is **not** in the background block —
it is in the words describing the subject's light. These are the culprits:

| Word | What it does |
|---|---|
| `bloom` | a post effect that **spreads into neighbouring pixels** — on black it directly paints the void |
| `flare`, `flaring` | a ray that travels across the frame, leaving the object behind |
| `halation`, `glow` | the same, softer |
| `crisp speculars` | tiny hard highlights that, with bloom, become diamonds |
| `rim light`, `backlight`, `kicker` | light from behind the subject always haloes it on black |
| `sparkle`, `glints`, `diffusion filter` | jewellery-advert look; each one radiates |

Writing "the light falls only on the subject" in one paragraph and `bloom on speculars` in
another is a contradiction, and the concrete instruction wins. **Do not describe an optic you
do not want**, not even to qualify it.

Three moves that hold:

1. **A BANNED OPTICS block, naming each effect.** `none of these may appear anywhere: bloom,
   glow, halation, lens flare, starburst, streaks, light rays, god rays, diffusion filter,
   soft-focus haze, sparkle, glints, twinkles, diamond or jewellery sheen.`
2. **No light from behind.** A broad soft key from the front plus a gentle top light. Any rim,
   backlight or kicker haloes the subject against a void — say `NO rim light, NO backlight, NO
   kicker`.
3. **A measurable test instead of an adjective.** `The pixel immediately outside the silhouette
   has exactly the same value as the pixel in the far corner: pure #000000.` A model can check
   that; it cannot check "no glow". Add `highlights stay SMALL, TIGHT and CONTAINED WITHIN the
   silhouette — a highlight ends where the object ends.`

Also set the overall register against the glamour default: `naturalistic, restrained,
documentary-clean — NOT a glamour or jewellery look`, and describe the surface as `matte`
rather than letting "glossy" and "crisp" accumulate.

If the glow survives all of this, the model is applying bloom unconditionally — fix it in post
with a black-point adjustment, not with more prompt.

## 9 — Do not put platform settings in the prompt

Resolution, aspect ratio and output size are chosen in the platform's UI. The prompt cannot
change them, so writing `8K`, `4K` or `16:9` in the text does nothing at best — and at worst
does real damage: an aspect stated in the text that disagrees with the blocking's actual aspect
gives the model two conflicting instructions, and it crops or pads to reconcile them.

Belongs in the technical block: render style, lens behaviour, lighting, audio policy, the locks.

Does **not** belong: `8K`, `4K`, `1080p`, `16:9`, `9:16`, bitrate, codec, frame rate.

Duration is the exception, and only when the prompt carries timecodes — `10s` gives `4.3-4.6`
a denominator. Set the real duration in the UI regardless.

The same applies to anything else the platform owns: seed, model name, sampler, guidance scale.
If a control exists in the interface, the prompt is the wrong place for it.

## 10 — Aspect mismatch is a file problem, not a prompt problem

**Measure the blocking's aspect before writing a single line.** `ffprobe` reports
`display_aspect_ratio`; compute width/height and compare it to the aspect the prompt asks for.

A 1132×822 blocking is 1.377:1. A prompt asking for 16:9 (1.778:1) forces the model to
reconcile a 29% difference — it crops, pads or stretches, and the framing no longer matches the
source. If the subject moves vertically, a height crop lands exactly on its motion.

No wording fixes this; it is geometry. Tell the user to re-render at the delivery aspect
(1920×1080 for 16:9). Until then, a FRAMING block limits the damage without removing it:

> FRAMING — match the aspect ratio and framing of `<<<video>>>` exactly: the subject occupies
> the same portion of the frame, at the same position, in every corresponding frame. Do not
> crop, pad, re-frame or zoom. If the output aspect differs, fit the source without cropping —
> never cut the top or bottom, since the subject's motion runs vertically.

Name the axis the motion runs along, so the model knows which edge it must not lose.

## 11 — Intensity words are not a quantity

"A splash" has no upper bound; the model picks one, and it picks big. Stacked intensifiers make
it worse: `BURST`, `EXPLODES`, `wide spray`, `mist`, `ribbons` and `high-detail fluid
simulation` are six requests for the same event, and they compound.

State a **count** and a **budget**, exactly as with countable constraints:

> As the cuts open, a SMALL number of juice droplets is flicked off the peel — roughly TEN TO
> FIFTEEN countable beads in the whole frame, each travelling a short distance and slowing to a
> stop. A light flick of juice — NOT a splash, NOT a spray, NOT a burst: no sheets of water,
> no mist, no particle cloud.

And a global ceiling in the technical block:

> WATER BUDGET — this film is nearly dry. Total liquid on screen: a thin sheen of condensation,
> juice on the cut faces, and at most a couple of dozen individual droplets. NO splashes,
> sprays, mist, fog, water sheets or streams.

Then remove the words that invite the simulation: `fluid simulation`, `high-detail`,
`hyper-detailed liquid`. A model reads them as a brief for a showreel.

Same pattern for smoke, sparks, debris, dust, petals, confetti — anything a model renders
generously by default. Count it, cap it, and delete the intensifiers.

### Calibrating rather than banning

A count kills the effect. If the user wants the effect **present but restrained**, a count is
the wrong tool — it makes the shot dry. Give a **size measured against something in frame**:

> WATER BUDGET — small, contained splashes, not a wall of water. Juice bursts and droplet
> scatters ARE wanted, but each stays SMALL: a splash reaches at most about **ONE LEMON-WIDTH**
> from the fruit it came off, then breaks into separate droplets that slow and drift. Water is
> always readable as **individual beads and short ribbons, never as a solid mass**.

Two mechanisms, both checkable:

- **Reach, in units of the subject.** "Small" has no scale a model can apply; "one lemon-width",
  "half the can's height", "no further than the character's shoulder" all do, because the
  reference object is on screen.
- **A readability test.** "Readable as individual beads, never a solid mass" separates *some
  water* from *a wall of water* far better than any adjective.

Keep the denials for genuine excess only — `frame-crossing spray`, `sheets or curtains`, `fog`,
`water on the lens` — and drop blanket bans like "NOT a liquid-simulation showreel" once the
effect is wanted.

The reach figure is then the single dial to turn: too dry → two subject-widths; too wet → half.

## 12 — Verify the reference token verbatim, before writing

The token is a **literal string the host matches**, not a description. `#Video1` and `#Video 1`
are different things; so are `@prop_super_car` and `@prop_super-car`. A wrong token **silently
unbinds the reference** — the model generates a plausible video with none of the art direction
in it, and reports no error. Nothing downstream can detect it.

So, before writing a single line:

- **Ask the user for the exact tag string** and echo it back in text for confirmation. A
  screenshot is not good enough: tags mixing underscores and hyphens are misread at small sizes,
  and that misreading is invisible by construction.
- **Re-check every occurrence after any edit.** The token appears in the reference definitions,
  the technical block, several beats and the closing recap; a rename that misses one place
  leaves a dead reference in the middle of the prompt.
- `grep -c` the exact string and compare it to the number of places you meant to use it.

Measured cost of skipping this: three rewrite rounds over `@Video 1` → `#Video 1` → `#Video1`,
none of which the output could have flagged.

## 13 — A multi-view sheet will reproduce its grid

A product or character reference is usually a **sheet**: several views laid out on a neutral
field. The model reproduces layouts, so the sheet's grid arrives in the shot — four cars in a
row, three heads floating on grey.

Disown the layout explicitly, in the reference's own definition:

> `#Image1` — appearance ONLY. Do NOT inherit its grey studio background, its lighting, its
> scale, its framing or its multi-view layout. **It is one lemon, not six.**

The count at the end is the load-bearing part: it is a countable constraint aimed at the
reference itself. Add it whenever the reference shows the subject more than once.

## 14 — Every element you did NOT block needs a containment rule

Anything absent from the blocking has **no structural lock on it**, which makes it the single
most likely thing to drift. And there is always at least one, because fluids, smoke and
particles are deliberately never blocked.

A limit alone is not enough. Pin three things — **source, direction, limit**:

> Tyre smoke comes ONLY from the rear wheels and always trails BEHIND the car. Never in front
> of it, never from the front wheels, never filling the frame.

> Juice leaves ONLY the cut faces and travels OUTWARD from them, reaching at most a lemon-width
> before it breaks into separate droplets. Never from the intact peel, never toward the lens,
> never across the frame.

Measured failure: a splash given only a limit ("at most one lemon-width") and no source or
direction filled the whole frame. Source and direction are what the blocking would have
supplied; without them the limit has nothing to attach to.

List every unblocked element before writing, and give each one its own numbered rule.

## 15 — Reference activity windows

A reference that governs only part of the timeline says so, in the reference definition:

> **Active for 00:00–00:03.3 only.**

> The HOODED panel applies only to 0–7.0s and 27.5–30s; the UNHOODED panels apply to 7.0–27.5s.

Without a window, an environment reference bleeds into every shot.

## 16 — Atmosphere master

A reference that supplies only light, weather and grade — never geometry, never a frame that
appears on screen. Declare all three negatives:

> **NOT a keyframe, NOT a location to reproduce, NOT a frame that ever appears in the film:**
> its corner store, its intersection, its framing are never seen in any shot. It defines ONLY
> the weather, light, color, atmosphere and consistency of the world: deep clean night just
> after the rain has stopped ... Every shot of the film — whatever geometry the blocking stages
> in it — is lit, colored and graded exactly in this regime for all 30 seconds.

Useful when one grade must hold across locations that look nothing alike.

## 17 — Duration in the token

Write the reference token with its duration every time: `@Video 8 · 30s`, not `@Video 8`. The
token then carries the timeline length wherever it appears, and every timecode in the prompt has
a denominator.

## 18 — Off-screen rule

Naming a speaker tempts the model to show them. Forbid it as a rule, not per line:

> OFF-SCREEN VOICES RULE: a line marked off-screen must STAY off-screen — never show the
> speaker, never move him into frame, never route the camera behind him because he spoke. If a
> speaker's back or shoulder is already in frame as a near dark mass, his voice comes from that
> mass and the camera does not turn to him.

Generalizes: any element the text mentions but the blocking does not show needs an explicit
"mentioning it does not put it on screen".

## 19 — Acting tasks

For performance shots, direction beats description. Per character:

> **SCENE DIRECTION** (shared, unspoken): keep tomorrow sounding like a fishing trip.
> **MOTIVE** (his fuel): the schedule is the only part of tomorrow he can control.
> **GOAL**: hear every man at this table give him "six" back.
> **OBSTACLE**: his own doubt — one crack and the argument stops being about time.
> **TACTIC**: pins the plan into one man at a time; after each point checks both eyes of the
> listener — did the number land, or is he being humored.
> **Moment to moment**: — "Six a.m. sharp." — plants the number in Blue's eyes, hunting for
> push-back before the sentence even ends.

Plus a **SCENE EVENT** the scene is really about, which no character names:

> the last calm check before a dangerous job — every man quietly inspecting every other man for
> cracks, disguised as an argument about a departure time.

## 20 — Closing recap

End the prompt with a condensed restatement of the hard rules — the tie-breaker, the counts, the
identity locks, the prohibitions:

> HOLD FOR THE FULL TIMELINE
> `<<<video>>>` camera path 1:1 — any deviation = failure.
> Six and ONLY six ninjas — always in FRONT of the Hero; bodies only BEHIND him.
> One attacker at a time; six falls at 12 / 13 / 14 / 16 / 20 / 26s.
> Identity constant; the tunnel in every frame; no subtitles, no watermarks.

Late tokens carry weight. The recap is where the rules that matter most get their third mention.