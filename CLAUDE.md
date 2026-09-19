# Altara — Project Notes

Multi-page luxury travel website for **Altara**, a private-journey travel brand
built around the Kingdom of Bhutan. Plain HTML/CSS/JS — no build step, no
framework, no external dependencies besides Google Fonts and one deliberate
exception on `enquire.html` (an embedded Google Map — see that page's notes
below).

## Structure

```
Altara/
├── index.html                     # Homepage markup only
├── about.html                     # About page markup only (story, philosophy,
│                                     values, team, dark CTA)
├── cultural-tours.html            # Cultural Tours page — 6-card itinerary grid
├── tigers-nest-pilgrimage.html    # Tour detail page for Cultural Tours' first
│                                     card — the template for future tour-detail
│                                     pages (day-by-day itinerary, district
│                                     highlight map, per-tour enquiry form)
├── trekking-tours.html            # Trekking Tours page — 6-card itinerary grid,
│                                     same template as cultural-tours.html
├── druk-path-trek.html            # Tour detail page for Trekking Tours' first
│                                     card — a richer variant of
│                                     tigers-nest-pilgrimage.html's template
│                                     (8 trip facts, What To Pack, 3-column
│                                     Included/Excluded breakdown, dedicated
│                                     FAQ accordion, footer photo gallery)
├── festival-tours.html            # Festival Tours page — NOT a tour-grid like
│                                     Cultural/Trekking; same hero, editorial
│                                     content about Bhutan's Tshechus, a 6-card
│                                     festival grid, and a downloadable festival
│                                     dates PDF instead of bookable itineraries
├── luxury-tours.html              # Luxury Tours page — same hero as the other
│                                     category pages, and .luxury-card matches
│                                     .tour-card's own sizing (3-up, 3/5 ratio),
│                                     but reads as a "book" cover instead of a
│                                     plain photo (spine, ribbon bookmark, plain
│                                     soft shadow — no border); the fourth and
│                                     last Experiences category page
├── why-bhutan.html                 # Information page — persuasive "why go"
│                                     page (Gross National Happiness, living
│                                     Buddhism, untouched culture), first in
│                                     the Information dropdown, ahead of
│                                     about-bhutan.html's country-facts page
├── about-bhutan.html              # Information page — country primer, quick
│                                     facts, best time to visit
├── tariff.html                    # Information page — Sustainable Development
│                                     Fee pricing (redesigned to a single big
│                                     stat, not three bordered cards),
│                                     what's included/not, visa
├── faq.html                       # Information page — full categorized FAQ
│                                     accordion (Planning, Visa & Entry, Health
│                                     & Practical)
├── enquire.html                   # Enquire Now page — split contact form,
│                                     "Reach Us Directly" icon row, embedded map
├── blog.html                      # Journal page — the homepage's .blog
│                                     teaser slider content (same six posts)
│                                     as a plain static grid; the "Blog" nav
│                                     link's real destination on every page now
├── inside-tigers-nest.html        # The first individual article page, for
│                                     the "Inside Tiger's Nest" Journal post —
│                                     both blog.html's and index.html's first
│                                     blog card link here now
├── style.css                      # ALL CSS for the whole site (every page)
├── script.js                      # ALL JS for the whole site (every page)
├── altara-brand-guidelines.html   # Standalone brand reference doc (not linked
│                                     from the nav — colors match the live site,
│                                     but documents an OLDER font pairing, see
│                                     "Known inconsistency" below)
└── assets/
    └── images/
        ├── logo/                  # logo.png (dark, light bg) + logo white.png (light, dark/overlay bg)
        │   └── brand/             # 6 partner/affiliate logos for the homepage's
        │                            Partners & Affiliates marquee — Bhutan Airlines,
        │                            Drukair, Tourism Council of Bhutan, ABTO, Guide
        │                            Association of Bhutan, Hotel Jakar View. Filenames
        │                            are inconsistent (mixed case, one has a typo —
        │                            "guide assocation of bhutan.png") — same
        │                            byte-exact-copy rule as assets/images/image/.
        ├── video/                 # hero background video
        ├── cloud.png              # .hero-mist overlay tile (see the .hero-mist note)
        ├── documents/             # downloadable PDFs — currently just
        │                            tentative_festival_dates_2026.pdf, linked
        │                            from festival-tours.html's festival-calendar
        │                            download button. Keep any future
        │                            downloadable document here too rather than
        │                            starting a second documents folder.
        └── image/                 # ~255 Bhutan travel photos — every actual photo
                                      reference in the site lives under this
                                      subfolder now (assets/images/image/<file>.jpg),
                                      NOT directly in assets/images/. The photo set
                                      was fully replaced at this path on 2026-09-09;
                                      every reference was re-picked from the new set
                                      with no two logical image slots sharing the
                                      same file (bg + .bg pairs on the same card are
                                      the only intentional 2x reuse), and every
                                      hero/full-bleed background is landscape. If
                                      the folder is ever moved/renamed again, that's
                                      a site-wide find-and-replace, not a one-off.
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
   hover/focus dropdown of exactly 4 items, and as of `luxury-tours.html`
   all four — Cultural Tours, Trekking Tours, Festival Tours and Luxury
   Tours — link to their own real pages (`cultural-tours.html`,
   `trekking-tours.html`, `festival-tours.html`, `luxury-tours.html`); none
   of the four is a placeholder anymore — deliberately no
   "Activities" item, since Activities is its own full section further down
   the page, not a tour-package category. "Information" is the same
   `.nav-item`/`.nav-dropdown` pattern too: the trigger itself still links to
   `#why`/`index.html#why` (the homepage's Why Altara section, exactly like
   "Experiences" still links to `#journeys`), and its dropdown holds the
   four Information pages — Why Bhutan?, About Bhutan, Tariff, FAQ (see
   below), in that order. On
   mobile the same links live in two separate `.mobile-nav-sub` blocks, one
   per trigger — `script.js`'s mobile-menu IIFE pairs every
   `.mobile-nav-toggle` with its own very next `.mobile-nav-sub` sibling
   (`nextElementSibling`), rather than assuming there's only one toggle on
   the page, specifically so a second category (Information) could be added
   without a JS rewrite; add a third the same way if needed. A `.lang-btn`
   placeholder sits in the header (no i18n wired up yet).
2. **Hero** — full-bleed video (`taktshang.jpg` poster), bottom-center
   aligned (`.hero`'s own `align-items:flex-end`/`justify-content:center`,
   so more of the video shows above the text) with just the headline +
   `.hero-sub` — no `.hero-ctas` button row (removed). `.hero` is
   reused on inner pages, always with `.hero--page` (see below) and usually
   `.hero--center`; `.hero--about` additionally sets `.hero-content{max-width:760px}`
   on About only. No modifier sets a background image anymore — every inner
   page sets its own hero photo inline via `style="background-image:url(...)"`
   directly on the `.hero` element (`about.html`, `cultural-tours.html`,
   `trekking-tours.html`), matching the inline `background-image` pattern
   used by `.tour-card`/`.journey-card`, so swapping an inner page's hero
   photo is a one-line HTML edit rather than a `style.css` lookup.
   `.hero--page` turns the same full-`100vh` hero into a bottom-left page
   title (see the `.hero--page` note below) instead of a centered one.
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
   "Read More" links to `about.html`. `.about-media-accent` (the small
   offset photo overlapping the main one) is `aspect-ratio:4/5` — a
   portrait crop matching `.about-media-main`'s own ratio, not the square
   `1/1` it started as; it's a shared component with `about.html`'s Our
   Story/Our Philosophy sections (see below), so this taller crop applies
   there too, not just on the homepage.
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
6. **Explore Bhutan map** — interactive SVG map of Bhutan's 20 dzongkhags.
   Scroll-pinned on desktop (`.bhutan-scroll-pin`, `position:sticky`) so the
   map holds in view while an animated sequence plays: an international
   flight arrives at Paro from outside the map outline, then three domestic
   flights depart Paro for Bumthang/Sarpang(Gelephu)/Trashigang(Yonphula),
   then three land border-gate markers fade in (Phuentsholing/Gelephu/
   Samdrup Jongkhar). On mobile (<901px) the same sequence just plays once,
   timed, as the section scrolls into view (no pin — too fragile on touch).
   Hover a district for a redesigned info-bar tooltip. Sidebar stat cards
   (`.bhutan-map-facts`) sit beside the map. Under 640px, `.explore-bhutan`
   gets extra top padding (`140px 0 80px`, not the generic `80px 0` other
   mobile sections use) specifically because this section's background
   (`--color-offwhite`) is the same color `header.scrolled` uses — with
   only 80px of clearance the fixed header (up to ~100px unscrolled/~70px
   scrolled, plus its blur/box-shadow) could sit flush over "Routes Into
   The Kingdom," and since the colors match it read as the heading fading
   out rather than being obviously covered, not as a section that's
   actually further down the page. Don't shrink this back to match Blog's/
   FAQ's `80px 0` without re-checking clearance. The desktop/pinned
   version (`min-width:901px`, `.explore-bhutan{position:sticky;top:0;
   height:100vh;...}`) had the exact same bug, worse — that variant sits
   flush against the viewport top with only `60px 0` padding, so the
   heading was even more directly under the fixed header there; it's now
   `padding:110px 0 60px` (asymmetric — more top clearance only, `flex`
   `align-items:center` still centers the content within the padded box,
   just shifted down). Both fixes address the same root mismatch; check
   both if this section's header ever goes invisible/faded again. Related: the header's own
   scroll-state IIFE (`script.js`) now also runs its `update()` once on
   load, not only on the next `scroll` event — previously a page that
   loaded already scrolled (mobile back/forward nav, bfcache, landing on
   an in-page anchor) could keep the transparent/white-text "top of page"
   header state indefinitely until the next scroll tick, which made the
   same mismatch worse on any light-background section, this one included.
