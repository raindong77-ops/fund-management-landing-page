# Bridgify Landing Page — Project Context

## Project Overview

**Bridgify** is a litigation finance risk management platform. This project is a single-page marketing landing page targeting fund managers (General Partners), Limited Partners, and law firms in the litigation finance space.

**Goal:** Communicate institutional authority and product confidence. Convert visitors to book a demo. The design must feel like a high-end physical portfolio — quiet, expensive, unfailingly accurate.

**Primary file:** `index.html` (single self-contained file, no build step)

---

## Tech Stack

| Layer | Choice |
|-------|--------|
| HTML | Semantic HTML5, single file |
| CSS | Tailwind CSS via CDN (`https://cdn.tailwindcss.com`) |
| Fonts | Google Fonts — Manrope (headlines), Inter (body) |
| Icons | Google Material Symbols Outlined |
| JS | Vanilla JavaScript (no frameworks) |
| Images | None — all visuals are inline SVG or CSS |

Tailwind is configured inline via `tailwind.config` in a `<script>` tag. All custom color tokens are defined there.

---

## Design System

### Color Tokens

| Token | Hex | Use |
|-------|-----|-----|
| `surface` | `#f9faf5` | Primary canvas / page background |
| `surface-container-low` | `#f3f4f0` | Secondary zones, utility panels |
| `surface-container` | `#edeeea` | Mid-level containers |
| `surface-container-high` | `#e8e8e4` | Hover states, active indicators |
| `surface-container-highest` | `#e2e3df` | Input field backgrounds |
| `surface-container-lowest` | `#ffffff` | Critical data cards — makes them "pop" |
| `on-surface` | `#1a1c1a` | Primary text |
| `on-surface-variant` | `#44474c` | Secondary/supporting text |
| `primary` | `#000000` | Brand black, primary actions |
| `on-primary` | `#ffffff` | Text on black backgrounds |
| `primary-container` | `#101b30` | Deep navy — gradient endpoint for CTAs |
| `outline` | `#74777d` | Muted labels, metadata |
| `outline-variant` | `#c4c6cc` | Subtle structural hints (ghost borders only) |
| `on-tertiary-container` | `#539072` | **Success / Low-risk only** — green checkmarks |
| `error` | `#ba1a1a` | Error states, "not available" indicators |

### Typography

- **Headlines:** `font-family: Manrope` — class `font-headline`. Use for all `h1`–`h4`, section labels, and display numbers.
- **Body/UI:** `font-family: Inter` — default body font. Use for paragraphs, table cells, button labels, metadata.
- **Headline sizing:** `display-lg` equivalent = `text-5xl` / `text-7xl` with `tracking-tighter` and `leading-[1.05]`.
- **Section labels:** Small uppercase pill tags, e.g. `text-[10px] font-bold tracking-[0.15em] uppercase`.

### Critical Design Rules

**The "No-Line" Rule — most important:**
Do NOT use `border` classes to divide sections or create visual separation. Structural zones must be defined solely through background color shifts (e.g., `surface` → `surface-container-low`). The only permitted border use is the `outline-variant` ghost border at ≤15% opacity (`border-outline-variant/15`) and only when absolutely required for accessibility.

**Sharp Corners:**
Use `rounded-sm` (0.125rem) for buttons and cards. Never use `rounded-full` or pill shapes — they're too casual for this brand.

**Custom Shadows:**
Tailwind's default shadows are prohibited. Use only:
```css
box-shadow: 0 20px 40px rgba(26, 28, 26, 0.06);
```
This tints the shadow with `on-surface` (#1a1c1a) rather than pure black.

**CTA Gradient:**
Primary "Book a Demo" buttons should use a subtle gradient, not flat black:
```css
background: linear-gradient(135deg, #000000 0%, #101b30 100%);
```

**Glassmorphism (modals/popovers):**
```css
background: rgba(249, 250, 245, 0.85);
backdrop-filter: blur(20px);
-webkit-backdrop-filter: blur(20px);
```

**Green accent rule:**
`on-tertiary-container` (#539072) is used ONLY for success states — checkmarks, "low risk" indicators. Never use it for branding or decoration.

**Input fields:**
Flat `bg-surface-container-highest` background, no border in default state. On focus: `border-b-2 border-primary` only (bottom border, not full outline).

---

## Page Structure

All 9 sections in order. Do not reorder or restructure without explicit instruction.

| # | Section | Background | Key Behavior |
|---|---------|-----------|--------------|
| 1 | **Nav** | `surface` | Sticky top, desktop links + mobile hamburger toggle |
| 2 | **Hero** | `surface` | Large editorial headline, asymmetric right panel (`surface-container-low`), two CTAs |
| 3 | **Market** | `surface-container-low` | Stat cards with left accent borders + CSS bar chart |
| 4 | **Problem Accordion** | `surface` | 3-panel accordion (GPs / LPs / Law Firms), auto-cycles every 4s with progress bar |
| 5 | **Platform Overview** | `surface-container-low` | 3 white cards: Sourcing / Underwriting / Operations |
| 6 | **Feature Rows** | `surface-container-low` | 3 alternating accordion+SVG rows (Stage 1/2/3), each auto-cycles every 7s |
| 7 | **Impact Stats** | `surface` | 3 metric cards: 350K+ / $1.8B+ / $4.5B+ |
| 8 | **Comparison Table** | `surface-container-low/50` | Bridgify vs Other Platforms, green checks / red X's |
| 9 | **Final CTA** | `primary` (black) | Full-width black section, white "Book a Demo" button |
| — | **Footer** | `surface-container-low` | Logo, copyright, Privacy / Terms / Disclosure links |

---

## JavaScript Behaviors

### Problem Accordion (`#problem-accordion`)
- `setProblemPanel(idx)` — opens one panel, collapses others
- Active: expands content via `maxHeight = scrollHeight`, rotates icon 45°, starts 4s progress bar animation
- Auto-advances to next panel after 4s; loops
- Manual click resets timer

### Stage Accordions (`#s1-accordion`, `#s2-accordion`, `#s3-accordion`)
- `setStageItem(stage, idx)` — opens one item per stage
- Active: expands content, adds background highlight, starts 7s progress bar
- Auto-advances every 7s per stage independently
- Manual click resets that stage's timer

### Scroll Fade-In
- All elements with class `.fade-in` start invisible (`opacity: 0`, `translateY(20px)`)
- `IntersectionObserver` adds `.visible` when element enters viewport (threshold: 10%)
- Transition: 0.6s ease

### Mobile Menu
- Hamburger button toggles `#mobile-menu` open/closed
- Icon swaps between `menu` and `close`
- Auto-closes when any nav link is clicked

---

## Content & Tone

- **Voice:** Institutional, authoritative, precise. Think financial reporting, not startup pitch.
- **Avoid:** Exclamation marks, emoji, buzzwords like "revolutionary" / "game-changing" / "disruptive"
- **Lead with:** Numbers, outcomes, specificity ("$16B → $50B+" not "massive market opportunity")
- **Personas addressed:** General Partners (GPs), Limited Partners (LPs), Law Firms
- **Core message:** Bridgify controls risk across the full investment lifecycle — sourcing, underwriting, operations

---

## What Not to Change Without Explicit Instruction

- Section order
- Color token values in the Tailwind config
- Font families
- The "No-Line" design rule
- Auto-rotation timing (4s problem accordion, 7s stage accordions)
