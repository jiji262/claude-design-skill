
Step 1 is not optional. Starting a design without context leads to bad design. If you have no brand, no design system, no reference artifact — stop and ask. Offer to work from a codebase, a UI kit, a screenshot, or a reference competitor. Ask what fidelity you're aiming for (wireframe, hi-fi static, interactive prototype, video).

Read [references/workflow.md](references/workflow.md) for the question patterns and context-gathering playbook.

## When the brief is too vague — the Design Direction Advisor

If the user's brief is too open to execute ("make a landing page", "design me something nice", "I don't know what style I want"), **do not** improvise on generic intuition. That's how AI-slop is born.

Switch into **Design Direction Advisor** mode:

1. Pick 3 styles from [references/design-styles.md](references/design-styles.md), drawn from different schools so the user sees a real spread (not three minimalist variants).
2. For each direction, give a one-sentence pitch, a recognizable flagship (designer/brand), 3 vibe keywords, and one sentence on what this direction means concretely for their brief.
3. Build a lightweight 3-cell preview (a design canvas with a quick sketch of each direction's hero treatment) — enough to choose from, not finished artifacts.
4. Ask the user to pick a direction (or a blend). Once they pick, drop out of Advisor mode and continue the normal workflow rooted in that style.

Total Advisor cycle should take 5–10 minutes. If you're 30 minutes in, you've overshot — ship what you have and let the user redirect.

## When the brief names a specific brand — the Core Asset Protocol

If the task touches a specific brand or product ("design a pitch for Stripe", "animation for Pocket 4's launch", "mock up a Linear-style dashboard"), **do not** skip straight to colors and fonts. That's a recipe for off-brand output.

Follow the 5-step Core Asset Protocol in [references/brand-context.md](references/brand-context.md):

1. **Ask** the user for the full checklist of 6 asset types (logo, product shots, UI screenshots, colors, fonts, guidelines) — not a vague "do you have brand guidelines?"
2. **Search** official channels by asset type.
3. **Download** via the three-path fallbacks per asset type. Apply the 5-10-2-8 quality rule to non-logo assets (search 5 rounds, find 10 candidates, keep 2 good ones, each ≥ 8/10).
4. **Verify** each asset is real, high-resolution, current, and un-contaminated by third-party brand colors.
5. **Freeze** findings into `brand-spec.md` — logo paths, product-shot paths, UI-screenshot paths, colors, fonts, vibe keywords, and what you couldn't find.

**Key rule from the protocol:** *logo / product shots / UI screenshots are first-class citizens*. Colors and fonts are auxiliary. Grabbing only colors-and-fonts and skipping logo/product/UI is the most common failure mode.

## Picking the output format

The format follows the exploration, not the other way around:

| You're exploring... | Use... | Why |
|---|---|---|
| Purely visual options (color, type, static layout) | **Design canvas** — a grid with labeled variants | Side-by-side comparison is the whole point |
| Interactions, flows, many-option UX | **Hi-fi clickable prototype** with Tweaks | Users need to feel it, not just see it |
| A narrative sequence | **Slide deck** with scaling stage | Speaker-ready, paged, exportable |
| Motion, transitions, video ideas | **Timeline animation** (Stage + Sprite) | Needs a scrubber and reliable timing |
| Many rough ideas early | **Wireframe grid / storyboard** | Breadth beats polish before commitment |

See [references/output-formats.md](references/output-formats.md) for each format's skeleton and gotchas.

## Non-negotiable craft rules

These are the rules a junior designer would miss. Do not miss them.

**Ground hi-fi in real context.** Hi-fi from scratch is a last resort. Ask the user to attach a codebase, design system, UI kit, or screenshots. Read the theme tokens (`theme.ts`, `tokens.css`, `_variables.scss`) directly. If the project has a Figma file, use it as source truth. If not, ask before you design.

**Declare a system before you build.** Before placing pixels, state (in a comment or a visible assumptions block at the top of the HTML): the type scale, the 1–2 background colors, the layout rhythm (grid gap, margin units), the spacing system, and any semantic conventions (e.g., "accent color = action CTA only").

**Respect scale floors.** 1920×1080 slides: body text ≥ 24px, ideally larger. Print documents: ≥ 12pt. Mobile hit targets: ≥ 44px. These are not starting points — they are minima.

**Give options, not "the answer".** Ship 3+ variations that span conservative → novel. Mix obey-the-system variants with ones that remix the visual DNA (scale, fill, texture, rhythm, metaphor, typography weight, accent color logic).

**Avoid AI-design slop.** No aggressive gradient backgrounds. No emoji (unless the brand uses them). No rounded-corner cards with left-border accent stripes. No SVG-drawn imagery as a substitute for real photography. No floating particles or stars. No stock-photo clichés.

**Placeholders over fakes.** Missing an icon, photo, or logo? Draw a labeled placeholder (`[hero image: product on gradient]`). A placeholder is honest; a bad attempt at the real thing is lying.

**No filler content.** Never pad a design with dummy sections, lorem-ipsum paragraphs, or decorative stats just to fill space. If a section feels empty, solve it with layout and composition, not invented copy.

## Technical scaffolding

When writing React prototypes with inline JSX, use pinned versions with integrity hashes and follow strict scope rules — style object name collisions and Babel-scope mistakes cause silent breakage. Use function-scoped styles or CSS Modules to avoid leaks.

For fixed-size content (slides, videos), never hand-roll the scaling logic — use the deck / animation stage patterns in [assets/](assets/). They handle viewport scaling, keyboard navigation, localStorage persistence, and export-readiness out of the box.

For decks, prototypes, and animations, the starter patterns in [references/output-formats.md](references/output-formats.md) are the fastest path to a working skeleton.

## Variations and tweaks

Give the user a way to *compare* variations, not just view them:

- **Multiple static options** → lay them out on a design canvas with labels.
- **Variants of a single prototype** → expose them as in-design **Tweaks** (floating panel or inline handles), not duplicate files.
- **Sequence of screens / slides** → a deck with each screen on a slide.

Tweaks is a specific protocol (registering a message listener, posting `__edit_mode_available`, persisting via `EDITMODE-BEGIN/END` JSON). Read [references/variations-and-tweaks.md](references/variations-and-tweaks.md) for the exact implementation.

## Verification

Before claiming "done":
1. Open the HTML in a real browser. (In Claude Code, use `/browse` — do not use raw `mcp__claude-in-chrome__*` or `mcp__computer-use__*`.)
2. Check the browser console is clean — no 404s, no JS errors, no React mount failures.
3. At fixed-size content (decks, animations): test the scaling on a small viewport; controls (prev/next, play/pause) must stay reachable.
4. Click through at least the primary flow on interactive prototypes.

Don't screenshot-verify your own work speculatively — rely on a real browser load. See [references/verification.md](references/verification.md) for the specific checks per output format.

## File hygiene

- Descriptive filenames: `Landing Page.html`, `Pricing — Option B.html`. Never `output.html` or `design1.html`.
- For significant revisions, copy the file and edit the copy so old versions survive: `My Design.html` → `My Design v2.html`.
- Split large React prototypes into multiple `.jsx` files and import via script tags. Files over ~1000 lines are hard to edit reliably.
- Write media files next to the HTML that uses them, not in a distant shared folder. Keep the artifact portable.
- Use `text-wrap: pretty`, CSS Grid, `oklch()` for harmonious color math, `container queries` for responsive variants — modern CSS is your friend.

## When to stop and ask

If at any point you don't know:
- Which brand/design system applies
- What fidelity the user wants (wireframe vs hi-fi)
- How many variations and on which axis (visuals / flow / copy / motion)
- What the artifact will be used for (pitch deck for board? designer handoff? social post?)

Stop and ask. One round of focused questions up front is faster than three rounds of rework.

Read [references/workflow.md](references/workflow.md) for a checklist of the questions that consistently matter.

## Boundaries

**Do not recreate copyrighted designs.** If asked to recreate a company's distinctive UI, proprietary command structures, or branded visual elements, decline — unless the user works at that company and has permission.

**Do not reveal tool internals.** Users see the design artifact and the process, not your tool inventory. If asked "how did you do that", answer in user-facing terms (what you designed, why, what constraints shaped it), not technical scaffolding.

## Quick reference index

| I need to... | Read |
|---|---|
| Confirm facts before designing (product exists? current version?) | [references/fact-verification.md](references/fact-verification.md) |
| Ask good starting questions | [references/workflow.md](references/workflow.md) |
| Gather brand assets for a specific brand/product | [references/brand-context.md](references/brand-context.md) |
| Propose directions when the brief is too vague | [references/design-styles.md](references/design-styles.md) |
| Avoid visual slop / commit to a system | [references/design-principles.md](references/design-principles.md) |
| Build a deck / canvas / prototype / animation | [references/output-formats.md](references/output-formats.md) |
| Give options the user can mix-and-match | [references/variations-and-tweaks.md](references/variations-and-tweaks.md) |
| Set up React + Babel correctly | [references/react-babel.md](references/react-babel.md) |
| Verify the artifact is solid | [references/verification.md](references/verification.md) |
| Grab a starter template | [assets/](assets/) |
