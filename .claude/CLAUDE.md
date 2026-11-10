# Emmanuel Lopez Portfolio

Personal portfolio for Emmanuel Lopez, a mechanical engineer who also does software engineering.
Theme: the "Dark Hour", a Persona 3-inspired night scene. Inspired by the mood only: never add
Atlus/Persona or other franchise characters, logos or official art.

## Structure

- `index.html`: the whole site (HTML, CSS and JS in one file, no build step, no framework)
- `images/background.jpg`: commissioned cathedral-city painting, animated by the WebGL shader
- `images/avatar.webp`: snow leopard avatar on the About page

## Run locally

Serve over HTTP; do not open `index.html` from disk. WebGL refuses to load the background image
from `file://`, and the page then falls back to a still image.

```
python -m http.server 3000 --bind 127.0.0.1
```

Then open http://127.0.0.1:3000. The launch config in `.claude/launch.json` does the same.

## How the page works

- **Background**: one fragment shader (`FRAG` in the script) draws `background.jpg` and animates it:
  slow zoom and mouse parallax, rotating rose window, flickering cyan windows with bloom, cloud
  shadows, fog banks, rising motes, and the time-stop pulse. Positions are in image pixels
  (1024×1536): `FOCUS` is the crop focal point, `ROSE` the rose-window center, `MOON` the moon.
  If the painting is ever replaced, re-measure these three constants.
- **Dark Hour toggle**: `uDark` blends between the green Dark Hour grade (1) and an ordinary
  blue night with warm windows (0). The choice is saved in `localStorage` (`darkHour`).
- **Intro**: a clock ticks 23:59:57 to 0:00, then the Dark Hour starts. Plays once per browser
  session (`sessionStorage` key `introSeen`).
- **HUD**: real date and the current moon phase, computed in JS (no network calls).
- **Routing**: hash routes `#projects`, `#about`, `#contact`; empty hash is the home menu.
  Page changes use the diagonal green wipe.

## Design tokens

Defined on `:root` at the top of the `<style>`. Use them instead of new literal colors.

- `--void` background, `--ink` text, `--dim` secondary text
- `--hour` Dark Hour green (primary accent), `--glow` cyan (secondary accent), `--blood` red (rare)
- Fonts: `--display` Big Shoulders Display (headings, menu), `--body` Chivo, `--mono` Chivo Mono (HUD, labels)

Headings and menu items are skewed (`skewX(-6deg)` to `-10deg`); cards use a clipped top-right corner.

## Content

- **Software tab**: Finance App V2, Engineering Career Roadmap (each links to its GitHub repo).
- **Mechanical tab**: undergraduate volunteer research (topology optimization filtering with the
  88-line MATLAB code; TPU lattice structures for measuring Poisson's ratio).
- Keep project summaries in plain English, two sentences or less, with a status pill
  (`live`, `wip`) and a few stack chips.

## Rules

- Keep it a single static `index.html` that runs on GitHub Pages with no build step.
- Respect `prefers-reduced-motion` (static frame, no wipe or intro), keep keyboard focus visible,
  and use real links (`<a href>`), not click handlers on divs.
- Keep render cost bounded: device pixel ratio capped at 1.5, pixel count capped, rendering paused
  in hidden tabs.
- The page must work at phone width (about 400px) with no horizontal scroll.
