# Changelog

## 1.5

- Translated `SKILL.md` and all templates/examples from German to English; the specification is now English-only.
- Kept section numbering and all cross-references (§ 1–36, R1–R10) stable so existing references remain valid.
- Added an explicit language note: the specification language does not determine the output language. Discord messages are still written in the user's language (§ 15, rule 2).
- Changed the default labels to English: changelog categories (`➕ Added`, `🔄 Changed`, `➖ Removed`, `🐛 Fixed`), callout labels (`⚠️ Warning`, `✅ Success`, `💡 Tip`) and status wording. These are defaults for English output and must be translated when `language.locale` is a different language.
- Changed the schema default `language.locale` from `de` to `en`.
- Translated all example content in the specification (change cards, hybrid post, status lists, checklists, regression cases) while preserving the mobile width budget of ≤ 36 characters for code block examples.
- Added the MIT license and published the repository.
- Reworked `README.md`: worked examples of real skill output up front, separate installation instructions for Claude Code, Claude and ChatGPT, and a header image.

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
