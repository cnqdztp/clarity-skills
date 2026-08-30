---
name: nutshell
description: >-
  Create or edit document pages in the Nutshell style, with in-context term
  expansion, recursive explanations, and a glossary at the end. Use for
  expandable documents, inline definitions, or requests to explore concepts
  without leaving the text. Applies to any document purpose; does not
  automatically add a review workflow.
---

# Nutshell

Make documents that readers can explore in place. The main text carries the argument; readers can expand a concept when they need context, then explore other concepts inside its explanation.

## Scope

Document purpose and presentation are independent choices. Reviews, explanations, proposals, specifications, tutorials, consensus records, and formal archives can all use Nutshell. A review can also use another presentation. Neither is a superset of the other. This skill is independent of any industry, engine, or review skill.

Organize the document around its readers, sources, and purpose. Do not add a fixed five-part structure, verdict buttons, annotations, decision logs, mandatory review, or another workflow merely because the page uses Nutshell. Do not introduce expandable explanations when the user asks for an ordinary table or explicitly does not want them. Write the deliverable in the requested language; the repository's English instructions do not limit its audience.

## Content and explanations

- **Make the main text readable on its own.** Explain the subject, background, and necessary conditions. Do not present isolated conclusions, code names, or implementation details. Keep essential conclusions and limitations in the main text. Expansions deepen understanding without becoming prerequisites for following the argument.
- **Explain terms where they are needed.** Keep explanations near the paragraph containing the term. Readers should not have to jump pages or consult footnotes. Section folding and single-level hover tips do not replace recursive concept expansion. Do not make every ordinary word interactive or repeat the same trigger throughout a short paragraph.
- **Use accurate, established names.** Prefer industry terminology and names already adopted by the project. Preserve technical terms and original-language names when useful, explaining them for the audience. Do not replace precise names such as Hash, Overkill, or Reprint with invented metaphors. Different projects do not share a mandatory vocabulary.
- **Explain the meaning in this context.** Start with a concrete situation before introducing an abstract definition. Leave out unrelated encyclopedic detail. Break complex explanations into smaller concepts when helpful; do not impose a mechanical sentence limit.
- **Name the components of a combined concept.** When several concepts are integrated into a new concept, its expansion must name the constituent concepts and explain their roles in the whole. Reference those components through the central dictionary so they can be explored further. A broad label is not enough. Do not invent combined concepts merely to demonstrate recursion, and do not confuse related concepts with constituent parts.
- **Put the glossary only at the end.** Do not insert term lists, glossaries, or preliminary definitions into the main text, at the top of the page, or before sections. Keep contextual expansion in the body and provide one complete glossary at the end. Naming a combined concept's components inside its own explanation is not a separate glossary.
- **Keep the document independent of the conversation.** Avoid phrases such as "as you said earlier," "from our discussion," or "in this review." Do not add skill labels, implementation explanations, change logs, restatements of the user's requirements, or instructions such as "click the underlined terms." Headings and interactive words should identify themselves. Preserve sources, dates, and factual status when the document requires them.

## Central dictionary

Maintain one set of definitions per document. Both contextual expansions and the final glossary read from it; do not duplicate definitions manually. This is document-level organization, not a request to build a terminology service across projects.

The template uses a `NUTS` object. Definitions are plain text. `[[term]]` references another definition; `[[term|label]]` uses the same definition with different display text.

```js
const NUTS = {
  'Registration confirmation': 'Registration confirmation combines [[Eligibility check]] with [[Place reservation]]. The first checks the conditions for attending; the second holds an available place. Registration is confirmed only after both steps.',
  'Eligibility check': 'Compare the application with the conditions for attending.',
  'Place reservation': 'Hold one of the remaining places for the applicant so it cannot be assigned twice.'
};
```

The combined concept explicitly names its two components rather than merely mentioning related ideas. Do not repeat them as a term list in the body.

Use a native button for a reference in the main text:

```html
<button type="button" class="nut" data-term="Registration confirmation">registration confirmation</button>
```

Every reference must have a real definition. Recursive references remain clickable, including references to a concept already open on the current path. Clicking such a reference briefly shimmers the corresponding ancestor explanation, scrolling it into view if necessary. Do not disable the button, replace it with gray text, or create a duplicate panel. Preserve the open hierarchy.

Resolve the target by the current expansion path and panel instance, not by the first matching term anywhere on the page. A glossary definition can also be the ancestor. The same concept on another path can expand independently. Repeated clicks should restart the highlight. Under reduced motion, keep only a brief outline highlight. Do not limit reading with a global visited set or silently ignore misspelled or missing definitions.

## Build the page

1. Read the actual material and identify the audience, main argument, and concepts that need explanation. Preserve facts and uncertainty. Do not invent rules, agreement, or conclusions to fill a template.
2. Read and reuse the dictionary and expansion mechanism in [assets/document.html](assets/document.html). Its event-registration rules are fictional examples. Replace the sample body, title, language, and dictionary for the real deliverable. Resolve the asset relative to this `SKILL.md`, not a particular agent's installation directory.
3. Design around the subject. Use paragraphs, tables, diagrams, or sections as appropriate without mechanically copying the example's section count, colors, or structure. For an existing document, preserve its suitable design and integrate the mechanism.
4. Add triggers where concepts occur, define recursive references in the dictionary, and explain the components of combined concepts. Put sources, appendices, and other content before the glossary. Generate the final content section from the same dictionary.
5. Default to a self-contained HTML file that opens directly without a CDN, build tool, or server. Follow the specified environment when the user asks for site integration or the official Nutshell library. This skill adopts the reading form; it does not require the official library for every document. Do not publish externally without authorization.

The template includes natural wrapping, local scrolling for wide tables, light and dark themes, keyboard-operable native buttons, and `aria-expanded` state. **An expansion must not split its source sentence.** Place a panel after the complete text of the trigger's containing text block, not immediately after the button with the rest of the sentence below the panel. Nested panels likewise follow the complete parent explanation while remaining inside its box. Closing a panel removes only that explanation and its descendants; the source text stays in place. Put body triggers inside complete text blocks such as paragraphs, headings, list items, or table cells. Printing should show the main text and complete glossary without duplicate expanded panels.

For a requested format without scripting, preserve the context and final glossary, and use short parenthetical or adjacent explanations where terms occur. State that the result has no clickable recursion; static folding markup is not an implemented Nutshell page.

## Delivery checks

Finish the content, page, and other work in the same batch before performing necessary checks. Do not add an unrelated review process. Check that the body has no glossary, combined concepts name their components, references resolve, root and nested concepts expand and collapse, and complete source sentences remain above their expansions. Cyclic references must still highlight the matching ancestor on the current path. This skill does not require hashes, smoke tests, or multiple review agents; respect the user's validation scope.

Distinguish verified facts, observed browser behavior, and checks that were not run. Do not claim browser validation after only writing or inspecting source.

Reading-form reference: [Nicky Case's Nutshell](https://ncase.me/nutshell/). Cross-site embedding and other capabilities of the official library are not default requirements of this skill.
