# Project handoff: discord-writer

## Current state

The project reached version **1.7.2** through iterative real-world Discord rendering tests on desktop and mobile. As of 1.5 the specification and all templates/examples are English-only; output language still follows the user.

The project began as a formatting helper and evolved into a generic content-aware Discord renderer with:

- mobile-first rendering rules,
- native Markdown and hybrid post layouts,
- multiple responsive change representations,
- changelog, status, callout, checklist, metadata, link and timestamp patterns,
- user-configurable style profiles,
- generic content profiles based on information shape rather than domain.

## Most important architectural decision

The base skill must stay domain-neutral.

A content profile describes structures such as:

- component-set
- changelog
- procedure
- schedule
- status
- comparison
- faq
- reference-list

It must not encode assumptions about a particular game, product, brand, company, or industry.

## Empirical Discord findings already incorporated

The current rules are based on actual rendering tests, including:

- native Markdown works well on mobile and desktop,
- narrow codeblock layouts are safer than wide tables,
- around 36 visible monospace characters is a conservative mobile target,
- ~40 characters is a practical upper edge observed in testing,
- wide 4+ column tables are not a safe generic mobile layout,
- `diff` renders usefully on tested mobile and desktop clients,
- ANSI color was not reliable on the tested mobile client,
- `>>>` effectively quotes the remaining message, so bounded multiline callouts should prefix each line with `>`,
- `[x]` / `[ ]` are not native interactive Discord checkboxes; Unicode checklists are preferable,
- `<URL>` suppresses link previews in the tested client,
- masked links remain readable but may still generate previews,
- Discord timestamps render locally and are useful for date/time presentation.

Treat these as regression-sensitive behavior.

## What to improve next

Good next tasks include:

1. review `SKILL.md` for duplication and reduce redundancy without losing rules,
2. create a compact regression-test document containing representative output cases,
3. formalize the style-profile schema and content-profile schema while keeping natural-language configuration supported,
4. add validation guidance for conflicting user preferences,
5. improve repository documentation and examples,
6. set up lightweight GitHub issue/PR templates only if they add value.

## What not to do

- Do not specialize the base skill for a particular use case.
- Do not infer missing domain fields.
- Do not force tables where responsive records/lists are safer.
- Do not use fake syntax highlighting as decoration.
- Do not treat desktop-only behavior as cross-platform behavior without testing.

## Suggested first Claude Code prompt

Use this after opening Claude Code in the repository:

```text
Read CLAUDE.md, PROJECT_HANDOFF.md, GENERICITY.md, and SKILL.md completely before making changes.

First, audit the repository without editing anything. Summarize:
1. the current architecture,
2. duplicate or contradictory rules,
3. gaps in documentation or test coverage,
4. any places where domain-specific assumptions accidentally remain,
5. a proposed low-risk cleanup plan.

Preserve the project's tested mobile-first Discord behavior and domain neutrality. Do not redesign the skill yet. Wait for my approval after the audit.
```

## GitHub publication

Published as a public repository under the MIT license: https://github.com/gomaaz/discord-writer
