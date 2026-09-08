# Worked example — soft-drink commercial, 30s, 13 cuts

Source blocking: grey/pastel primitives on a flat black void, pale blue mottled floor,
tracking grid in one cut, two black gaps. Seven reference images (six can designs, three
environments).

Trimmed to show the shape. Full-length output follows the same pattern for all 13 cuts.

---

## Block 1 — REFERENCE DEFINITIONS

> **`<<<video_6>>>`**: Blocking render of the full 30s commercial — a pale neutral hero can,
> colored clone cans, whole fruit spheres that split in half, a translucent glass tumbler,
> glassy ice cubes and red sphere stand-ins on a pale blue floor. THE FLAT BLACK BACKGROUND
> everywhere in `<<<video_6>>>` is a PLACEHOLDER — replace it in every shot with the location
> assigned in the LOCATION MAP below; the pale blue mottled floor is calm matte water
> reflecting the tones of that location; in CUT 8 the floor carries a thin dark grid — that
> grid is a motion-tracking aid, render it as delicate caustic ripple lines on the water
> surface, never as wireframe or tiles. FRUIT MAP: the falling hook sphere and the green
> spheres beside the hero can are whole LIMES, the yellow sphere is a whole LEMON; every small
> red sphere elsewhere in the film is a ripe berry. CLONE COLOR MAP: the blue-label clone is
> `<<<image_17>>>`, red is `<<<image_20>>>`, green is `<<<image_16>>>`, orange is
> `<<<image_15>>>`, yellow is `<<<image_18>>>`, pink is `<<<image_14>>>`. MANDATORY MOTION
> SOURCE — replicate every camera move, camera angle, framing, cut point, shot duration and
> object trajectory from this video one-to-one, frame-accurate. Nothing about the motion or
> editing may deviate from it. Do NOT copy its flat placeholder materials — product visuals
> come from the design sheets. Reference.

Note the four moves inside one paragraph: **placeholder declared → destination given →
motion authority asserted → look authority denied**.

Environment tokens are short and end with the scope limit:

> **`<<<image_19>>>`**: Lush green meadow hills with small puddles, floating water droplets,
> bright blue sky. Environment for CUT 1, CUT 6, CUT 8 and the CUT 13 packshot — the film opens
> and closes in this world. **Environment look only.** Reference.

Product tokens are one line each:

> **`<<<image_14>>>`**: Higgs Raspberry v2 can design sheet — hero flavor. Product appearance
> for the main can throughout the film. Reference.

---

## Block 2 — TECHNICAL BLOCK

> Polished digital CGI product commercial — hyperclean 3D aesthetic, flawless surfaces, perfect
> gradients, designed render look. 16:9. 30s. SFX only, no music. 8K, ultra glossy, crisp
> specular highlights, high-detail fluid simulation look, caustics, refraction, vibrant colors.
> LOCATION MAP — the flat black background of `<<<video_6>>>` is a placeholder; replace it per
> cut: CUT 1 lives in `<<<image_19>>>` from its very first frame; CUT 2, CUT 3 and CUT 4 live in
> `<<<image_13>>>`; CUT 5 in `<<<image_21>>>`; ... the water floor always reflects the tones of
> the active location; underwater gaps (CUT 7, CUT 9) need no location. FULL LENS REALISM:
> physically accurate camera — chromatic aberration creeping in at the frame edges, natural
> motion blur on every fast move, shallow depth of field with creamy bokeh and visible
> rack-focus shifts, subtle barrel distortion on the wide lenses, faint bloom on hot speculars,
> fine photographic grain. STRICT MOTION LOCK: reproduce the camera motion, lens feel, shot
> angles, cut timing and object blocking of `<<<video_6>>>` EXACTLY, one-to-one, frame by frame
> — same shot order, same shot durations, same framing, same speed-ramp rhythm. OBJECT LOCK:
> every object trajectory matches `<<<video_6>>>` one-to-one; do not add, remove, re-time or
> re-path any object. Do not invent new camera moves, do not reframe, do not add or drop cuts,
> do not improvise extra elements. `<<<video_6>>>` defines 100% of the motion, editing and
> object choreography; the text below only defines look, materials, locations and liquid
> effects layered onto that motion. NON-IP (original brand from the design sheets).

---

## Block 3 — PROMPT, three representative cuts

**A hook cut with a complex event:**

> CUT 1 (0.0-4.0), the hook — we open already inside `<<<image_19>>>`, green meadow hills and
> bright sky over calm water: one glossy whole lime drops through the frame like a bullet and
> strikes the water with a sharp crown of spray, a thin ring racing outward across the surface;
> out of the impact point the can ROCKETS upward — wearing the `<<<image_16>>>` design for this
> opening cut — flanked by a lemon and two limes bursting up beside it, everything freezing
> weightless at the apex; then the money moment, exactly as in `<<<video_6>>>`: the can carves
> itself into THREE clean horizontal slices that drift apart with a slight tilt — all three
> slices `<<<image_16>>>`, and their cross-sections are juicy sliced-lime flesh with glistening
> pulp — while the lemon and the limes split clean in half in the same instant, every piece
> hovering open in one suspended exploded-view breath — then the whole composition SNAPS back
> together and the can rips a full 360 spin on a tilt, micro-bubbles sparkling around it.

**A black-gap cut, filled deliberately:**

> CUT 7 (14.5-17.0, black gap in `<<<video_6>>>`) — fully submerged: bubbles spiral in slow
> turbulence, carbonation columns swirling around the lens in a hypnotic vortex.

**A cut carrying its own prohibitions:**

> CUT 10 (21.5-23.5) — the violet skies of `<<<image_21>>>`, open air, no floor in frame: five
> flavor cans descend from above one after another and stop LEVITATING in mid-air on a wide
> circle around the hero — `<<<image_17>>>`, `<<<image_20>>>`, `<<<image_16>>>`,
> `<<<image_15>>>`, `<<<image_18>>>` — nothing ever lands, everything floats weightless against
> the purple-pink sky, while the wide camera sweeps low between the levitating cans in one clean
> arc — no collisions, no pass-through, just a straight cut into the next shot.

Note `nothing ever lands`, `no collisions, no pass-through` sitting **inside** the cut. Pooled
globally they would be ignored.

Closing line of the block — third motion lock:

> Every camera and object trajectory in this film matches `<<<video_6>>>` one-to-one — fruits
> and berries replace the blocking spheres exactly where and when they move; nothing is added,
> re-timed or improvised.

---

## Block 4 — SFX

> SFX only: bright meadow ambience, bullet-drop whistle, crown-splash thump with a racing spray
> ring, rocket-rise whoosh freezing to weightless silence, three crisp slice-slides with
> metallic shings and soft juicy fruit-half slices, satisfying snap-back, whip-spin whoosh, ...
> five springy letter pops building the headline, serene meadow ambience fading to silence.

Chronological, one phrase per visible event, in cut order.

---

## What made this one work

- Identity by hue, six clearly separated colours, map written twice
- Black void read as placeholder, never as a night shot
- Tracking grid explicitly redirected to caustics
- Two black gaps turned into underwater shots that need no blocking
- One neutral hero, unique in frame, identified by uniqueness alone
- Motion lock stated three times, in three different blocks