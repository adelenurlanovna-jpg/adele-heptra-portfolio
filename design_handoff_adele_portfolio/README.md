# Handoff: Adele Keutayeva — Product Studio Portfolio

## Overview
Single-page personal portfolio landing for **Adele Keutayeva** — a solo product studio building AI agents, bots, landings, platforms, and automations. Heavily inspired by the visual language and structure of [shrug-person-78902957.figma.site](https://shrug-person-78902957.figma.site) (Jack — 3D Creator), rebuilt with Adele's content, palette, and typography.

The page has five vertically-stacked sections: Hero → Scroll-driven Marquee → About → Services → Projects (with sticky stacking cards).

## About the Design Files
The files in this bundle are **design references created in HTML** — prototypes showing the intended look and behavior. They are not production code to ship as-is.

The task is to **recreate this HTML design in the target codebase's environment** (React, Next.js, Astro, etc.) using its established patterns, component libraries, and routing. The HTML here uses CDN-loaded React 18 + Babel-in-the-browser for live editability; the production version should use a proper build pipeline.

If no codebase exists yet, **Next.js 14 with TypeScript + Tailwind + Framer Motion** is the recommended stack — it matches the services Adele offers and the patterns used in the prototype translate 1:1.

## Fidelity
**High-fidelity (hifi).** Pixel-perfect mockup with final colors, typography, spacing, animations, and interactions. All values below are exact — recreate using your codebase's existing libraries and patterns where possible, but match the visual output precisely.

## Tech Stack Used in Prototype
- **React 18.3.1** (UMD, CDN)
- **Framer Motion 10.18.0** (UMD, CDN)
- **Tailwind CSS** (CDN, JIT)
- **Babel Standalone 7.29.0** (in-browser JSX transpiler — DEV ONLY, must be replaced in production)
- **Google Fonts**: Kanit, Fraunces, DM Serif Display, Italiana, Manrope, Tenor Sans

## Theme System
The page supports 4 swappable themes via CSS custom properties + a Tweaks panel. Currently the **default theme is "Mauve Magenta"**. All theme tokens are defined in `THEMES` constant in the script.

### Mauve Magenta (default / current)
```css
--bg: #0F0A14;                     /* deep mauve-black background */
--text: #F0D9E8;                   /* lavender-pink body text */
--border: #F0D9E8;
--service-bg: #FFFFFF;             /* white Services section */
--service-text: #0F0A14;
--service-border: rgba(15,10,20,0.18);
--heading-from: #9B6A8F;           /* heading gradient top */
--heading-to:   #FFC9E3;           /* heading gradient bottom */
--font-display: 'DM Serif Display', serif;
--font-display-weight: 400;
--font-body: 'Manrope', sans-serif;
```

### Other themes (kept available)
- **default** — Original Jack style: Kanit Black, cold gray gradient
- **roseNoir** — Fraunces serif, warm rose-gold on dark warm background
- **softPastel** — Italiana display + Manrope body, cream background, dark wine Services section
- **mauveMagenta** — current

## Screens / Views

This is a single long-scroll page. Each "screen" below is a stacked section.

---

### 1. Hero (full viewport)
**Purpose:** Introduce Adele with a giant gradient heading, photo, tagline, contact CTA.

**Layout:**
- Full-height (`100vh`) section, vertical flex, padding: `1.5rem` mobile / `2.5rem` desktop
- **Top:** Nav bar — 4 items (was "About / Price / Projects / Contact", **Price removed**; current: `About`, `Projects`, `Contact`), justify-between, uppercase tracking-wider
- **Middle:** Huge animated headline `HI, I'M ADELE` — text size scales fluidly from `14vw` (mobile) → `17.5vw` (desktop), single-line, gradient fill, font: DM Serif Display 400, italic-ready
- **Center-overlay (absolute):** 3D-stylized portrait (PNG with transparency, `assets/adele-portrait-hq.png`), magnetic mouse-follow effect (3px / pixel of cursor offset within 150px radius). Width: 340 → 620px responsive.
- **Bottom-left:** Glowing tagline paragraph — `ai automations, landings, platforms & agents — i build whatever you need`. White text with multi-layer pink/magenta `text-shadow` neon glow. Max-width 260px.
- **Bottom-right:** "Contact Me" pill button (see Components > ContactButton)

**Components:**
- `<HeroSection />`
  - Nav (uppercase, tracking-wider, hover opacity 0.7, font-size: 0.875rem → 1.4rem responsive)
  - H1 with gradient text fill
  - Magnet wrapper around portrait
  - Tagline paragraph with `.subtitle-glow` class
  - `<ContactButton />`

**Animations:**
- Nav: fade in from y:-20, delay 0
- H1: fade in from y:40, delay 0.15s
- Portrait: fade in from y:30, delay 0.6s + magnetic mouse-follow on all subsequent moves
- Tagline + button: fade in from y:20, delay 0.35s / 0.5s
- All entry animations: duration 0.7s, easing `cubic-bezier(0.25, 0.1, 0.25, 1)`

---

### 2. Marquee (between Hero and About)
**Purpose:** Show project previews as scroll-driven dual-row image strip.

**Layout:**
- Background matches `--bg`
- Two rows of fixed-size tiles (each `420 × 270px`, `border-radius: 1rem`, `gap: 0.75rem`)
- Top row drifts **right** as user scrolls down (multiplier 0.3)
- Bottom row drifts **left** (inverse)
- Each row is `[...row, ...row, ...row]` (tripled) for seamless loop
- 21 placeholder GIFs from motionsites.ai (split 11/10 across rows) — **TO BE REPLACED** with Adele's project scroll-recordings (see "Pending Content" below)

**Scroll behavior:**
```js
const offset = (window.scrollY - sectionTop + window.innerHeight) * 0.3;
row1.style.transform = `translateX(${offset - 200}px)`;
row2.style.transform = `translateX(${-(offset - 200)}px)`;
```

---

### 3. About (full viewport)
**Purpose:** Personal intro with character-by-character scroll-revealed body text.

**Layout:**
- Min-height 100vh, centered content
- **Decorative 3D-rendered corner objects** (4 PNGs from Jack's original site — moon, lego, p59, group134) — slide in from sides on scroll. **TO BE REPLACED** with Mauve-themed icons.
- **Center:** Giant "ABOUT ME" heading (gradient, same style as Hero H1, font-size: `clamp(3rem, 12vw, 160px)`)
- **Below heading:** Body text rendered via `<AnimatedText>` — every character starts at `opacity: 0.2` and fades to `1.0` based on individual scroll progress (`useScroll` offset `start 0.8` → `end 0.2`).
- **Bottom:** Another `<ContactButton />` (CTA repeat)

**Current body text:**
> i build products end-to-end — design, code, deploy. from a one-pager that converts to a full saas with ai agents under the hood. solo studio, fast loops, no agency overhead. let's ship something that actually works.

---

### 4. Services (light section)
**Purpose:** List 5 service categories.

**Layout:**
- White background (`--service-bg: #FFFFFF` in current theme)
- Top corners rounded `40px → 60px` (creates "lifting" effect over hero)
- Heading "SERVICES" — centered, dark text, `clamp(3rem, 12vw, 160px)` size, font-family: display
- **5 rows**, each:
  - Border-top + border-bottom (only first has top border)
  - Padding `2rem → 3rem` vertical
  - Left: huge number `01`–`05`, font-display, size `clamp(3rem, 10vw, 140px)`
  - Right column: H3 uppercase name (size `clamp(1rem, 2.2vw, 2.1rem)`) + description paragraph (light weight, opacity 0.6, max-width 2xl)

**Service content (verbatim):**

| # | Name | Description |
|---|---|---|
| 01 | AI Agents | Autonomous agents on Claude / GPT: receive tasks by text or voice, navigate the web, messengers, databases, return results. Built for personal assistants, sales bots with tool-use, research agents. |
| 02 | Bots | Telegram and WhatsApp bots: booking, sales funnels, support, notifications. With integrations (CRM, calendars, payments) and a human-in-the-loop when needed. |
| 03 | Landings | High-conversion one-pagers: design, copy, development, deploy — in days, not weeks. Mobile-first, A/B-ready, analytics out of the box. |
| 04 | Platforms | Full SaaS and marketplaces: front + back + DB + admin. Stack: Next.js / TypeScript / Supabase. From MVP to production-grade with auth, roles, billing. |
| 05 | Automations | n8n flows and custom scripts that wire your tools together and remove manual work. From email to CRM, from CRM to spreadsheet, from spreadsheet to Telegram — no human touch required. |

---

### 5. Projects (sticky stacking cards)
**Purpose:** Showcase featured work as scaling sticky cards.

**Layout:**
- Background `--bg` (dark mauve)
- Top corners rounded `40px → 60px`, `-mt-10 → -mt-14` (overlap with Services white above)
- Heading "PROJECT" (singular as in original) — gradient, centered, same scale as ABOUT
- **3 project cards stacked vertically.** Each is `position: sticky` with `top` increasing by 28px per card index (creates layered fan effect at scroll-end).
- Each card uses `useScroll` + `useTransform` to scale from `1.0` → `1 - (totalCards - 1 - i) * 0.03` as it leaves the viewport, so earlier cards visibly shrink behind later ones.

**Card structure:**
- Outer: rounded `40-60px`, padded
- Inner: 2px border `--border`, dark bg, rounded same
- **Top row:** big number (left) + category label + project title (small caps, medium weight) + `<LiveProjectButton />` (right)
- **Image grid:** 40% column (2 stacked images) + 60% column (1 tall image)

**Current placeholder project data** (3 entries with AI-generated images from `images.higgs.ai`) — **TO BE REPLACED** with Adele's real projects.

---

## Pending Content (to be filled by Adele)

Adele provided this list of real projects (Russian → English summary below) — they should replace the placeholders in `PROJECTS` array and Marquee `MARQUEE_URLS`:

1. **QOSVANTA** — global marketplace for payment providers with an AI consultant "Damir" (built on Claude). Fintech / iGaming / High-risk. *Personal (founder).* URL: `qosvanta.vercel.app`
2. **Loom Lending Protocol** — Web3, non-custodial lending protocol with plain-English risk preview. *Personal (founder).* URL: `loom-protocol.vercel.app`
3. **Skyrise** — booking site for a paragliding service in Shymkent. Tandem flights over Tien Shan. *Client.* URL: `skyrise-flight.vercel.app`
4. **Heptra (Arman)** — AI agent landing. URL: `heptrai.vercel.app/en/agents/arman`
5. **Sigma World** — URL: `sigma-world-seven.vercel.app`

For Marquee + Project cards, each needs:
- 1× 4-6sec scroll-recording GIF/MP4 (420×270 ratio) for Marquee
- 3× still screenshots for Project card grid (40%/60% layout)

Tools suggested for recording: Kap (Mac), ShareX (Win), Loom, ezgif.com for MP4→GIF conversion.

---

## Components / Helpers (inside the prototype)

| Component | Purpose | Notes for porting |
|---|---|---|
| `FadeIn` | Entrance fade + translate. Animates on mount (not scroll-trigger). | Replace with intersection-observer based component in production (Framer Motion's `whileInView` or your lib's equivalent). The mount-animate version was used because of an iframe rendering quirk in this preview environment. |
| `Magnet` | Mouse-following element with elastic snap-back. | 100px (or `padding` prop) active radius, divides cursor delta by `strength` (3) for offset. |
| `ContactButton` | Pulsing neon-glow pill CTA. | See `.contact-btn` CSS — multi-layer box-shadow + 2.4s `contactGlow` keyframe animation + intensified `:hover`. |
| `LiveProjectButton` | Outline pill button on project cards. | Border 2px `--border`, text `--text`, hover bg `rgba(215,226,234,0.1)` (legacy color — should be updated to theme-aware). |
| `AnimatedText` | Per-character scroll-progress fade-in. | Uses `useScroll` + `useTransform` per char. Each char gets a `motion.span` with computed opacity. Expensive for very long strings — keep paragraphs ≤ 300 chars. |
| `ThemeManager` | Reads current theme key, writes CSS vars to `:root`. | Simple `useEffect` setting custom properties. |
| `useTweaks` | Hook that reads/persists tweaks via parent postMessage. Specific to the prototype host — **remove in production.** | Replace with regular `useState`. |
| `TweaksPanel` | Floating control panel for theme switcher. **Remove in production.** | |

---

## Interactions & Behavior

### Magnetic Portrait
On `mousemove` anywhere in viewport, compute distance from center of wrapper. If within `padding + halfDimensions`, translate inner div by `delta / strength`, with `transition: 0.3s ease-out`. When mouse exits, snap back with `transition: 0.6s ease-in-out`.

### Marquee Scroll
On `window scroll`, compute distance from section top + viewport height, multiply by 0.3, apply as `translateX` to row 1 (positive) and row 2 (negative).

### Sticky Project Cards
Each card has `position: sticky; top: 96px + index * 28px`. Cards arriving later in scroll cover earlier ones. Earlier cards' `scale` transforms down based on their `useScroll` progress (offset `start end` → `start start`) so they appear to recede.

### Per-character About text
`useScroll` on the paragraph with offset `['start 0.8', 'end 0.2']`. Each character's opacity is `useTransform(progress, [i/total, (i+1)/total], [0.2, 1])`.

### Contact Button
- Idle: pulses brightness/glow at 2.4s loop
- Hover: pulse pauses, brightness +15%, scale 1.02, glow intensified, transform `translateY(-2px)`

---

## State Management
The prototype has minimal state:
- `useTweaks(TWEAK_DEFAULTS)` — currently only holds `{ theme: 'mauveMagenta' }`. **Remove this hook in production** and either:
  - Hardcode the chosen theme, OR
  - Use `useState` + a localStorage-backed theme switcher if multi-theme stays a feature

In production, projects/services data should be:
- Hardcoded constants in a `data/` module (simplest), OR
- Fetched from CMS (Sanity, Contentful, Notion) if Adele wants to edit without redeploying

---

## Design Tokens (Mauve Magenta — current default)

### Colors
| Token | Hex | Use |
|---|---|---|
| `--bg` | `#0F0A14` | Page background, Hero, About, Projects |
| `--text` | `#F0D9E8` | Body text on dark bg |
| `--border` | `#F0D9E8` | Project card borders, LiveProjectButton |
| `--service-bg` | `#FFFFFF` | Services section bg |
| `--service-text` | `#0F0A14` | Services section text |
| `--service-border` | `rgba(15,10,20,0.18)` | Services row dividers |
| `--heading-from` | `#9B6A8F` | Gradient heading top |
| `--heading-to` | `#FFC9E3` | Gradient heading bottom |
| Contact glow | `#E0008C → #B228D9 → #FF4FB6` | Button gradient |
| Contact halo | `rgba(255, 79, 182, ...)` | Outer box-shadow glow |
| Subtitle glow | `#FFFFFF + #FFC8E6 + #FF4FB6 + #FF32AA + #B228D9` text-shadows | Hero tagline |

### Typography
| Token | Family | Weight | Usage |
|---|---|---|---|
| `--font-display` | DM Serif Display | 400 | Hero H1, section headings, service numbers |
| `--font-body` | Manrope | 300–500 | All paragraph and nav text |

### Spacing & Sizing
- Section padding: `5rem → 8rem` vertical, `1.25rem → 2.5rem` horizontal
- Border radius (large): `40px → 60px` on top corners of light/dark switches
- Border radius (cards/buttons): `1rem` for marquee tiles, `9999px` (pill) for buttons
- Gap units: `0.75rem`, `1.25rem`, `2.5rem`, `4rem`, `5rem`

### Animation
- Entry transitions: `0.7s cubic-bezier(0.25, 0.1, 0.25, 1)`
- Magnet active: `0.3s ease-out`
- Magnet release: `0.6s ease-in-out`
- Contact glow pulse: `2.4s ease-in-out infinite`
- Theme background transition: `0.4s ease`

---

## Assets

| File | Source | Notes |
|---|---|---|
| `assets/adele-portrait.png` | Generated in ChatGPT (DALL·E), background removed via remove.bg | 500×500, transparent PNG. Original master. |
| `assets/adele-portrait-hq.png` | Upscaled 2× + unsharp-masked from the original (canvas convolution) | 1000×1000. **Currently used in the prototype.** |
| Marquee GIFs | `motionsites.ai/assets/hero-*.gif` (21 files) | **Placeholders** — replace with Adele's project recordings |
| About corner decorations | `shrug-person-78902957.figma.site/.../*.png` (moon, lego, p59, group134) | **Borrowed from Jack's reference site** — should be replaced with theme-appropriate 3D objects (suggestion: re-generate similar shapes in Leonardo with magenta/violet lighting). |
| Project card images | `images.higgs.ai/...` AI-generated placeholders | **Placeholders** — replace with screenshots of QOSVANTA / Loom / Skyrise etc. |

**Action items for replacing assets:**
1. Record 4-6sec scroll-loops of each of Adele's 5 live sites → convert to optimized GIF or `<video autoplay loop muted playsinline>` for Marquee
2. Take 3 high-quality screenshots per project (different scroll positions) for Projects card grids
3. (Optional) Regenerate the 4 About corner decorations in Leonardo with prompts matched to Mauve palette
4. (Optional) Animate the portrait via Hedra.com (upload PNG → idle motion → MP4) and swap `<img>` → `<video>`

---

## Files in This Bundle
- `index.html` — Single-file prototype with all components inlined inside one `<script type="text/babel">` block
- `assets/adele-portrait.png` — Original portrait (500px)
- `assets/adele-portrait-hq.png` — Upscaled portrait currently rendered (1000px)

The HTML is intentionally a single file for portability. When porting:
- Split the inline `<script>` into separate component files (`Hero.tsx`, `Marquee.tsx`, `About.tsx`, `Services.tsx`, `Projects.tsx`, `components/FadeIn.tsx`, etc.)
- Move theme tokens to a Tailwind config or a CSS file
- Remove the `useTweaks` / `TweaksPanel` infrastructure (it's prototype scaffolding)
- Replace Babel-in-browser with a real Vite/Next.js build
- Swap Tailwind CDN for a proper Tailwind install with PostCSS

---

## Recommended Port (Next.js 14 + App Router)

```
app/
  layout.tsx               # Imports fonts, sets <html className> w/ theme vars
  page.tsx                 # Composes <Hero/> <Marquee/> <About/> <Services/> <Projects/>
  globals.css              # Theme custom properties + Tailwind directives
components/
  Hero.tsx
  Marquee.tsx
  About.tsx
  Services.tsx
  Projects.tsx
  ContactButton.tsx
  LiveProjectButton.tsx
  Magnet.tsx
  FadeIn.tsx
  AnimatedText.tsx
data/
  projects.ts              # PROJECTS array
  services.ts              # SERVICES array
  marquee.ts               # Asset paths for marquee
public/
  portrait.png             # Use the -hq version
  marquee/01.mp4 ... etc.  # Recorded scroll loops
  projects/qosvanta-1.jpg ... etc.
```

Install: `next@14 react@18 framer-motion@11 tailwindcss`

Use `next/image` for all static images, `next/font/google` for fonts (Manrope, DM Serif Display).

For Marquee video tiles, replace `<img src=".gif">` with `<video autoPlay loop muted playsInline>` — much smaller files and smoother playback.
