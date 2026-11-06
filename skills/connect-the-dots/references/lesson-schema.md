# Lesson page: the `LESSON` object

`assets/lesson.html` is a fixed engine. Copy it, replace only the `LESSON` object between the `EDIT THIS OBJECT ONLY` markers, and publish. The example already in the file (scaling a web service) uses every main field; read it once as a worked example.

Text fields accept light markup: `**bold**`, `==highlight==`, `` `code` ``, blank line = new paragraph, lines starting `- ` = bullet list. Everything is escaped, so source text can't inject HTML.

## Top level

| Field | Required | What it is |
|---|---|---|
| `topic` | yes | Title and "Learning:" line. Also the browser-storage key, so keep it stable across republishes. |
| `goal` | yes | One sentence: what the learner will be able to do. Used on the map and in "I'm lost". |
| `summary` | yes | The whole thing in plain English, 2–3 sentences. |
| `runningExample` | yes | One line describing the single example used throughout. |
| `principle` | yes | The one memorable takeaway for the whole lesson (footer of the map and revision sheet). |
| `source` | for documents | `{title, read:[…], notRead:[…]}`: what was and wasn't readable. |
| `map` | yes | `{nodes, edges}` (below). 3–7 boxes. |
| `steps` | yes | The route, in learning order (below). |
| `ledger` | for documents | One row per source item (below). The Coverage tab and gap check read it. |
| `terms` | recommended | `[{term, meaning, step}]`: `step` = id of the step that teaches it (checkpoint only lists taught terms). |
| `recap` | recommended | `{connections:[…], misunderstanding:{wrong, right}, unexplored:[…]}` for the Revision sheet. |
| `lang`, `dir`, `ui` | optional | e.g. `lang:'ar', dir:'rtl'`, and `ui:{…}` to translate button labels (keys are listed at the top of the engine). |

## Map

```js
map: {
  nodes: [{id:'gw', label:'Logistics Gateway', x:20, y:120, w:180, status:'notread'?}],
  edges: [{from:'gw', to:'bus', label:'publishes event', kind:'async'}]
}
```
- `x, y` are the top-left corner in a ~900-wide space; boxes are 180×62 (override with `w`). That fits 4 columns at x = 20, 260, 500, 740 (60px gaps) or 3 columns with wider gaps. Leave ~100px between rows.
- Arrow labels: 1–3 words ("publishes", "reads status"). Put long names (event names, URLs) in the step text, not on the arrow.
- A data store gets its own box when the lesson teaches it separately; otherwise mention it in its owner's label ("Tracking Service + DB").
- Steps with no box of their own (the problem, a before/after, open points) use `concepts: []`.
- `kind`: `flow` (solid: leads to / calls), `async` (dashed: event or message), `depends` (dotted, open arrowhead), `causes` (thick). A legend is generated from the kinds used.
- `status:'notread'` draws a dashed "(not read)" box for parts of the source you couldn't read.
- Every box should be taught by at least one step (`concepts`); the gap check flags orphans.

## Step

| Field | What it is |
|---|---|
| `id` | Short unique id, e.g. `s3`. Ledger rows and terms point at it. |
| `title` | The concept name, exactly as on the map. |
| `concepts` | Map node ids this step explains (highlighted "we are here"; ✓ once viewed). |
| `why` | "This matters because…" (one sentence). |
| `idea` | The plain explanation. Short paragraphs. |
| `picture` | One layout (below). |
| `pictureNote` | One line: what the picture shows. |
| `simpler`, `example`, `whyLayer`, `deeper` | The four buttons. Write all four: the page has no live model behind it. `example` uses the running example. |
| `connection` | How this links to the previous step and to the goal. |
| `takeaway` | One sentence. Shown in the box at the bottom, and used by "I'm lost" and the Revision sheet. |
| `marks` | `[{kind:'source'|'inference'|'open'|'background', text}]` where the distinction matters. |
| `refs` | `['IA §3.2', 'p. 4', 'US-12']`: only real references. |
| `check` | Optional `{q, a}`: a prediction question, answer hidden until opened. |

## Ledger row

```js
{id:'L7', item:'Gateway is idempotent on courier event id', kind:'text', ref:'HLD §4', step:'s6'}
{id:'L8', item:'Storm-cloud icon', kind:'diagram', ref:'p. 2', status:'skipped', note:'Decoration only'}
{id:'L9', item:'Page 4 content', kind:'text', ref:'p. 4', status:'unreadable', note:'Export failed'}
```
`kind`: `text`, `number`, `diagram`, `workflow`, `ac`, `image`, `code`, or similar. Ledger rows are things *in the source*; your own syntheses (an end-to-end walkthrough) don't need rows. Invented running-example details (names, order numbers) are labelled illustrative in `runningExample`. A row must have a `step`, or a `status` of `skipped` or `unreadable` with a `note`. The Coverage tab shows "Gaps found" until that holds, so open the page (or reason through the check) before handing it over.

## Picture layouts

| `type` | Use for | Fields |
|---|---|---|
| `map` | The concept map again, with other focus | `focus:[nodeIds]`, optional `map` (a different `{nodes, edges}`) |
| `compare` | Two options side by side (scale up vs out, build vs buy, today vs proposal) | `left`, `right`: `{title, subtitle, tagline, tone:'warm'|'cool', visual?, pros:[], cons:[], when}`; optional `verdict` |
| `beforeAfter` | What a change introduces | `before`, `after`: `{title, items:[text or {text, tag:'new'|'changed'|'removed'}], caption?, visual?}`; `arrowLabel`, `caption` |
| `flow` | A linear process / workflow | `steps:[{label, sub?, who?, kind?:'decision'|'end', branch?}]`, `arrows:[labels between steps]`, `focus:index` |
| `sequence` | Who talks to whom, in order (up to ~6 participants) | `participants:[{id,label}]`, `messages:[{from,to,label,kind?:'async'|'reply'} or {note, over}]`, `focus:index` |
| `cards` | A few related ideas, fixes or facts (like "Fix the tail") | `items:[{icon?, title, text?, caption?}]`, `focus:index` |
| `table` | Exact comparisons, mappings, responsibilities | `columns:[]`, `rows:[[]]`, `focus:rowIndex` |
| `figure` | Explaining a source image or diagram element by element | `src`, `alt`, `pins:[{x:%, y:%, label, text}]` |
| `svg` | Anything else, drawn per `visual-style.md` | `svg:'<svg…>'` |

`compare.left.visual` and `beforeAfter.*.visual` take any picture object, so you can nest a small flow or cards inside a side.

Branching flows (decisions with several paths) are clearer as a `map` with positioned boxes than as a `flow`; use `flow` for straight-line processes with an optional `branch` note per step.

For `figure`, `src` must be a URL the published page can load: upload the image to the artifact's asset store, or use a `data:` URI for a small image. Pin every element listed in the ledger for that image, so the picture itself is fully explained.

## Before publishing

1. Every ledger row has a step or a reasoned status; every map box is taught by a step.
2. Every step has all four layers, a connection and a takeaway.
3. Names are identical across map, steps, terms and pictures.
4. Numbers and labels match the source exactly; illustrative numbers are labelled as such.
5. Set the page `<title>`: the engine sets it from `topic` at runtime, but also edit the static `<title>Lesson</title>` to the topic so the published page is named correctly.
