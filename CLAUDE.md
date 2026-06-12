# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Running the app

This is a zero-dependency, single-file HTML app. Open it directly in a browser:

```bash
open index.html
# or serve locally to avoid any browser file-protocol restrictions:
python3 -m http.server 8080
```

## Architecture

Everything lives in `index.html` — styles, markup, and script are all inline. There is no build step, no bundler, and no external dependencies.

The page is divided into three tabs, each rendered as a `<section class="tab-panel">`:

| Tab | ID | Description |
|-----|----|-------------|
| Counter | `tab-counter` | Manual ±step counter with a progress ring (0–100 range) and change history pills |
| Countdown | `tab-countdown` | Auto-decrementing seconds timer; ring drains as time elapses, turns red below 25% |
| Stopwatch | `tab-stopwatch` | Elapsed-time tracker with lap splits |

**Tab switching** is handled by a single `querySelectorAll('.tab')` listener that toggles `.active` on both the tab button and its matching `<section>`.

**Shared constant:** `CIRC = 2π × 90 ≈ 565.5` — the SVG ring circumference used by both the Counter and Countdown rings to compute `stroke-dashoffset`.

**Countdown timing** uses `Date.now()` anchoring (`cdEndTime`) rather than counting ticks, so pause/resume is drift-free. The interval runs at 50 ms for visual smoothness.

**Stopwatch timing** similarly stores `swElapsed` (accumulated ms before the last start) plus a live `Date.now() - swStartTime` delta; the display interval runs at ~37 ms (~27 fps).

**Keyboard shortcuts** (Counter tab only): `↑`/`+` increment, `↓`/`-` decrement, `R` reset.
