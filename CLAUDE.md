# Altara — Project Notes

Multi-page luxury travel website for **Altara**, a private-journey travel brand
built around the Kingdom of Bhutan. Plain HTML/CSS/JS — no build step, no
framework, no external dependencies besides Google Fonts.

## Structure

```
Altara/
├── index.html                     # Homepage markup only
├── about.html                     # About page markup only (story, philosophy,
│                                     values, team, dark CTA)
├── cultural-tours.html            # Cultural Tours page — 6-card itinerary grid
├── style.css                      # ALL CSS for the whole site (every page)
├── script.js                      # ALL JS for the whole site (every page)
├── altara-brand-guidelines.html   # Standalone brand reference doc (not linked
│                                     from the nav — colors match the live site,
│                                     but documents an OLDER font pairing, see
│                                     "Known inconsistency" below)
└── assets/
    └── images/
        ├── logo.png               # Dark logo, for light backgrounds
        ├── logo white.png         # White logo, for dark/overlay backgrounds
        └── *.jpg                  # ~103 Bhutan travel photos, flat, no subfolders
```

`style.css` and `script.js` are shared across every page — there is no
per-page CSS/JS and no build tool, no bundler, no package.json. Every page's
`<head>` links `style.css`; every page ends with `<script src="script.js">`
before `</body>`. JS that targets an element only present on one page
(e.g. the Explore Bhutan map, the journeys slider) guards itself with an
early `if(!el) return;` inside its own IIFE so it's harmless to include
`script.js` on a page that doesn't have that element.

## Pages

### index.html (homepage), in page order

