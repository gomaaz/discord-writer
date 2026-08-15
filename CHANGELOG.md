# Changelog

## 1.6

- Added § 2.1 "Layout mode": `target.platform` now has a defined effect instead of a vague note. Each mode carries a concrete width budget, column budget and table policy, so a user can state that mobile does not matter and keep a wide table.
- Reframed § 2 from "mobile-first is mandatory" to "mobile-first is the default, not a law of nature". Mobile-first remains the default; it is no longer presented as a technical hard rule it never was.
- Documented `codeblocks.allow_wide_tables`, which existed in the schema since 1.2 without any prose defining what it does.
- Stated explicitly that Discord has no native Markdown table rendering, so "keeping a table" always means an aligned code block (§ 2.1, § 15 rule 12).
- Wired the mode through § 7 (column rule), § 17 (new step 0), § 21.4, § 24 (new adaptation case), § 26 (new "Layout mode" checklist) and § 28 rule 9.
- Added regression case R11: the same four-column data set must render as records in `mobile-first` and as a table in `desktop-first`.
- A wide table requested without a mode is now honored for that post, with a one-line note that it scrolls on phones — instead of being silently restructured.

## 1.5

- Translated `SKILL.md` and all templates/examples from German to English; the specification is now English-only.
- Kept section numbering and all cross-references (§ 1–36, R1–R10) stable so existing references remain valid.
- Added an explicit language note: the specification language does not determine the output language. Discord messages are still written in the user's language (§ 15, rule 2).
- Changed the default labels to English: changelog categories (`➕ Added`, `🔄 Changed`, `➖ Removed`, `🐛 Fixed`), callout labels (`⚠️ Warning`, `✅ Success`, `💡 Tip`) and status wording. These are defaults for English output and must be translated when `language.locale` is a different language.
- Changed the schema default `language.locale` from `de` to `en`.
- Translated all example content in the specification (change cards, hybrid post, status lists, checklists, regression cases) while preserving the mobile width budget of ≤ 36 characters for code block examples.
- Added the MIT license and published the repository.
- Reworked `README.md`: worked examples of real skill output up front, separate installation instructions for Claude Code, Claude and ChatGPT, and a header image.
- Changed the default changelog category emoji from `➕`/`➖` to `✨`/`🗑️`. Rendering test on a mobile dark-theme client showed `➕` and `➖` as thin, pale glyphs, which made *Added* and *Removed* the visually weakest headings of a release post. Documented as a new finding in § 5.3 and as an acceptance criterion in R7.
- Added `showcases/`: a gallery of nine worked showcases, each with input, exact output and a slot for a rendering screenshot. The README now leads with two highlights and links to the gallery.

## 1.4

- Reinforced strict domain neutrality in the base skill.
- Replaced domain-oriented build/loadout concepts with generic information-shape profiles.
- Added/standardized generic profiles such as `component-set`, `changelog`, `procedure`, `schedule`, `status`, `comparison`, `faq`, and `reference-list`.
- Added `preserve-source` field policy: preserve user-supplied field names, do not invent domain-specific fields, and omit missing optional fields.
- Clarified separation between global style, content profile, current-post overrides, and content-aware/mobile-safe adaptation.

## 1.3

- Introduced content profiles and a content-profile builder.
- Added structured handling for recurring multi-object/multi-field information shapes.

## 1.2

- Added user guidelines/style profiles and preference/override hierarchy.
- Added configurable density, emoji, headings, changes, links, timestamps, checklist and splitting preferences.

## 1.1

- Added tested changelog, callout, status-list, strikethrough-change, change-card, metadata, timestamp, checklist, and link behaviors.
- Added tested link-preview suppression using angle-bracket URLs.

## 1.0

- Established the mobile-first Discord formatting core from desktop/mobile rendering tests.
- Added native Markdown, compact change lists, hanging changes, record layouts, `diff`, hybrid posts, blockquotes, spoilers, nested lists, and conservative codeblock width rules.
