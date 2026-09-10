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
├── about-bhutan.html              # Information page — country primer, quick
│                                     facts, best time to visit
├── tariff.html                    # Information page — Sustainable Development
│                                     Fee pricing, what's included/not, visa
├── faq.html                       # Information page — full categorized FAQ
│                                     accordion (Planning, Visa & Entry, Health
│                                     & Practical)
├── enquire.html                   # Enquire Now page — split contact form,
│                                     "Reach Us Directly" icon row, embedded map
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
   hover/focus dropdown of exactly 4 items — Cultural Tours and Trekking
   Tours link to their own real pages (`cultural-tours.html`,
   `trekking-tours.html`), Festival Tours and Luxury Tours are still
   placeholder `#journeys`/`index.html#journeys` links — deliberately no
   "Activities" item, since Activities is its own full section further down
   the page, not a tour-package category. "Information" is the same
   `.nav-item`/`.nav-dropdown` pattern too: the trigger itself still links to
   `#why`/`index.html#why` (the homepage's Why Altara section, exactly like
   "Experiences" still links to `#journeys`), and its dropdown holds the
   three Information pages — About Bhutan, Tariff, FAQ (see below). On
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
10. **Partners & Affiliates** (`.partners`) — an infinite CSS marquee of 6
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
11. **FAQ** — accordion
12. **Dark CTA** (`id="enquire"`) — `.dark-cta.dark-cta--photo`, same pattern
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
13. **Footer** — 4-column dark footer

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
Festival Tours and Luxury Tours still point at `index.html#journeys`/`#journeys`
as a placeholder until those pages exist too. ("Activities" is deliberately
not in this dropdown — it's its own homepage section, not a tour-package
category.)

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
every page. Content, top to bottom (new CSS at the end of `style.css`
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
   bespoke slider. `.tour-hero-info` is a plain white section right
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
   same 820px reading column the itinerary/enquiry card use: "Tour
   Overview" (one paragraph) and "Tour Highlights" (a 2-col `.tour-highlights`
   checklist, gold checkmarks — same checkmark glyph Tariff's
   `.tariff-includes` uses).
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

No "Related Tours" sidebar yet (the reference page has one) — there's only
one tour-detail page on the site so far, so there's nothing real to link
to; add one once a second tour-detail page exists rather than linking
sideways to pages that don't exist. No sitewide Dark CTA section on this
page either — the tour-specific enquiry card already is the page's CTA,
same reasoning as `enquire.html` dropping its own Dark CTA.
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
if this page is ever renamed or removed. If Festival Tours or
Luxury Tours get their own page next, copy this page (or `cultural-tours.html`)
the same way rather than starting from scratch.

### Information pages (about-bhutan.html, tariff.html, faq.html)

The header's "Information" dropdown (see above) links to these three. Same
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
- `.tariff-grid`/`.tariff-card`/`.tariff-price` — Tariff's three SDF
  pricing cards; `.tariff-card--featured` puts a gold border on the
  standard-adult-rate card. `.tariff-includes` is the two-column
  included/not-included checklist below it (`.tariff-includes--exclude` on
  the second `<ul>` grays out its check icons). `.tariff-note` is the
  visa/payment paragraph block underneath, styled like `.faq-note`.
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
added under "Our Story" — keep that list in sync with the Information
dropdown if any of the three is ever renamed or removed.

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

Every inner-page hero (`about.html`, `cultural-tours.html`, `trekking-tours.html`) shows **only**
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
