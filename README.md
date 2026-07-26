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

Every colour on the page already exists in the app, so the site and the product cannot
drift apart:

| Token | Value | Comes from |
|---|---|---|
| Ground / panel | `#08080f` · `#0f0f1e` | `app-icon.svg` background gradient |
| Violet accent | `#a78bfa` · `#7c3aed` | `app-icon.svg` accent gradient |
| Emerald (`READY`) | `#34d399` | `app-icon.svg` status gradient |
| Amber (`NEEDS INPUT`) | `#fabd2f` | Gruvbox Dark, Soprano's default theme |
| Codex | `#10a37f` | `Config/DefaultAgents.swift` |
| Claude Code | `#da7756` | `Config/DefaultAgents.swift` |
| OpenCode | `#58a6ff` | `Config/DefaultAgents.swift` |

Type pairs a classical serif for the wordmark against monospace for every status label —
a musical name set against a terminal voice. Both stacks are system fonts, so there are
no webfont requests and nothing to self-host.

The hero is "the score": one lane per agent, running in parallel, with a single lane
stalling into amber. It is the product's actual question — *which agent needs me right
now?* — rather than a screenshot in a browser frame. Lane motion is disabled under
`prefers-reduced-motion`.

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
