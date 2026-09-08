# seedance-reskin

Turns a grey 3D blocking animation into a finished video-generation prompt.

The blocking owns the motion. The prompt owns the look. Nothing crosses.

## What it is for

You have a previz / blocking / animatic render — primitives, flat colours, no materials — and
a set of reference images (products, characters, environments). You want an AI video model to
reproduce that exact camera work and object choreography with the real look.

The skill writes the prompt that does that.

## Use it

Give the assistant the blocking video, the reference images, and what you want:

> Here is the blocking. Six can flavours, three environments — make the prompt.

Or invoke directly:

```
/seedance-reskin
```

Triggers on: "blocking", "previz", "animatic", "re-skin this", "make a prompt from this video",
"napravi prompt od blockinga".

## What comes back

Five blocks:

1. **Reference definitions** — each asset labelled with what it governs and what it does not,
   plus the translation maps (black background → this location, grey sphere → a lime, blue
   clone → this design sheet)
2. **Technical block** — format, render style, lens realism, motion lock, object lock
3. **Prompt** — one block per cut, with timecodes: world, camera, objects, materials
4. **SFX** — chronological, event-aligned
5. **Guardrails** — distributed prohibitions

Before the prose you get a **cut list** to approve. Check the timecodes there — wrong cuts are
cheap to fix at that stage and expensive later.

Then a **hardening pass** over the draft — counts that stop the model multiplying objects,
forward-only locks that stop it looping an action, an anti-freeze clause so background figures
stay alive, and a NOT list for every blocking shape that could be misread. Plus the one sentence
a long prompt cannot ship without: *if the text and the blocking disagree, the blocking wins.*

## Reuse the blocking

The motion is the expensive artefact; the look is cheap. The same blocking can carry any number
of finished films — one car-commercial previz shipped as photoreal night chase, hand-painted
2.5D, black-and-white manga and toy stop-motion western. Ask for another style and the shot
list and camera lock stay; only the nouns change.

## What it will ask you

Anything it cannot infer from the blocking:

- which colour maps to which product
- which environment belongs to which cut
- what the placeholder artefacts (grids, nulls, empty background) should become

Answer those and it writes the rest.

## Before you render the blocking

Read [`references/blocking-spec.md`](references/blocking-spec.md). It is written to hand
straight to the 3D artist.

The one rule: **every object the prompt must name has to be nameable from the blocking alone.**
If identifying an object needs "the third one from the left, roughly", the blocking fails —
give it a distinct colour.

Short version of the spec:

**Do** — flat matte saturated colours, clearly distinct hues, maximum background contrast,
primitives are fine, motion visibly above noise, leave black gaps where you have nothing.

**Do not** — chrome / glass / transparency on anything that needs an identity (chrome shows
the room, not its own colour, so a green can and a red can are both just silver), same-colour
objects touching, background sharing a colour with an object, many tiny objects that each need
naming.

Counter-intuitive one: **do not make the blocking look near-final.** The closer it is to a
finished render, the more the model copies its materials instead of your reference sheets.
Crude and colourful beats polished and grey.

## Files

| File | Contents |
|---|---|
| `SKILL.md` | The method — workflow, output structure, self-check |
| `references/blocking-spec.md` | What to render. Hand to the 3D artist. |
| `references/worked-example.md` | A full 30s commercial, annotated |
| `references/hardening-rules.md` | Counts, forward-only locks, anti-freeze, NOT lists, tie-breaker |
| `references/style-layer.md` | One blocking, many finished films |
| `references/model-notes.md` | Per-platform syntax, length budgets, per-cut splitting |

## Why it works

Video models are bad at coherent motion across 30 seconds and multiple cuts, and good at
materials and light. The blocking supplies exactly what they lack. The text supplies exactly
what the blocking cannot carry — brand, colour, world, matter.

The failure mode in both directions is the same: the model does not know which reference owns
which property. So every reference gets labelled with its scope, and the motion lock is stated
three times in three different blocks. Repetition is weight, not redundancy.