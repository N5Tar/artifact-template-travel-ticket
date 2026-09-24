---
name: artifact-template-travel-ticket
description: "Turn a travel or everyday photo into a collectible keepsake poster, in one of five layouts: Travel Ticket 旅行票根 (default), Boarding Pass 登机牌, Vintage Postcard 复古明信片, Instant Film 拍立得, or Film Frame 胶片帧. Use when the user selects this template, names any of these styles, or explicitly invokes $artifact-template-travel-ticket. 把照片制作成复古票根、登机牌、明信片、拍立得或胶片帧海报，五种样式可选。"
---

# Travel Ticket 旅行票根（多样式版）

Create an image in one of five keepsake layouts. Keep the reference files unchanged.

## Styles

Read `artifact-template.json` and pick the style reference file from its `variants` array.

| Style ID | Name | Aliases to recognize | Reference file | Canvas |
|---|---|---|---|---|
| `travel-ticket` | Travel Ticket 旅行票根 | ticket, ticket stub, 票根, 票券, default | `assets/reference.png` | 3:4 vertical |
| `boarding-pass` | Boarding Pass 登机牌 | boarding pass, 登机牌, flight card | `assets/styles/boarding-pass.png` | 3:4 vertical |
| `postcard` | Vintage Postcard 复古明信片 | postcard, 明信片, post card | `assets/styles/postcard.png` | 3:4 vertical |
| `instant-film` | Instant Film 拍立得 | instant film, polaroid, 拍立得, 即时成像 | `assets/styles/instant-film.png` | 3:4 vertical |
| `film-frame` | Film Frame 胶片帧 | film frame, negative, 胶片, 负片, 35mm | `assets/styles/film-frame.png` | 3:2 horizontal |

## Workflow

1. Read `artifact-template.json` and resolve its paths relative to this skill directory.
2. Select the style using the rules below, then load only that style's reference file.
3. Invoke $imagegen with the selected reference as a layout and material reference, and the user's photo as the content and color source.
4. Treat the user's prompt and available sources as the content input. Do not invent factual claims merely to fill the composition.
5. Apply the semi-adaptive background rules, then the per-style layout keys below.
6. Preserve the selected reference's visual language unless the user explicitly requests a deviation. Never blend two references into one image.
7. Visually inspect the generated image for fidelity and defects, then return the final image.

## Style selection

Explicit request first, default second.

1. If the user names a style or uses one of its aliases, use that style. This always wins.
2. If the user asks for several styles at once, for example "每种都来一张" or "all five", generate one image per style, each with its own background color.
3. If the user does not specify a style, use `travel-ticket`. This preserves v1.0 behavior, so existing prompts keep working.
4. If the request is ambiguous or names a style that does not exist, pick the closest match and state which style you used. Never silently invent a sixth layout.

## Shared rules

- Every style keeps the photo's subject, composition and recognizable details. Re-stage the photo, do not replace it.
- Location text stays within 12 characters so it never wraps or is clipped.
- Barcodes, perforation holes, postmarks and frame numbers are decorative. Do not imply they are scannable or real.
- Text must render correctly. If glyphs garble, regenerate with simpler, shorter strings.
- One background color is chosen independently per image in a set.

## Semi-adaptive background color

The background hue is variable. Do not copy the hue from the style reference by default. A reference controls layout, texture, lighting, shadow and spatial composition—not a fixed background color.

For every source photo:

1. Identify one supportive dominant or secondary color from a large, visually important area of the photo, such as sky, water, foliage, masonry, night, or warm artificial light.
2. Convert it into a quiet background color by reducing saturation and, when needed, shifting lightness so the keepsake stays clearly separated from the backdrop.
3. Favor harmony over literal color matching. The background should echo the photo without competing with it.
4. Choose the color independently for each image in a set. Do not force all outputs to share one hue.
5. Use blue only when the source photo genuinely supports blue or blue-gray. Otherwise use an appropriate muted family such as sage, moss, warm sand, ochre, terracotta, mauve, charcoal, or stone gray.
6. If the photo has no stable color cue, fall back to a neutral warm gray or stone tone rather than the reference hue.

Never introduce a saturated unrelated color, a multicolor gradient, or a background that reduces the contrast of the keepsake edge.

## Per-style layout keys

Apply the relevant block after the shared rules.

### travel-ticket (default)

Centered warm ivory ticket on a woven fabric backdrop; rounded photo window; punched perforations and a tear-off dashed line; location in elegant serif capitals above the window; serial number below the window; decorative barcode near the bottom.

### boarding-pass

Tall white cardstock card; header band in a deep muted tone reading BOARDING PASS; rounded photo window in the upper third; tidy field grid below with monospace labels and serif values (DESTINATION, FLIGHT, DATE, GATE, SEAT); dashed tear-off line with perforations; barcode near the bottom. Use the location text as DESTINATION. Derive the header band color from the photo, keeping it deep enough for white lettering.

### postcard

Photo fills nearly the whole card with only a slim off-white margin; aged cardstock with fine grain, subtle vignette and gently faded edges; perforated stamp block at the top right with a circular postmark overlapping it; location in serif capitals plus a short handwritten-style line; small letterspaced caption. Keep printed text off the main subject's face or focal detail.

### instant-film

Thick off-white frame with even side margins and a distinctly wide bottom chin; photo inset with softly rounded corners and gentle chemical fade; location written on the chin in casual marker handwriting; small monospace date in a corner. Keep the chin text short, since the handwriting area is narrow.

### film-frame

Horizontal composition only. Near-black film base running full width with rows of sprocket perforations top and bottom; one clean photo frame between them with fine grain, leaving base visible at both ends; tiny letterspaced edge markings including a frame number; faint dust and hairlines. The backdrop here is the film base itself, so the semi-adaptive rule applies only to the photo's own color rendering, not to the surroundings.

## Fidelity

Preserve the selected reference's composition, visual hierarchy, typography, material treatment, lighting and recurring elements. Preserve the palette relationship and restraint, but derive the backdrop hue from the user's photo according to the semi-adaptive color rules.

User instructions control requested content, style choice and explicit deviations. The selected reference controls layout and formatting where the user has not requested a change.
