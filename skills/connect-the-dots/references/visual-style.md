# Visual style for SVG / HTML visuals

Goal: a clean, modern product UI (think a 2026 design tool or docs site), with the precision of a technical diagram. The learner asked for this explicitly: no handwriting fonts, no italics, no graph paper, no "sketchy" strokes. When style and legibility conflict, legibility and correctness win.

`assets/lesson.html` already implements this; use it for lessons and copy its tokens for any one-off visual.

## Layout

- Calm background (`--bg`), content on white cards (`--card`) with a 1px hairline border, 20px radius and a soft shadow.
- Generous whitespace: 28px card padding, 16–24px between blocks.
- A clear title; small uppercase eyebrow labels (0.7rem, letter-spacing .08em, muted) for section names such as "Takeaway" or "Example".
- **Small, distinct sections**, 2 to 4 per sheet, each in its own soft panel (`--paper`).
- Short labels (1–4 words) beside each box or arrow, never paragraphs inside a diagram.
- Arrows show relationships and always carry a short verb label.
- **One takeaway** at the bottom in a tinted panel (accent-to-teal gradient at ~10% opacity), not a heavy outlined box.
- Max ~9 boxes per diagram. If you need more, split into map + zoom-in.

## Sheet designs

All are built into `assets/lesson.html` (see `lesson-schema.md`); use the same shapes for a one-off sheet.

| Design | Looks like | Use for |
|---|---|---|
| **Concept map** | Rounded boxes with hairline borders, muted arrows with small labels, the current box tinted with the accent | One topic with a few connected parts |
| **Split comparison** | Two soft panels side by side; each has a title with a small colour square, a muted subtitle, Pros / Cons with small round ✓ / ✕ badges, and a "When to use" row | Two options, approaches or states |
| **Before → After** | Two panels joined by a small arrow; the "after" panel tinted with the accent; items tagged NEW / CHANGED / REMOVED as small pills | What a change introduces |
| **Cards** | A grid of soft panels, each with an icon, a semibold title, one muted line and a small uppercase caption | A family of techniques, risks, fixes or open questions |
| **Flow strip** | Numbered step panels left to right with small labelled arrows; the current step tinted; failure branches in red under a step | A workflow or process |
| **Annotated original** | The source image with numbered accent pins and a numbered legend | Explaining a diagram from the source |

If the learner shares sample notes, borrow their *structure* (sections, pros/cons, one decision rule), not a hand-drawn look, unless they ask for it. Don't copy their errors: cut-off labels, numbers that don't add up, misspellings, generalisations stated as absolutes.

## Palette (stable meanings across a lesson)

| Token | Light | Dark | Meaning |
|---|---|---|---|
| `--bg` | `#f6f6f8` | `#0e0e12` | Page background |
| `--card` | `#ffffff` | `#17171d` | Cards, boxes |
| `--paper` | `#f2f2f5` | `#1d1d24` | Inner panels, table headers |
| `--text` | `#16161d` | `#ececf1` | Text and box labels |
| `--muted` | `#6b6b78` | `#9a9aa8` | Secondary text, arrows, not read / done earlier |
| `--line` | `rgba(17,17,26,.10)` | `rgba(255,255,255,.08)` | Hairline borders |
| `--accent` | `#5b5bd6` | `#8b8bf5` | **Current focus / what's being explained**, primary buttons |
| `--cool` | `#0e9384` | `#3fd0bd` | Second option in a comparison |
| `--good` | `#16a34a` | `#4ade80` | Outcome, success path, pros |
| `--bad` | `#dc2626` | `#f87171` | Failure, risk, cons, open questions |

Tints use the same colour at ~10% opacity (`--accent-bg`, `--good-bg`, ...). Colour never carries meaning alone. Pair it with:
- current focus → tinted fill + thicker accent stroke + "◀ we are here" label
- failure → ✕ badge + label
- not read → dashed outline + "not read" label
- async / event → dashed arrow; depends on → dotted arrow; sync call → solid arrow (state this in a tiny legend)

## Type

