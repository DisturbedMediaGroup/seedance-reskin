# Model notes

The five-block structure is portable. What changes per target model: reference-token syntax,
length budget, how many reference assets are accepted, and whether a video reference is
supported at all.

**Verify these against the platform's current docs before writing.** Limits move. The notes
below are shape guidance, not a specification.

## Reference-token syntax

Platforms differ in how a prompt points at an uploaded asset:

- **Slot tokens** — `#Video1`, `#Image1` (Higgsfield / Seedance — **no space between the word
  and the number**), `@Video 6 · 30s`, `@Image 14` (older Higgsfield blog posts), or
  `<<<video_6>>>` style, injected by the host tool. **Ask which form the target platform uses,
  or copy the form from a prompt the user has already run** — a wrong sigil, or a stray space
  inside the token, means it is read as plain text and the reference is silently ignored.
  Used in the worked example. Best case: the prompt can name each asset inline, anywhere.
- **Ordinal prose** — "the first reference image", "the second image". Weaker; ambiguity grows
  with asset count. Compensate by describing the asset alongside the ordinal every time:
  *"the second image (violet fuzzy hills)"*.
- **Single implicit reference** — one image or one video, never named in text. The whole
  translation-map approach collapses. Fall back to describing objects by colour in prose:
  *"the blue can becomes blueberry"*.

Check which one applies before writing block 1.

## Known limits

| Model | Limit | Source |
|---|---|---|
| Seedance 2.5 | ~1000 words recommended | user, 2026-09-07 |

**Count the words before delivering** — `wc -w` on the draft, not an estimate. A prompt of this
shape lands at 1100–1200 words without trying; the trim below is a normal step, not an
exception. Report the count to the user with the prompt.

## Length budget

Prompts of this shape run long — the worked example is roughly 900 words. Where the budget is
tight, cut in this order:

1. **SFX block** — drop entirely if audio is generated separately
2. **Per-cut material detail** — keep the camera verb and object action, drop the adjectives
3. **Lens realism list** — collapse to `physically accurate camera, motion blur, shallow DOF,
   fine grain`
4. **Environment paragraphs** — collapse to one line each

Never cut, at any budget:

- the translation maps
- the motion lock (all three instances)
- the timecodes
- `do NOT copy its placeholder materials`

Those four are what the technique *is*.

## Duration and cut count

Most models generate clips far shorter than a full commercial. Two paths:

**Per-cut generation** — generate each cut separately, assemble in an editor. The prompt
structure stays identical; you emit one prompt per cut, each carrying its own trimmed copy of
the reference definitions and technical block, and the corresponding segment of the blocking
video as its motion reference.

This is usually the better path: shorter clips hold motion lock far better, and a failed cut
is re-rolled alone instead of re-rolling 30 seconds.

**Single-pass** — only where the model genuinely accepts the full duration with a video
reference. Expect motion-lock drift to grow with length; the later cuts drift first.

When splitting, keep the timecodes in each per-cut prompt anyway. They tell the model the
clip's duration and its speed-ramp position.

## Video reference support

If the target model accepts no video reference at all, this skill does not apply — there is no
motion to lock. Say so rather than producing a prompt that pretends otherwise. The blocking can
still be used as a shot list for a human, or frame-extracted into per-cut still references,
but the one-to-one motion claim must be dropped from the text.

## Per-model quirks worth checking

Before writing, confirm for the target platform:

- Does it accept a video reference, and in what role (motion / style / first-frame)?
- How many image references, and are they addressable individually?
- Is there a separate negative-prompt field? If yes, move the prohibitions there instead of
  inlining them.
- Does it accept explicit timecodes, or does it ignore them?
- Native aspect and duration — a prompt asking for an unsupported combination gets silently
  resampled.

Record what you find here for next time.