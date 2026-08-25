# TP Traders

Static one-page site for TP Traders, Tirupur - buyer of banian and hosiery cutting waste.

Plain HTML and CSS. No build step, no bundler, no npm dependencies. The page renders
correctly with JavaScript disabled, and there is no JavaScript on it at all.

```
index.html      the whole page, including the Tamil string index near the top
styles.css      the whole stylesheet
assets/         favicon and the Open Graph image master
BRIEF.md        the original project brief
```

## Local preview

Open `index.html` in a browser. That is enough for everything except checking the
Open Graph tags.

To serve it over http instead:

```bash
npx serve .
```

Or with a Python that is actually installed:

```bash
python -m http.server 8080
```

## Before launch

Everything that needs a real value is marked `TODO` in the source. Find them all with:

```bash
grep -rn "TODO" index.html assets/
```

The list, in the order it matters:

1. **Phone number.** Appears in the hero button, the sticky call bar, the contact block
   and the JSON-LD. Both `tel:` links and both `wa.me/` links.
2. **WhatsApp number.** `https://wa.me/<number>` takes the number in international format
   with no plus and no spaces, for example `919876543210`.
3. **Godown address, map link, GSTIN, working hours.** In the contact block and the
   JSON-LD `PostalAddress`.
4. **Site URL.** `canonical`, `og:url`, `og:image` and the JSON-LD `url`.
5. **Rates.** Eight `TODO` placeholders in the grade table.
6. **Record retention period** in the traceability section.

The JSON-LD will not validate while `latitude`, `longitude` and `openingHours` hold the
string `TODO`. Either fill them in or delete those three keys.

## The Tamil

Every Tamil string is listed in one commented index at the top of `index.html`, with a
literal English gloss for each. Each string carries a `data-ta-id` in the markup, so a
proofreader can name a string and it can be found with:

```bash
grep -n "ta.grade.g4" index.html
```

The register is deliberately Tirupur trade Tamil - English loanwords in Tamil script
(`கட்டிங் வேஸ்ட்`, `ரேட்`, `லோடு`) are intentional and should not be replaced with
purist Tamil. Get a native speaker to read it before launch.

`ta.wa.text` is percent-encoded inside the two `wa.me` links. If that message is edited,
re-encode it and change both links:

```bash
node -e "console.log(encodeURIComponent('your new message'))"
```

## The Open Graph image

`assets/og-image.svg` is the editable master. It must be converted to a 1200x630 PNG
before launch, because WhatsApp does not render SVG link previews - and WhatsApp is how
this link will spread.

```bash
npx svgexport assets/og-image.svg assets/og-image.png 1200:630
```

Then point `og:image` and the JSON-LD `image` at the absolute URL of the PNG. Relative
paths do not work for link previews; scrapers need the full `https://` URL.

The machine doing the conversion needs Archivo Narrow and a Tamil font installed. If the
Tamil comes out as boxes, install Noto Sans Tamil and run it again.

## Fonts

Archivo Narrow is loaded from Google Fonts, weights 600 and 700 only, and is the only
network font on the page.

Tamil uses the system Noto Sans Tamil, which is already present on Android, and falls
back to Nirmala UI on Windows and Tamil Sangam MN on Apple devices. No Tamil font file
is downloaded. If the Tamil rendering on desktop turns out to matter, add
`Noto+Sans+Tamil:wght@400;600` to the existing Google Fonts link - that is the only
change needed, the font stack already names it first.

## Deploy

Both hosts serve this repository as-is. There is no build command and no output
directory.

### Cloudflare Pages

1. Create a project and connect this repository.
2. Framework preset: **None**. Build command: leave empty. Output directory: `/`.
3. Production branch: `main`. Every push to `main` deploys.

### GitHub Pages

1. Settings, Pages, Source: **Deploy from a branch**.
2. Branch `main`, folder `/ (root)`.

If the site is served from a subpath such as `username.github.io/tp-traders/`, the
relative asset paths still work, but `og:image` must be the full absolute URL.

## Accessibility notes

Contrast was checked against the card palette rather than assumed:

| Combination | Ratio | Use |
| --- | --- | --- |
| Turmeric `#E8A32B` on ink `#131F36` | 7.6:1 | call button, accents on dark |
| Turmeric on deep `#0C1526` | 8.4:1 | hero rule |
| Turmeric on chalk `#F6F4EE` | 2.0:1 | fails, never used for text on light |
| Darkened turmeric `#9A6410` on chalk | 4.5:1 | small turmeric text on light |
| Mute `#8C99B2` on deep | 6.4:1 | secondary text on dark |
| Mute on chalk | 2.6:1 | fails, used only for hairlines |

Two tokens were derived from the card palette because it has no colour that passes for
small text on chalk: `--ink-2` for secondary text and `--mark-deep` for turmeric text on
light surfaces. Both are documented at the top of `styles.css`.

The grade table becomes a stack of cards below 720px rather than scrolling sideways.
Because `display: block` on table elements drops the implicit table semantics, the markup
carries explicit `role="table"`, `role="row"`, `role="cell"` and header roles, and the
column headers stay in the accessibility tree while visually hidden.
