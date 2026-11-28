# CLAUDE.md — Firas Alfadda portfolio site

## 1. Old file
`old-reference/Claude (1).md` is an old, generic rules file. Do not follow it. The only rules taken from it are the ones listed in section 6.

## 2. About the site
A static site with no build step and no framework: plain HTML, inline CSS and inline JS. It is hosted on Vercel from this GitHub repo.

### File structure
- `index.html`: the home page (all sections, styles and scripts inline).
- `project.html`: one template page for every project, filled in from JSON.
- `projects/<slug>/data.json` and `projects/<slug>/images/`: content for each project (`rehal-mosque`, `lamina-cafe`, `alyamamah-house`).
- `projects/<Name> hero.webp` (+ `-600w`, `-1200w`): the project card images on the home page (white line sketches on black).
- `images/`: hero portrait and Works images.
- `Photography/` (full size) and `Photography/thumbnails/`: photography library.
- `brand_ _assets/`: brand assets (currently the signature SVG used in the hero).
- `serve.mjs`: local dev server.
- `sw.js`: service worker that caches images.
- `Desktop.png`, `blur while hover.png`: reference screenshots, not used by the site.
- `.claude/commands/`: old `test` / `merge` / `cancel` commands (they use a `test-changes` branch). Section 3 overrides them.

### index.html sections
- **Nav**: fixed bar (gets a blurred dark background after scrolling 50px), links to each section, the AR/EN button, and a hamburger menu on mobile.
- **Hero** (`#hero`): portrait photo with a dark gradient, animated hand-drawn signature (SVG paths, replays when the hero comes back into view), and a short bio.
- **Projects** (`#projects`): grid of `.project-card` links (3 columns, 2 at ≤900px, 1 at ≤600px). A live card links to `project.html?project=<slug>`. A card with the `coming-soon` class has no link, a faded greyscale image, a "Coming Soon" label, and a "Coming Soon" overlay on hover (on tap on touch devices).
- **Works** (`#works`): intro text, a quote, and three image tiles with text overlays on hover (on tap on touch devices).
- **Photography** (`#photography`): five thumbnails plus an "Explore More" tile that opens the full library modal. Library images open a lightbox (arrows, keyboard, swipe) that loads the full-size file from `data-full`.
- **Contact** (`#contact`): Instagram, LinkedIn and Email links. The heading glows once when the section scrolls into view.
- **Footer**: © line and "Back to top".
- Scroll reveals fade/slide elements in with IntersectionObserver. Lazy images fade in on load. Google Analytics is loaded on both pages.

### project.html + projects/<slug>/data.json
- Reads `?project=<slug>`, fetches `projects/<slug>/data.json` and renders it. Shows a not-found screen if it fails.
- data.json fields: `slug`, `year`, optional `cover`, `images` (all paths must start with `images/`), optional `gallerySections` (each with optional `labelEn` / `labelAr` and its own `images`), and `en` / `ar` objects with `title`, `category`, `description`, optional `course`, `location`, `region`, `area`, and optional `moduleSectionTitle`, `moduleSectionDesc`, `modules` (`number`, `title`, `description`).
- Hero image is `cover`, or the first of `images`. Without `gallerySections`, all `images` show as one flat gallery. Gallery images open a lightbox.
- `PROJECT_ORDER` in the `<script>` of `project.html` lists every project slug with `comingSoon: true/false`. Prev/next links at the bottom skip coming-soon projects. It must match the order of cards in `index.html`.

### serve.mjs
`node serve.mjs` serves the repo root at `http://localhost:3000` (`/` → `index.html`). Use it for every local check and screenshot, never `file:///`. Don't start a second copy if one is already running.

### sw.js image cache
Registered by both pages. It only handles image requests (webp, jpg, jpeg, png, gif, svg): cache first, then network. The cache name is `img-cache-vN` (now `img-cache-v2`). Changing the name makes every visitor drop the old cache on the next visit. Because files are cached by URL, an image that changes but keeps its file name needs a version bump.

### Fonts
Google Fonts: **Albert Sans** for English, **IBM Plex Sans Arabic** for Arabic (switched by `html[lang="ar"] body`). Arabic text uses `letter-spacing: 0` instead of the English tracking.

### Colors
- Background `#0d0d0d`, body text `#e8e8e8`, headings `#fff`.
- Signature stroke `#e8dfd0` (warm off-white).
- Dark surfaces and lines: `#151515`, `#1f1f1f`, `#252525`, `#2a2a2a`.
- Muted text and borders use white at low opacity (`rgba(255,255,255,0.18–0.75)`).

### Arabic / English switch
- Every translatable element carries `data-en` and `data-ar`. The AR/EN button calls `toggleLang()`, which swaps the text (innerHTML when the value contains tags like `<br>` or `<em>`), sets `<html lang>`, and saves the choice in `localStorage` (`lang`), so both pages open in the last language.
- `html[lang="ar"]` sets `direction: rtl` and holds all RTL overrides (mirrored nav, hero, gradient, text alignment). Because the grid flows RTL, the first project card shows on the left in English and on the right in Arabic.
- On `project.html`, switching language re-renders the page from the `en` / `ar` parts of data.json.

## 3. Preview before publishing (always)
- Never push changes straight to `main`.
- Work on a branch, push it, and give Firas the Vercel preview link.
- Merge into `main` only when Firas says "publish" or "انشر".

## 4. Adding a project: two stages

### Stage 1 – Coming soon (when Firas says "add the project <name>")
- Find the project's working folder by name in `C:\Users\Firas\Desktop` or `C:\Users\Firas\Documents\Projects`. Never edit, move or delete anything there.
- The card image must be Firas's own white line sketch on black, like the other project cards. Look for it in the project folder first. If it's not there, ask Firas for it and stop: don't do anything else for this project until Firas sends it. Never draw or generate it yourself.
- Add the project card first in the projects list in `index.html` (newest first: left in English, right in Arabic), with the same "Coming soon" markup as the current Lamina Cafe and Alyamamah House cards. Clicking it shows coming soon.
- Add the slug first in `PROJECT_ORDER` in `project.html` with `comingSoon: true`.
- Send Firas the preview link.

### Stage 2 – Full project page (when Firas says the project is finished)
- Read the posters, renders and PDFs, work out the concept, site and program, and pick the best final images. Skip drafts and duplicates.
- Create `projects/<slug>/` with `images/` and `data.json` in English and Arabic, matching the other projects.
- Remove "Coming soon" from the card (remove the `coming-soon` class, the overlay and the label, and add `href="project.html?project=<slug>"`), and set `comingSoon: false` in `PROJECT_ORDER`.
- Show Firas the chosen images and text with the preview link.

## 5. Images
- Always work from full-size originals. Export WebP q88 at 800w and 1200w plus the largest real size. Never upscale.
- If an original is too small, tell Firas instead of exporting it.
- If an image keeps its file name but changes, update the `sw.js` cache version.

## 6. Design rules (from the old file)
- Animate only `transform` and `opacity`. Never use `transition-all`.
- Every clickable element has hover, focus-visible and active states.
- Check the brand assets folder (`brand_ _assets/`) before design changes.
- Use the frontend-design skill for bigger design changes.
- After every visual change, take screenshots at mobile and desktop width (from `node serve.mjs`) to check it. Tell Firas when something can only be checked in iPhone Safari.

## 7. General rules
- Do only what Firas asks. Don't add sections or content that wasn't asked for.
- Keep Firas's Arabic names exactly as written (e.g. فراس الفداء، مسجد رِحال).
- For a mobile fix, don't change desktop, and the reverse.
- If something goes wrong, tell Firas how to undo it.
