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

The mark is the app icon itself (`logos/edi-1a-active-pane-1024.png` in the app repo):
three panes with the active one glowing. Sampled straight out of the PNG, its background
is `#282828` and its active pane is `#fe8019` — gruvbox `bg` and `accent` exactly, so the
icon and the page were already in the same palette before either was designed around the
other.

The palette is **Gruvbox Dark**, taken value-for-value from `AppTheme.gruvboxDark` in
`Sources/Soprano/Config/Theme.swift` — the app's own default theme. Nothing here is a
generic Gruvbox approximation, so the site and the product cannot drift apart.

| Token | Value | Role |
|---|---|---|
| `--bg-hard` · `--bg` | `#1d2021` · `#282828` | Panels and page ground |
| `--fg0` · `--fg` | `#fbf1c7` · `#ebdbb2` | Wordmark and body text |
| `--orange` | `#fe8019` | `accent` — primary action, hover rules |
| `--yellow` | `#fabd2f` | `accentStrong` — `NEEDS INPUT`, the playhead |
| `--green` | `#b8bb26` | `success` — `READY`, shell prompts |
| `--red` | `#fb4934` | `danger` — `ERROR` |
| `--aqua` · `--blue` | `#8ec07c` · `#83a598` | Agent voices |

Agents map onto the palette rather than carrying their own brand colours, so the page
stays in one key: Codex → aqua, Claude Code → orange, OpenCode → blue.

Type pairs a classical serif for the wordmark against monospace for every status label —
a musical name set against a terminal voice. Both stacks are system fonts, so there are
no webfont requests and nothing to self-host.

## Motion

The hero is "the score": one lane per agent, running in parallel, with a single lane
stalling into amber. It states the product's actual question — *which agent needs me
right now?* — rather than showing a screenshot in a browser frame.

- **Entrance.** The wordmark's letters rise one at a time, then the score's lanes enter
  in sequence, the way voices enter a score.
- **Playhead.** A conductor's baton sweeps the score on a 9s loop.
- **Split tree.** In the Layout section, one pane becomes four on a 14s loop, following
  the same percentages a real `SplitNode` tree would produce.
- **Notification.** A macOS banner arrives the way a real one does, subtitled
  `window ▸ pane`, next to a live agent dashboard.
- **Typing.** The install block types itself out the first time it scrolls into view.
- **Ambient.** A slow warm drift plus a light film grain, suiting Gruvbox's analogue feel.

Every one of these is disabled under `prefers-reduced-motion: reduce`, where the split
tree settles into its finished four-pane state and the install block renders instantly.

## The theme switcher

All seven built-in themes are in the page as data, copied field-for-field from
`Theme.swift`, and the buttons in the Themes section rewrite the same CSS custom
properties the whole page is built on. Switching recolours everything — ambient wash,
notification banner, status dots, the playhead — because nothing on the page hardcodes a
colour. The choice is kept in `localStorage` and does not touch the app's own settings.

When a theme is added to `Theme.swift`, add it to the `THEMES` array in `index.html` and
it appears here too. The names are asserted to match the app's `name:` fields.

## Deploying

GitHub Pages, from the `main` branch root:

1. **Settings → Pages → Source:** Deploy from a branch, `main`, `/ (root)`.
2. Add a `CNAME` file containing the bare domain, e.g. `soprano.sh`.
3. At the DNS host, point the apex at GitHub Pages:
   `185.199.108.153`, `185.199.109.153`, `185.199.110.153`, `185.199.111.153`.
4. Back in **Settings → Pages**, set the custom domain and tick **Enforce HTTPS** once
   the certificate is issued.

The `CNAME` file is deliberately absent until the domain is actually registered —
committing one for a domain you do not own breaks the Pages deployment.

## Still to do

- A real screenshot or short screen recording of Soprano running. The score carries the
  hero, but a genuine shot of four panes belongs further down the page.
- An OG preview image (`og.png`, 1200×630) for link unfurls.

## License

[MIT](LICENSE), matching the app.
