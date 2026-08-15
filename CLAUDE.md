# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Project

This repository contains `discord-writer`, a domain-neutral skill for rendering arbitrary information as robust Discord messages.

There is no source code, no build system, no dependencies and no test runner. The deliverable is a **specification that another assistant executes at runtime**. `SKILL.md` (~2100 lines) is the single canonical source of behavior; every other file clarifies, configures, tests or demonstrates it.

**Language:** talk to the user in German; write everything in the repository in English — `SKILL.md`, CHANGELOG entries, templates, examples, YAML comments. Do not confuse this with the skill's *output* language: the rendered Discord message follows the user's language (`SKILL.md` § 15, rule 2), and the English default labels are translated accordingly.

## Read first

Before changing anything, read:

1. `PROJECT_HANDOFF.md`
2. `SKILL.md`
3. `GENERICITY.md`
4. `README.md`
5. templates and examples only as needed

## Verification instead of tests

There is nothing to build or lint. Changes are verified by **rendering, not by running commands**:

- `SKILL.md` § 18 holds the regression suite `R1`–`R10` (normal post, short `ALT → NEU`, long change, multi-dimensional record, `diff`, hybrid post, changelog, callout + status list, metadata + timestamp, links). Each case has explicit acceptance criteria.
- To "run a single test": produce the output for that case according to the changed rules and check it against that case's acceptance criteria — mentally for layout rules, in a real Discord client (desktop **and** mobile) for anything that touches rendering behavior.
- § 26 is the pre-output quality checklist that applies to every generated message; § 31.6 adds the extra checklist for multi-component content.
- Rules justified as "empirically tested" must not be changed on theory alone. New behavior claims need a new rendering test.

## Architecture

### Layer model

The skill is a content-aware rendering engine, not a template filler. Three configuration layers sit on top of the skill defaults:

```text
Global Style Profile   (visual signature: tone, density, emoji, links, splitting)
   ↓
Content Profile        (structure of a recurring information shape)
   ↓
Current Post Override  (one-off deviations for a single post)
   ↓
Content-aware Safety Adaptation
   ↓
Discord Output
```

The binding precedence for the final rendering (`SKILL.md` § 21, § 34) is:

1. technical hard rules — never overridable by a profile
2. explicit instruction for the current post
3. global style profile
4. active content profile
5. semantic content-aware adaptation
6. skill defaults

`adaptation.policy` (`silent-safe` | `explain-on-deviation` | `strict`) only controls how loudly the skill deviates — never whether hard rules apply.

### Format selection

The core principle (§ 1) is that the **shape of the information** picks the layout, not visual appeal: prose → native Markdown, short replacements → compact list, long replacements → hanging change, object with many properties → record layout, added/removed → `diff`, release notes → changelog categories, warnings → blockquote callouts, and so on. § 17 is the decision matrix (A–R) that operationalizes this, and § 28 is the 28-point short form of all key rules.

### Configuration formats

A style profile may be YAML (`discord_writer:` root key, full reference schema in § 22.1) **or** plain language (§ 22.3) — both are equally valid input; the skill translates prose into profile fields internally. Content profiles live under `discord_writer.content_profiles.<name>` and inherit everything they do not override.

Files backing this:

- [templates/GUIDELINE-TEMPLATE.yaml](templates/GUIDELINE-TEMPLATE.yaml) — full style profile with all content profiles present but `enabled: false`
- [templates/CONTENT-PROFILE-TEMPLATE.yaml](templates/CONTENT-PROFILE-TEMPLATE.yaml) — single reusable content-profile skeleton
- [examples/PROFILE-EXAMPLES.md](examples/PROFILE-EXAMPLES.md) — Corporate / Technical / Community / Executive examples, matching the presets in § 20.4
- [examples/COMPONENT-SET-PROFILE-EXAMPLE.yaml](examples/COMPONENT-SET-PROFILE-EXAMPLE.yaml) — the `component-set` profile in isolation

Keep the YAML schema in `SKILL.md` § 22.1 / § 30 and the templates in sync; the templates are copies of the schema, not an independent definition.

### Generic content profiles