7. **Why Altara** — image/copy split with bullet list
8. **Blog** (`.blog`, `id="blog"`) — a horizontal photo-card slider, same
   scroll-snap + arrow-button mechanics as Explore Trips/Activities
   (`initCardSlider('blogTrack','blogPrev','blogNext','blog-card')` — a
   **fourth** call to that function, not a new bespoke slider). `.blog-card`
   is full-bleed-photo-with-overlay like `.tour-card`/`.journey-card`
   elsewhere (a `.bg` div for the hover zoom, a bottom-anchored gradient,
   text pinned to `bottom:0`) rather than the plain white
   image-on-top-then-text card an earlier version used — don't recreate
   that older `.blog-card-image`/`.blog-card-tag` version from memory.
   Six cards now (not three) so the slider has real overflow to scroll
   through, not just three cards that already fit one view. Each card at
   rest shows only a white `.blog-card-meta` date (no byline anymore — an
   earlier version added "By <author>", bylined to already-established
   About-page team members on rotation, which was later dropped; don't
   reintroduce the author back into `.blog-card-meta` from memory) and a
   white title — no description paragraph either (`.blog-card-body p` was
   removed entirely, not just hidden; don't reintroduce a `<p>` inside
   `.blog-card-body`). Only the "Read More" link
   (`.blog-card-link`, `max-height`+`opacity` transition — the same
   resting-clean / reveal-on-hover technique `.activity-cta` uses on the
   Activities slider, just with a top-rule instead of an underline)
   reveals on hover, directly under the title.
   `.blog-head` also grew a
   `.blog-subtitle` line under the h2, and its right side is now
   `.blog-arrows` (two `.slider-arrow`s, `position:static` overriding
   their usual absolute-over-the-track positioning since these sit inline
   in the flex header row instead) rather than the earlier "View All
   Stories" `.btn-outline-green` link, which was dropped, not hidden —
   don't reintroduce it without being asked. `.blog-card::after`'s bottom
   gradient also darkens across the **whole** card on hover
   (`.blog-card:hover::after`, same gradient shape just raised to
   `rgba(14,21,18,0.55)→0.92)` instead of `0→0.9`) rather than staying
   only a bottom band, so the CTA revealed on hover stays
   readable no matter where it lands on the photo. This teaser section
   itself is untouched, but the header/footer "Blog" link site-wide no
   longer points here (`index.html#blog`/`#blog`) — it now goes to the
   real `blog.html` page (see below), which reuses these same six posts
   in a plain static grid.
9. **Partners & Affiliates** (`.partners`) — an infinite CSS marquee of 6
    partner/affiliate logos (Bhutan Airlines, Drukair, Tourism Council of
    Bhutan, ABTO, Guide Association of Bhutan, Hotel Jakar View), each
    linking out to that org's real official site
    (`target="_blank" rel="noopener"`), sourced from
    `assets/images/logo/brand/` (byte-exact filenames — same rule as the
    photo library, copy them exactly rather than guessing). `.partners-track`
    holds the 6 logos once, then the same 6 again immediately after,
    `aria-hidden="true" tabindex="-1"` on the second set since they're
    decorative repeats — the whole row is exactly double its visible width,
    so animating `translateX` from `0` to `-50%` loops seamlessly (same
    "duplicate content + translate a fixed distance" trick `.hero-mist`
    already uses for its drifting clouds). Logos render grayscale/faded at
    rest and go full-color on hover (`.partner-logo:hover`), which also
    pauses the scroll (`.partners-track:hover{animation-play-state:paused;}`)
    so a name can actually be read before clicking. `prefers-reduced-motion`
    stops the animation and falls back to a manually-scrollable row rather
    than removing the logos. This is a **slider**, not a manual prev/next
    one like `initCardSlider` — don't wire arrow buttons onto it.
