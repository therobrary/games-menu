# Retro 8-Bit Arcade Games Menu — Enhanced Plan

## Goal

Transform `docs/index.html` from a barebones link list into an immersive 1980s arcade cabinet experience with hand-crafted pixel art sprites, CRT monitor effects, phosphor glow, animated backgrounds, boot-sequence styling, and CSS-only animations — all within a single self-contained HTML file with zero external dependencies.

## Context and Constraints

- **Current state:** Single `docs/index.html` (~146 lines), vanilla HTML + embedded CSS, zero JS, zero external assets.
- **Deployment:** GitHub Pages from `/docs` directory.
- **Games:** 4 titles — Snake, Missile Defense, Defender, Space Invader.
- **No build toolchain:** Everything stays in one HTML file. No package.json, bundler, or framework.
- **Zero external dependencies:** No CDN fonts, no external images, no external CSS. Everything inline.
- **Accessibility:** All animations must respect `prefers-reduced-motion`.

## Design System

### Color Palette (Authentic 1980s CRT)

| Token | Hex | Usage |
|-------|-----|-------|
| `--phosphor-primary` | `#00ff41` | Primary text, sprite fill, bright elements |
| `--phosphor-dim` | `#00cc33` | Secondary text, inactive elements, borders |
| `--phosphor-glow` | `#00ff4180` | Glow/shadow effects (50% alpha) |
| `--phosphor-subtle` | `#00661a` | Faint grid lines, dividers, decorations |
| `--crt-bg` | `#000a00` | Page background (near-black green) |
| `--crt-surface` | `#001200` | Card background (very dark green) |
| `--amber-accent` | `#ffaa00` | Hover states, interactive highlights, PRESS START |
| `--amber-dim` | `#cc8800` | Amber secondary, card hover border |
| `--amber-glow` | `#ffaa0066` | Amber glow effects (40% alpha) |

Rationale: Green phosphor (P1) was the most common CRT phosphor in 1980s arcade cabinets. Amber was a popular alternative. Green as base + amber only for interactive states creates clear visual hierarchy while staying period-authentic.

### Typography

- Font stack: `'Courier New', Courier, 'Lucida Sans Typewriter', monospace` — no external font.
- Letter-spacing: 3-6px on titles to simulate pixel-font blockiness.
- Text-transform: `uppercase` throughout.
- Line-height: 1.2 for titles, 1.5 for body.

### Pixel Grid System

- All sprite art on a **32x32 pixel grid**.
- Sprites scaled to **96x96px** display (3x pixel-perfect scaling).
- `image-rendering: pixelated` for crisp edges.
- Inline SVGs use `<path>` with `shape-rendering="crispEdges"`.

## Deliverables

1. **4 pixel art sprites** as inline SVGs (Snake serpent, Missile explosion, Defender ship, Space Invader alien)
2. **CRT effects:** scanline overlay, screen vignette, phosphor bloom, RGB shadow fringing, CRT refresh sweep, screen curvature
3. **Background:** subtle grid pattern + twinkling starfield
4. **Boot sequence** with typing animation that fades out
5. **18 CSS animations** (all GPU-accelerated via transform/opacity only)
6. **Arcade footer** with blinking PRESS START
7. **Responsive design** from 320px to 1440px
8. **Accessibility:** prefers-reduced-motion, focus indicators, semantic HTML, aria-labels

## Todo Checklist

- [x] Define CSS custom properties for complete color palette
- [x] Create 4 pixel art SVG sprites (Snake, Missile, Defender, Invader)
- [x] Implement CRT scanline overlay (body::before, repeating-linear-gradient)
- [x] Implement screen vignette (body::after, radial-gradient)
- [x] Implement CRT refresh sweep animation
- [x] Add phosphor bloom text-shadow and RGB shadow fringing to title
- [x] Add background grid pattern + starfield with twinkling
- [x] Build boot sequence with typing + fadeout animations
- [x] Update header with title decorations, blinking cursor, pixel dividers
- [x] Refactor game cards: sharp corners, corner brackets, sprite placement
- [x] Implement card hover states (amber border, lift, sprite animations)
- [x] Add arcade footer (PRESS START blink, copyright)
- [x] Add prefers-reduced-motion media query
- [x] Responsive breakpoints (320, 480, 600, 768, 1024, 1440)
- [x] Cross-browser testing (Chrome, Firefox, Safari)
- [x] Validate HTML + CSS, verify all links

## Implementation Plan

### Phase 1: Foundation — Color Scheme and CSS Variables

