# soprano-site

The website for [Soprano](https://github.com/frknue/soprano), a native macOS tiling
terminal multiplexer for AI coding agents.

One self-contained `index.html`. No build step, no dependencies, no framework — edit the
file, reload the browser. The CSS lives in a single `<style>` block so the page is one
request and works when opened straight from disk.

## Run it locally

```bash
python3 -m http.server 8000
# open http://localhost:8000
```

## Design notes

The site is set in **Folio**, the design the whole Soprano family shares (Soprano, Soprano
Git, Soprano Cast, Soprano Tab): paper and ink, a serif voice for titles, and one sienna
pencil mark wherever the keyboard is. It follows `prefers-color-scheme`: **Paper** in light
mode, **Ink** in dark. The values are the app's own Folio Paper and Folio Ink themes, so the
site and the product cannot drift apart.

| Slot | Paper | Ink | Role |
|---|---|---|---|
| `--bg-base` | `#f6f2ea` | `#1d1b18` | Page ground, terminals |
| `--bg-panel` | `#ede7db` | `#181613` | Title bars, pane headers, status bar |
| `--bg-raised` | `#fcfaf5` | `#26231f` | Panels, focused header, palette, keycaps |
| `--text-primary` | `#2b2620` | `#e2dacb` | Ink: text and the primary button |
| `--text-muted` | `#62594d` | `#a0968a` | Body copy, eyebrows, hints |
| `--accent` | `#a34a17` | `#e08a52` | Sienna: focus rule, pencil tick, caret, cursor only |
| `--border-subtle` · `--border-strong` | `#e0d8c9` · `#cbc0ad` | `#322e29` · `#443e37` | Hairlines · control edges and keycaps |
| `--success` · `--yellow` · `--blue` · `--danger` | `#456b30` · `#835f0b` · `#2f5f8a` · `#ae2f3c` | `#97ae76` · `#d6aa4f` · `#86a6cb` · `#e0707a` | WORKING · NEEDS INPUT · READY and attention · ERROR |

The rest (selection, attention wash, veil, keycap fill) are derived from those slots with
the same formulas the apps use, so a third-party theme inherits them. Colour only ever means
status: every status also carries a shape (ring = starting/ready/stopped, dot =
working/needs input, diamond = error). Regions are separated by one 1 px rule, never boxes
or double rules. The primary button is ink, not accent.

**Type** is system fonts only — no webfonts, no CDN, nothing to self-host:

- Serif `ui-serif, "New York", "Iowan Old Style", Georgia, serif` — the wordmark, headlines,
  captions and figures, and only at 15 px and up.
- Sans `system-ui, -apple-system, BlinkMacSystemFont, sans-serif` — everything you scan.
- Mono `ui-monospace, "SF Mono", Menlo, monospace` — terminals, paths, code.

Keycaps follow the family's K1 rule: one cap per chord (`⌘P`, `⇧⌘D`, `⌃A`), a prefix
sequence as separate caps (`⌃A` then `[`), letter legends uppercase, `esc` lowercase.

**Icons.** `assets/icons/` holds the family's app icons, Paper (`<app>.svg`) and Ink
(`<app>-dark.svg`), copied from the Folio design; the page swaps them with the mode.
`assets/soprano-icon-{1024,256,64}.png` (Paper) and `assets/soprano-icon-dark-*.png` (Ink)
are rendered from those SVGs with `rsvg-convert -w N -h N` and serve as favicon, touch icon
and `og:image`. The UI glyphs are inline Lucide paths (ISC) in one SVG sprite.

## Motion

Folio is static. There is no grain, drift, spotlight, typing, sweep, count-up or cycling
copy: the hero's lanes, the split tree and the install block are drawn in their final state.
The only movement on the page is the command palette fading in and out, opacity only, over
120 ms — and under `prefers-reduced-motion: reduce` that is 0 ms too. Section jumps scroll
instantly.

### The command palette

Soprano has one, so the site has one: press <kbd>⌘P</kbd> / <kbd>⌃P</kbd>, or `/`, or
click the **Jump to…** field in the header (its keycap reads `⌃P` off a Mac). Subsequence
filtering with the matched characters in semibold (sienna on the selected row), tighter
matches first, `↑`/`↓` to move, `↵` to open, `esc` to close. Sections scroll into view;
links open in a new tab.

## The theme switcher

Folio plus the seven third-party themes from `Theme.swift` (Gruvbox Dark, Catppuccin
Mocha, Dracula, Solarized Dark, Nord, Tokyo Night, Atom One Dark) are in the page as data,
copied value for value: the 16 `ThemeColors` slots and the full terminal palette. Picking
one in the Themes section rewrites the same custom properties the page is built on, so the
whole page — terminals and ANSI colours included — recolours. Third-party themes are dark
only and switch the page to Ink; they inherit Folio's formulas (attention turns yellow when
the accent sits within 40° of blue; `raised` mixes 5 % ink into the panel when `bgRaised`
equals `bgPanel`). Folio keeps following the system appearance live.

The choice is kept in `localStorage` under `soprano-theme` and does not touch the app's
own settings. Unknown ids — including the ids this page stored before Folio — fall back to
Folio. When a theme is added to `Theme.swift`, add it to the `THEMES` array in `index.html`
and it appears here too.

## Deploying

**Live at <https://frknue.github.io/soprano-site/>** — GitHub Pages, `main` branch root.
Pushing to `main` redeploys; it takes a minute or two.

To move it onto a custom domain:

1. Add a `CNAME` file containing the bare domain, e.g. `soprano.sh`.
2. At the DNS host, point the apex at GitHub Pages:
   `185.199.108.153`, `185.199.109.153`, `185.199.110.153`, `185.199.111.153`.
3. In **Settings → Pages**, set the custom domain and tick **Enforce HTTPS** once the
   certificate is issued.
4. Update `og:url` and `og:image` in `index.html` — social scrapers do not resolve
   relative URLs, so both are absolute and still point at the github.io address.

The `CNAME` file is deliberately absent until the domain is actually registered —
committing one for a domain you do not own breaks the Pages deployment.

## Still to do

- A real screenshot or short screen recording of Soprano running. The score carries the
  hero, but a genuine shot of four panes belongs further down the page.
- An OG preview image (`og.png`, 1200×630) for link unfurls.

## License

[MIT](LICENSE), matching the app.
