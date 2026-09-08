# seedance-reskin

**Turn a grey 3D blocking animation into a finished video-generation prompt.**

The blocking owns the motion. The prompt owns the look. Nothing crosses.

AI video models are bad at coherent camera motion across 30 seconds and multiple cuts, and good
at materials and light. A blocking render supplies exactly what they lack. The prompt supplies
exactly what the blocking cannot carry — brand, colour, world, matter.

This is a [Claude Code skill](https://docs.claude.com/en/docs/claude-code/skills), but the
reference documents are plain Markdown and useful to anyone writing these prompts by hand.

---

## Install

Copy the `seedance-reskin/` folder into your skills directory:

```bash
# personal (all your projects)
cp -r seedance-reskin ~/.claude/skills/

# or per project
cp -r seedance-reskin .claude/skills/
```

Restart Claude Code. Verify with `/seedance-reskin`.

**Not using Claude Code?** Read `seedance-reskin/SKILL.md` as a method document and
`seedance-reskin/references/` as the detail. Everything works as plain reading.

---

## Use

Give the assistant the blocking video, your reference images, and what you want:

> Here is the blocking. Six can flavours, three environments — make the prompt.

Triggers: `blocking`, `previz`, `animatic`, `re-skin this`, `make a prompt from this video`,
`napravi prompt od blockinga`.

You get back a five-block prompt:

1. **Reference definitions** — each asset labelled with what it governs and what it does not,
   plus the translation maps (black background → this location, grey sphere → a lime, blue
   clone → this design sheet)
2. **Technical block** — format, render style, lens realism, motion lock, object lock, and the
   tie-breaker
3. **Prompt** — one block per cut, with timecodes: world, camera, objects, materials
4. **SFX** — chronological, event-aligned
5. **Guardrails** — prohibitions attached to the cut they apply to

Before the prose you get a **cut list** to approve. Check the timecodes there — wrong cuts are
cheap to fix at that stage and expensive later.

---

## What it protects you from

Video models fail in specific, repeatable ways. The skill's hardening pass writes the
counter-measures:

| Failure | Counter-measure |
|---|---|
| Reproduces the grey blocking look | `do NOT copy its placeholder materials` + per-object translation maps |
| Invents its own camera, re-times cuts | Motion lock stated three times, in three blocks |
| Text and blocking disagree, model picks wrong | **Tie-breaker**: *if the text disagrees with the video, the video wins* |
| Multiplies objects — six ninjas become nine | **Countable constraints**: the number, the enumeration, and the denial of one more |
| Loops an action — reloads the gun three times | **Forward-only lock**: each step happens exactly once, never backward |
| Background figures stand frozen | **Anti-freeze**: the blocking gives positions, not life |
| Misreads an abstract shape — a curved lamp becomes a snake | **Negative shape assertion**: name the wrong reading and refuse it |
| Renders a cause that is not in frame — "six blades" become six visible blades | **Never name an off-screen cause**: describe the result, refuse the tool |
| Lights the void — black drifts to grey on the push-in | A void is not a studio: deny every surface word, confine the light, lock the black as `#000000` |
| Glow bleeds past the subject | `bloom`, `flare`, `crisp speculars` are requests — ban them by name |
| Ignores a reference because the token is off by one space | **Verify the token verbatim** with the user before writing — a wrong tag unbinds with no error |
| Reproduces a product sheet's grid — four cars in a row | **Disown the layout**: "appearance only… it is one car, not four" |
| An unblocked effect (fluid, smoke, sparks) fills the frame | **Containment rule**: name its source, direction *and* limit — a limit alone has nothing to attach to |
| Blocking colour survives into the render — magenta spheres stay pink | Colour can select a *variant* of a recognisable shape; it cannot carry the *identity* of an ambiguous one. Re-render near the final colour. |

---

## Before you render the blocking

Read [`seedance-reskin/references/blocking-spec.md`](seedance-reskin/references/blocking-spec.md).
It is written to hand straight to the 3D artist, and it covers render settings, what to turn
off, and what to hand over besides the file.

The one rule: **every object the prompt must name has to be nameable from the blocking alone.**
If identifying an object needs *"the third one from the left, roughly"*, the blocking fails —
give it a distinct colour.

Short version:

**Do** — flat matte saturated colours, clearly distinct hues, maximum background contrast,
primitives are fine, motion visibly above noise, leave black gaps where you have nothing.

**Do not** — chrome / glass / transparency on anything that needs an identity (chrome shows the
room, not its own colour, so a green can and a red can are both just silver), same-colour
objects touching, a background sharing a colour with an object, many tiny objects that each
need naming.

Counter-intuitive one: **do not make the blocking look near-final.** The closer it is to a
finished render, the more the model copies its materials instead of your reference sheets.
Crude and colourful beats polished and grey.

---

## Reuse the blocking

The motion is the expensive artefact; the look is cheap. The same blocking carries any number of
finished films — one car-commercial previz has shipped as a photoreal night chase, a hand-painted
2.5D film, a black-and-white manga, and a toy stop-motion western. The structural lock (cuts,
camera, timing, geometry) never changes; only the nouns do. See
[`references/style-layer.md`](seedance-reskin/references/style-layer.md).

---

## Contents

| File | Contents |
|---|---|
| `seedance-reskin/SKILL.md` | The method — workflow, output structure, hardening pass, self-check |
| `seedance-reskin/README.md` | Short usage guide |
| `references/blocking-spec.md` | **What to render.** Hand to the 3D artist. |
| `references/hardening-rules.md` | Counts, forward-only locks, anti-freeze, NOT lists, tie-breaker |
| `references/worked-example.md` | A full 30s commercial, annotated |
| `references/style-layer.md` | One blocking, many finished films |
| `references/model-notes.md` | Token syntax, length budgets, per-cut splitting |
| `examples/` | Two finished prompts from the same 10s blocking |

---

## Examples

Both built from one 10-second blocking — a yellow ellipsoid on black that slices into seven
pieces:

- [`lemon-black-void.txt`](seedance-reskin/examples/lemon-black-void.txt) — the black void kept
  as a deliberate look, 944 words
- [`lemon-with-vortex.txt`](seedance-reskin/examples/lemon-with-vortex.txt) — the void replaced
  by a lemon-and-water vortex, 1000 words

Same motion lock, same counts, same cut timings. Only the world changes.

---

## Length

Prompts of this shape land at 1100–1200 words without trying. Seedance 2.5 recommends staying
under ~1000. **Count the words** (`wc -w`) rather than estimating; `references/model-notes.md`
gives the order to trim in, and the four things never to cut.

---

## Credit

The technique was reverse-engineered from a
[Higgsfield + Blender workflow write-up by @adilinthewild](https://higgsfield.ai/@adilinthewild/blogs/this-blender-higgsfield-ai-workflow-changes-how-you-make-ai-video)
and from prompts developed in production. This repository is an independent method document —
it ships no code from, and is not affiliated with, Higgsfield.

## Licence

MIT.