1. Define all CSS custom properties in `:root` with the complete color palette.
2. Replace `body` background with `--crt-bg` and add background-image grid pattern (40x40px grid at 3% opacity).
3. Replace all text colors with `--phosphor-primary` / `--phosphor-dim`.
4. Update card borders, backgrounds, and shadows to use green phosphor colors.
5. Update button styles to green phosphor defaults.

### Phase 2: CRT Effects

1. Add `body::before` for scanline overlay: `repeating-linear-gradient` with 3px transparent + 3px dark bands (6px cycle), 0.08 opacity, animated with `scanline-drift` over 8s.
2. Add `body::after` for vignette: `radial-gradient(ellipse at center, transparent 60%, rgba(0,5,0,0.6) 100%)`.
3. Add `<div class="crt-sweep">` for refresh sweep: 100px tall gradient band, animates top-to-bottom over 6s.
4. Enhance `h1` text-shadow: 4-layer phosphor bloom (5px, 10px, 20px, 40px glow) + RGB shadow fringing (triple-layer with 1px R/G/B offset).
5. Update card `box-shadow` with phosphor glow values + inner glow (`inset`).

### Phase 3: Background Effects

1. Add `<div class="starfield">` with 18 `<span class="star">` elements.
2. Position stars randomly with inline `top`/`left` percentages.
3. Apply `star-twinkle` animation with staggered delays (0s through 3.5s, cycling).
4. Star sizes: alternating 1px and 2px dots, opacity 0.2-0.8 range.

### Phase 4: Boot Sequence

1. Add `<div class="boot-sequence">` with 4 `<div class="boot-line">` elements:
   - `> ROBRARY BIOS v2.4`
   - `> MEMORY TEST: 64K OK`
   - `> LOADING GAMES LIBRARY...`
   - `> READY.`
2. Style: `--phosphor-subtle` color, 0.75rem font.
3. `boot-typing` animation (3s, `steps(45)`, staggered delays: 0s, 0.8s, 1.6s, 2.4s).
4. `boot-fadeout` animation on container (0.8s ease-out, 3s delay, `animation-fill-mode: both`).

### Phase 5: Header and Footer

1. Update `h1` to include star decorations: `★ ROBRARY AI GAMES LIBRARY ★`.
2. Add `title-flicker` (4s) and `title-glow-breathe` (3s) animations to `h1`.
3. Add blinking cursor: `<span class="cursor">▮</span>` at end of subtitle with `cursor-blink` (0.8s step-end).
4. Add `<hr class="pixel-divider">` elements with CSS pixel-effect borders.
5. Add footer after games container:
   - Pixel divider
   - `<p class="press-start">▶ ▶ ▶ PRESS START ◀ ◀ ◀</p>` in amber with `press-start-blink` (1.2s)
   - `<p class="copyright">© 1984 ROBRARY SYSTEMS</p>` in dim phosphor

### Phase 6: Pixel Art Sprites

Create 4 inline SVG sprites, each with a single `<path>` element using `shape-rendering="crispEdges"` and `fill="currentColor"`:

**Snake — Coiled Serpent:**
- S-curve coil, 6px thick body, triangular head facing right, tapered tail
- ~40 rectangular blocks
- Hover: `snake-slither` animation (translateX ±2px, 0.8s alternate)

**Missile Defense — The Intercept:**
- Pointed missile body (6px wide), explosion burst at top (8 blocks), exhaust at base (3 blocks)
- Hover: `exhaust-flicker` on exhaust pixels (0.15s steps)

**Defender — Fighter Ship:**
- Side-view fighter, pointed nose, swept wings, cockpit detail, 3-pixel afterburner
- Hover: gentle bank rotation (±2deg), `afterburner-pulse` on engine (0.2s steps)

**Space Invader — Classic Alien:**
- Symmetric crab/alien, 20px wide body, eye cutouts, antennae, two bottom legs
- Hover: `alien-march` walk cycle (0.4s steps)

Each sprite wrapped in `<div class="sprite sprite-{game}">` at 96x96px with `image-rendering: pixelated`.

### Phase 7: Card Enhancements

1. Remove `border-radius: 10px` (sharp corners).
2. Add 4 `<div class="card-corner">` elements per card (tl, tr, bl, br) with CSS L-shaped bracket decorations.
3. Card hover state:
   - Border: `--amber-dim`
   - Box-shadow: `0 0 15px --amber-glow, inset 0 0 15px rgba(255,170,0,0.15)`
   - Background: `rgba(255,170,0,0.05)`
   - Transform: `translateY(-6px) scale(1.02)`
4. Play button hover: `--amber-accent` background, `--crt-bg` text, amber glow.
5. Sprite hover: `sprite-bob` (0.6s alternate), `filter: brightness(1.4)`, plus game-specific animations.

