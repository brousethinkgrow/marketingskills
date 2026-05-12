# newsletter-header

## Purpose
Generate a newsletter header (issue cover image) for a client's recurring newsletter, using their Claude Design System and the established header pattern from previous issues.

## When to use
- Weekly or monthly newsletter production
- Any recurring newsletter format where the header layout stays consistent and only specific fields (issue number, headline, date) change

Do NOT use this skill for:
- The first-ever newsletter header (no pattern exists yet — use `campaign-creative` skill with newsletter framing instead)
- Standalone campaign creatives
- One-off blog post hero images

## Inputs

Required:
- An existing newsletter header from a previous issue (uploaded to chat by the human, OR referenced from `/06-published/` if available)
- The new issue's headline
- The new issue's number and date
- The client's Design System must be configured in Claude Design

If no previous issue header exists, stop and tell the human:
"This skill requires a previous newsletter header to extract the pattern from. For a first-issue header, use the campaign-creative skill instead."

## Output spec

- Dimensions: match the source header exactly (commonly 1200×670px for LinkedIn newsletters, but extract from the source)
- Format: PDF by default. If client needs PNG (e.g. for HubSpot email embed), the Send-to-Canva path is required and Canva MCP must be active.
- Style: identical to the source header — same layout, same typography, same colour treatment, same recurring visual elements

## Prompt

Upload the source header image first, then paste this prompt verbatim.

```
I'm uploading the existing [Newsletter Name] newsletter 
header. Extract the layout, typography, colour treatment, 
and any recurring visual elements exactly as they are.

Then produce a new version for Issue [X] with:
- Headline: "[new headline]"
- Date: "[date]"
- Issue number: [X]

Keep everything else identical to the source. Do not 
reinvent the design. The goal is recognisability — readers 
should immediately see this is the same newsletter at a 
glance.

Same dimensions as the source.
```

## Quality checks

Before exporting, confirm:
1. The new header is visually nearly identical to the source except for the three variable fields
2. The headline is legible and fits the available space (truncate or adjust line breaks if needed)
3. Issue number and date are in the correct position from the source
4. Brand colours, fonts, and recurring motifs are preserved exactly

## Approval gate

Show the generated header alongside the source for comparison. Do not save to `/04-drafts/` until human confirms.

## Edge cases

**Source header has placeholder text or low quality:** If the source header is from an early issue where the layout was still evolving, ask the human to upload a more recent issue's header instead.

**Headline is too long:** Ask Claude Design to produce two variants — one with the headline as-is (possibly wrapping to two lines), one with a shortened version. Surface both for the human to choose.

**Client wants a one-off variation:** If the issue is themed (e.g. anniversary issue), use the `campaign-creative` skill for that issue instead and resume the standard pattern from the next issue.

## PNG export for email distribution

If the newsletter is distributed via email (e.g. HubSpot, Mailchimp) and PNG is required:

1. Generate the header in Claude Design as normal
2. Use Send to Canva from Claude Design
3. Retrieve the PNG via Canva MCP (Canva MCP must be active in the chat)
4. Save PNG to `/04-drafts/[client-name]/`

The standing instructions handle this routing automatically based on `png-needed` in the discovery map. This skill just produces the design.

---

*Skill version: 1.0 — May 2026*
*Maintained in: brousethinkgrow/marketingskills*
