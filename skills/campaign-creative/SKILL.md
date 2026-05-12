# campaign-creative

## Purpose
Generate a multi-size campaign creative for a client's LinkedIn campaign — feed post, square post, and story format — using their Claude Design System.

Produces visually cohesive creatives across all three sizes for a single campaign or thought-leadership moment.

## When to use
- LinkedIn thought leadership campaigns
- Standalone campaign moments (product launch, event teaser, announcement)
- One-off LinkedIn posts where the visual is the hero
- Any post that needs to work across multiple LinkedIn surface areas (feed, square reshare, story)

Do NOT use this skill for:
- Recurring newsletter headers (use `newsletter-header`)
- LinkedIn company page banners (use `linkedin-header`)
- Email banners (use `email-banner`)
- Generic post images where text-only would work fine

## Inputs

Required:
- Campaign topic or theme
- Headline (one main line, ideally under 12 words)
- The client's Design System must be configured in Claude Design

Optional:
- Supporting subline (one short phrase under the headline)
- Specific motif preferences (if the brand has a signature visual element — arc, dot, line, geometric shape)

Read from `/01-client-knowledge/`:
- `brand-voice.md` — for tone direction
- `style-guide.md` — for visual style notes

## Output spec

Three sizes produced in one pass:
- **LinkedIn feed post:** 1200×627px
- **Square post:** 1080×1080px
- **LinkedIn story:** 1080×1920px

Format: PDF by default. PNG only if Send-to-Canva path is configured.

Cohesion: same visual language, same headline, layouts tuned to each format.

Style: clean and minimal, no stock photography, no gradients.

## Prompt

Paste verbatim into Claude Design. Replace bracketed variables.

The prompt is calibrated against learnings from real production runs. Do not shorten or simplify the format-specific instructions — the verbosity is the value. Generic prompts produce unbalanced feed posts, mid-frame floating elements in stories, and inconsistent strikethrough treatment.

```
Create a campaign creative for [client name] for a LinkedIn 
[campaign theme] campaign. Use the [Design System name] 
Design System.

Produce three sizes:
- LinkedIn post (1200×627px)
- Square post (1080×1080px)
- LinkedIn story (1080×1920px)

The creative should feel cohesive across all three — same 
visual language, same headline, layouts tuned to each format. 
No stock photo feel, no gradients, clean and minimal.

Headline: "[headline]"

If the headline contains a negated phrase (e.g. "not X but Y", 
"the companies winning aren't doing X, they're doing Y"), 
show the negated phrase with a strikethrough — not greyed 
out — consistently across all three formats. The strikethrough 
should be visible and intentional, not subtle. The text 
remains readable; the strikethrough makes the negation visual.

LAYOUT BY FORMAT:

For the feed post (1200×627px):
Use a balanced two-column layout. Headline and body text 
left-aligned on the left half. The brand motif contained and 
purposeful on the right. Avoid large empty areas. Both columns 
should feel weighted — neither should dominate or feel empty.

For the square post (1080×1080px):
Headline top half. Brand motif spanning the lower half 
edge-to-edge. The motif should feel like the foundation 
the headline sits on top of, not a separate decorative 
element.

For the story (1080×1920px):
Fill the vertical space intentionally. The brand motif should 
grow from the bottom of the frame upward toward the headline 
— not float in the middle of empty space. No gap between the 
headline block and the motif. If the brand has a primary 
accent colour, that element should rise highest. The eye 
should travel from bottom motif → top headline naturally.
```

## Why this prompt is calibrated this way

The format-specific layout instructions exist because earlier production runs surfaced consistent failures:

- **Feed posts** without explicit two-column instruction produced unbalanced layouts with large empty areas on one side
- **Stories** without "fill vertically" instruction produced floating motifs in mid-space with awkward gaps between headline and visual
- **Strikethrough** without explicit instruction was inconsistently rendered — sometimes as greyed text, sometimes as small subtle line, sometimes not at all

Each instruction in the prompt addresses a real failure mode observed in testing. Do not remove format-specific instructions to shorten the prompt.

## Quality checks

Before exporting, confirm:
1. All three sizes are exactly the correct pixel dimensions
2. The headline reads the same way across all three (same wording, same hierarchy)
3. Any strikethrough is visible at LinkedIn display size, not just at full resolution
4. The feed post has balanced columns — no large empty quadrant
5. The square post has the motif as a foundation under the headline, not floating
6. The story fills the vertical space — no awkward mid-frame gap
7. Brand colours and motifs are consistent across all three
8. Text is legible at LinkedIn's compressed display sizes (especially on mobile)

## Approval gate

Show all three sizes side-by-side before exporting. Do not save to `/04-drafts/` until human confirms each.

## Variants

If the human wants variation within a campaign:
- Ask Claude Design to produce a second set with a different accent colour
- Ask for a "darker" or "lighter" variant
- Ask for a typography variant (e.g. serif vs sans-serif headline)

Run variants as separate Claude Design generations, not as additional sizes in the same generation.

## Common refinement requests

If the first generation isn't right, these are the most useful refinement prompts:

- **"More white space"** — if the design feels cluttered
- **"Make the motif more contained"** — if the motif is dominating the frame
- **"Increase contrast on the headline"** — if text is hard to read
- **"Make the strikethrough heavier"** — if the negation isn't visually obvious
- **"Reduce the empty area in the [feed post / story]"** — if a specific format has a layout issue despite the prompt

---

*Skill version: 1.0 — May 2026*
*Maintained in: brousethinkgrow/marketingskills*
