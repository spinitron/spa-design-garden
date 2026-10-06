# SPA Design Garden for the Spinitron listener app


The Spinitron listener app allows radio fans to listen to live and Ark streams while browsing Playlists, Schedule, Shows, DJs, and so on.

As a single-page app (SPA), listening is uninterrupted as the visitor navigates from page to page.

As a web app it has the superpower called **CSS**.

**Garden? What garden?** In May 2003 Dave Shea published the monumental [CSS Zen Garden](https://csszengarden.com/). It showed that, 
given a sensible HTML document, a designer can do _a lot_ with just a stylesheet. The CSS Zen Garden had hundreds of radically different designs
but only one HTML doc. It showed how much you can do with CSS, given a decent HTML basis – whole page designs including layout. CSS ain't just 
colors, font sizes and margins! This repo is named in honor of Shea's CSS Zen Garden.

The Spinitron listener app was built with this kind of CSS-based design as a priority so that
you don't have to accept someone else's branding or design choices. 

You control the Look and feel (LAF) of the app. 

- Try the stock LAFs in the app. Each LAF has a name.
- Modify one of the stock LAFs to your needs and use your version.
- Write a LAF (i.e. CSS stylesheet) of your own and use that.

You do **not** change the app’s HTML. You just choose or add a CSS file. The app already has a layout (the **functional base**). Your file paints over it.

Try while you read (any Station works):

- A stock: [https://wzbc.q.spinitron.com/?laf=paper](https://wzbc.q.spinitron.com/?laf=paper)
- Bare layout: [https://spa.spinitron.com/?laf=null](https://spa.spinitron.com/?laf=null)

# 1. Use a stock in one minute

Add `?laf=` and a name to a Station view or the Catalog.

`https://wzbc.q.spinitron.com/?laf=night`

That is enough to demo.

Spinitron can change the default LAF for your Station. Call or email with the **name** (`night`, `canvas`, …) of your choice.

We divide the stock LAFs into two categories:

- Primer — just changing colors and type
- Garden — goes beyond Primer into real design

## Primer — same furniture, different paint

These files only set color and sometimes type. They share the same layout that Spinitron's default Catalog skin uses.

| File | Look |
| --- | --- |
| `paper.css` | Light cream, bookish |
| `night.css` | Dark blue |
| `fern.css` | Dark green |
| `tape.css` | Dark amber |
| `signal.css` | Dark, red accent |
| `ink.css` | Black and white, no serif |

Also: `spinitron.css` is the Catalog’s own skin (same system as the primers).

The CSS files are in this repo. On https:

`https://spa.spinitron.com/laf/paper.css`


## Garden — more personality

These sit on the functional base and may move layout.

| File | Look |
| --- | --- |
| `frumpy.css` | Dated civic — Times, navy, 3D buttons |
| `spine.css` | Magazine, navigation on a left spine |
| `dock.css` | The player is at the top |
| `broadside.css` | Broadsheet, rules, masthead |
| `flyer.css` | Xerox poster |
| `canvas.css` | Handmade / gallery |

`https://spa.spinitron.com/laf/canvas.css`


# 2. How to recolor a primer

This is the simplest step in customizing one of the stock LAFs, within reach if you can
edit a `.css` file and the app can fetch it over https.

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

The value after `laf_css=` is your stylesheet URL, **percent-encoded** (`:` → `%3A`, `/` → `%2F`).

When you like it, Spinitron can store that https URL as the Station default and then public links do not need `laf_css`.

### If you copy a primer to your own server

Primers start with:

```css
@import url("/laf/_visual.css");
```

That path only works **on the listener app**. On your domain, ship `_visual.css` next to your file and change the import to a path you control, for example `url("./_visual.css")`. Garden files do not use this import.

# 3. See the layout you must keep

`?laf=null` loads **no** Look and feel file. You see the functional base: spacing, the player, lists, navigation. If your stylesheet is lost, empty, or broken, this much will always be there and it will remain usable even if it doesn't look good.

Do not _use_ `null` as your Station’s LAF. It is not a skin. It's there for a CSS
designer to study — it's the ground on which they build.


# 4. Write a LAF from scratch

1. Open the app with `?laf=null`.
2. Create an empty `.css` file. You do not import the base; the app already loaded it.
3. Target the **class names** below. Start with `html, body` (background, color, font), then `a`, then `h1`, then `.topbar` and `.player-bar`.
4. Preview with `laf_css=` as above.
5. Keep every control usable: Listen live, Play Ark, Schedule week buttons, search, links, the player.

You may change layout (garden files do). Do not hide Listen live, the player, or errors (`.page-status.error`, `.player-bar__error`). These are basic components for the app to function.

# 5. Modify a garden file

Copy `frumpy.css` if you want something close to an ordinary station site. Copy `broadside.css` or `spine.css` if you want editorial. Copy `canvas.css` or `flyer.css` if you want handmade or loud.

Change colors and type first. Then, if you want, override layout for `.station-nav`, `.player-bar`, `.spin-list`. Test a playlist with cover art, the schedule, and a phone-width window.

# 6. How a visit picks a stylesheet

First match wins:

1. `laf_css` — https URL to a CSS file  
2. `laf` — stock name (from the lists above), or `null`  
3. The Station’s Catalog setting (a stock name, or an https URL)

Unknown LAF names and non-https URLs are ignored and the page still opens.

`return_url`, `laf`, and `laf_css` stay on in-app links (when they are valid), so a preview does not reset on navigation within the app.

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
| `.player-bar.is-playing` / `--live` / `--ark` / `--starting` | State |
| `.player-bar__clock` / `__spin` / `__hint` / `__link` / `__error` / `__ellipsis` | Lines in the bar |
| `.play-btn` / `.speaker-btn` / `.cast-icon` | Play, Pause, Cancel, and Speaker. Speaker casts (Chromecast / AirPlay) when this browser can |
| `.player-audio` | Hidden `<audio>`; do not `display: none` in a way that stops playback |

Idle and starting: no Play/Pause/Speaker. Starting shows Cancel. Do not put Listen live in the player (it belongs on `.station-head__actions`). Loading ellipsis is the bar only, not lock-screen text.

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

# 10. Get a coding assistant to write your LAF

Use an assistant that accepts an image (ChatGPT is fine). 
Give it a **screenshot of your site** and the prompt below including the HTML.
The HTML is the listener app’s layout. It is the same for every Station.

**Screenshot.** Open your Station website. Capture the **header strip** — the
colored bar at the top, with its type — not the whole homepage and not a
hero photo unless that photo *is* the bar. If the header has two bands (a
color strip above a white nav), shoot the color strip. That strip is the
app’s top bar and player bar.

Attach that image, then paste the prompt.

Open the CSS it returns with `laf_css=` (percent-encode the file’s https URL)
next to your real site. If the bars are the wrong band or the type is off, say
so in one sentence and go again.

When you have a file you like, send it (or a link) to Spinitron and ask them
to make it your Station default.

-------- copy from here --------
```
Paint this HTML to match the attached screenshot of our station website.
One CSS file. No HTML. No @import.
.topbar and .player-bar use the screenshot’s header-strip background and
header-strip text color (e.g. if that strip is teal, use that teal, not something else).
Page background, body text, headings, and links come from the screenshot too.

HTML (do not change it; only write CSS for these class names):

<header class="topbar">
  <a class="topbar__return"></a>
  <span class="topbar__note"></span>
</header>
<div class="station">
  <header class="station-head">
    <img class="station-head__logo">
    <div>
      <p class="eyebrow"></p>
      <h1 class="station-head__title-row">
        <span class="badges"><span class="lamp"></span><span class="lamp lamp--ark"></span></span>
      </h1>
      <div class="station-head__actions">
        <button class="listen-live"></button>
        <a class="text-link"></a>
      </div>
    </div>
  </header>
  <nav class="station-nav"><a class="active"></a></nav>
  <main class="page">
    <div class="card card--live"><div class="card__when"></div><a class="card__title"></a></div>
    <ul class="spin-list"><li><span class="spin-list__time"></span><span class="spin-list__track"></span></li></ul>
  </main>
</div>
<div class="player-bar player-bar--live is-playing">
  <div class="player-bar__main">
    <button class="play-btn"></button>
    <div class="player-bar__copy">
      <div class="player-bar__clock"></div>
      <div class="player-bar__spin"></div>
      <a class="player-bar__link"></a>
    </div>
  </div>
</div>
```
-------- copy to here --------


# 11. Rules that keep the app honest

Please observe these rules for a well-behaved LAF.

- HTTPS only for `laf_css` and for the Station website link.
- Keep Listen live, Play Ark, navigation, and the player operable.
- Do not cover the player with `position: fixed` content that cannot be dismissed.
- Prefer the class names above. If a hook is missing, ask — do not scrape inner playlist HTML.

Thank you.

If you need help, call or email Spinitron.