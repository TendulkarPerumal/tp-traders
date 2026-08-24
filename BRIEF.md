# TP Traders - website brief

Build a static marketing site for TP Traders, a cutting-waste procurement business in
Tirupur, Tamil Nadu. We buy banian and hosiery cutting clips from garment units and
stock them in our own godown.

## Who opens this and why

A garment unit owner or a floor supervisor, on an Android phone, from a WhatsApp link,
for about twenty seconds. He has waste piling up and wants to know two things: will this
fellow take it, and will he pay a fair rate.

The site has exactly one job: get him to press call.

A second reader matters less often but matters more when he shows up - a brand auditor,
a Reverse Resources onboarding contact, or a TEA member checking we are a real registered
operation. He must be able to read the whole page in English and find GSTIN, address and
traceability language without hunting.

## Stack

* Plain static site. `index.html`, `styles.css`, minimal vanilla JS. No React, no build
  step, no bundler, no npm dependencies.
* Deployable to Cloudflare Pages or GitHub Pages by pushing to `main`.
* Total page weight under 150 KB excluding photos. No web font files if a system stack
  will do; if using Google Fonts, load only the weights actually used.
* Must render correctly with JavaScript disabled.

Reason: this page changes maybe four times a year. A build pipeline is a liability, not
an asset, and I don't want to be fixing a broken toolchain in 2028 to correct a phone
number.

## Language

Bilingual, but not a translated duplicate. Tamil carries the persuasion - the selling
lines, the material names, the promises. English carries the structure - section
headings, the grade table's column labels, the legal and address block. A supervisor
should be able to skim only the Tamil and understand the offer.

Do not use a language toggle. One page, both languages, interleaved.

Leave every Tamil string in a clearly marked block near the top of the HTML or in a
comment index, because a native speaker will proofread it before launch.

## Page structure

1. Hero. Business name, one Tamil line saying what we buy, one English subline. A large
   tap-to-call button and a WhatsApp button. Nothing else. No carousel, no hero video, no
   stock photography of a recycling arrow.
2. The grade table - this is the centrepiece. Cutting waste is sorted by colour and
   composition, and that sort is how the whole trade thinks and prices. Build a real
   table of grades: white banian, mixed colour, dark, lycra/spandex blend, fleece, rib,
   collar/cuff, and sweeping/floor waste. Each row: Tamil name, English name, a short
   note on what qualifies, and a rate column left as a `TODO` placeholder.
   Make this visually the most memorable thing on the page. It is the reason a supervisor
   forwards the link to his owner. Do not render it as a generic striped table - treat it
   as a sorting chart. It must stay readable on a 360 px screen; a horizontally scrolling
   table is acceptable only if the first column stays pinned.
3. How it works. Three steps, actually sequential, so numbering is justified: you call ->
   we weigh and quote at your unit -> we load and pay. One line each. State that we bring
   our own vehicle and our own loading men.
4. Why sell to us. Four short claims maximum, in Tamil, each one concrete: better than
   market rate, payment the same day the load leaves, own transport, we take small
   quantities too. No adjectives like "trusted" or "leading".
5. Traceability / compliance strip. Short English paragraph for the auditor: we maintain
   weighment records and DC documentation per pickup, waste is segregated by composition
   at the godown, and we can supply source-unit records on request. Keep it factual and
   modest.
   Do not invent certifications, memberships, tonnage figures, client names, or years in
   business. If a fact is unknown, write `TODO:` and move on.
6. Contact. Godown address, map link, phone, WhatsApp, GSTIN, working hours. All as
   `TODO` placeholders - I will fill them in.

Footer: name, Tirupur, year, nothing else.

## Design direction

Carry the identity from the visiting card so the two match:

* `--ink: #131F36` (indigo - dye house)
* `--deep: #0C1526`
* `--chalk: #F6F4EE`
* `--mark: #E8A32B` (turmeric - the accent, use it sparingly and on the call-to-action)
* `--mute: #8C99B2`

Type: a condensed grotesque for headings and the phone number, a plain grotesque for
body, `Noto Sans Tamil` for all Tamil. Tamil script needs more line-height than Latin -
set it separately, don't inherit.

Set the grade table's rate column in tabular figures.

The design should look like it came from the material's own world - bales, weighbridge
tickets, sorted colour lots - not from a SaaS template. Be disciplined everywhere except
the grade table, and spend the boldness there.

## Behaviour

* Sticky call bar on mobile, fixed to the bottom, always reachable. It must not cover the
  last section's content - pad the footer accordingly.
* `tel:` and `https://wa.me/<number>?text=` links, with a prefilled Tamil WhatsApp message
  asking for a rate.
* WhatsApp link previews are how this site will actually spread, so Open Graph tags and a
  1200x630 OG image are required, not optional. Generate the OG image as an SVG in the
  repo and note that it needs converting to PNG.
* `LocalBusiness` JSON-LD with `TODO` values for address and phone.
* `<title>` and meta description targeting: cutting waste buyer Tirupur, banian waste,
  hosiery cutting clips, and the Tamil equivalents.
* Accessible: real semantic headings in order, visible keyboard focus, contrast ratios met
  on the turmeric accent.
* `prefers-reduced-motion` respected. Honestly, this page needs almost no motion.

## Repo hygiene

Create `README.md` with local preview and deploy instructions, a `.gitignore`, and a
single `assets/` folder. Commit in logical chunks with plain commit messages. No emoji in
commits or code comments.
