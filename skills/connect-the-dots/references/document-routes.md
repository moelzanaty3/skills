# Routes by document type

A route is the ordered set of questions the lesson answers. Pick the closest one, adapt it, and drop steps the material doesn't support (say which ones you dropped and why).

## High-level design (HLD) / architecture

Route (each step is usually one chunk):

1. **What problem are we solving?** In business terms first, then technical.
2. **What exists today?** The "before" picture. Small diagram.
3. **What changes?** Before/after view; highlight new and changed parts.
4. **What does each component do, and why is it needed?** One component per chunk if there are many; for each: its job in one line, what breaks without it.
5. **One real request, end to end.** Sequence diagram following the running example (e.g. "Sara places an order"). Mark where **decisions** are made and where **data is changed or stored**.
6. **What happens when something fails?** Pick the most important dependency, walk the failure: who notices, what the user sees, retries/fallbacks, what the doc says vs leaves open.
7. **Trade-offs and open questions.** What the design chose, what it gave up, what isn't decided.

Tips:
- Build the component list once and keep it stable; every later diagram uses the same boxes in the same positions.
- Distinguish sync calls, async events and data stores visually (solid arrow / dashed arrow / cylinder) and say so in a legend.
- Name services as the doc names them. If the doc is inconsistent (e.g. "OMS" and "Order Service"), pick one, say they're the same in one line, flag it as an open question if unsure.
- NFRs (latency, availability, security) belong next to the component they constrain, not in a separate abstract list.

## Impact assessment (IA), Epic, stories, acceptance criteria

The chain to make visible:

**Business need → user behaviour → system change → affected components → work required → dependencies → evidence it works**

Route:

1. **The business need** — why anyone is spending money on this. One customer persona as the running example.
2. **What the customer will do differently** — their journey, before vs after.
3. **What has to change in the system** — screens, rules, data, services.
4. **Who/what is affected** — components and teams *as named in the source*.
5. **The work** — Epic → features → stories, shown as a small tree. Map each story back to a link in the chain above ("US-3 exists because of step 2: the customer now sees …").
6. **Dependencies** — what must happen first, who else must deliver. Draw as a dependency graph, not a timeline, unless dates are in the source.
7. **Evidence it works** — acceptance criteria and success measures; for each AC, say which customer behaviour it proves.

Rules:
- Never invent owners, estimates, scope, dates or ACs. Missing → open question, with who would likely know *only if the source says*.
- If stories contradict the IA or each other, show the contradiction plainly once (a two-column "IA says / Story says" table) rather than smoothing it over.
- For a big backlog, teach the *shape* (features and the chain) first, then individual stories on request.

## Multipage artifacts, wikis, slide decks

- Build the map from the *ideas*, then show which pages hold them (a small "page → concept" table helps when pages are out of learning order).
- Keep a visible coverage note: "Read: pages 1–6, 8. Not read: page 7 (embedded video)".

## Code

- Start from what the code is *for* (the behaviour a user would notice), then the entry point, then follow one input through.
- Name real files/functions only; quote small snippets, never whole files.

## Non-technical topics (e.g. a legal process, a health insurance claim, a history topic, a recipe science question)

Use the same scaffolding, but use the subject's own vocabulary — don't force "components", "services" or "flows" onto it.

Typical route: **the goal or problem → the main actors or parts → how they connect (cause, sequence, dependency) → one real case walked through → where it commonly goes wrong → the takeaway.**

Visuals: concept map for relationships, timeline for history, flowchart for processes, before/after for change, annotated sketch for physical things.

## Partially readable material

- First line after the plain-English summary: what you read, and what you couldn't (pages, images, encrypted files, broken links). Be specific.
- Build the map only from what you read. Mark unknown areas on the map as "not read" boxes (dashed outline + label) rather than guessing their content.
- Offer one concrete way to fill the gap (e.g. "paste pages 7–9 as text" or "export as PDF").
