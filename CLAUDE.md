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
   hover/focus dropdown of exactly 4 items — Cultural Tours (the only one
   linking to a real page, `cultural-tours.html`), Trekking Tours, Festival
   Tours, Luxury Tours (all still placeholder `#journeys`/`index.html#journeys`
   links) — deliberately no "Activities" item, since Activities is its own
   full section further down the page, not a tour-package category. On
   mobile the same 4 links live in `.mobile-nav-sub`. A `.lang-btn`
   placeholder sits in the header (no i18n wired up yet).
2. **Hero** — full-bleed video (`taktshang.jpg` poster), left with just the
   headline + `.hero-sub` — no `.hero-ctas` button row (removed). `.hero` is
   reused on inner pages with two modifiers: `.hero--about`/`.hero--cultural`
   etc. set the background image, and `.hero--page` turns the same
   full-`100vh` hero into a bottom-left page title (see the `.hero--page`
   note below) instead of a centered one.
3. **Explore Our Experiences slider** (`id="journeys"`, eyebrow "Plan Your
   Trip") — dark full-bleed horizontal card slider (scroll-snap + JS arrow
   controls) sitting directly under the hero. Exactly 4 cards, one per
   dropdown category above (Trekking/Cultural/Festival/Luxury Tours) — each
   card's `.journey-nights` badge shows a package count ("6 Packages" etc.)
   instead of a night count, and there's no `.journey-country` line (these
   are categories, not destinations). This replaced an earlier 6-card
   "Signature Journeys" version keyed to specific trips (Bumthang Valley,
   Dochula Pass, ...) — don't recreate that content from memory.
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
   without being asked. Each card's `.activity-info p` description AND its
   `.activity-cta` ("Explore This →") both stay hidden by default and only
   reveal together on `:hover` or on whichever card the autoplay/scroll
   currently has centered (`.is-active`, toggled by that same IIFE).
   Replaced the old five-tile "Find Your Journey" category grid — don't
   recreate `.category-tile`/`.category-row` from memory.
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
11. **Dark CTA** (`id="enquire"`) — `.dark-cta.dark-cta--photo.dark-cta--dark-photo`:
    a `buddha point.jpg` background with a heavy black overlay (`.dark-cta--dark-photo`
    stacks a darker `::after` on top of the lighter `.dark-cta--photo` one
    used by Cultural Tours — don't change the shared `.dark-cta--photo`
    opacity to darken this one, add/adjust `.dark-cta--dark-photo` instead).
    Content is an `.eyebrow--on-dark` ("Begin Your Story"), a **plain**
    `<h2>` ("Your **Bhutan** Awaits" — the middle word wrapped in `.accent`
    for gold, everything else the same uppercase League Gothic every other
    h1/h2/h3 on the site uses) — an earlier version used a custom mixed-case
    `.dark-cta-title` class here; that was reverted to stay on-theme, don't
    reintroduce it — a description, and two CTAs:
    `.btn-solid-gold` (text forced dark via `.dark-cta--dark-photo
    .btn-solid-gold`, scoped so it doesn't affect the plain gold button on
    About's own Dark CTA) and a new `.btn-outline-gold` linking to the same
    `wa.me/9752333456` WhatsApp number used on Cultural Tours. About's Dark
    CTA is untouched — still the plain dark-green/gold-glow `.dark-cta` with
    `.btn-outline-white`/`.btn-solid-gold`.
12. **Footer** — 4-column dark footer

(The old **Editorial** and **Press strip** sections have been removed
entirely — don't recreate them from memory.)

### about.html

Header/footer are the same markup as `index.html`, but every in-page anchor
that only exists on the homepage (`#journeys`, `#why`, `#blog`) is written as
`index.html#journeys` etc. The logo links to `index.html`. `#enquire` stays a
same-page anchor since About has its own dark-CTA section.

Sections: full-`100vh` image hero (`.hero.hero--about.hero--page.hero--center`,
same bottom-center-title + `.hero-mist` cloud overlay as cultural-tours.html
— see the `.hero--center`/`.hero-mist` notes below) → Our Story (`.about`) →
Our Philosophy (`.about.about--reverse`, mirrors the layout) → Values
(`.values-section`, 3 cards) → Team (`.team-section`, 4 cards with
gold-initial avatars — there are no real team photos in `assets/images/`,
so avatars are deliberately initials-only; swap in real photos via the same
`.team-avatar` markup if/when they exist) → Dark CTA → Footer.

### cultural-tours.html

Same header/footer pattern as `about.html`. `.hero.hero--cultural.hero--page.hero--center`
full-viewport title-only hero, **bottom-center**-aligned (`.hero--center`
overrides `.hero--page`'s horizontal alignment only — it stays vertically
bottom, just centered instead of left; both inner pages' heroes now use
this). It also has a `.hero-mist` overlay — see the `.hero-mist` note below.
Then a 6-card `.tour-grid`. `.tour-card` is a **full-bleed portrait image
card** (the white-card-with-icons version was tried and reverted) —
`aspect-ratio:3/5`, the whole card is an `<a>` with a `.bg` div for the
hover zoom, and `.tour-card-body` sits at `bottom:0` over the photo (moved
down from an earlier `top:30%`). Content is deliberately minimal: an
uppercase title only (`.tour-card-body h3` — plain `var(--font-body)` bold
uppercase white, same treatment as `.journey-title` on the homepage's
overlay cards; an earlier version used an upright `var(--font-italic)`
Playfair Display serif here instead, which was reverted to stay on-theme —
the Google Fonts `<link>` in every page's `<head>` is still widened to
`Playfair+Display:ital,wght@0,700;1,400` from the original `ital@1` in case
an upright weight is wanted again later, but nothing currently uses it), a
compact `.tour-meta` row (duration + altitude only), and a `.tour-cta`
underline that only appears on hover/focus. **No description paragraph** —
tried and removed; don't reintroduce a `<p>` in `.tour-card-body`. The dark
gradient (`.tour-card::after`) is also concentrated at the bottom ~45% of
the card now (`0% → 0% → 0.85` at 55%/100%) rather than washing the whole
photo, since the text lives at the bottom. No tag/index badge, no
`.tour-divider`, no white card body — don't reintroduce those from memory.
Its Dark CTA is
`.dark-cta.dark-cta--photo` (a background image + a light black overlay via
`.dark-cta--photo` — homepage and About keep their own `.dark-cta` variants,
see above; `.dark-cta-inner` text is forced white on this variant) with
`.contact-pill` WhatsApp/email links (white border on hover, not gold)
instead of the usual `.btn` pair. The header's "Experiences" dropdown
"Cultural Tours" item and the footer's "Cultural Travelers" link both point
here from every page — keep those in sync if this page is ever renamed or
removed. This is the template to copy for the other dropdown items
(Trekking Tours, Festival Tours, Luxury Tours) once those pages exist —
they currently all still point at `index.html#journeys`/`#journeys` as a
placeholder. ("Activities" is deliberately not in this dropdown — it's its
own homepage section, not a tour-package category.)

### `.hero--page` content pattern

Every inner-page hero (`about.html`, `cultural-tours.html`) shows **only**
the `<h1>` page title inside `.hero-content` — no eyebrow, no `.hero-sub`
description, no CTA buttons (those belong on the homepage hero only). An
earlier version put an eyebrow + description underneath the title; that was
deliberately simplified down to just the title. Follow this for any new
`.hero--page` hero rather than reintroducing the eyebrow/description.
`.hero--page` alone is bottom-**left**; both current inner pages add
`.hero--center` on top of it, which only overrides the horizontal alignment
(`justify-content`) to center the title — vertical stays bottom (`align-items`
is inherited from `.hero--page`, not reset) so the hero photo shows through
above the title. If a future inner page wants the original bottom-left, just
omit `.hero--center`.

### `.hero-mist` (drifting cloud overlay)

A `.hero-mist` div (absolutely positioned across the bottom ~42% of the
hero, `z-index:1` — below `.hero-content`'s `z-index:2`) holds two
`.hero-mist-layer` divs, each the same `cloud.png` tiled via
`background-repeat:repeat-x` on an element 3x viewport width, both animated
with the same `heroMistDrift` keyframe (`translateX` 0 → -33.33%, i.e.
right-to-left) at different speeds/opacities/`animation-delay` for a bit of
parallax — they used to drift in opposite directions (a `heroMistDriftB`
keyframe) but that read as clouds arriving from both sides at once, so both
layers now share one direction. A `min-width:901px` media query lengthens
the duration a lot (65s/100s → 150s/230s) because the same %-based
`translateX` covers far more pixels/second on a wide desktop viewport than
on mobile — don't "fix" desktop speed by changing the base (mobile)
duration, add/adjust the desktop override instead. Used on both
`about.html` and `cultural-tours.html`. `prefers-reduced-motion` freezes
both layers in place rather than removing them. Copy this same 3-element
structure (`.hero-mist` + two `.hero-mist-layer` children) if another hero
needs the same drifting-mist look.

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