`component-set`, `changelog`, `procedure`, `schedule`, `status`, `comparison`, `faq`, `reference-list` — these describe *information shapes*, never a domain. `component-set` (§ 31) is the general case for "several related entries, each with properties" and is the profile that absorbs most new use cases; the builder in § 33 must first try to map a new request onto an existing generic shape before proposing a new profile.

## Non-negotiable constraints

- Keep the **base skill domain-neutral**.
- Do not add default logic tied to a specific game, brand, product, company, community, or industry.
- Domain-specific terminology is allowed only when it is user-provided or belongs to a user-created profile outside the generic core.
- Preserve the empirically tested mobile-first rules unless there is new evidence that justifies changing them.
- Do not replace tested Discord behavior with assumptions from generic Markdown implementations.
- Keep the skill usable by both Claude-family and OpenAI-family assistants; avoid vendor-specific requirements in `SKILL.md` unless placed in a clearly optional integration note.
- Do not silently remove existing capabilities when refactoring.

## Regression-sensitive Discord behavior

These rules come from real rendering tests on desktop and mobile. Treat them as findings, not as style opinions:

- ~36 visible monospace characters is the conservative mobile target; ~40 is the practical upper edge; ~44+ likely wraps. These are layout guidance, not Discord protocol limits.
- Wide 4+ column pseudo-tables are not a safe generic mobile layout — use record layouts instead.
- `diff` renders usefully on tested mobile and desktop clients; ANSI color did **not** work reliably on the tested mobile client and stays desktop-only/experimental.
- `>>>` quotes the entire remainder of the message, so bounded multiline callouts prefix every line with `>`.
- `[x]` / `[ ]` are not interactive Discord checkboxes; use Unicode `☑` / `☐`.
- `<URL>` suppresses the link preview; masked links stay readable but may still generate an embed.
- Discord timestamps `<t:...>` render in the reader's local time and are preferred for dates/times.
- `➕` and `➖` render as thin, pale, low-contrast glyphs in the dark theme on mobile, while `🔄` and `🐛` carry strong color. Changelog defaults are therefore `✨ Added` / `🗑️ Removed` (`SKILL.md` § 5.3). Do not "restore" the plus/minus symbols as a tidiness fix — they were measured, not assumed.
- No fake YAML/INI/CSS syntax highlighting as decoration — a neutral code block instead.
- Transport rule (§ 3): every finished Discord message is wrapped in **four** outer backticks so inner triple backticks survive copy/paste. The outer fence is the AI surface's container, not part of the Discord content. Multiple messages get one container each.

## README and specification must not contradict each other

`SKILL.md` is canonical, but `README.md` is what people read first and copy from. Every example, default label and factual claim in the README must match the specification exactly.

Tokens that must be identical everywhere they appear — `README.md`, `SKILL.md` (rule, schema § 22.1 and regression case), `templates/GUIDELINE-TEMPLATE.yaml`, `showcases/README.md`:

- changelog category labels and their emoji
- callout labels, status wording and symbols
- the width budgets per layout mode (36/40 · 48/56 · 80/100)
- the `SKILL.md` file size quoted in the install instructions
- section numbers referenced by prose
- the Unix timestamp used in timestamp examples

After changing either file, grep the other for the same tokens. A README improvement is not finished until the specification carries the same value — otherwise readers get output that does not match what the skill produces, and it is unclear which of the two is wrong.

The README's worked examples double as documentation of the defaults. If an example there looks better than the specification's default, that is a signal to change the default (with a rendering result to back it), not to let the two drift.

## Change discipline

For meaningful changes:

1. explain the motivation,
2. identify affected rules,
3. update examples/templates if needed,
4. update `CHANGELOG.md`,
5. bump `VERSION` and the version field in `SKILL.md` only when the change warrants a release.

A release bump touches four places that must stay consistent: `VERSION`, the `version:` field in the `SKILL.md` frontmatter, `README.md` ("Current version"), and the version comment in `templates/CONTENT-PROFILE-TEMPLATE.yaml`. `PROJECT_HANDOFF.md` also names the current version.

`SKILL.md` sections are numbered and cross-referenced by number throughout the repository. When inserting a section, prefer a sub-number (e.g. `9.3`) over renumbering everything that follows.

## Repository goal

Maintain a small, understandable repository whose canonical source of behavior is `SKILL.md`. Supporting files should clarify, configure, test, or demonstrate the skill rather than duplicate the entire specification.
