---
name: make-slide
description: "Generate single-file HTML presentations with 10 professional themes and speaker notes. Use this skill whenever the user mentions presentations, slides, deck, talk, keynote, pitch, slideshow, or wants to turn any content into a visual presentation — even if they don't explicitly say 'slide'. Also triggers for requests like 'make a talk about', 'create a pitch for', 'turn this into slides', or any request to visualize content for an audience."
---

# make-slide — AI Presentation Skill

## Overview

Generate single-file HTML presentations with speaker notes. The output is always a standalone `.html` file with all CSS and JS inlined — no build step, no framework, no dependencies beyond font CDNs.

**Repository**: [github.com/kuneosu/make-slide](https://github.com/kuneosu/make-slide)
**Theme Gallery**: [make-slide.vercel.app](https://make-slide.vercel.app)

---

## When to Use

Activate this skill when the user's request matches any of these patterns:

- "Make a presentation about..."
- "Create slides for..."
- "Build a slide deck..."
- "Turn this into a presentation"
- "Make a talk about..."
- "Create a pitch deck for..."
- "Generate presentation slides..."
- Any request involving: **presentation**, **slides**, **deck**, **talk**, **keynote**, **pitch**, **slideshow**
- Any request to visualize or structure content for an audience, even without explicit "slide" mention

---

## Input Modes

Determine which mode applies based on the user's input:

### Mode A: Topic Only
The user provides a topic and optionally a duration or audience.
```
"Make a 15-min talk about AI trends"
"Create a presentation on microservices architecture"
```
→ Design the structure, content, and speaker notes from scratch.

### Mode B: Content/Material Provided
The user provides source material (documents, notes, data, articles).
```
"Turn this document into a presentation"
"Create slides based on these meeting notes"
```
→ Organize and distill the material into slides + speaker notes.

### Mode C: Script Provided
The user provides a written speech or speaking script.
```
"Create slides for this speech script"
"Make a visual deck that accompanies this talk"
```
→ Analyze the script's structure and create matching visual slides. The script becomes the speaker notes.

---

## Workflow

Follow these steps in order:

### Step 1: Analyze Input
- Determine the input mode (A, B, or C)
- Estimate the appropriate number of slides:
  - 5-min talk → 8–12 slides
  - 10-min talk → 12–18 slides
  - 15-min talk → 18–25 slides
  - 20-min talk → 25–30 slides
  - 30-min talk → 30–40 slides
- Identify the target audience and tone (technical, business, casual, academic)
- Detect the user's language from the conversation — generate content in that language

### Step 2: Choose a Theme
Present the user with the theme gallery link for browsing:
> **Browse themes here**: https://make-slide.vercel.app

Read `references/themes.md` for the full theme list with GitHub raw URLs and recommendations by context.

If the user doesn't choose a theme, recommend one based on context:
- Tech/developer talk → `minimal-dark` or `neon-terminal`
- Business/corporate → `corporate` or `minimal-light`
- Startup pitch → `gradient-pop` or `keynote-apple`
- Data/analytics → `data-focus`
- Education/workshop → `paper` or `playful`
- Design/creative → `magazine`
- Product launch → `keynote-apple`
- Casual/team event → `playful`

### Step 3: Check Image Needs
Ask the user:
> "Would you like me to identify image insertion points in the slides? If yes, I'll mark them in the outline and you can provide image URLs. If no, I'll use styled placeholders."

- **Yes** → Mark image positions in the outline, ask user for image URLs
- **No** → Use CSS placeholders (emoji, SVG icons, CSS shapes) that match the theme

### Step 4: Generate Outline
Create a slide-by-slide outline with:
- Slide number
- Slide type (from slide types reference)
- Title
- Key points / content summary
- Image placement (if applicable)

Read `references/slide-types.md` for the 12 available slide type patterns.

Present the outline to the user for confirmation. Wait for approval or modifications before proceeding.

### Step 5: Fetch Theme Reference
Fetch the chosen theme's reference files from GitHub:

1. **reference.html** — The complete example presentation with all slide types, styles, navigation, and interactive features. This is the primary template.
2. **README.md** — Design guidelines including color palette, typography, spacing rules, and design philosophy.

Use the raw GitHub URLs listed in `references/themes.md`.

### Step 6: Generate HTML
Using the reference.html as the template:
- Replicate the exact HTML structure, CSS styles, and JS functionality
- Replace the demo content with the user's actual content
- Keep all interactive features (navigation, fullscreen, speaker notes, etc.)
- Match the theme's typography, colors, spacing, and animations
- Ensure all slide types used match the patterns in the reference

Read `references/html-spec.md` for the full HTML requirements including navigation, fullscreen, PDF export, CDN dependencies, image handling, and code blocks.

### Step 7: Generate Speaker Notes
Add speaker notes as `data-notes` attributes on each slide's `<div>`:
```html
<div class="slide" data-notes="Welcome everyone. Today I'll be talking about...">
```
- Notes should be conversational and natural
- Include timing cues where helpful (e.g., "[PAUSE]", "[2 min]")
- Expand on the slide text — don't just repeat it
- Include transitions between slides (e.g., "Now let's move on to...")

### Step 8: Generate Script (Mode A and B only)
For Mode A and B, also generate a separate `script.md` file containing:
- Full speaking script organized by slide
- Timing estimates per section
- Audience interaction cues
- Key points to emphasize

For Mode C, the user already has a script — skip this step.

### Step 9: Save and Deliver
- Save the presentation as `index.html` (or user-specified filename)
- Save the script as `script.md` (if generated)
- Offer to start a local server for preview:
  ```bash
  # Python
  python -m http.server 8000
  # Node.js
  npx serve .
  ```

---

## Output Rules

1. Always output a single `.html` file with all CSS and JS inlined
2. Only allowed external dependencies: Font CDNs (Pretendard, JetBrains Mono) + Prism.js CDN (conditional)
3. Speaker notes embedded as `data-notes` attributes on each slide `<div>`
4. Optionally output `script.md` for full speaking script (Mode A and B)
5. Generate content in the user's language — detect from the conversation context
6. Filename: `index.html` by default, or as specified by the user
7. Vanilla JS only — no JavaScript frameworks
8. Vanilla CSS only — no CSS frameworks

---

## Tips for Best Results

### Content
- Keep slides concise: max 6–8 bullet points per slide
- One core idea per slide
- Use short phrases, not full sentences, on slides
- Save detailed explanations for speaker notes

### Design
- Use the theme's accent color for emphasis and key terms
- Match typography hierarchy exactly from the reference.html
- Maintain consistent spacing and padding throughout
- Work within the theme's design system — don't override its core design

### Speaker Notes
- Write notes in a conversational tone
- Expand, explain, give examples — don't just repeat slide text
- Include timing hints: "[This section: ~3 minutes]"
- Add transition phrases: "Now let's look at...", "Building on that..."
- Mark audience interaction points: "[ASK AUDIENCE]", "[DEMO]", "[PAUSE]"

### Data Visualization
- Use CSS-only charts when possible (bar charts, progress bars, donut charts)
- Keep numbers round and memorable (say "nearly 50%" not "49.7%")
- Highlight the most important metric with size or color

### Performance
- Keep total HTML file size under 200KB ideally
- Use `loading="lazy"` on images
- Minimize complex CSS animations on slides with heavy content

### Accessibility
- Maintain sufficient color contrast ratios
- Include `alt` text on all images
- Ensure keyboard navigation works for all interactive elements
- Use semantic HTML where appropriate

---

## Quick Start Example

If the user says: *"Make a 10-minute presentation about Python best practices"*

1. **Mode**: A (Topic only)
2. **Slides**: ~15 slides
3. **Theme recommendation**: `minimal-dark` or `neon-terminal` (developer topic)
4. **Ask**: Theme preference + image needs
5. **Generate outline** → Get confirmation
6. **Fetch**: `minimal-dark/reference.html` + `minimal-dark/README.md`
7. **Generate**: `index.html` + `script.md`
8. **Offer**: Local server preview

---

*Skill version: 2.0 | Repository: [kuneosu/make-slide](https://github.com/kuneosu/make-slide) | Gallery: [make-slide.vercel.app](https://make-slide.vercel.app)*
