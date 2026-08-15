# Changelog

## 1.8.3

- Added the nine showcase screenshots. Every gallery entry now shows real Discord rendering next to its source, and the README leads with the hybrid post and the desktop/phone pair.
- Rendering test confirmed a stronger timestamp behavior than documented: Discord localizes the **language** as well as the offset. `<t:...:R>` renders as "in 5 Tagen" on a German client while the surrounding English message is untouched. Recorded in § 13.1 — writing times out by hand is strictly worse for any reader outside the author's zone.
- The changelog capture confirms the 1.7 emoji decision: `✨` and `🗑️` stay legible in the dark theme where `➕` and `➖` washed out.

## 1.9

- **Added § 2.2 "Destination".** The skill previously assumed every message was a plain text-channel post. It now resolves where the message is going — text, forum, media, thread, announcement, embed text or voice message — and applies that destination's limits, with a table of all of them. Unknown values fall back to safe defaults that hold everywhere.
- 2,000 characters is stated as the universal safe budget. The 4,000-character Nitro budget requires all three of manual delivery, confirmed Nitro and a user who wants longer messages — the previous wording ("unless a higher limit is known") was too vague to act on.
- Forum and media posts now require a title of 1–100 characters, overflow goes into thread replies rather than a longer opening post, media is never treated as mandatory, and tags are never invented.
- Embeds are in scope for their **text** (title 256, description 4,096, 6,000 across all embeds of one message) and out of scope for their JSON. § 0 keeps bot and API development excluded; the boundary is now stated explicitly instead of implied.
- Added the constraint-stability classification: API hard limit, client limit, server-configurable, environment-dependent, recommendation. The § 2 width budgets are explicitly the last of these — tested layout guidance, not something Discord enforces. Attachment upload size must never be quoted as a fixed number.
- **Added § 14.1 "Where exactly the cut goes":** a split-priority ladder from section boundary down to a hard character cut, a list of structures that must never be cut through, the rule to close and reopen a code fence across a split, and continuation labels as counted content.
- **Added § 14.2 "Count, validate, then repair":** what counts as a character (Markdown syntax, fences, full-length URLs, `<t:...>` at syntax length; the transport backticks do not), validation after formatting rather than before, the eight-step render order, and an explicit allow/deny list for automatic repair — rearranging is repair, removing is the user's decision.
- Wired the destination through § 0 (embed boundary), § 14, § 17 (step 0 split into 0.1 destination / 0.2 layout mode, new step S), § 19.1 Layer A, § 20.3, § 21.1, § 22.1 (new `destination` block), § 24 (two new adaptation cases) and § 26 (new "Destination and limits" checklist).
- Added regression cases R12 (forum post: title present, overflow as replies, no invented tag, no demanded image) and R13 (splitting mechanics: cut on a section boundary, fence closed and reopened, both parts within 2,000 characters).
- § 28 short form grew from 28 to 31 rules.
- Corrected the `SKILL.md` size quoted in § 0 and the README install instructions: ~57 KB → ~82 KB. It had been stale since 1.5.
- Source of these rules: `rules/discord_destination_constraints.md`, kept as the origin document. `SKILL.md` remains canonical.

## 1.8.2

- Added `showcases/messages/`: one file per showcase message, holding the Discord raw text with no fence around it. Testing showed why — pasting from a fenced block on a documentation page pulls the outer backticks along, which turns the whole post into a code block and breaks any message containing its own ``` block.
- `CAPTURE-GUIDE.md` now links the files instead of embedding blocks to be trimmed by hand. Select all, copy, paste.

## 1.8.1

- Merged showcases "One layout, two screens" and "Layout mode" into a single "Screen priority" entry. Both needed the same screenshot, and the part that distinguished them — the offer to narrow the layout — is a chat reply, not a Discord message, so it could not be captured at all. Eight showcases, nine captures.
- Added `showcases/CAPTURE-GUIDE.md`: the messages to post in order, which device each screenshot needs, and the exact filename to hand back.
- Overview table now lists filename and device per showcase instead of a status marker.

## 1.8

- **Changed the default layout mode from `mobile-first` to `desktop-first`.** Composing happens at a keyboard, and structure that was pre-shrunk for a phone cannot be recovered afterwards, while narrowing a layout later is straightforward. Tables now stay tables by default.
- Mobile is deferred, not discarded: a mobile-safe layout is produced when the user asks for it, sets `mobile-first`/`balanced`, or states the audience reads on phones. Such a statement carries forward to later posts in the conversation.
- Added the post-delivery offer: after a result wide enough to break on a phone, one short line asks whether a mobile version is wanted. Output that already reflows gets no such line, and a delivered post is never converted unasked.
- The empirical mobile findings are unchanged and still binding — 36/40/44 characters, records instead of 4+ columns. They are now the `mobile-first` budget rather than the budget for every post.
- Rewrote § 2, reordered the § 2.1 mode table, and updated § 7, § 17 step 0, § 20.3, § 22.1, § 26, § 28 rules 2 and 7–9, § 29.1 and the § 24 adaptation cases accordingly.
- Rewrote R11 with three acceptance criteria: the default keeps the table, a mobile request actually changes it, and the offer appears only for wide output. R2 and R4 restated as mobile-budget cases.
- Profile examples switched to `desktop-first`, except Community, which keeps `mobile-first` deliberately as the demonstration case.

## 1.7.2

- Fixed a false activation found in testing: "schreib mir einen post über ein division 2 striker build" loaded the skill although Discord was never mentioned.
- Rewrote the frontmatter `description`. The previous one listed generic content types (announcements, patch notes, updates, summaries) as triggers, which any "write me a post" request matches on similarity. Generic wording now appears only in the negative clause, with the Discord requirement stated as a hard condition.
- Moved § 0 ahead of § Purpose. It sat behind a broad purpose statement, so the first thing read after loading was an invitation to apply the skill rather than the check on whether it applies.
- Added an explicit stop instruction plus a table of requests that must not activate the skill, and stated that gaming, community or release topics do not imply Discord.
- Made the silent-withdrawal behavior explicit: no mention of the skill, no announcement of non-use, no clarifying question.

## 1.7.1

- Added `.claude-plugin/plugin.json` and `.claude-plugin/marketplace.json`: the repository is now installable as a Claude Code plugin and acts as its own marketplace.
- Works as a single-skill plugin — `SKILL.md` stays at the repository root, no `skills/` directory and no `skills` manifest field, so nothing had to be restructured.
- README documents both paths: plugin install via `/plugin marketplace add`, and the manual copy into the skills directory.

## 1.7

- Added § 0 "When this skill applies". The skill had no activation rule at all: whether it fired was decided solely by the frontmatter `description`, which described capabilities rather than triggers.
- Made Discord a hard activation condition. Discord must be named in the request or be unambiguous from context; once established in a conversation, follow-up requests inherit it.
- Added explicit non-application: Discord bot and API development (`discord.py`, `discord.js`, embed JSON, slash commands, webhooks, gateway events), other platforms, general Markdown work, and plain questions about Discord as a product.
- Rewrote the frontmatter `description` as a trigger description, since it is the only signal a skill host has when deciding whether to load the specification.
- Ruled that an unclear case means not applying the skill, and not asking a clarifying question merely to justify applying it.

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