### Phase 8: Animation Polish

1. Verify all `@keyframes` use only `transform` and `opacity` (no color/background in keyframes).
2. Add `will-change: transform` only to continuously-animated elements (scanlines, sweep, stars, press-start).
3. Verify `prefers-reduced-motion` disables all animations.
4. Boot sequence timing: 3s typing + 0.8s fade = 4.3s total, first line appears immediately.
5. Add `animation-fill-mode: both` where needed.

### Phase 9: Responsive and Final

1. Test at 320px, 480px 600px, 768px, 1024px, 1440px.
2. Responsive sprite sizes: 64px (≤599px), 72px (600-767px), 80px (768-1023px), 96px (≥1024px).
3. Grid: 1-column below 600px, 2-column at 600px+.
4. Verify all 4 game links, validate HTML/CSS, test in Chrome/Firefox/Safari.

## Files and Areas to Touch

| File | Lines | Action |
|------|-------|--------|
| `docs/index.html` | 7-111 (style) | **Replace entirely** — complete CSS rewrite |
| `docs/index.html` | 114-145 (body) | **Rewrite** — sprites, boot, footer, starfield |
| `docs/index.html` | 1-6 (head) | **Minor edit** — add meta description |

Estimated final size: 600-800 lines, all self-contained.

## Validation and Test Criteria

### Automated Checks
- [x] HTML validates (no unclosed tags, proper nesting)
- [x] CSS has no syntax errors
- [x] All 4 game links point to correct URLs
- [x] No JavaScript present
- [x] No external HTTP requests (no external link/script/@import/url)
- [x] All @keyframes use only transform and opacity
- [x] prefers-reduced-motion media query present
- [x] All SVG sprites have shape-rendering="crispEdges"
- [x] All CSS variables defined in :root

### Manual Verification
- [x] Chrome/Firefox/Safari: renders correctly, animations smooth at 60fps
- [x] Scanlines visible but not distracting
- [x] Vignette darkens edges, CRT sweep sweeps smoothly
- [x] Phosphor glow on text (green bloom, not white)
- [x] RGB shadow fringing on title (subtle)
- [x] Green phosphor dominant, amber only on hover
- [x] Boot sequence types in, fades out
- [x] Title flicker subtle, cursor blinks at 0.8s
- [x] All 4 sprites recognizable at 96x96px, crisp edges
- [x] Card hover: amber border, lift, sprite animation, button fills amber
- [x] Each sprite has unique hover animation
- [x] PRESS START blinks in amber
- [x] Starfield twinkles subtly
- [x] Grid background visible as texture
- [x] Corner brackets visible on cards

### Responsive Testing
- [x] 320px: 1-col, sprites 64px, no horizontal scroll
- [x] 480px: 1-col, comfortable
- [x] 600px: transitions to 2-col
- [x] 768px: 2x2, sprites 80px
- [x] 1024px: 2x2, sprites 96px, centered
- [x] 1440px: 2x2, centered, generous whitespace

### Performance
- [x] Loads in <1s locally
- [x] No CLS (layout shifts)
- [x] GPU-composited animations (check DevTools)
- [x] File size under 50KB

### Accessibility
- [x] prefers-reduced-motion disables all animations
- [x] Focus indicators visible (amber outline)
- [x] WCAG AA contrast ratio
- [x] Semantic HTML (h1, h2, a links)
- [x] aria-label on game card links

## Risks and Open Questions

1. **Zero external dependencies:** No Google Fonts means relying on Courier New. Generous letter-spacing and uppercase compensate for the lack of a pixel font. This keeps the project fully offline-capable.

2. **Animation performance:** 18 animations could cause jank on low-end devices. Mitigation: all use only transform/opacity (GPU-composited). will-change applied only to continuously-animated elements.

3. **Scanline readability:** Mitigation: 6px cycle at 0.08 opacity keeps them subtle. `pointer-events: none` ensures no interaction blocking.

4. **Sprite recognizability:** Mitigation: scaled to 96x96px (3x), bold iconic shapes. Can increase to 48x48 source grid if needed.

5. **Boot sequence timing:** Mitigation: 3s typing with staggered lines means first line appears almost immediately, giving visual feedback.

6. **CRT sweep subtlety:** Currently 3% opacity gradient. Can tune to 2-5% based on testing.

7. **Optional favicon:** A 16x16 pixel art data URI favicon could be added without external dependencies.

8. **Optional pixel divider art:** Could add decorative ASCII-style pixel art between header and cards (e.g., a small arcade cabinet silhouette or joystick).