1. **Header** — fixed, transparent over hero; on scroll >60px, background
   flips to white and the logo swaps from `logo white.png` → `logo.png`
   (handled in JS, not CSS, since it's a src swap). "Experiences" has a
   hover/focus dropdown (Cultural / Trekking / Festivities / Activities /
   Luxury Tours). A `.lang-btn` placeholder sits in the header (no i18n
   wired up yet).
2. **Hero** — full-bleed video (`taktshang.jpg` poster), centered
   headline/CTA. `.hero` is reused on inner pages with two modifiers:
   `.hero--about`/`.hero--cultural` etc. set the background image, and
   `.hero--page` turns the same full-`100vh` hero into a bottom-left page
   title (left-aligned `.hero-content`, no CTAs) instead of a centered one.
3. **Explore Trips slider** — dark full-bleed horizontal card slider
   (scroll-snap + JS arrow controls), sits directly under the hero.
4. **About (teaser)** — two-column stacked-photo + copy block (`id="story"`),
   "Read More" links to `about.html`.
5. **Activities slider** (`id="activities"`) — horizontal scroll-snap card
   slider (Meditation Retreats / Spiritual Retreats / Archery / White-Water
   Rafting / Fly Fishing / Mountain Biking / Bird Watching), built on the
   same `initCardSlider` mechanics as the Explore Trips slider, plus its
   own autoplay IIFE ("Activities slider" in `script.js`) that advances one
   card every ~4.2s and pauses on hover/touch. `.activity-card` is
   `flex:0 0 100%` (one full-width card per view, not a multi-card strip)
   and both `#activityPrev`/`#activityNext` are square (`border-radius:0`)
   — don't reintroduce the multi-card width or round the arrows back off
   without being asked. Each card's `.activity-info p` description is
   hidden by default and only reveals on `:hover` or on whichever card the
   autoplay/scroll currently has centered (`.is-active`, toggled by that
   same IIFE) — there's no separate per-card CTA button. Replaced the old
   five-tile "Find Your Journey" category grid — don't recreate
   `.category-tile`/`.category-row` from memory.
6. **Testimonials** — static 4-column quote grid
7. **Explore Bhutan map** — interactive SVG map of Bhutan's 20 dzongkhags.
   Scroll-pinned on desktop (`.bhutan-scroll-pin`, `position:sticky`) so the
   map holds in view while an animated sequence plays: an international
   flight arrives at Paro from outside the map outline, then three domestic
   flights depart Paro for Bumthang/Sarpang(Gelephu)/Trashigang(Yonphula),
   then three land border-gate markers fade in (Phuentsholing/Gelephu/
   Samdrup Jongkhar). On mobile (<901px) the same sequence just plays once,
   timed, as the section scrolls into view (no pin — too fragile on touch).
   Hover a district for a redesigned info-bar tooltip. Sidebar stat cards
   (`.bhutan-map-facts`) sit beside the map.
8. **Why Altara** — image/copy split with bullet list
9. **Blog** — teaser cards
10. **FAQ** — accordion
11. **Dark CTA** — near-black band, `id="enquire"`
12. **Footer** — 4-column dark footer

(The old **Editorial** and **Press strip** sections have been removed
entirely — don't recreate them from memory.)

### about.html

Header/footer are the same markup as `index.html`, but every in-page anchor
that only exists on the homepage (`#journeys`, `#why`, `#blog`) is written as
`index.html#journeys` etc. The logo links to `index.html`. `#enquire` stays a
same-page anchor since About has its own dark-CTA section.

Sections: full-`100vh` image hero (`.hero.hero--about.hero--page`) → Our
Story (`.about`) → Our Philosophy (`.about.about--reverse`, mirrors the
layout) → Values (`.values-section`, 3 cards) → Team (`.team-section`,
4 cards with gold-initial avatars — there are no real team photos in
`assets/images/`, so avatars are deliberately initials-only; swap in real
photos via the same `.team-avatar` markup if/when they exist) → Dark CTA →
Footer.

### cultural-tours.html

Same header/footer pattern as `about.html`. `.hero.hero--cultural.hero--page`
full-viewport title hero (eyebrow → h1 → description, same 3-part pattern as
every `.hero--page` hero — see below), then a 6-card `.tour-grid`. `.tour-card`
is now the same full-bleed editorial image-card family as `.journey-card` in
the Explore Trips slider — the whole card is an `<a>`, with a `.bg` div for
the hover zoom, a `.tour-tag` category pill (top-left), a `.tour-index`
numbered circle (top-right), and a `.tour-card-body` overlaid at the bottom
(title, `.tour-meta` icon+text stats, a `.tour-divider` rule, then a
description that's hidden until hover, same interaction as `.activity-info
p`). The CTA reuses `.journey-btn` directly rather than a separate
`.tour-cta` class — don't recreate the old white-card-with-icons version
from memory. Its Dark CTA is `.dark-cta.dark-cta--photo` (a background
image + black overlay, via the `.dark-cta--photo` modifier — homepage and
About keep the plain dark-green/gold-glow `.dark-cta`, unmodified) with
`.contact-pill` WhatsApp/email links instead of the usual `.btn` pair. The
header's
"Experiences" dropdown "Cultural Tours" item and the footer's "Cultural
Travelers" link both point here from every page — keep those in sync if
this page is ever renamed or removed. This is the template to copy for the
other dropdown items (Trekking Tours, Festivities, Activities, Luxury
Tours) once those pages exist — they currently all still point at
`index.html#journeys` as a placeholder.

### `.hero--page` content pattern

Every inner-page hero (`about.html`, `cultural-tours.html`) follows the same
3-part structure inside `.hero-content`, top to bottom: a small
`.eyebrow.eyebrow--on-dark` category label, the `<h1>` page title, then a
`.hero-sub` description — no CTA buttons (those belong on the homepage hero
only). Follow this order for any new `.hero--page` hero rather than
inventing a different hierarchy.

### Nav dropdown — don't reintroduce the hover gap

`.nav-dropdown` (the invisible bridge, `top:100%`, no margin) and
`.nav-dropdown-panel` (the visible white box, nested one level in) are two
separate elements on purpose. A CSS dropdown that opens on `:hover` breaks
the moment there's a real gap (a `margin-top`, etc.) between the trigger and
the panel — the cursor crosses dead space and `:hover` is lost before it
reaches the menu. Any visual spacing has to live as `padding-top` on
`.nav-dropdown` itself (which stays part of the hoverable box), never as a
margin between it and `.nav-item`.

The header's language switcher (`.lang-item`) reuses this exact same
`.nav-item`/`.nav-dropdown`/`.nav-dropdown-panel` pattern, just with
`.nav-dropdown--right` (`left:auto;right:0`) since it's the last item in
the header rather than the first — a plain `.nav-dropdown` would overflow
off the right edge of the viewport. It has no real i18n behind it: clicking
a `.lang-option` just swaps the `#langCode` text and remembers the choice
in `localStorage` (see the "Language dropdown" IIFE in `script.js`) — there
is no translated content anywhere on the site yet.

## Interactions (all in `script.js`, vanilla JS, one IIFE per feature)

- Header scroll-state + logo swap (throttled via `requestAnimationFrame`)
- `initCardSlider(trackId, prevId, nextId, cardClass)` — generic scroll-snap
  + prev/next-button slider, called once for the Journeys slider and once
  for the Activities slider. Add any future horizontal card slider as a
  third call to this same function rather than writing a new one.
- Explore Bhutan map: district hover tooltip, scroll-pinned/timed flight +
  border-gate animation (see above)
- `IntersectionObserver` scroll-reveal on `.reveal` elements (skipped under
  `prefers-reduced-motion`)
- FAQ accordion
- Language dropdown (visual-only, see above)

## Known inconsistency

`altara-brand-guidelines.html` documents **Cormorant Garamond + Cabin +
Playfair Display** as the type system. The live site actually uses
**League Gothic + Josefin Sans + Playfair Display** (a later font pivot made
mid-project). Colors match exactly between the two files — only the font
pairing has drifted. Treat `style.css` as the source of truth; the
guidelines doc has not been updated to match.

## Conventions to follow when editing

- All CSS goes in `style.css`, all JS goes in `script.js` — do not add
  inline `<style>`/`<script>` blocks back into the HTML pages.
- All colors/fonts are defined once as CSS custom properties in `:root` —
  change values there, not at each usage site. `--color-cream` was removed
  deliberately (all section backgrounds are now plain white with
  border-based separation) — don't reintroduce a cream/off-white token.
- Reference real files only from `assets/images/` — filenames are
  inconsistent (mixed case, spaces, a couple of typos like
  `punakha.1jpg.jpg`) — copy them byte-exact, never invent a filename.
- Two logo variants exist for a reason: `logo white.png` on dark/transparent
  surfaces, `logo.png` on light/white surfaces.
- When adding a new page, copy the header/footer markup from an existing
  page rather than reinventing it, and rewrite homepage-only anchors as
  `index.html#section`.
