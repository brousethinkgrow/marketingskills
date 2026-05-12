# email-banner

## Purpose
Generate a hero email banner for a client's monthly or weekly newsletter, using their Claude Design System. Handles the Claude Design → Canva → PNG export path required by most email service providers.

## When to use
- Monthly newsletter hero banners
- Email campaign hero images
- Any image embedded in an HTML email (HubSpot, Mailchimp, ConvertKit, Klaviyo, ActiveCampaign)

Do NOT use this skill for:
- LinkedIn newsletter cover images (use `newsletter-header` — different distribution, PDF acceptable)
- Email signature graphics (different proportions and constraints)
- Social post images (use `campaign-creative`)

## Why this skill is different

Email banners are the one image type where PNG export is mandatory. Most email service providers do not embed PDF reliably — HubSpot specifically strips or breaks PDF embeds in the email rendering engine. Claude Design exports PDF natively but not PNG, so the export path runs through Canva:

```
Claude Design (produces the image)
  ↓
Send to Canva (Claude Design feature)
  ↓
Canva MCP (retrieves the PNG)
  ↓
Save to /04-drafts/[client-name]/
```

Canva MCP must be active in the chat session for this path to work. If Canva MCP is not active, the skill falls back to producing PDF only and flagging for manual PNG conversion.

## Inputs

Required:
- Theme or focus of the email (e.g. "this month's newsletter theme is AI adoption")
- The client's Design System configured in Claude Design
- Canva MCP active in the chat session (verify via integrations.md `Canva MCP: required`)

Read from `/01-client-knowledge/`:
- `style-guide.md` — for brand assets, colour treatment, design system reference
- `org-knowledge.md` — for client wordmark/logo specifications

Read from `/01-client-knowledge/[client-name]/integrations.md`:
- Design System name
- `email-provider` (HubSpot / Mailchimp / etc — affects optimal dimensions)

## Output spec

Default dimensions:
- **HubSpot:** 600×280px (standard hero banner)
- **Mailchimp:** 600×300px (standard)
- **Klaviyo:** 600×300px (standard)
- **ConvertKit:** 600×280px
- **ActiveCampaign:** 600×280px

If email provider is unknown or "other", default to 600×280px which renders acceptably in most clients.

Format: PNG (required for email embed)
Style: premium and editorial — not promotional. The banner is the hero of a monthly read, not a sales banner.

## Prompt

Paste verbatim into Claude Design. Replace bracketed variables.

```
Create a HubSpot email banner for [client name]. Use the 
[Design System name] Design System.

Width: 600px. Height: 280px.

The banner is the hero image at the top of a monthly 
newsletter email. The tone is premium and editorial, 
not promotional — closer to a magazine masthead than a 
campaign ad.

Headline: "[this month's theme]"

Include the [client name] wordmark in a corner — small 
and unobtrusive, not the focal point.

Use the primary brand colour on a dark background. 
Light typography on dark works best for editorial 
email banners. No gradients. Clean and minimal.

Avoid:
- Stock photo feel
- Generic illustration
- Multiple competing focal points
- Anything that feels like a sales banner
```

## Quality checks

Before exporting, confirm:
1. The image is exactly the right dimensions for the client's email provider
2. The headline is legible at standard email display sizes (banners render at 600px wide on desktop, ~320px on mobile)
3. The client wordmark/logo is visible but not dominant
4. Dark background + light typography (or the brand's editorial treatment)
5. No promotional language ("Buy now", "Limited time") — this is editorial

## Export path

After human approves the design in Claude Design:

1. Click **Send to Canva** in Claude Design
2. Wait for the design to appear in the user's Canva account
3. Use Canva MCP to retrieve the PNG export:
   - Search for the design by name in the user's Canva
   - Export as PNG
   - Download to `/04-drafts/[client-name]/`
4. Filename: `[email-banner]-[YYYY-MM-DD].png`

If Canva MCP is not active or the retrieval fails:
- Save the Claude Design output as PDF to `/04-drafts/[client-name]/`
- Surface a flag to the human: "PNG export failed. The PDF version is in your drafts folder. Convert to PNG manually before uploading to [email provider]."
- Do not block the rest of the workflow

## Approval gate

Show the generated banner before exporting. Two-step approval:
1. Approve the Claude Design output
2. Approve the final PNG after Canva export (visual check that nothing degraded in the conversion)

## Edge cases

**Banner needs to feature a specific image or photograph:** Claude Design is not always strong with photo composition. For banners featuring a specific photo (e.g. event recap banner with a photo from the event), consider asking the design team or using Canva directly with a template.

**Animated GIF or video banner needed:** This skill does not produce animation. Most email clients strip animation anyway — recommend a static PNG with motion-implying composition.

**Client has an in-house design team:** If `integrations.md` shows `Method: designer-brief`, this skill generates a brief instead of going through Claude Design. The brief format matches the standard designer brief structure in Stage 2 Step B.

---

*Skill version: 1.0 — May 2026*
*Maintained in: brousethinkgrow/marketingskills*
