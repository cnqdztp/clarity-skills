---
name: design-review
description: >-
  Write decision-review HTML pages for a producer or designer who will read,
  annotate, and decide on proposals. Includes contextual term explanations,
  per-item verdicts, locally saved notes, annotation JSON export, and a revision
  workflow that preserves earlier decisions. Use for design proposals, plans,
  audits, or reviews prepared for a human decision-maker, not private notes or
  intermediate agent-to-agent material.
---

# Design review

Give a decision-maker enough context to assess a proposal and respond to each decision separately. These rules cover both the initial draft and revisions after feedback.

## 0. When to use it

Use for game-design proposals, design-oriented accounts of technical changes, audits, content or terminology proposals, and other documents awaiting individual decisions. Do not apply it to private notes, intermediate agent material, or engineering plans concerned only with implementation.

The default deliverable is HTML using the supplied template. If the user requests Markdown, retain the writing and entry-structure rules and replace interactive explanations with parenthetical or immediately adjacent definitions. Do not leave readers dependent on a glossary at the end. Write the deliverable in the requested language; these English instructions do not impose English on its audience.

## 1. The page is decision material

Prepare for the full feedback cycle:

1. Draft the page using the rules below.
2. Obtain a neutral review before delivery, as described in section 9.
3. Let the reader mark each item Accept, Revise, or Reject and leave a note. Export all annotations as JSON.
4. Record returned annotations individually in the corresponding decision log under `docs/qa/`. Quote the user's feedback and identify any earlier ruling or pending proposal it overturns. Ask about ambiguous feedback rather than guessing.
5. Update authoritative documents, then revise this page. Preserve overturned text with `<s>` instead of deleting it, as described in section 7.

An annotation belongs to a unit the reader can accept or reject independently. A section or proposal card can have its own annotation; one note box for the entire page is insufficient. Multiple boxes in one section are allowed. Each `.verdicts` element must have a page-wide unique `data-id` that remains stable after publication. It is the annotation JSON identifier used to match returned feedback to the proposal.

## 2. Perspective and prose

### 2.1 Write for the designer

Start with what the player experiences, not with a disconnected function or implementation detail.

| Avoid | Prefer |
| --- | --- |
| The combiner uses argmax for the highest score. | Every ordinary encounter on this route contains the same enemies. |
| Regional preference is a no-op in reward calculation. | The reward does not change with the region where the battle takes place. |
| Duplicate weighting in the loot screen is dead code. | Every keyword has the same chance of appearing. |
| The missing `%DrawRow` node prevents the connection. | The player cannot buy the extra-card-per-turn upgrade on the evolution screen. |

Implementation details can appear in a short note about affected code or cost at the end of an item. They should not be the subject of the proposal.

### 2.2 Prefer established terminology

Choose names in this order:

1. **Established names.** Use industry terms, including their original English names when appropriate: Hash, Overkill, Reprint, pity system, build, modifier, drop rate, and balancing. Avoiding jargon does not mean replacing precise names with literary metaphors. Preserve names used by this project's code and documents, but do not impose project-specific vocabulary elsewhere.
2. **Plain descriptive phrases.** When no established name fits, use ordinary words whose meaning is clear on first reading: conversion table, reference graph, or grouping by row.
3. **Do not invent metaphors.** A project may have a few metaphors its readers already accept, such as a gate or building blocks. Keep those exceptions local. They are not permission to invent a new metaphor for every mechanism.

### 2.3 Remove programmer shorthand

Replace implementation language with what it means for this reader. For example, "argmax" can become "always selects the same option," "fallback" can become "returns to the built-in version," and "no-op" can become "has no effect." Instead of calling something a consumer or a hook, say who reads it or where it appears.

The test is whether a player or designer can understand the sentence without reading the code.

### 2.4 Avoid unrelated industry metaphors

Financial, business, and management metaphors are also jargon when they do not describe anything happening in the game. Say "the actual cost" instead of "the bill we owe," or "whether to write all of this now" instead of inventing an installment-payment analogy.

### 2.5 Do not manufacture abbreviations

Compressing a sentence into a new label does not make it a term. Prefer "how many keywords are available" to an invented label such as "pool width" unless that name is established for this audience. Saving a few words is not worth interrupting comprehension.

### 2.6 Writing constraints

- Do not use the middle-dot separator, U+00B7. Use punctuation, hierarchy, or spacing.
- For a game-design audience, discuss gameplay decisions rather than recasting them as product-management decisions.
- Do not add section introductions that merely announce the heading again. Do not put reading instructions, implementation notes, or restatements of the user's requirements into the delivered page. Let actual controls identify their actions.

## 3. The structure of a decision item

Give each item the following context, in order:

1. **Current situation:** a concrete encounter, action, or interface the player experiences.
2. **Problem:** which part of the experience suffers and why. "It differs from the design" is not enough.
3. **Proposed change:** the actual rule, with numbers when they are known.
4. **Player impact:** how the experience changes. If the player will not notice directly, explain why the work is justified.
5. **Cost:** content work, balance effects, necessary retesting, and anything the change invalidates. End with a `.cost` paragraph. If there is no additional cost, state that explicitly.

Group related items and order them by their effect on players rather than implementation difficulty.

## 4. Keep the proposal proportionate

