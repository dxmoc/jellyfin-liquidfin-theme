# LiquidFin

A Jellyfin theme built around Apple's Liquid Glass material: translucent panes
with refracted edges, specular rim light, capsule controls and iOS spring
motion. Dark only, and deliberately colourless — the artwork behind the glass is
what supplies the colour.

One CSS file. No JavaScript, no plugin.

## Preview

![The home screen: floating header, glass tabs, rounded artwork](assets/home.webp)

| | |
| --- | --- |
| ![A film's detail page, its logo over the backdrop](assets/detail.webp) | ![The player OSD floating over the video](assets/player.webp) |

## Install

Dashboard → **Branding** → **Custom CSS code**, paste this and save:

```css
@import url("https://cdn.jsdelivr.net/gh/dxmoc/jellyfin-liquidfin-theme@main/dist/liquidfin.min.css");
```

Swap `@main` for a tag — `@v0.2.2` — to pin a release. `@main` follows every
change on its own, though a new one can take a while to reach you: jsDelivr
caches it for 12 hours, and your browser holds its copy longer still. The same
file is on [GitHub Pages](https://dxmoc.github.io/jellyfin-liquidfin-theme/dist/liquidfin.min.css)
if you would rather not use a CDN, or paste
[`dist/liquidfin.min.css`](dist/liquidfin.min.css) straight into the box.

`raw.githubusercontent.com` does **not** work: it serves `text/plain` with
`nosniff`, so the browser drops the stylesheet without an error.

Custom CSS reaches the web client and anything rendering the web UI. Native
apps (Android TV, Roku, Swiftfin) do not read it.

## Customise

Every value is a custom property. Redefine any of them *after* the `@import`:

```css
:root {
  --lf-accent: #0a84ff;      /* iOS blue instead of neutral white */
  --lf-blur: 30px;           /* thicker glass */
  --lf-refract-blur: 0px;    /* switch off the refracted edge */
  --lf-detail-logo: none;    /* text title instead of the film's logo */
}
```

| Token | Default | What it does |
| --- | --- | --- |
| `--lf-accent` | `#f5f5f7` | Fill of the one primary action per screen |
| `--lf-blur` | `22px` | Glass thickness |
| `--lf-saturate` | `155%` | Colour pulled through the glass |
| `--lf-refract-blur` | `6px` | Refracted outer edge; `0px` disables |
| `--lf-mesh-opacity` | `1` | Ambient colour mesh behind everything |
| `--lf-ground` | `#06070a` | Page background |
| `--lf-detail-logo` | `block` | Show the item's logo instead of its title |
| `--lf-title-weight` | `200` | Weight of the text title |

Full list: [`src/00-tokens.css`](src/00-tokens.css).

## Compatibility

Built against the Jellyfin **10.10 / 10.11** web client.

Needs `backdrop-filter`; without it the theme falls back to opaque surfaces on
its own. It also honours `prefers-reduced-transparency`, `prefers-reduced-motion`
and `prefers-contrast: more`, and switches to solid surfaces with a wider focus
ring on TV clients, where stacked blur layers are a slideshow.

Sluggish on a weak machine? Set `--lf-refract-blur: 0px`, then `--lf-blur: 0px`.

Nothing is loaded from anywhere else: no CDN assets, no remote images, no web
fonts. The type is whatever the machine already has — SF Pro on Apple hardware,
Segoe UI on Windows, Roboto on Android. Name a face in `--lf-font` if you would
rather use your own.

## Develop

`src/*.css` is the source, `dist/` is generated, and the filename prefixes are
the cascade order — `99-fallbacks.css` must stay last.

```sh
python build.py
```

Python 3.9+, standard library only. `tools/` renders a live Jellyfin page with
the local build swapped in, which is the only reliable way to check a change;
see [`tools/README.md`](tools/README.md).

## Licence

MIT. Structural cues for which selectors matter were taken from
[ElegantFin](https://github.com/lscambo13/ElegantFin); none of its CSS is used.
