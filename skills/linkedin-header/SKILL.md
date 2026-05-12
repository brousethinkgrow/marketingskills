# linkedin-header

## Purpose
Generate a LinkedIn company page header (banner) for a client using their Claude Design System.

## When to use
- Initial brand asset production during onboarding
- Periodic refresh of the LinkedIn company page header
- After a brand visual refresh
- When the client requests a new header for a campaign window

Do NOT use this skill for personal LinkedIn banners or for feed posts within the company page. Those use different skills.

## Inputs

Read from the client's knowledge files in `/01-client-knowledge/`:
- `org-knowledge.md` — for client name, tagline, positioning
- `style-guide.md` — for brand voice direction
- `brand-voice.md` — for messaging cues

Read from `/01-client-knowledge/[client-name]/integrations.md`:
- Design System name (saved during Stage 0D Check 3)

If a Design System is not yet configured, stop and output:
"The LinkedIn header skill requires the client's Design System to be configured in Claude Design first. Run Stage 0D Check 3 before continuing."

## Output spec

- Dimensions: 1584×396px (LinkedIn's current company page banner spec)
- Format: PDF by default
- Tone: professional and premium
- Style: clean, minimal, no stock photography feel, no gradients, no generic clip art

## Prompt

Paste verbatim into Claude Design. Replace bracketed variables.

```
Create a LinkedIn company page header for [client name].
Use the [Design System name] Design System.

Dimensions: 1584×396px.

The header should feel professional and premium. No stock 
photo feel, no gradients, no clip art. Clean and minimal.

Treat the banner as the company's first impression on 
LinkedIn — it should communicate positioning at a glance, 
not list features or services.

If a tagline or positioning statement fits naturally, use:
"[tagline from org-knowledge.md if available, else omit]"

Use the primary brand colour as the dominant background or 
foreground tone. Use one accent colour sparingly. White 
space is preferable to clutter.
```

## Quality checks

Before exporting, confirm:
1. The image is exactly 1584×396px
2. The client wordmark or logo is visible and on-brand
3. No stock-photo-feel imagery, no generic gradients
4. The banner reads cleanly at LinkedIn's display size (banners are often shown at half-size on desktop, smaller on mobile)
5. Any text in the header is legible at small sizes — avoid more than one short line

## Approval gate

Show the generated header to the human before exporting. Do not save to `/04-drafts/` until human confirms.

## Variants

The default behaviour produces one header. If the client wants seasonal or campaign variants, ask Claude Design to produce up to three in one pass — same composition, different accent colour or supporting motif.

---

*Skill version: 1.0 — May 2026*
*Maintained in: brousethinkgrow/marketingskills*