Do not turn a one-sentence requirement into an elaborate system. Remove emotional justifications added to make an unnecessary rule sound important. Cut unsolicited "we could also" additions. When a fixed value and small variation solve the problem, do not introduce a mechanism players must learn separately.

Prefer the smallest rule that can be explained clearly.

## 5. Contextual definitions

The template uses the reading form of [Nutshell](https://ncase.me/nutshell/): explanations open in context rather than sending readers to another page or footnote. Maintain all definitions in one central `NUTS` object:

```js
var NUTS = {
  'Pity system': 'After repeated attempts without a rare reward, the chance increases until a success resets it. Its starting probability comes from the base [[Drop rate|drop rate]].',
  'Drop rate': 'The probability of receiving a reward in one attempt.'
};
```

- **One source.** Change a definition in the dictionary; body expansions and the final glossary must use the same text.
- Use `<button class="nut" data-term="Pity system" aria-expanded="false">pity system</button>` where a term first needs explanation. It need not be interactive every time it repeats in a paragraph.
- Explain what the term means in this game. State its decision status when relevant. Two or three sentences are usually enough; split longer explanations into smaller concepts when helpful. A combined concept must name its constituents and their relationship.
- `[[term]]` creates a nested reference; `[[term|label]]` supplies a display label. The existing review template warns about unregistered terms and renders them as text; resolve every warning before delivery.
- The review template's legacy cycle guard renders terms already on the ancestor path as noninteractive text. When a task calls for the current Nutshell interaction, follow the installed `nutshell` skill's clickable ancestor highlights instead.
- Generate the final glossary from the dictionary into `<dl id="glossary-list">`; never maintain a handwritten copy. Keep term lists out of the main text.

The self-contained implementation is in [template.html](template.html). Review is the document's purpose; Nutshell is one possible presentation, not a prerequisite for every review format.

## 6. Annotations

Give every independent decision a `.note-box` with Accept, Revise, and Reject buttons (`data-v` values `accept`, `revise`, and `reject`). Selecting the active verdict again clears it. Include a free-text `textarea`.

Keep a sticky top bar with a Copy annotations JSON button. Export this structure:

```json
{
  "notes": [
    { "id": "budget", "title": "How the power budget affects play", "verdict": "revise", "note": "Use the curve as a reference, not a restriction." }
  ]
}
```

The identifier comes from the box's `.verdicts` `data-id`. Derive the title from the nearest preceding h2, h3, or h4, stripping numbering and status chips. Do not maintain a second title map. An item without a verdict still exports with an empty verdict string.

Preserve the template's local persistence and copy behavior. Notes and verdicts save immediately in `localStorage`, scoped by page path. If the Clipboard API is unavailable, try the existing hidden-textarea copy path; if that also fails, show a visible textarea for manual copying. Do not silently lose feedback.

## 7. Revisions and decisions

- Wrap overturned statements in `<s>` and follow them with the new ruling and date. Markdown strikethrough syntax does not work in HTML.
- Use Decided, Proposed, and Open status chips, with the template's green, cyan, and yellow treatments. Update the status instead of erasing the chapter's history.
- Revise the same file. Put the draft version in `.doc-date`; keep the filename, title, favicon, and published artifact URL stable so readers do not retain a link to an abandoned draft.
- Near the end, provide a decision index with each decision, its location, and its source: a decision-log entry, an authoritative-document section, or an explicitly unrecorded verbal decision requiring confirmation.
- Give each open question its own annotation box. An unresolved issue still needs a response.
- State the document's sources in the footer before the final glossary. Do not add instructions for operating the page there.

## 8. Template, paths, and layout

Start by copying [template.html](template.html), then replace its example content. Store review pages under `docs/review/YYYY-MM-DD-topic-review.html`; show the date and draft version in `.doc-date`. Keep the same file for revisions. Provide the local path, or publish an artifact when authorized by the user.

Preserve these template behaviors:

- Light and dark tokens, system color preference, both directions of `data-theme` override, and matching `color-scheme` values.
- Readable system fonts, serif headings, generous line spacing, and a narrow reading column. Adapt typography to the document's language.
- `scroll-margin-top` on `section.block` so navigation targets are not hidden by the sticky bar.
- Visible keyboard focus and relevant `aria-expanded` and `aria-pressed` states. Respect reduced motion.
- A single file without CDN or webfont requests, suitable for opening locally or publishing as an artifact.

Use `pre.flow` or `pre.ir` for compact text diagrams. Wrap wide tables in `.table-scroll`; numeric columns can use `td.num` for tabular numerals. The page itself should not scroll horizontally.

Number sections only when decision records or annotation replies refer to them by number. Use `<span class="secnum">1</span>` so exported titles can omit the number. Do not add decorative numbering.

## 9. Review before delivery

After finishing the draft and all other work in the same batch, ask a neutral review agent to assess it. When this review applies and delegation is authorized, explicitly select a model one tier below the current session rather than inheriting the model or choosing the strongest tier. Cover these perspectives:

1. **Language:** find unclear shorthand, invented terminology, and unrelated metaphors using section 2.
2. **Facts:** trace assertions to code or sources. Watch for comments that promise more than the code implements, and completed-looking scaffolding that has no caller.
3. **Proportionality:** check whether the actual requirement warrants the proposal's complexity.

Resolve findings before presenting the page. A user's explicit waiver applies to that document. Current user instructions and authorization boundaries take precedence over this workflow.