10. **FAQ** — accordion
11. **Dark CTA** (`id="enquire"`) — `.dark-cta.dark-cta--photo`, same pattern
    and markup as Cultural Tours' Dark CTA (see below), just its own
    `buddha point.jpg` background and copy: a plain `<h2>` ("Begin Your
    Story", no eyebrow, no `.accent` span), a description, and the same
    `.contact-pill` WhatsApp (`wa.me/9752333456`)/email (`hello@altara.travel`)
    pair Cultural Tours uses — no `.btn`/`.btn-solid-gold`/`.btn-outline-gold`
    buttons here anymore (the earlier button-pair version, and the
    `.dark-cta--dark-photo` heavier-overlay modifier it needed, were
    replaced so homepage matches Cultural Tours' design exactly, just with a
    different background image — don't reintroduce either from memory).
    An earlier version also used a custom mixed-case `.dark-cta-title` class
    for the heading; that stays reverted too, don't bring it back. About's
    Dark CTA is untouched — still the plain dark-green/gold-glow `.dark-cta`
    with `.btn-outline-white`/`.btn-solid-gold`.
12. **Footer** — 4-column dark footer

(The old **Editorial** and **Press strip** sections have been removed
entirely — don't recreate them from memory.)

### about.html

Header/footer are the same markup as `index.html`, but every in-page anchor
that only exists on the homepage (`#journeys`, `#why`, `#blog`) is written as
`index.html#journeys` etc. The logo links to `index.html`. `#enquire` stays a
same-page anchor since About has its own dark-CTA section.

Sections: full-`100vh` image hero (`.hero.hero--about.hero--page.hero--center`,
background image set inline on the `.hero` element (see the Hero note
above), same bottom-center-title + `.hero-mist` cloud overlay as
cultural-tours.html — see the `.hero--center`/`.hero-mist` notes below) →
Our Story (`.about`) →
Our Philosophy (`.about.about--reverse`, mirrors the layout) → Values
(`.values-section`, 3 cards) → Team (`.team-section`, 4 cards with
gold-initial avatars — there are no real team photos in `assets/images/`,
so avatars are deliberately initials-only; swap in real photos via the same
`.team-avatar` markup if/when they exist) → Dark CTA → Footer.

### cultural-tours.html

Same header/footer pattern as `about.html`. `.hero.hero--cultural.hero--page.hero--center`
full-viewport title-only hero, its background image set inline via
`style="background-image:url(...)"` on the `.hero` element (not in
`style.css` like `.hero--about` — see the Hero note above), **bottom-center**-aligned (`.hero--center`
overrides `.hero--page`'s horizontal alignment only — it stays vertically
bottom, just centered instead of left; both inner pages' heroes now use
this). It also has a `.hero-mist` overlay — see the `.hero-mist` note below.
`.tour-head` (eyebrow + h2 above the grid) had its `.eyebrow.eyebrow--accent`
("Six Ways In") removed — same for `trekking-tours.html`'s ("Six Trails
In"), `luxury-tours.html`'s ("Six Private Circuits") and
`festival-tours.html`'s `.festival-head` ("Worth Timing Your Trip
Around"), the four Experiences-category child pages' shared one-eyebrow-
per-page pattern. Only this page's `.tour-head` gained a short `<p>` in
its place (`.tour-head p`, styled like `.info-lead p`) — a couple of
sentences on what Cultural Tours actually means day to day; the other
three pages' headings were left standing alone with no replacement text.
`festival-tours.html`'s other three eyebrows (its `.info-lead` intro, the
"What Happens At A Tshechu" `.about-copy` block, and the calendar-download
card) were deliberately left as-is — only the one structurally analogous
to `.tour-head` came out.
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
`.dark-cta.dark-cta--photo` (a background image + a black overlay via
`.dark-cta--photo`'s shared `::after` — homepage now reuses this same
variant/markup with its own background image, see above; About keeps its
own plain `.dark-cta`; `.dark-cta-inner` text is forced white on this
variant) with `.contact-pill` WhatsApp/email links (white border on hover,
not gold) instead of the usual `.btn` pair. The header's "Experiences" dropdown
"Cultural Tours" item and the footer's "Cultural Travelers" link both point
here from every page — keep those in sync if this page is ever renamed or
removed. This was the template copied for `trekking-tours.html` (see below);
Festival Tours and Luxury Tours both got real pages of their own too
(`festival-tours.html`, `luxury-tours.html`, see below), though neither
reuses this page's `.tour-grid` pattern — Festival Tours went editorial,
Luxury Tours built its own book-styled card grid. ("Activities" is
deliberately not in this dropdown — it's its own homepage section, not a
tour-package category.)

One card is the exception to "every card still points at `#enquire`": the
first card, **The Tiger's Nest Pilgrimage**, links to its own dedicated
page, `tigers-nest-pilgrimage.html` (see below) — the other five still
scroll to this page's own Dark CTA. If any of those five gets its own
detail page next, repoint its card the same way.

### tigers-nest-pilgrimage.html

The template for individual-tour detail pages — the first one built, for
Cultural Tours' "The Tiger's Nest Pilgrimage" card (5 Days/4 Nights,
3,120m, `taktshang.jpg`). Modeled on a reference page a prior version of
this brand used (`Himaquest Travels`' own `tigers-nest-pilgrimage.html`),
**restructured to match, reskinned entirely in Altara's own design
system** — its dark/amber palette, pill buttons and Oswald/Cabin type
never made it in; only the content architecture did. Same header/footer as
every page.

Every content block below (`.tour-facts-card`, `.tour-note`,
`.tour-content-inner`, `.itinerary-list`, `.tour-map-card`,
`.tour-cost-card`, `.tour-enquire-card`, plus the `.itinerary-head`/
`.tour-map-head` intro labels) is deliberately **left-anchored, not
centered** — `margin:0` (not `margin:0 auto`) within `.container`, and
`text-align:left` on the two head blocks that used to be centered —  so
the reserved width beyond each block's own `max-width` shows up as empty
space on the right only, not split evenly on both sides. Every section
wrapper (`.tour-hero-info`, `.tour-facts-section`, `.tour-content`,
`.itinerary`, `.tour-map-section`, `.tour-cost`, `.tour-enquire`) also
shares the exact same `padding:70px 0`. Every content block additionally
shares one `max-width:1100px` (`.tour-facts-card`, `.tour-note`,
`.tour-content-inner`, `.itinerary-list`, `.tour-map-card`,
`.tour-cost-card`, `.tour-enquire-card`) — keep all three of these
consistent across the template: a new content block gets `margin:0`
(never `auto`) and `max-width:1100px`, a new section reuses the same
`padding:70px 0` — don't tune any one of these to look tight against its
specific neighbor, or they drift out of sync the way they did before.

Content, top to bottom (new CSS at the end of `style.css`
under "TOUR DETAIL PAGES"):

1. **Hero — `.tour-gallery-hero` + `.tour-hero-info`** (also modeled on a
   second Himaquest reference page, `five-lodge-valley-circuit.html`,
   which uses this gallery hero instead of that site's single-photo one).
   `.tour-gallery-hero` is a horizontal scroll-snap strip of 5
   `.tour-gallery-slide`s sitting directly under the header (not behind
   it like every other hero — `<body class="tour-gallery-page">` forces
   `header:not(.scrolled)` to a solid dark-green background instead of
   its usual transparent-over-photo start state, since there's no
   full-bleed image behind the header here for transparency to read
   against; forgetting that class on a gallery-hero page leaves white nav
   text invisible on a white page background). The first slide is a
   local-video slide (`data-video-src`, no YouTube/external embed —
   clicking `.tour-gallery-play` swaps its poster image for a `<video>`
   pointed at the homepage's own brand film,
   `assets/video/Bhutan Believe_ Land of the Thunder Dragon I New Brand
   Film.mp4`, staying inside the "no external dependencies besides Google
   Fonts [and the one approved Maps exception]" rule below). Prev/next
   (`#tourGalleryPrev`/`#tourGalleryNext`, styled with the existing
   `.slider-arrow` class) are wired via `initCardSlider` in
   `script.js` — this is that function's **third** call, per the standing
   "add future horizontal sliders as a new call to it" rule, not a new
   bespoke slider. `.tour-gallery-slide img{object-fit:cover}` is
   deliberate, matching the reference exactly (a `contain` version was
   tried — it letterboxes on the dark slide background instead of filling
   the frame edge-to-edge, which isn't what was wanted here); each slide's
   landscape source photo instead gets its own inline `object-position` on
   the `<img>` tag so the actual subject (the dancer, the tiger mural,
   Taktsang itself) stays in frame in this tall narrow crop rather than
   an arbitrary 50/50 center — set one on any new slide whose subject
   isn't already dead-center. Clicking any non-video slide's photo opens
   it uncropped in a full-screen `.tour-gallery-lightbox`
   (`object-fit:contain`, so this is where a visitor actually sees the
   whole image rather than the strip's necessarily-cropped view), with its
   own `.tour-gallery-lightbox-arrow` prev/next (`#tourGalleryLightboxPrev`/
   `#tourGalleryLightboxNext`) cycling through the same photo set —
   `#tourGalleryLightbox`/`#tourGalleryLightboxImg`/`#tourGalleryLightboxClose`
   in the HTML, a small dedicated IIFE in `script.js` (its own click/keydown
   listeners — including ArrowLeft/ArrowRight — not routed through
   `initCardSlider`, since this is a separate index/render loop over the
   same image set rather than a scroll-snap track), closes on the × button,
   clicking the dark backdrop, or Escape. `.tour-hero-info` is a plain white section right
   after the gallery holding the `.hero-tour-badge` ("5 Days / 4
   Nights") + `<h1>` + one-line summary — content that's normally
   forbidden on `.hero--page` (title-only) lives here instead because
   it's a separate section below the gallery, not overlaid on a photo.
   `style.css` also still carries a `.hero--tour` single-full-bleed-photo
   hero variant (bordered badge + h1 + description overlaid on one photo,
   `.hero.hero--page.hero--tour`) as an alternative for a future
   tour-detail page that doesn't have 4-5 spare unique photos for a
   gallery — pick one hero style or the other per page, never both.
2. **`.tour-facts-card`** — a bordered card holding a 2-col
   `.tour-facts-grid` of six `.tour-fact`s (icon-in-a-circle, matching
   `enquire.html`'s `.contact-icon-circle` treatment + label + value):
   Destinations, Best Season, Max Altitude, Accommodation, Meals,
   Transport — richer than a bare duration/altitude strip. A
   `.btn.btn-solid-gold` "Book This Tour" button at the card's bottom
   links to `#enquire` further down this same page (not to
   `enquire.html` — this is the page's own contextual quick-action,
   same convention as every other page's local `#enquire` anchor).
   `.tour-note` below the card is a small gold-left-border caveat that
   the itinerary is customized further once someone actually enquires.
3. **`.tour-content`** — two `.tour-content-inner` prose blocks in the
   same 1100px column every other content block on this page uses now
   (`.tour-facts-card`, `.tour-note`, `.itinerary-list`, `.tour-map-card`,
   `.tour-cost-card`, `.tour-enquire-card` — all explicitly unified to one
   `max-width:1100px` after a round of individually-tuned widths, 760–1000px,
   drifted out of sync with each other): "Tour Overview" (one paragraph)
   and "Tour Highlights" (a 2-col `.tour-highlights` checklist, gold checkmarks — same checkmark
   glyph Tariff's `.tariff-includes` uses).
4. **`.itinerary`** — day-by-day, now built as an **accordion** reusing
   `.faq-item`/`.faq-question`/`.faq-answer`/`.faq-icon` verbatim (an
   earlier version used a static always-open `.itinerary-day` list;
   reusing the FAQ accordion needed zero new JS, since `script.js`'s FAQ
   IIFE already closes every other open `.faq-item` on the page when one
   opens — exactly the one-open-at-a-time behavior wanted here too).
   `.itinerary-list .faq-item:first-child` gets its own top rule since
   these items aren't inside `.faq-columns` here. One `.faq-item` per
   night+1 for whatever `X Days / Y Nights` that tour's card says — this
   one has 5.
5. **`.tour-map-section`/`.tour-map-card`** — a **static, non-interactive**
   "Where You'll Go" district map, copied from the homepage's Explore
   Bhutan SVG (`index.html`'s `#bhutan-svg`, the `<defs>` + 20 `<path
   class="district">` district shapes only — not the flight-path/
   border-gate/tooltip machinery, which is JS-driven and specific to that
   section) since the site has no include/templating system to share it
   any other way (same reasoning as copying header/footer markup
   verbatim). Renamed to `.tour-district` on this page specifically so it
   never inherits the homepage map's `.bhutan-map-container .district`
   hover/tooltip CSS or needs any of that section's JS; districts the
   tour actually covers get `is-on-tour` added to their class (here:
   `BT-11` Paro and `BT-15` Thimphu, matching the itinerary above) which
   fills them the site's primary dark green instead of light gray, on a
   `2px solid var(--color-primary-dark)` + shadow card border (bumped up
   from a barely-visible `1px var(--color-rule)` after it was flagged as
   invisible against the map — `.enquire-map`'s border got the same fix
   at the same time). A `.tour-map-caption` line under the map spells out
   the district names in prose too. When copying this section to a new
   tour page, re-derive which `id`s to mark `is-on-tour` from that tour's
   own itinerary, and re-check the map still matches.
6. **`.tour-cost`/`.tour-cost-card`** — a short "Package Cost" note (SDF
   + trip-cost model, links to `tariff.html`) on the same faint gold tint
   background used for read-only form fields elsewhere
   (`rgba(176,141,87,0.06)`) rather than reintroducing the removed cream
   token.
7. **`.tour-enquire`/`.tour-enquire-card`** — a bordered "Enquire About
   This Tour" form card, `id="enquire"` (this page's own quick-contact
   anchor, same convention every other page follows). Reuses
   `.enquire-form`/`.form-row`/`.form-field`/`.field-underline` from the
   ENQUIRE PAGE block as-is — no new form CSS, just a card wrapper
   instead of the split-photo layout `enquire.html` uses (a single tour
   page doesn't need its own planner-photo panel). The one field type
   specific to this template is a **read-only "Tour" field**, pre-filled
   with the page's own title (`value="The Tiger's Nest Pilgrimage"`) so
   the emailed enquiry always says which tour it's about without the
   visitor having to type it — every future tour-detail page copying this
   template must set that field's value to its own title. Below the
   form, a `.tour-enquire-quick` row repeats the WhatsApp/email quick
   actions.

No "Related Tours" sidebar yet (the reference page has one) — even now
that `druk-path-trek.html` exists too (see below), there's still nothing
built to point a "Related Tours" list at beyond just that one other page,
so it stays deferred rather than linking to a thin one-item list. No
sitewide Dark CTA section on this page either — the tour-specific enquiry
card already is the page's CTA, same reasoning as `enquire.html` dropping
its own Dark CTA.
cultural-tours.html's Tiger's Nest card links here instead of `#enquire`
(see above) — keep that in sync if this page is ever renamed or removed.

### trekking-tours.html

Byte-for-byte the same template as `cultural-tours.html` (header/footer,
`.hero.hero--page.hero--center` with an inline background image, `.tour-section`
→ `.tour-grid` of six `.tour-card`s, `.dark-cta.dark-cta--photo` with
`.contact-pill` links) — no new CSS or JS was needed. Differences are just
content: hero title "Trails Into Bhutan's Untouched High Country", eyebrow
"Six Trails In" / h2 "Trekking Itineraries", and the six treks named in
`assets/images/image/altara-bhutan-seo-keywords-tour-packages.md` (that file
is a pre-existing SEO/content brief with 6 named itineraries per category —
Cultural, Trekking, Festival, Adventure, Luxury — worth checking before
inventing new tour names on any future tour-category page): Druk Path Trek,
Jomolhari Trek, The Snowman Trek, Laya Gasa Trek, Dagala Thousand Lakes
Trek, Bumdra Trek. Each card uses its own unique, previously-unused photo
from `assets/images/image/` (mostly from the `snowmentrek*`-prefixed set,
whose filenames don't literally match each card's trek name — same
filename-vs-content looseness already seen elsewhere on the site, e.g.
Cultural Tours' "Khoma..." images); duration/altitude in `.tour-meta` are
approximate real-world figures for each named route, not placeholders. The
header's "Experiences" dropdown "Trekking Tours" item and the footer's
"Trekkers" link both point here from every page now — keep those in sync
if this page is ever renamed or removed. Festival Tours and Luxury Tours
have since gotten their own real pages too (`festival-tours.html`,
`luxury-tours.html`, see below), though both were built to a different,
non-`.tour-grid` brief in each case. All four Experiences categories now
have a real page — there's no fifth to add without a new request.

Like Cultural Tours, one card is now an exception to "every card still
points at `#enquire`": the first card, **Druk Path Trek**, links to its own
dedicated page, `druk-path-trek.html` (see below) — the other five still
scroll to this page's own Dark CTA. If any of those five gets its own
detail page next, repoint its card the same way.

### druk-path-trek.html

The **second** tour-detail page, and the template for a richer variant of
`tigers-nest-pilgrimage.html`'s pattern — for Trekking Tours' "Druk Path
Trek" card (6 Days/5 Nights, 4,210m at Phume La, `Snowman Race48.jpg`).
Modeled on a second Himaquest Travels reference page
(`jomolhari-trek.html`), again restructured to match and reskinned
entirely in Altara's own design system — only the content architecture
carried over. New CSS lives right after `.tour-hero-info` in the "TOUR
DETAIL PAGES" block, commented as the "Second tour-detail template (richer
variant)". Reuses almost everything from the Tiger's Nest template
(`.tour-facts-card`, `.tour-content`/`.tour-highlights`,
`.itinerary`/`.faq-item` accordion, `.tour-cost-card`,
`.tour-enquire-card`) plus a few new pieces this longer page needed:

1. **Hero — `.hero.hero--page.hero--tour`**, the single-full-bleed-photo
   variant (badge + h1 + description over one photo), not
   `.tour-gallery-hero` — this page instead moves its photo gallery to a
   **supplementary section near the bottom** of the page (see point 8
   below), so `<body>` does NOT need the `tour-gallery-page` class here
   (a real photo already sits behind the header as normal, so the header's
   default transparent-then-white-on-scroll behavior needs no override).
   Picking one hero style per page instead of both is the same rule
   `tigers-nest-pilgrimage.html`'s notes already call out.
2. **`.tour-facts-card`** — 8 facts instead of 6 (Region, Duration, Max
   Altitude, Best Season, Difficulty, Accommodation, Meals,
   Transportation), and **two** buttons under the grid via the new
   `.tour-facts-actions` wrapper (`display:flex;gap:16px`) instead of one:
   `.btn.btn-solid-gold` "Book This Trek" (`#enquire`) and
   `.btn.btn-outline-green` "View All Trekking Tours"
   (`trekking-tours.html`).
3. **`.tour-content`** — same Overview + Highlights pattern as Tiger's
   Nest, then a **third** `.tour-content` block, "What To Pack For The
   Druk Path Trek" (one plain paragraph, no list).
4. **`.itinerary`** — 6 accordion days (one per night+1, same
   `.faq-item` reuse as Tiger's Nest).
5. **Included/Excluded — `.tour-cost-columns`** — a new 3-column grid
   (`grid-template-columns:repeat(3,1fr)`, stacks to one column under
   900px) breaking the cost down into "Trip Cost Includes" (a
   `.tour-highlights.tour-highlights--stack` gold-check list — the new
   `--stack` modifier just forces `.tour-highlights` to one column instead
   of its normal 2-up grid, for use inside a narrower 3-up column), "Trip
   Cost Excludes" (the new `.tour-excludes` list — same row/gap shape as
   `.tour-highlights` but with a muted-gray × icon instead of a gold
   check, matching Tariff's `.tariff-includes--exclude` convention rather
   than inventing a third checklist style), and "Trek Cost Includes"
   (another `.tour-highlights--stack` list). Each column is labeled with
   the new `.tour-detail-subhead` (small uppercase label + bottom rule).
   Closes with a `.tour-note` about the 15kg trek load allowance.
6. **A dedicated FAQ accordion** — "Frequently Asked Questions" reusing
   the exact same `.itinerary`/`.faq-item` markup as the day-by-day
   itinerary above it (just a second, separate `.itinerary-list` further
   down the page) — the FAQ IIFE in `script.js` already treats every
   `.faq-item` on the page as one shared accordion, so this needed no JS
   changes either. Five trek-specific Q&As.
7. **`.tour-cost-card`** and **`.tour-enquire-card`** — identical pattern
   to Tiger's Nest, with the read-only Tour field set to
   `value="Druk Path Trek"`.
8. **`.tour-gallery-hero.tour-gallery-hero--footer`** — the exact same
   gallery/lightbox component `tigers-nest-pilgrimage.html` uses as its
   hero, just relocated to a section near the bottom of the page (before
   the footer) and given the new `--footer` modifier
   (`padding:70px 0` — the same section rhythm every other section on
   this page uses, since it's no longer sitting flush under a fixed
   header). Deliberately reuses the exact same element IDs
   (`tourGalleryTrack`/`tourGalleryPrev`/`tourGalleryNext`/
   `tourGalleryLightbox` and its child IDs) as Tiger's Nest's gallery —
   since `script.js`'s slider/lightbox IIFEs look those IDs up with
   `getElementById`/`querySelector` rather than anything page-scoped, the
   exact same JS already runs correctly on this page's gallery with zero
   `script.js` changes. Only one gallery per page still applies — don't
   add a second on any future page without giving it its own ID set.
   5 slides: 1 video (`data-video-src`, same brand-film MP4 as Tiger's
   Nest) + 4 photos (`snowmentrek7.jpg`, `snowmentrek12.jpg`,
   `snowmentrek10.jpg`, `phobjikha3.jpg`).

`trekking-tours.html`'s Druk Path Trek card links here instead of
`#enquire` (see above) — keep that in sync if this page is ever renamed or
removed. No sitewide Dark CTA section, same reasoning as Tiger's Nest — the
page's own enquiry card is already its CTA.

### festival-tours.html

The header's "Experiences" dropdown "Festival Tours" item is real now — it,
and every page's matching mobile-nav-sub entry, previously pointed at the
`index.html#journeys`/`#journeys` placeholder (see the `index.html` Header
bullet above); all ten other pages plus this one were updated in the same
pass so every "Festival Tours" link site-wide now points to
`festival-tours.html`. The homepage's Explore Our Experiences card for this
category also now links here instead of `#journeys`, and its badge was
changed from "5 Packages" to "6 Festivals" to match — that slider still
shows a package/night-style count per card, but "Festivals" reads more
honestly than "Packages" for content that isn't itself bookable
inventory. Every page's footer "Who We Serve" column also grew a
"Festival Travelers" entry alongside the existing "Trekkers"/"Cultural
Travelers" links, all pointing here — keep both the nav dropdowns and this
footer list in sync if this page is ever renamed or removed. Luxury Tours
got the exact same site-wide link-and-footer treatment when
`luxury-tours.html` was built (see below) — the pattern to repeat if a
fifth Experiences category is ever added.

Unlike `cultural-tours.html`/`trekking-tours.html`, this page deliberately
does **not** reuse the `.tour-grid` 6-card-of-bookable-itineraries pattern
— there's no per-festival detail page for a card to link to, and the
brief was to talk about the festivals themselves rather than sell six more
packages. Same header/footer and same
`.hero.hero--page.hero--center`/`.hero-mist` hero as Cultural/Trekking
Tours (own photo, `Thimphu Tshechu by Bassem Nimah18.jpg`) — "keep the
hero section the same" was explicit — but everything under it is new,
built from mostly-reused components (new CSS only for the festival grid,
under "FESTIVAL TOURS PAGE" at the very end of `style.css`):

1. **Intro — reuses `.info-lead` verbatim** (the same centered
   eyebrow+h2+lead-paragraph block Information pages use directly under
   their hero) explaining what a Tshechu actually is in general terms
   (the lunar-calendar "tenth day" naming, masked cham dances, the once-
   a-year community gathering) before naming any specific festival.
2. **"What Happens At A Tshechu" — reuses `.about`/`.about-media`/
   `.about-copy`/`.about-link` verbatim**, the exact split-photo editorial
   block About's Our Story/Our Philosophy sections use, one level deeper
   than the intro: what cham dances actually depict and why people
   believe watching them confers a blessing, plus the predawn thongdrel
   unveiling most Tshechus close on.
3. **`.festival-section`/`.festival-grid`/`.festival-card`** — the one
   genuinely new component, a 3-up (2-up/1-up responsive) grid of the six
   real festivals from
   `assets/images/image/altara-bhutan-seo-keywords-tour-packages.md`'s
   Festival Tours list (Paro Tshechu, Thimphu Tshechu, Punakha Drubchen &
   Tshechu, Jambay Lhakhang Drup, Black-Necked Crane Festival, Gangtey/
   Wangdue Tshechu), each a bordered white `.festival-card` (photo top,
   gold month label, title, 2–3 sentence description, an `.about-link`
   "Enquire About This Festival" reusing that same link style again)
   rather than a new card design from scratch. Not full-bleed-photo
   overlay cards like `.tour-card`/`.journey-card` — a plain photo-on-top
   card instead, deliberately different from the tour-grid pattern per
   the "don't add contents like other pages" instruction this page was
   built under.
4. **`.festival-calendar`/`.festival-calendar-card`** — a centered,
   faint-gold-tint bordered card (same visual language as
   `.tour-cost-card`) offering a direct download of
   `assets/images/documents/tentative_festival_dates_2026.pdf` (opens in
   a new tab, `target="_blank" rel="noopener"`, no JS needed — a plain
   link to a static file, same no-backend approach as every `mailto:`
   link on the site) via a `.btn.btn-solid-gold` button, closing with a
   `.tour-note`-style caveat (reused as-is) that festival dates are
   lunar-calendar-based and tentative until reconfirmed at booking — same
   reasoning Tariff's copy already applies to its own SDF figures. The
   PDF itself lives in `assets/images/documents/` (a new subfolder of
   `assets/images/`, alongside `logo/`, `video/`, `image/` and
   `cloud.png`) — keep any future downloadable document there too rather
   than starting a second documents folder elsewhere.
5. **Dark CTA** — same `.dark-cta.dark-cta--photo`/`.contact-pill`
   pattern and copy structure every tour-category page ends on, its own
   background image (`Thimphu Tshechu by Bassem Nimah3.jpg`).

No sitewide "Related Tours"-style grid and no per-festival detail pages —
there's nothing to link a festival card to yet, same reasoning
`tigers-nest-pilgrimage.html`'s notes give for deferring a "Related Tours"
sidebar. If a specific festival (Paro Tshechu, say) gets its own detail
page later, give it its own tour-detail template (following
`tigers-nest-pilgrimage.html`'s or `druk-path-trek.html`'s pattern) and
repoint that one card's link the same way `cultural-tours.html`'s Tiger's
Nest card and `trekking-tours.html`'s Druk Path Trek card already do.

### luxury-tours.html

The fourth (and, for now, last) Experiences category page — Luxury Tours
was the last of the four header-dropdown items still pointing at the
`index.html#journeys`/`#journeys` placeholder; that's now gone site-wide
too (every nav dropdown, every mobile-nav-sub, the homepage's own
Explore Our Experiences card — which also had its badge corrected from
"4 Packages" to "6 Journeys" to match — and every footer's "Who We Serve"
column, which grew a fourth "Luxury Travelers" entry alongside Trekkers/
Cultural Travelers/Festival Travelers). There is no fifth Experiences
category left to build.

Same header/footer and same `.hero.hero--page.hero--center`/`.hero-mist`
hero as Cultural/Trekking/Festival Tours (own photo, `Omba Ney.jpg`) — the
brief was explicit that the hero stay identical to the other category
pages. Below it, `.tour-section`/`.tour-head` are reused as-is for the
section wrapper and centered eyebrow+heading intro (same rhythm as
Cultural/Trekking's own itinerary sections), but the grid inside is a new
pair — `.luxury-grid`/`.luxury-card`, at the very end of `style.css` under
"LUXURY TOURS PAGE" — not `.tour-grid`/`.tour-card`. The brief asked for a
card modeled on a 2-up screenshot reference (title + description near the
top, a thin rule + CTA link at the bottom, all on a tall full-bleed photo)
translated into Altara's own system, **and** asked for the card to
specifically read as a book. `.luxury-grid`/`.luxury-card` were later
explicitly asked to match `cultural-tours.html`'s `.tour-grid`/`.tour-card`
sizing exactly, so they now share those dimensions verbatim — **3-up**
grid, `20px` gap, `aspect-ratio:3/5`, the same `900px`/`580px`
breakpoints — rather than the smaller, capped-width 2-up layout an
earlier pass used; don't shrink this back down to a 2-up/narrower layout
without being asked again. What stays different from `.tour-card` is
everything that makes it read as a book:

- `.luxury-card` carries a plain soft `box-shadow` for lift, not a
  hard-edged one: an earlier version used a stepped, doubled `box-shadow`
  (offset copies in `var(--color-offwhite)` *plus* a `1px var(--color-rule)`
  line on each) meant to look like a couple of page edges peeking out from
  the corner, but the rule-colored line read as a rectangular border/frame
  around the whole card rather than "pages," so it was removed — don't
  reintroduce a `var(--color-rule)` line on this card from memory.
- `.luxury-card-frame` is the inner `overflow:hidden` layer that clips the
  photo (`.bg`) and its gradient overlay (`.luxury-card-frame::after` —
  dark at *both* the top and the bottom, clear through the middle, since
  text sits at both edges here, unlike `.tour-card`'s bottom-only
  gradient).
- `.luxury-card-spine` is a solid `var(--color-primary-dark)` strip down
  the left edge with a thin gold foil rule and an inset shadow, styled
  like a hardcover's binding. `.luxury-card-body`'s left padding starts
  clear of it, so the title reads as printed beside the spine rather than
  over it.
- `.luxury-card-ribbon` is a small gold tab hanging from the top edge,
  `clip-path`-cut to a V at the bottom like a real ribbon bookmark.
- `.luxury-card-text h3` uses `var(--font-body)` bold uppercase (18px,
  22px at 900px, matching `.tour-card-body h3`'s own breakpoint bump)
  — **not** `var(--font-display)` (League Gothic), which an earlier
  version used and which read noticeably heavier/bolder than every other
  card title site-wide (`.tour-card-body h3`, `.journey-title`,
  `.blog-card-body h3` all use font-body, never font-display, for card
  titles). Keep using font-body here even though this card is otherwise a
  bespoke component — font-display stays reserved for `<h1>`/`<h2>`
  section headings, not card titles.
- Hover just lifts the card (`translateY(-6px)`, shadow deepens slightly)
  and zooms the photo, matching every other card's hover language
  site-wide — the book cues themselves are visible at rest, not only
  revealed on hover.

Six real luxury circuits (from the same
`assets/images/image/altara-bhutan-seo-keywords-tour-packages.md` brief
used for the other category pages): Amankora Journey, Six Senses Wellness
Journey, COMO Uma Paro & Punakha, Luxury Honeymoon Package, Heritage Lodge
Tour (Taj Tashi/Zhiwa Ling), Private Helicopter & Lodge Tour. None of
these link to a dedicated detail page yet (all six point at this page's
own `#enquire` Dark CTA) — same reasoning as the five non-detail-page
cards on Cultural/Trekking Tours; give one its own tour-detail template
and repoint its card the same way if it ever needs one. No literal
photo of a specific helicopter or hotel brand's own property was used for
the Helicopter/Heritage cards (a remote lake and an aerial chorten shot
instead) — avoid a photo that reads as a specific operator's branded
aircraft or a specific hotel's actual property unless that partnership is
real and disclosed, the same caution the Partners & Affiliates marquee's
real, disclosed logos already follow.

### why-bhutan.html

A fourth Information page, added ahead of `about-bhutan.html` in the
dropdown — deliberately persuasive ("why go") rather than informational
("facts about the country"), so it doesn't duplicate About Bhutan's Quick
Facts/geography/best-time-to-visit content; the two are meant to be read
back to back, this one first. Same header/footer/hero pattern as the other
Information pages. Content, top to bottom, built entirely from reused
components (no new CSS):

1. **Intro — `.info-lead`** — why a "measures progress differently"
   country behaves the way it does on the ground (capped rooms/roads,
   opened to tourism only in 1974), teeing up the reasons below.
2. **"Gross National Happiness" — `.about`** — the four-pillars policy
   framework and what it actually changes about a trip here (no resort
   strips, no billboards, no fast food), tied explicitly to Altara's own
   homepage tagline ("travel as transformation") to connect the country's
   philosophy to the brand's.
3. **"The Last Vajrayana Kingdom" — `.about.about--reverse`** — Bhutan as
   the only country where Vajrayana Buddhism is still the living state
   religion, Taktsang (Tiger's Nest) as the emblem, with an inline
   `.faq-contact-link` (the existing small gold-underline in-paragraph
   link style, not the block-level `.about-link`) pointing to
   `festival-tours.html` for anyone who wants to time a trip around a
   Tshechu.
4. **"What Else Sets It Apart" — reuses `.values-section`/`.value-grid`/
   `.value-card` verbatim**, About page's own 3-card component, for three
   more reasons that don't need a full photo treatment: untouched culture,
   the SDF-funded high-value/low-impact tourism model (another
   `.faq-contact-link` to `tariff.html`), and carbon-negative status.
5. **A stat strip — reuses `.fact-section--tight`/`.season-grid`/
   `.bhutan-fact-card`** (About Bhutan's own "Best Time To Visit" number
   treatment) for four quick figures: 1974 (first opened to tourism), 70%+
   (constitutional forest cover), 20 (dzongkhags), 3,120m (Taktsang's
   elevation).
6. **Dark CTA** — same pattern, own photo
   (`Punakha Dzongkhag Header.jpg`).

Content themes (GNH, living Buddhism, untouched culture, high-value/low-impact
tourism, carbon-negative status) were drawn from the shape of Breathe
Bhutan's own "Why Travel Bhutan" page as a starting brief, then written
fresh in Altara's own voice and cross-linked to Altara's own pages — not
copied. The header/footer "Information" dropdown, every page's matching
mobile-nav-sub, and every footer's "Company" column all got a "Why
Bhutan?" entry (placed first, before "About Bhutan") in the same pass —
keep all three in sync if this page is ever renamed or removed, the same
convention every other new page in this project has followed.

### Information pages (about-bhutan.html, why-bhutan.html, tariff.html, faq.html)

The header's "Information" dropdown (see above) links to these four (in
dropdown order: Why Bhutan?, About Bhutan, Tariff, FAQ). Same
header/footer/hero/`.dark-cta.dark-cta--photo` pattern as the tour pages,
but the content section underneath is page-specific, built from a small
set of reusable "INFORMATION PAGES" components added to `style.css` (all
kept in one block near the end of the file):

- `.info-lead` — the centered intro paragraph directly under the hero on
  all three pages (eyebrow + h2 + one lead paragraph, `max-width:760px`).
- `.fact-grid`/`.fact-card`/`.fact-value`/`.fact-label` — About Bhutan's
  Quick Facts grid (4-up, bordered card + gold hover, same visual language
  as `.value-card`). `.fact-section` is the section wrapper;
  `.fact-section--tight` is a modifier for a second one stacked directly
  under another section on the same page (About Bhutan's "Best Time To
  Visit" reuses this wrapper but with `.season-grid` inside instead of
  `.fact-grid` — that one deliberately reuses `.bhutan-fact-card`'s own
  number/label styling from the homepage's Explore Bhutan map sidebar
  rather than inventing a third card style).
- Tariff's SDF pricing was redesigned to be simpler — a first pass with
  three bordered `.tariff-card`s, a separate regional-rate caption, a
  bordered-rule two-column checklist and its own bordered visa/payment note
  (four distinct boxed treatments stacked on one page) was flagged as too
  busy and replaced. `.tariff-stat-section`/`.tariff-stat`/
  `.tariff-stat-figure`/`.tariff-stat-secondary` is what's there now: one
  large `$100` display-type figure (the "impactful" part) with a short
  label, then three smaller secondary figures (child rate, under-6 exempt,
  regional rate) in a plain row underneath a single rule — no cards, no
  boxes. The Included/Not Included list below it reuses
  `.tour-highlights`/`.tour-excludes` (druk-path-trek.html's border-free
  check/× list, inside a new plain `.tariff-simple-grid` two-column
  wrapper) instead of the old bordered-row `.tariff-includes` component,
  and the visa/payment paragraph reuses `.tour-note`'s plain gold-left-border
  caveat instead of its own bordered block. Don't reintroduce
  `.tariff-grid`/`.tariff-card`/`.tariff-price`/`.tariff-includes`/
  `.tariff-note`/`.tariff-regional-note` from memory — this fully replaces
  them, and the reasoning (fewer boxed treatments per page, reuse the
  simpler existing list components) applies to any future redesign request
  on this page too.
- `.faq-category`/`.faq-category-head` — wraps each themed group of
  questions on faq.html (Planning & Booking, Visa & Entry, Health &
  Practical) around an otherwise-unmodified `.faq-columns`/`.faq-item`
  pair per group, so the existing FAQ accordion CSS/JS needed zero changes
  — it already closes every other open `.faq-item` on the page when one
  opens, category boundaries included, which is intended (single accordion
  across the whole page, not one per category).

Content specifics: About Bhutan's Quick Facts (capital, population,
currency, etc.) and Tariff's SDF figures ($100/night adult, $50/night ages
6–12, free under 6, since Bhutan dropped its old fixed daily package rate
in September 2022) are real published figures as of this writing, not
placeholders — but government rates change, so Tariff's copy explicitly
tells readers rates are confirmed again at booking rather than stating them
as fixed. FAQ's three categories combine the homepage's existing 6-question
teaser (`index.html#faq`, left as-is) with new questions — the homepage's
"View All FAQs" button (previously a `href="#"` placeholder) now points to
`faq.html`. Every "Company" footer column site-wide got these three pages
(now four, with `why-bhutan.html`) added under "Our Story" — keep that
list in sync with the Information dropdown if any of the four is ever
renamed or removed.

### blog.html

The Journal's own dedicated page — the header/footer/every-page "Blog" link
previously pointed at `index.html#blog`/`#blog` (a same-page anchor to the
homepage's `.blog` teaser slider); that's now `blog.html` on **every**
page, including index.html's own header/mobile-nav and every footer's
"Journal" link (index.html's own footer previously left "Journal" as a bare
`href="#"` placeholder — that's fixed too). This is the same
"give it a real destination page, repoint every nav reference" pattern
`enquire.html` already established (see below) — apply it again if another
same-page anchor ever gets its own page.

Same header/footer/hero pattern as the other inner pages (own photo,
`paro attractions header.jpg`). Content is deliberately minimal — this
page's whole job is to give the homepage's teaser slider content a
permanent, browsable home, not to add a new content system:

1. **`.tour-head` (reused, with its `p` variant)** — "Stories From The
   Journal" + a short lead paragraph, same component
   `cultural-tours.html` uses for its own heading+description.
2. **`.blog-grid`/`.blog-card`** — the **same six posts** as the
   homepage's `.blog` teaser slider (same images, dates, titles — copy
   stays word-for-word in sync between the two; neither card shows a
   byline or description anymore, see the `index.html` Blog section notes
   above), just laid out as a plain static 3-up grid (`.blog-grid`, new CSS under
   "BLOG PAGE" at the end of `style.css`) instead of a scroll-snap slider.
   `.blog-card` itself needed zero changes — its `flex` property (written
   for the homepage's flex-based `.blog-track`) is simply inert inside a
   CSS Grid parent, so the exact same class works in both contexts
   unmodified. Five of the six cards still link to `#` (no article page
   yet) — the first, **"Inside Tiger's Nest: What To Know Before You
   Climb,"** links to its own dedicated page, `inside-tigers-nest.html`
   (see below), same exception-card pattern `cultural-tours.html`'s
   Tiger's Nest Pilgrimage tour card and `trekking-tours.html`'s Druk
   Path Trek card already follow. Give another post its own page and
   repoint its card the same way.
3. **Dark CTA** — same pattern, own photo (`bumthang view1.jpg`).

The homepage's own `.blog` teaser section is untouched (still a slider,
still six cards, first one now linking to `inside-tigers-nest.html` like
its `blog.html` counterpart) — this page doesn't replace it, it just gives
"Blog" in the nav somewhere real to go. Don't add a
"View All Stories" link back onto the homepage teaser to point here
without being asked — that button was deliberately removed earlier (see
the `index.html` Blog section notes above) for a different reason (visual
clutter in the slider's header row), unrelated to this page existing now.

### inside-tigers-nest.html

The first individual article page — for the Journal's "Inside Tiger's
Nest: What To Know Before You Climb" post (both `index.html`'s teaser
slider and `blog.html`'s grid link their first card here now). Modeled on
a Breathe Bhutan blog-post page the user shared as a reference,
**restructured to match, reskinned entirely in Altara's own system** —
same reasoning every other reference-modeled page on this site follows
(`tigers-nest-pilgrimage.html`, `druk-path-trek.html`, `why-bhutan.html`):
the reference's actual layout shape carried over, not its fonts/colors.
Same header/footer/hero as every other inner page — `.hero.hero--page.hero--center`
with `.hero-mist`, title overlaid on the photo (`LLL05202.jpg`, the same
photo the teaser card uses — the same "one photo for both the card and
its own detail page" pattern `taktshang.jpg` already follows for the
Tiger's Nest Pilgrimage tour). An earlier pass gave this page its own
plain-image-then-title-below hero instead (`.article-hero`/
`.article-header`), matching the Breathe Bhutan reference's own hero
layout more literally, but that was reverted in favor of staying
consistent with every other inner page's hero — don't reintroduce
`.article-hero`/`.article-header` from memory. `.article-meta` (the small
gold byline/date line) now sits at the very top of `.article-body`
instead, directly above the intro paragraph, since the title itself lives
in the hero like it does everywhere else.

1. **`.article-list`** — the numbered-tips list, translating the
   reference's own numbered-experience list into this page's five tips
   (same content as the previous H2-per-tip version, just restructured):
   a plain `<ol>` with `.article-list-number` (a large gold "01"/"02"/...)
   beside `.article-list-content` (`<h3>` + `<p>`), separated by rule
   lines rather than boxed cards — matching the site's existing rule-line
   list language (`.faq-item`) rather than inventing a card style. The
   "What To Bring" item (05) holds `.tour-highlights.tour-highlights--stack`
   verbatim (the same single-column checklist modifier
   `druk-path-trek.html`'s cost breakdown introduced) as its content
   instead of a paragraph.
2. **`.article-share`** — the translated version of the reference's "Love
   This? Share It" prompt: plain Facebook/WhatsApp/email share-URL links
   (`facebook.com/sharer/sharer.php?u=...`, `wa.me/?text=...`,
   `mailto:?subject=...&body=...`), no JS, same no-backend approach every
   other link on the site already uses.
3. **`.article-layout`/`.article-sidebar`** — the translated version of
   the reference's "Related Posts" sidebar: `.article-layout` splits
   `.article-section .container` into a `1fr`/`340px` grid (main article
   left, sidebar right, `max-width:1100px`), replacing an earlier pass
   that instead reused `.blog-section`/`.blog-grid` as a full-width
   "More From The Journal" grid *below* the article — don't bring that
   version back without being asked; the sidebar is what's there now.
   `.article-sidebar` is a bordered white card (`position:sticky` on
   desktop, same "card floating on off-white" contrast every other card
   on the site uses) holding `.article-sidebar-list` — 4 of the other 5
   Journal posts (Punakha Dzong, Tshechu Festivals, Bumthang Valley Diary,
   First Time In Bhutan; only "What To Pack For A Bhutan Trek" is left
   out), each `.article-sidebar-item` a small thumbnail + title + date,
   still linking to `#` since none of those has its own article page
   yet — and a `.btn.btn-outline-green` "View All Journal Posts" linking
   to `blog.html` underneath the list, translating the reference's pill
   "VIEW ALL BLOGS" button into the site's own sharp-corner button
   language rather than copying its rounded shape. Below `900px` the grid
   collapses to one column (sidebar drops below the article, sticky
   positioning turns off) same as every other two-column layout on the
   site collapsing at that breakpoint.
4. **Dark CTA** — same pattern, own photo (`Lhuentse dzong.jpg`), then the
   standard footer.

New CSS under "BLOG ARTICLE PAGE" at the end of `style.css` covers
`.article-layout`/`.article-list`/`.article-share`/`.article-sidebar` — the
reading column stays a narrower `720px` (`.article-body`,
`.tour-content-inner`'s `1100px` column reads too wide for a plain
editorial article). If a second article page is ever built, copy this
page's structure (not `tigers-nest-pilgrimage.html`'s tour-detail
template) — the two are unrelated despite the similar name.

### enquire.html

The dedicated contact page — same header/footer/hero pattern as the other
inner pages (hero image `Wangdue.jpg`). Its content is three stacked
sections (new CSS at the very end of `style.css` under "ENQUIRE PAGE"),
deliberately not a single generic two-box "form | details" layout (an
earlier version was exactly that plain symmetric layout and was reworked
to feel less like a generic template contact page):

1. **`.enquire-split`** — an editorial split intro: `.enquire-visual` is a
   tall photo (`Omba Ney.jpg`) with a dark bottom gradient and a small
   `.enquire-planner-card` anchored over it (reuses `.team-avatar`'s exact
   gold-initial-circle styling from about.html's team section, featuring
   Sonam Tenzin — "Guest Relations & Planning" there, already written as
   "usually the first voice you'll hear," which is why she's the one
   featured here). `.enquire-form-card` sits beside it and **overlaps the
   photo** on desktop via a negative `margin-left` + `box-shadow` — the
   same offset-card-over-photo idea `.about-media-accent` already uses on
   `.about-media-main`, just a form card over a photo instead of a second
   photo. Collapses to a plain stacked, non-overlapping layout under
   900px. The form itself (`.enquire-form`/`.form-row`/`.form-field`) uses
   plain bottom-border inputs (no boxes, matching `.faq-item`/
   `.tariff-includes li`'s rule-line look) but each field has a sibling
   `.field-underline` span that animates a gold underline in on focus
   (width 0 → 100%) instead of a flat border-color swap. Fields: purpose
   of enquiry, name, email, phone, home country, preferred dates, number
   of travelers, message. No backend —
   `action="mailto:hello@altara.travel" method="post" enctype="text/plain"`
   opens the visitor's own email client with the fields prefilled, the
   same no-backend pattern the site's `mailto:` links already use
   elsewhere, just applied to a `<form>`.
2. **`.contact-band`** — "Reach Us Directly": a full-width 4-up
   `.contact-icon-grid` of `.contact-icon-card`s (icon circle + label +
   value), not a boxed sidebar list — WhatsApp, email, office (Thimphu,
   Bhutan — no street address exists anywhere else on the site, so none
   was invented here), office hours.
3. **`.enquire-map-section`** — a full-width, cinematic
   (`aspect-ratio:21/8` on desktop) embedded Google Maps iframe
   (`google.com/maps?q=Thimphu,Bhutan&output=embed`, no API key needed),
   pinned at city level since there's no real street address to give it.
   This is a deliberate, explicitly-approved exception to the "no external
   dependencies besides Google Fonts" rule below — it's the one place on
   the site that calls out to a third-party service at runtime. Don't add
   further external embeds elsewhere without the same kind of explicit
   sign-off.

No Dark CTA section on this page — the whole page already is the contact
destination, so a second "Begin Your Journey" WhatsApp/email CTA
immediately after would be redundant; it goes straight from the map to the
footer.

The header/footer "Enquire Now" link and the footer's "Contact" link now
point to `enquire.html` on **every** page (previously `#enquire`, a
same-page anchor jump to that page's own Dark CTA section) — keep those in
sync if this page is ever renamed or removed. Every other `#enquire`
same-page anchor on the site — the Dark CTA's own WhatsApp/email
`.contact-pill`s, every `.tour-card`/`.activity-card`/`.journey-card`
"interested in this?" link, About's "Speak To A Planner" button, FAQ's
inline "contact us" links — was deliberately left pointing at the local
`#enquire` Dark CTA section on its own page; those are fast same-page
WhatsApp/email actions, a different concern from the header's dedicated
"go fill out the enquiry form" destination, so don't redirect them to
`enquire.html` without being asked. `script.js`'s mobile-menu IIFE needed
no changes for this page (see the Header bullet under index.html above for
the `.mobile-nav-toggle` generalization that already supports it).

### `.hero--page` content pattern

Every inner-page hero shows **only** the `<h1>` page title inside
`.hero-content` — no eyebrow, no `.hero-sub` description, no CTA buttons
(those belong on the homepage hero only). An earlier version put an
eyebrow + description underneath the title; that was deliberately
simplified down to just the title. Follow this for any new `.hero--page`
hero rather than reintroducing the eyebrow/description.

The `<h1>` itself is the **plain page title** — the same short label used
in the nav dropdown/footer for that page (`about.html` → "About",
`cultural-tours.html` → "Cultural Tours", `trekking-tours.html` →
"Trekking Tours", `festival-tours.html` → "Festival Tours",
`luxury-tours.html` → "Luxury Tours", `why-bhutan.html` → "Why Bhutan",
`about-bhutan.html` → "About Bhutan", `tariff.html` → "Tariff",
`faq.html` → "FAQ", `enquire.html` → "Enquire Now", `blog.html` →
"Blog") — not evocative marketing copy. An earlier version used longer,
two-line taglines instead (e.g. cultural-tours.html's own
"Journeys Into Bhutan's Living Heritage", trekking-tours.html's
"Trails Into Bhutan's Untouched High Country"); those were replaced with
the plain title site-wide, so don't reintroduce a tagline-style `<h1>`
on any of these pages without being asked. This doesn't apply to
tour-detail pages (`tigers-nest-pilgrimage.html`, `druk-path-trek.html`,
which use `.hero--tour` and already show their own specific tour name)
or article pages (`inside-tigers-nest.html`, which already shows its own
specific post title) — both of those were already "the page's own title"
before this convention was written down, so nothing changed there.

`.hero--page` alone is bottom-**left**; most inner pages add
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
duration, add/adjust the desktop override instead. Used on the inner-page
heroes only — `about.html`, `cultural-tours.html`, `trekking-tours.html` —
**not** the homepage hero (`index.html`), which was tried and then removed since the video
background there didn't need it; don't reintroduce it on `index.html` without
being asked. `prefers-reduced-motion` freezes both layers in place rather
than removing them. Copy this same 3-element structure (`.hero-mist` + two
`.hero-mist-layer` children) if another hero needs the same drifting-mist
look.

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

- Header scroll-state + logo swap (throttled via `requestAnimationFrame`) —
  also now hides the whole header on scroll-down and slides it back in on
  scroll-up (`.header-hidden`, `transform:translateY(-100%)` in
  `style.css`), as a second class toggled independently of `.scrolled` off
  the same scroll tick, not a replacement for it. Direction comes from the
  raw delta between this tick's `scrollY` and the last one (a `±6px`
  dead-band absorbs momentum/trackpad jitter so it doesn't flicker), the
  header always stays visible inside the top `120px` (`REVEAL_ZONE`) so it
  never hides right at the top of a page, and it's forced visible whenever
  `#mobileNav` has `.is-open` so the panel's own close button (which lives
  inside the header) can't slide away mid-interaction. `update()` still
  runs once on load in addition to every scroll tick, for the same
  bfcache/anchor-landing reason noted below.
- `initCardSlider(trackId, prevId, nextId, cardClass)` — generic scroll-snap
  + prev/next-button slider, called for the Journeys, Activities, and Blog
  sliders on the homepage, and the tour-gallery-hero slider on
  `tigers-nest-pilgrimage.html` (four calls total so far). Add any future
  horizontal card slider as a new call to this same function rather than
  writing a new one.
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
  change values there, not at each usage site. A `--color-cream` token was
  removed early on (section backgrounds were plain white then, border-based
  separation only) and stayed out for a long time — that's since been
  superseded: `--color-offwhite` (`#FAFAF8`) now exists and is deliberately
  what `body` and every section wrapper use for their background (`.about`,
  `.values-section`, `.team-section`, `.tour-section`, `.activities-section`,
  `.testimonials`, `.explore-bhutan`, `.why-us`, `.blog`, `.faq`,
  `header.scrolled`, `.info-lead`, `.fact-section`, `.enquire-split`,
  `.contact-band`, `.enquire-map-section`, `.partners`, `.itinerary`,
  `.tour-map-section`, `.tour-enquire`, `.tour-facts-section`,
  `.tour-content`, `.tour-cost`, `.tour-hero-info` — i.e. every section
  background site-wide). `--color-white` (`#FFFFFF`) stays pure white and
  is still what every **card/surface floating on that background** uses
  (`.value-card`, `.bhutan-fact-card`, `.fact-card`, `.tariff-card`,
  `.enquire-form-card`, `.nav-dropdown-panel`), plus text-on-dark and
  button/icon states (`.btn-outline-white:hover`, `.journey-btn:hover`,
  `.tour-gallery-play:hover`, `#tourGalleryPrev`/`#tourGalleryNext`,
  `.hamburger-btn span`) — the barely-there contrast between the two
  (off-white page, pure-white card) is the point, so a card reads as
  faintly lifted off the page rather than flush with it. When adding a new
  full-width section, give it `background:var(--color-offwhite)`; when
  adding a new bordered card/popover/button state, use
  `background:var(--color-white)`.
- Reference real files only from `assets/images/image/` (not `assets/images/`
  directly — that only holds `logo/`, `video/`, and `cloud.png` now).
  Filenames are wildly inconsistent (mixed case, spaces, camera-default names
  like `DSC00561.jpg`/`LLL03236.jpg`, `.JPG`/`.jpeg` mixed in) — copy them
  byte-exact, never invent or guess a filename; `ls` the folder or check the
  existing `<img>`/`background-image` references first. Every image already
  in use across the four pages is unique to its one slot (see the `assets/`
  tree comment above) — when swapping or adding an image, keep it that way
  rather than reusing a file that's already placed somewhere else on the
  site.
- Two logo variants exist for a reason: `logo white.png` on dark/transparent
  surfaces, `logo.png` on light/white surfaces.
- When adding a new page, copy the header/footer markup from an existing
  page rather than reinventing it, and rewrite homepage-only anchors as
  `index.html#section`.
