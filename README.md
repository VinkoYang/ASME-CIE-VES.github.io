# VAR-E — Virtual and Augmented Reality Environments

Website for the **VAR-E** symposium, part of the ASME IDETC-CIE conference.

**Live site:** https://asme-cie-ves.github.io/ → redirects to `/VARE/`

This is a plain static site served by GitHub Pages. There is no build step, no
package manager, and no framework. Edit the HTML/CSS and the change is live on
the next push to `main`.

---

## Repository layout

| Path | Purpose |
|------|---------|
| `index.html` | Root redirect; sends visitors to `./VARE/` |
| `VARE/` | **Current edition.** The only directory that should be edited |
| `VARE2025-2026/` | Archived 2025–2026 edition — frozen |
| `VES2024/` | Archived 2024 edition — frozen |

Inside an edition directory:

| Path | Purpose |
|------|---------|
| `index.html` | Home page, incl. the countdown timer |
| `cfp.html` | Call for Papers and the deadline table |
| `organizer.html` | Organizing committee |
| `special.html` | Special sessions |
| `assets/css/templatemo-lugx-gaming.css` | Main stylesheet — nearly all visual changes go here |
| `assets/css/` (others) | Vendor styles: FontAwesome, Owl Carousel, Animate.css, FlexSlider. Do not hand-edit |
| `assets/js/custom.js` | Site-specific script |
| `assets/js/` (others) | Plugins: counter, Owl Carousel, Isotope. Do not hand-edit |
| `vendor/bootstrap`, `vendor/jquery` | Third-party libraries. Do not hand-edit |
| `bu/` | Unused pages inherited from the original template. Not linked from the site |

### External dependencies

Bootstrap, jQuery and the FontAwesome webfonts are vendored into the repository.
Two stylesheets are pulled from CDNs at page load: Google Fonts (Poppins) and
`unpkg.com/swiper@7`.

Some organizer headshots on `organizer.html` are **hotlinked from external
sites** rather than stored in `assets/images/` — currently `cdn.sanity.io`,
`lamar.edu` and `mecc.polimi.it`. These break silently if the remote file is
moved or removed, so check that the photos still render after any staff page
changes upstream. Downloading them into `assets/images/` would make the site
self-contained.

---

## Local preview

A file server is required — opening the HTML directly with `file://` breaks
relative paths.

```sh
python -m http.server 8000
```

Then open http://localhost:8000/VARE/

---

## Starting a new edition

The site keeps one live edition plus frozen archives. To roll over:

1. **Archive the outgoing edition.** Copy `VARE/` to `VARE<years>/`
   (e.g. `VARE2025-2026/`) and leave the copy untouched from then on.
2. **Edit `VARE/` in place.** It stays the live edition, so `index.html` and
   every internal link keep working with no changes.
3. **Update the year and dates.** Search the whole directory first:

   ```sh
   grep -rn "2026" VARE/
   ```

Places that carry edition-specific content:

- **`cfp.html`** — the deadline table. Six entries: full paper submission,
  review complete, acceptance notification, copyright form, final paper
  submission, and the conference dates.
- **`index.html`** — the hero heading and the conference dates.
- **`index.html`, countdown timer** — a hard-coded date in a `<script>` block
  near the bottom of the file:

  ```js
  const finaleDate = new Date("August 23, 2026 09:00:00").getTime();
  ```

  This one is easy to miss because it is JavaScript, not visible page text. If
  it is not updated the countdown shows a negative or expired value.
- **`organizer.html`** — committee names and affiliations.
- **`special.html`** — special session titles and organizers.

### Editing gotchas

- **There is no template inheritance.** The header and footer are copy-pasted
  into all four pages. A navigation change means editing four files.
- **Archived editions are not linked from anywhere.** They remain reachable by
  direct URL (e.g. `/VES2024/`) but no page links to them.
- **Page titles are inconsistent.** Three of the four pages still read
  `VES Website` in their `<title>` tag; only `index.html` reads `VAR-E Website`.

---

## Contributing

Changes reach the live site through a pull request against `main`.

```sh
# 1. Sync with the upstream repository
git fetch upstream
git merge upstream/main

# 2. Work on a branch — never commit directly to main
git checkout -b my-change

# 3. Make changes, then commit
git commit -am "Describe the change"

# 4. Push to your fork
git push -u origin my-change

# 5. Open a pull request
gh pr create --repo ASME-CIE-VES/ASME-CIE-VES.github.io --base main
```

Merging to `main` publishes the site, so every change is reviewed before it
lands.

---

## Credits

Built on the *LUGX Gaming* HTML template by
[TemplateMo](https://templatemo.com/), adapted for VAR-E.
