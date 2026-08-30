# Clarity Skills

A collection of [Agent Skills](https://agentskills.io) for clear documents and
considered design decisions. Each skill lives in `skills/<name>/SKILL.md` and can
be installed independently. The collection also ships as a Claude Code plugin,
following the same repository layout as Godot Skills.

**Homepage:** [index.html](index.html), a self-contained page built with the
included `nutshell` skill. Open it directly in a browser; no build or CDN is needed.

The homepage is available in English, Simplified Chinese, and Japanese. It uses
the browser's language preference initially and remembers a manual selection.
Skill instructions, templates, and agent metadata are maintained in English.
Generated documents can use the language requested by their readers.

## Skills

| Skill | Purpose |
| --- | --- |
| [nutshell](skills/nutshell/SKILL.md) | Documents with contextual explanations, recursive references, explicit constituent concepts, and a glossary only at the end. Includes an offline HTML template. |
| [design-review](skills/design-review/SKILL.md) | Decision-review pages for producers and designers, including per-item verdicts, persistent annotations, JSON export, and revision history. Includes an offline HTML template. |

Document purpose and presentation are independent choices. Nutshell can be used
for explanations, proposals, specifications, tutorials, or reviews. Design review
is a decision workflow that can use this form of presentation; neither skill is
the parent of the other.

## Install

### Agent Skills

Install from a local checkout with the [skills CLI](https://github.com/vercel-labs/skills):

```bash
npx skills add /Users/cnzang/Developer/calarity-skills
```

Choose `nutshell`, `design-review`, or both, and the agents and scope you need.
In Codex, invoke `$nutshell` or `$design-review`.

For local development, this computer uses shared symlinks:

```text
~/.agents/skills/nutshell       -> <checkout>/skills/nutshell
~/.agents/skills/design-review  -> <checkout>/skills/design-review
~/.claude/skills/nutshell       -> ../../.agents/skills/nutshell
~/.claude/skills/design-review  -> ../../.agents/skills/design-review
```

The checkout is `/Users/cnzang/Developer/calarity-skills`. Editing the source skill
also updates these installations. A new agent session may be needed to refresh
the available-skill catalog.

### Claude Code plugin

As an alternative to individual skill installation, use the
[local marketplace workflow](https://code.claude.com/docs/en/plugin-marketplaces):

```text
/plugin marketplace add /Users/cnzang/Developer/calarity-skills
/plugin install clarity@clarity-skills
```

Plugin invocations are `/clarity:nutshell` and `/clarity:design-review`.
Choose either the plugin or individual skills to avoid duplicate entries.

## Homepage and source

`index.html` reuses the Nutshell template's central dictionary and recursive
explanations. References back to an already open concept highlight the matching
ancestor panel without creating another copy or disabling the word.

The site can be served by any static host from the repository root. No public
repository or site URL is configured yet.

The document skills moved from Godot Skills; `design-review` was previously named
`design-review-html`. Their current source of truth is this collection.

## Credit and license

The expandable reading form is inspired by [Nicky Case's Nutshell](https://ncase.me/nutshell/).
This repository contains its own implementation rather than a bundled copy of
the official Nutshell library.

MIT — see [LICENSE](LICENSE).
