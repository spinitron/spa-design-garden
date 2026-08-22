# SPA Design Garden

These files are copies. The originals live in the Spinitron listener app (`public/laf`, `src/base.css`, `docs/garden`). Edit them there.

Stylesheets for the Spinitron listener app. Use a stock as-is, recolor one, or write your own.

You do **not** change the app’s HTML. You add a CSS file. The app already has a layout (the **functional base**). Your file paints over it.

Try while you read (any Station works):

- A stock: [https://wzbc.q.spinitron.com/?laf=paper](https://wzbc.q.spinitron.com/?laf=paper)
- Bare layout: [https://spa.spinitron.com/?laf=null](https://spa.spinitron.com/?laf=null)

# 1. Use a stock in one minute

Add `?laf=` and a name to a Station view or the Catalog.

`https://wzbc.q.spinitron.com/?laf=night`

That is enough to demo. To make it the Station default, give helpdesk the **name** (`night`, `canvas`, …).

## Primer — same furniture, different paint

These files only set color (and sometimes type). They share one layout with the Catalog skin.

| File | Look |
| --- | --- |
| `paper.css` | Light cream, bookish |
| `night.css` | Dark blue |
| `fern.css` | Dark green |
| `tape.css` | Dark amber |
| `signal.css` | Dark, red accent |
| `ink.css` | Black and white, sans |

Also: `spinitron.css` is the Catalog’s own skin (same system as the primers).

Live copies:

`https://spa.spinitron.com/laf/paper.css`

## Garden — more personality

These sit on the functional base and may move layout. A dated civic page is a real Look and feel, not a failed design.

| File | Look |
| --- | --- |
| `frumpy.css` | City-hall civic: Times, navy, 3D buttons |
| `spine.css` | Magazine; navigation as a left spine |
| `dock.css` | The player is the stage |
| `broadside.css` | Broadsheet |
| `flyer.css` | Xerox poster |
| `canvas.css` | Handmade / gallery |

`https://spa.spinitron.com/laf/canvas.css`

# 2. Recolor a primer (novice)

This is the intended first custom job.

1. Open `paper.css` (or `night.css` if you want dark).
2. You will see two things: a line that pulls in `_visual.css`, and a `:root { … }` list of **custom properties** (variables).
3. Change only the values in `:root`. Save. Reload the app with `?laf=` that file’s name, or with `laf_css=` pointed at your copy (step 4).

Meaning of the names:

| Variable | Used for |
| --- | --- |
| `--paper` | Page background |
| `--paper-2` | Slightly different panels |
| `--ink` | Main text |
| `--ink-soft` | Quieter text |
| `--rule` | Lines |
| `--signal` | Accent (links, live, emphasis) |
| `--tape` | Second accent |
| `--bar` | Player bar background |
| `--bar-text` | Player bar text |
| `--font` | Body type |
| `--display` | Headings |
| `--clock` | Times |

`color-scheme: light` or `dark` tells the browser which default form controls to use.

**Contrast:** text must stay readable on the background. If you cannot read a playlist row, pick a darker `--ink` or a lighter `--paper`.

4. Host your file on **https** (your Station site is fine). Preview:

```
https://wzbc.q.spinitron.com/?laf_css=https%3A%2F%2Fwww.example.org%2Flisten.css
```

The value after `laf_css=` is your stylesheet URL, **percent-encoded** (`:` → `%3A`, `/` → `%2F`). `http://` is ignored.

When you like it, helpdesk can store that https URL as the Station default. Then public links do not need `laf_css`.

### If you copy a primer to your own server

Primers start with:

```css
@import url("/laf/_visual.css");
```

That path only works **on the listener app**. On your domain, ship `_visual.css` next to your file and change the import to a path you control, for example `url("./_visual.css")`. Garden files do not use this import.

# 3. See the layout you must keep

`?laf=null` loads **no** Look and feel file. You see the functional base: spacing, the player, lists, navigation. If your stylesheet is empty or broken, this is what must remain usable.

Do not assign `null` as the Station’s Catalog Look and feel. It is a workbench, not a skin.

# 4. Write one from scratch

1. Open the app with `?laf=null`.
2. Create an empty `.css` file. You do not import the base; the app already loaded it.
3. Target the **class names** below. Start with `html, body` (background, color, font), then `a`, then `h1`, then `.topbar` and `.player-bar`.
4. Preview with `laf_css=` as above.
5. Keep every control usable: Listen live, Play Ark, Schedule week buttons, search, links, the player.

You may change layout (garden files do). Do not hide Listen live, the player, or errors (`.page-status.error`, `.player-bar__error`).

# 5. Modify a garden file

Copy `frumpy.css` if you want something close to an ordinary station site. Copy `broadside.css` or `spine.css` if you want editorial. Copy `canvas.css` or `flyer.css` if you want handmade or loud.

Change colors and type first. Then, if you must, override layout for `.station-nav`, `.player-bar`, `.spin-list`. Test a playlist with cover art, the schedule, and a phone-width window.

# 6. How a visit picks a stylesheet

First match wins:

1. `laf_css` — https URL to a CSS file  
2. `laf` — stock name, or `null`  
3. The Station’s Catalog setting (a stock name, or an https URL)

Unknown names and non-https URLs are ignored. The page still opens.

`return_url`, `laf`, and `laf_css` stay on in-app links when they were valid, so a preview does not reset on the next click.

# 7. Class names (the page’s hooks)

The HTML is stable. Style these; do not depend on tag soup inside a playlist description (that HTML is sanitized).

**Shell (every page)**

| Class | What it is |
| --- | --- |
| `.shell` | Whole window |
| `.shell--catalog` / `--station` / `--not-found` | Which kind of visit |
| `.shell__top` | Top bar region |
| `.topbar` | Top bar |
| `.wordmark` | Catalog mark (link) |
| `.topbar__return` | Station name; link home when there is a website |
| `.topbar__return--plain` | Station name, not a link |
| `.topbar__note` | “Listening stays put” |
| `.legal` / `.shell__legal` | Catalog harvesting line (Station view omits this) |
| `.shell__body` | Main column |
| `.page` / `.station` | Content width |
| `.shell__player` | Player region (when present on the markup) |

**Catalog**

| Class | What it is |
| --- | --- |
| `.catalog-head` | Catalog title |
| `.search` | Search field |
| `.station-list` | Station list |
| `.count` | “N stations” |
| `.visually-hidden` | Label for assistive tech; keep it |

**Station header and nav**

| Class | What it is |
| --- | --- |
| `.station-head` | Logo + title |
| `.station-head__logo` | Logo (or empty box) |
| `.station-head__title-row` | Title + Live/Ark badges |
| `.badges` / `.lamp` / `.lamp--ark` | Live / Ark labels |
| `.station-head__meta` | “Playing” |
| `.station-head__actions` | Listen live, website |
| `.listen-live` | Listen live |
| `.listen-live--muted` | No live stream |
| `.text-link` | Ordinary text link (website) |
| `.station-nav` | Playlists / Schedule / … |

**Lists and cards**

| Class | What it is |
| --- | --- |
| `.card` / `.card--live` / `.card__when` / `.card__title` / `.card__actions` | On air / coming up |
| `.stack` | Vertical list |
| `.spin-list` | Spins |
| `.spin-art` | Cover |
| `.spin-list__time` | Spin time |
| `.spin-list__meta` / `__track` / `__release` | Title block; album; other credits in `.muted` |
| `.hero-art` | Large show/DJ image |
| `.prose` | Bio / description HTML |
| `.muted` / `.eyebrow` | Secondary text |
| `.page-status` / `.page-status.error` | Loading / error |
| `.dj-grid` / `.dj-ph` | DJ index |
| `.sched-row` / `.sched__time` / `.week-nav` | Schedule |
| `.ark-play` | Play Ark |

**Player**

| Class | What it is |
| --- | --- |
| `.player-bar` | Sticky player |
| `.player-bar.is-playing` / `--live` / `--ark` | State |
| `.player-bar__clock` / `__spin` / `__hint` / `__link` / `__error` | Lines in the bar |
| `.play-btn` / `.speaker-btn` / `.cast-icon` | Controls |
| `.player-audio` | Hidden `<audio>`; do not `display: none` in a way that stops playback |

Times on Station pages are **Station time** (that Station’s zone). The player clock is local when Live, and the playhead when Ark.

# 8. Learn CSS

Read in this order. All of these are current, maintained courses or references — not random blog posts.

1. **[Learn CSS (web.dev)](https://web.dev/learn/css)** — Google’s course. Start at [Welcome](https://web.dev/learn/css/welcome). Box model, cascade, flexbox, grid, color.
2. **[Styling basics (MDN)](https://developer.mozilla.org/en-US/docs/Learn_web_development/Core/Styling_basics)** — Mozilla’s beginner path, part of [Learn web development](https://developer.mozilla.org/en-US/docs/Learn_web_development).
3. **[CSS (MDN)](https://developer.mozilla.org/en-US/docs/Web/CSS)** — the reference. Look up a property when you need the exact meaning.
4. **[CSS selectors](https://developer.mozilla.org/en-US/docs/Web/CSS/CSS_selectors)** and **[Using custom properties](https://developer.mozilla.org/en-US/docs/Web/CSS/Using_CSS_custom_properties)** — what this garden uses all day.

You need a text editor and a browser. You do not need a build step for a Look and feel file.

# 9. Files in this repository

This tree (and [spa-design-garden](https://github.com/spinitron/spa-design-garden)) contains:

| File | Role |
| --- | --- |
| `_visual.css` | Shared primer layout + Source Serif |
| `spinitron.css` `paper.css` `night.css` `fern.css` `tape.css` `signal.css` `ink.css` | Primer + Catalog |
| `frumpy.css` `spine.css` `dock.css` `broadside.css` `flyer.css` `canvas.css` | Garden |
| `base.css` | Functional base (copy from the app) — read-only reference |

The listener app also loads webfonts from `/fonts/`. Primers expect Source Serif there. On your own host, use system fonts or host the fonts yourself and point `--font` at them.

# 10. Rules that keep the app honest

- HTTPS only for `laf_css` and for the Station website link.
- Keep Listen live, Play Ark, navigation, and the player operable.
- Do not cover the player with `position: fixed` content that cannot be dismissed.
- Prefer the class names above. If a hook is missing, ask — do not scrape inner playlist HTML.
- Frumpy is allowed. A Look and feel does not have to look “designed.”
