# Demos

Real sample outputs produced following the patterns in this skill. Open each in a browser to see them in motion.

| File | Shows | Style / system |
|---|---|---|
| [demo-1-deck.html](demo-1-deck.html) | 5-slide pitch deck with scaling, keyboard nav, 1-indexed labels, localStorage persistence | **Swiss Editorial** — numbered sections, hairline rules, oversized folios, one accent color, Inter at several sizes |
| [demo-2-canvas.html](demo-2-canvas.html) | 4 hero variations for the same fictional product, labelled cleanly so variants can be compared | Four schools side-by-side: **Swiss Editorial**, **Kenya Hara minimalism**, **Brutalist web**, **Editorial magazine** |
| [demo-3-prototype.html](demo-3-prototype.html) | Interactive iOS prototype — 3 screens side-by-side with real state. Click the "+5p / +20p / Finish" buttons on the detail screen and watch the home screen's current-book card and the stats screen's numbers propagate live. | **Warm neutral reading app**, Fraunces serif display + Inter body, iOS 15 device frame |
| [style-gallery/index.html](style-gallery/index.html) | Same 1-page intro to the skill, rendered **10 times** — once in each philosophy listed in [references/design-styles.md](../references/design-styles.md). Open the index and click any cell. | All 10 directions side-by-side: **01 Swiss Editorial**, **02 Bauhaus**, **03 Kenya Hara**, **04 Dieter Rams**, **05 Magazine Editorial**, **06 Zine/Risograph**, **07 Field.io**, **08 Brutalist Web**, **09 Sagmeister**, **10 Y2K Futurist** |

## What to notice

- Each demo **declares its system up front** in a top-of-file comment — type scale, colors, grid, signature moves. Every styling choice in the file answers to that declaration.
- Each demo has **no AI-slop tells**: no purple gradients, no emoji bullets, no rounded-card-with-left-border, no SVG-drawn "product", no Inter as display face.
- The deck **1-indexes** slide labels (`data-screen-label="01 Title"`) matching the visible counter — a small discipline that makes "slide 5" unambiguous.
- The prototype **does not use `scrollIntoView`**, loads **pinned React + Babel with integrity hashes**, and exports components to `window` via the Babel scope pattern described in [../references/react-babel.md](../references/react-babel.md).
- The canvas **does not** ship as 4 separate HTML files — it's one comparison artifact, which is cheaper to iterate on and gives the user side-by-side visibility.
- The **style gallery** is the opposite tradeoff from the canvas: 10 fully committed pages rather than 4 quick sketches, because the point is to feel what "same content, different committed system" reads like. Use it as a concrete reference when running the Design Direction Advisor.

## Running the demos

Just open the `.html` file directly:

```bash
open demo-1-deck.html
open demo-2-canvas.html
open demo-3-prototype.html
open style-gallery/index.html
```

No build step, no server. Each file is self-contained (CDN dependencies only).
