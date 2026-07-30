# TECHFEST — Signal

A single-file, 3D interactive landing page for a fictional college tech festival. Built around one idea: the site is a "transmission" you scroll through — a Three.js aurora core and particle field live behind the content, with camera movement, color, and section reveals all tied to scroll position.

**File:** `index.html` — everything (HTML, CSS, JS) lives in this one file. No build step, no install. Just open it in a browser.

---

## What's in it

- **3D aurora core** — a morphing icosahedron (organic vertex displacement, no external noise library) with a wireframe shell, layered glow, and physical material. Rotates toward your cursor. **Click it** for a color-burst + shockwave ring.
- **Particle field** — thousands of aurora-colored points the camera flies through as you scroll.
- **Scroll-driven camera** — GSAP ScrollTrigger moves the camera and rotates the scene as scroll progress changes; no separate "3D section," the whole page sits inside one continuous scene.
- **Signal HUD** — a small progress ring (bottom-right) that fills as you scroll and labels the current section as a "waypoint."
- **Tilting event cards** — 3D perspective tilt on hover, click to open a modal with full details, one per track.
- **Animated timeline** — the vertical line draws itself in sync with scroll position.
- **Live countdown** — to a hardcoded festival date.
- **Custom cursor** — dot + trailing ring, expands and labels itself over clickable elements (disabled automatically on touch devices).
- **Scroll reveals + animated stat counters**, a mobile nav drawer, back-to-top button, and full keyboard focus states.
- **Respects `prefers-reduced-motion`** — heavy motion (vertex morphing, blob drift, auto-rotation) is switched off; reveals still happen, just without the extra motion.

## Stack

All loaded via CDN (`cdnjs.cloudflare.com`), no npm install required:

| Library | Version | Used for |
|---|---|---|
| [Three.js](https://threejs.org/) | r128 | The aurora core + particle scene |
| [GSAP](https://gsap.com/) | 3.12.5 | All animation tweening |
| GSAP **ScrollTrigger** | 3.12.5 | Scroll-linked animation & reveals |
| GSAP **ScrollToPlugin** | 3.12.5 | Smooth in-page anchor scrolling |
| Google Fonts | — | Space Grotesk (display), Inter (body), JetBrains Mono (data/labels) |

Requires an internet connection on first load (for the CDN scripts and fonts). If you need a fully offline version, download the four scripts and the font files and point the `<link>`/`<script>` tags at local copies.

## Running it

Just double-click `index.html`, or serve it locally if you prefer:

```bash
python3 -m http.server 8000
# then visit http://localhost:8000
```

## Customizing

Everything is in one file, organized top to bottom as: `<style>` block → HTML body → `<script>` block.

**Colors & type** — edit the CSS custom properties at the top of the `<style>` block:
```css
:root{
  --violet:#6c3fff;
  --cyan:#35e7c7;
  --magenta:#ff3fa4;
  --amber:#ffb84d;
  --font-display:'Space Grotesk', sans-serif;
  ...
}
```

**Event/track content** — the modal copy lives in one object in the `<script>` block, keyed by the `data-key` on each `.freq-card`:
```js
const cardData = {
  hack: { eyebrow, title, desc, duration, format, prize },
  robotics: { ... },
  ...
};
```
Match a card's `data-key="hack"` in the HTML to a key here to change what its modal shows.

**Festival date / countdown** — one line, near the countdown logic:
```js
const FEST_DATE = new Date("2026-10-16T09:00:00+05:30").getTime();
```

**Speakers, timeline entries, gallery tiles** — plain HTML in their respective `<section>` blocks (`#speakers`, `#timeline`, `#gallery`) — copy/paste a block and edit the text.

**Particle count** — inside `buildParticles()`, tuned down automatically under 720px width:
```js
const count = window.innerWidth < 720 ? 1100 : 2400;
```

## Notes

- All names, speakers, dates, and prize amounts are placeholder content — swap in your real festival's details before shipping.
- The gallery uses gradient placeholder tiles rather than real photos; drop in `background-image: url(...)` on `.archive-item` once you have actual event photos.
- Tested against modern evergreen browsers (Chrome, Firefox, Safari, Edge). `backdrop-filter` (used for the glass panels) needs a reasonably recent browser — it degrades gracefully to a solid panel on ones that don't support it.