- One family: **Inter** from fonts.googleapis.com (`wght@400;500;600;700`), falling back to `system-ui`.
- Weights do the hierarchy: 700 for page titles, 600 for headings and box labels, 400–500 for body.
- Sizes: body 16px; headings 1.75rem / 1.1rem; labels in diagrams 12–14px; eyebrow labels 0.7rem uppercase.
- **No italics anywhere** (set `em,i{font-style:normal}`), no handwriting fonts, no underlines except on hover links.
- Slight negative letter-spacing (−0.02em) on headings only.

## Shapes

- Boxes: `rx` 10–14, fill `--card`, stroke `--line-strong` at 1.5px. Focused box: fill `--accent-bg`, stroke `--accent` at 2.5px.
- Arrows: `--muted` at 1.6px with round caps, small filled arrowheads in the same colour; the one causal or highlighted arrow in `--accent`.
- Edge labels: 12px, `--muted`, with a `--card` halo (`paint-order:stroke`) so they stay readable over lines.
- No grid backgrounds, filters, textures or clip-art. Small glyph icons (✓, ✕, ⏱, 👤) only when they label something.

## Minimal SVG skeleton

```html
<svg viewBox="0 0 800 300" role="img" aria-labelledby="t d" xmlns="http://www.w3.org/2000/svg">
  <title id="t">Order flow — we are at Payment</title>
  <desc id="d">Customer sends order to Order Service, which asks Payment to charge the card.</desc>
  <defs>
    <marker id="arrow" viewBox="0 0 10 10" refX="9" refY="5" markerWidth="8" markerHeight="8" orient="auto-start-reverse">
      <path d="M0 0L10 5L0 10z" fill="var(--muted)"/>
    </marker>
  </defs>
  <text x="24" y="40" class="h">Order flow</text>

  <rect x="40" y="100" width="150" height="56" rx="12" class="box"/>
  <text x="115" y="133" class="lbl" text-anchor="middle">Customer</text>

  <rect x="320" y="100" width="170" height="56" rx="12" class="box"/>
  <text x="405" y="133" class="lbl" text-anchor="middle">Order Service</text>

  <rect x="600" y="100" width="160" height="56" rx="12" class="box focus"/>
  <text x="680" y="133" class="lbl" text-anchor="middle">Payment</text>
  <text x="680" y="178" class="here" text-anchor="middle">◀ we are here</text>

  <path d="M190 128H316" class="arrow" marker-end="url(#arrow)"/>
  <text x="253" y="118" class="note" text-anchor="middle">sends order</text>
  <path d="M490 128H596" class="arrow" marker-end="url(#arrow)"/>
  <text x="543" y="118" class="note" text-anchor="middle">charge card</text>

  <rect x="24" y="220" width="752" height="56" rx="14" class="takeaway"/>
  <text x="44" y="254" class="h2">No payment, no order: Payment is the gate.</text>
</svg>
```

```css
.h { font: 700 22px Inter, system-ui, sans-serif; fill: var(--text); letter-spacing: -.02em; }
.h2 { font: 600 16px Inter, system-ui, sans-serif; fill: var(--text); }
.lbl { font: 600 14px Inter, system-ui, sans-serif; fill: var(--text); }
.note { font: 500 12px Inter, system-ui, sans-serif; fill: var(--muted); }
.here { font: 600 12px Inter, system-ui, sans-serif; fill: var(--accent); }
.box { fill: var(--card); stroke: var(--line-strong); stroke-width: 1.5; }
.box.focus { fill: var(--accent-bg); stroke: var(--accent); stroke-width: 2.5; }
.arrow { stroke: var(--muted); stroke-width: 1.6; fill: none; stroke-linecap: round; }
.takeaway { fill: var(--accent-bg); stroke: none; }
```

Define the colour tokens on `:root`, redefine them under `prefers-color-scheme: dark` (and `:root[data-theme="dark"]`), and give `body` an explicit background. Make the SVG `width:100%; height:auto` with a `max-width` near its viewBox width so small diagrams don't blow up on wide screens.

## Correctness checks before showing a visual

- Every box name matches the lesson's concept names exactly.
- Every number on it comes from the source (or is clearly marked as illustrative).
- Percentages that should add up do add up.
- Arrow directions match the real direction of the call, data or cause.
- No label is cut off at a box or frame edge (size note boxes to their text).
- The takeaway line is true.
