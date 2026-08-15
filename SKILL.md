---
name: discord-writer
description: Formats content as a ready-to-paste Discord message using native Discord Markdown, changelog categories, callouts, diff blocks, record layouts and timestamps, steerable by user style profiles. HARD REQUIREMENT - the user must explicitly say Discord, or Discord must already have been established as the target earlier in the same conversation. If Discord is not mentioned, do NOT use this skill - not for "write me a post", not for "write an announcement", not for patch notes, release notes, updates, status reports or summaries. A generic request to write a post is NOT a Discord request. Never use this skill for Discord bot or API development (discord.py, discord.js, embed JSON, slash commands, webhooks, gateway events), for other chat platforms, or for general Markdown and documentation work.
version: 1.8
---

# discord-writer

# 0. When this skill applies

## Check this before reading any further

**Did the user say "Discord", or was Discord already established as the target in this conversation?**

**If no → stop here.** Ignore the rest of this file and answer the request the way you would without it. Do not mention that this skill exists, do not announce that you are not using it, and do not ask whether the user meant Discord. Just answer.

Being loaded is not the same as being applicable. A skill host loads this file from a similarity match on its description; that match is a guess, and this section is the check on that guess. Overriding the check because the file is already in context is the one failure mode this section exists to prevent.

These requests must **not** activate the skill:

| Request | Why not |
|---|---|
| "write me a post about X" | A post is not a Discord post. Blogs, forums and social platforms all have posts. |
| "write an announcement for the team" | No channel named, no Discord. |
| "make patch notes for 2.5" | Patch notes are published in many places. |
| "summarize this for the community" | Community ≠ Discord. |
| "format this nicely" | No target platform at all. |
| "help me fix my discord.py bot" | Says Discord, but it is code — see *Do not apply to*. |

Gaming, community or release topics do not imply Discord either. A request about a game build, a raid schedule or a server event is still not a Discord request unless Discord is named.

## Hard condition

**Discord must be named in the request, or be unambiguous from context.** If it is not, do not apply this skill.

Unambiguous from context means, for example:

- an active Discord style profile or guideline for this conversation
- an ongoing exchange in which the target was already established as Discord
- the user pasting Discord output back in for revision

Once Discord is established in a conversation, follow-up requests inherit it. The user does not have to repeat the word for every message.

This skill is built for one job: turning content into a Discord message. Outside that job it adds ~57 KB of rules that get in the way, so its scope is deliberately narrow.

## Apply when the user wants

- an announcement, update, changelog, release or patch note for a Discord channel
- a status post, incident notice, schedule or event announcement for Discord
- notes, data or a summary reformatted so it can be pasted into Discord
- an existing Discord message restructured, shortened or split
- a Discord style profile or content profile created (§ 20, § 33)

## Do not apply to

- **Discord bot and API development** — `discord.py`, `discord.js`, embed JSON, slash commands, webhooks, gateway events, intents, rate limits. That is code, and the formatting rules here do not apply to it. A bot's *message text* may still be formatted with this skill, but only when the user asks for that specifically.
- **other platforms** — Slack, Teams, Matrix, forums, email, GitHub. Their Markdown and their width behavior differ; applying these rules there produces confidently wrong output.
- **general Markdown, documentation or README work**, even when it looks similar.
- **plain questions about Discord** — features, permissions, moderation, Nitro. Answer those normally.

## When it is unclear

Do not apply the skill, and do not ask a clarifying question just to justify applying it. Answer the request normally. A user who wants Discord output will say so, and the cost of a missed activation is one short follow-up — while a wrong activation reshapes an answer that was never meant to be a chat message.

---

## Purpose

Prepare content so that it is quick to grasp in Discord, cleanly structured, directly copyable and clearly organized. Compose for a desktop client by default; produce a mobile-safe layout when it is asked for (§ 2).

The skill is fully generic and **must never be tailored, in the base skill, to a specific game, product, brand, community or industry**. Domain-specific terms and structures may only come from the current user input or from a guideline supplied by the user.

It is suitable for, among other things:

- announcements
- updates and changelogs
- release notes
- status reports
- summaries
- technical information
- comparisons
- instructions
- configurations
- lists and data sets

Priorities, in this order:

1. Factual correctness
2. Readability
3. Direct copyability
4. Clear information hierarchy
5. High scannability
6. Compact presentation
7. Decoration only when it supports information

**Language note:** This specification is written in English. That does not determine the output language. Write the Discord message in the user's language (see § 15), and translate the default labels of this skill — changelog categories, callout labels, status wording — into that language unless a profile defines its own wording.

---

# 1. Core principle: the type of information determines the format

Choose the presentation not by what looks visually striking, but by what kind of information is present.

| Type of information | Preferred format |
|---|---|
| normal text / explanation | native Discord Markdown |
| short `OLD → NEW` comparisons | Native Compact List |
| a few clearly replaced values | Strikethrough Change |
| 1–3 particularly important changes | Change Card |
| release notes / version changes | changelog categories |
| many short values | compact neutral code block |
| long `OLD → NEW` values | Hanging Change |
| object with multiple properties | Record Layout |
| simple states of multiple objects | Status List |
| removed / added | `diff` code block |
| warning / info / success / error | callout via blockquote |
| task status | Unicode checklist |
| release/event metadata | compact Metadata Block |
| date / time | Discord timestamp `<t:...>` |
| named link | Masked Link `[Name](URL)` |
| link without preview | `<URL>` |
| secondary information | subtext `-#` |
| hidden content | spoiler `||...||` |
| real code / configuration | matching real language tag |
| semantic colors | do not use by default; ANSI only desktop-only / experimental |

When several types of information occur together, a **hybrid post** may combine multiple of these formats.

---

# 2. Desktop-first is the default

Compose Discord output for a desktop client by default. Use the full width a desktop window gives you: aligned tables stay tables, columns stay columns, and a comparison that reads best in a grid is written as a grid.

The reason is that composing is a desktop activity. Someone writing an announcement is at a keyboard, judging the result on the screen in front of them — and a layout that was pre-shrunk for a phone looks needlessly cramped there. Narrowing the layout is a transformation that can be applied afterwards; recovering a structure that was flattened away is not.

**Do not silently sacrifice structure for phone width.** That is the previous default, and it is no longer the default.

## Mobile is deferred, not discarded

Mobile readability is still a real constraint — just not one that shapes the first draft.

Switch to a mobile-safe layout when:

- the user asks for it, in any wording ("make it mobile-friendly", "this has to work on phones", "our members read on mobile")
- the user sets `target.platform: mobile-first` or `balanced` in a profile
- the user states that the audience reads on phones

**After delivering a post**, add one short line offering the mobile version **only when the output would actually break on a phone** — a table wider than about 44 characters, 4+ columns, or long aligned rows. For example:

```text
This uses a wide table. Want a mobile-friendly version?
```

Do not append that line to posts that are already narrow. Native Markdown, compact lists, callouts, status lists and short records reflow fine on any screen — offering to fix them is noise.

Never restructure a delivered post into a mobile layout on your own initiative. Offer, then wait.

## Empirical width behavior on mobile

These values come from rendering tests and describe what a phone actually does. They are the budget for `mobile-first` and the reference for any mobile rework; § 2.1 gives the budget per mode.

- up to about **36 characters**: safe target range
- around **40 characters**: practical upper limit
- from about **44 characters**: wrapping becomes likely

Treat these values as **layout guidance, not as Discord protocol limits**.

When a mobile-safe layout is requested or applied:

1. Plan manually aligned code block lines with **≤ 36 visible characters** where possible.
2. Only go up to about **40 characters** for a good reason.
3. Do not force column alignment if it makes lines wider.
4. For long content, use responsive native Markdown layouts or stacked layouts.

---

# 2.1 Layout mode: how much narrow screens constrain the layout

The user decides how much the output has to respect narrow screens. This is what `target.platform` controls, and it is the single switch that decides whether wide tables are allowed to survive.

## The three modes

| | `desktop-first` (default) | `balanced` | `mobile-first` |
|---|---|---|---|
| Code block target width | ≤ 80 chars | ≤ 48 chars | ≤ 36 chars |
| Soft maximum | ~100 | ~56 | ~40 |
| Logical columns | as many as the data needs | up to 3, four if short | 2, three only if very short |
| Wide aligned tables | **keep as a table** | only when compact | convert to Record Layout |
| A table supplied by the user | **preserve its structure** | restructure only if it would wrap badly | restructure it |
| Long `OLD → NEW` pairs | one line is fine | Hanging Change when long | Hanging Change |

`balanced` is the honest middle: it assumes a mixed audience and buys some table width at the cost of a phone still having to scroll horizontally on the widest rows.

Two things the default does **not** change. Native Markdown remains the standard for prose and short structured content (§ 4) — `desktop-first` is a permission to use width where width helps, not an instruction to put everything in a code block. And the mobile follow-up offer from § 2 still applies: a wide result gets one line asking whether a phone version is wanted.

## What `desktop-first` does and does not license

Allowed in `desktop-first`:

- keep a 4+ column aligned table instead of splitting it into records
- keep the column order and headers the user supplied
- use the full width a desktop client shows without horizontal scrolling

Still forbidden, because these are technical hard rules and not width preferences (§ 19.1, Layer A):

- fake syntax highlighting for color (§ 12)
- ANSI as an automatic choice — it stays opt-in and experimental even on desktop (§ 11)
- inventing values or columns to fill a table (§ 30.2)
- exceeding the Discord character limit (§ 14)
- unstable layout hacks such as Unicode box frames or decorative `|` borders (§ 6)

`desktop-first` widens the layout budget. It does not switch off correctness.

## Tables in Discord are always code blocks

Discord has no native Markdown table rendering. A `| a | b |` row is displayed as literal text with pipe characters, not as a table, on every client. So "keeping a table" always means **an aligned monospace code block**, in every mode — `desktop-first` only changes how wide that block may get and how many columns it may carry.

If the user supplies a Markdown pipe table and wants it "kept", convert it to an aligned code block and say so briefly. That is not a deviation from their wish; it is the only form in which a table renders in Discord at all.

## How the mode is set

`desktop-first` applies unless something selects otherwise. A statement about the current post always wins over the profile (§ 21.2):

```yaml
target:
  platform: mobile-first
```

```text
Make it mobile-friendly.
Our members read this on their phones.
This has to work on a narrow screen.
```

Any such statement switches the current post to `mobile-first`. It also applies to follow-up posts in the same conversation, in the same way an established Discord target carries forward (§ 0) — the user should not have to repeat it for every message.

If the user accepts an offered mobile version (§ 2), rebuild that post under the `mobile-first` budget. Keep the content identical; only the layout changes.

## Related fields

`codeblocks.mobile_target_width` and `codeblocks.mobile_soft_max_width` override the numbers above directly. `codeblocks.allow_wide_tables: true` permits 4+ column tables independently of the platform mode — use it when the audience is mixed but the data genuinely is tabular.

When the fields conflict with the mode, the more specific field wins: an explicit width beats the mode's default width.

---

# 3. Transport rule for ChatGPT, Claude and other Markdown interfaces

## Goal

The finished Discord message must be emitted as **one contiguous, copyable block of raw text**.

If the Discord message itself contains triple backticks, they must not be consumed by the AI interface.

## Hard rule

Wrap **every finished Discord message** in the answer with **four outer backticks**.

The four outer backticks are **not** part of the Discord content. They are only the transport container of the AI interface.

Inside this transport container, the actual Discord message may contain normal Discord syntax, including triple backticks.

Example of the principle:

~~~~
# Title

## Changes

```
Timeout    30 s → 45 s
```

> Note
~~~~

If multiple Discord messages are intended, **each message gets its own transport container**.

Do not wrap individual sections separately if they belong together as one Discord message later.

---

# 4. Native Discord Markdown: the standard for normal content

Prefer native Discord formatting for running text, hierarchy and short structured information.

## Headings

```text
# Main title
## Section
### Subsection
```

Rules:

- Keep headings **short**.
- For `#` main headings use titles that are as short as possible; a single unbreakable word should ideally **not go much beyond about 20 visible characters**.
- Very long single words or compound terms can break mid-word on narrow smartphones. In that case shorten the title, rephrase it naturally, or drop to a smaller heading level.
- One functional emoji at the start is allowed, but it visually counts toward the available width. With overly long titles the emoji can end up on its own line.
- No emoji chains.
- Natural line breaks between multiple words are acceptable; but avoid involuntary word breaks caused by overly long uninterrupted titles.

## Emphasis

```text
**important**
*emphasized*
__underlined__
~~outdated~~
`inline value`
```

## Lists

```text
- First item
- Second item
  - Sub-item
    - Third level
```

Rule: about **3 hierarchy levels** at most, unless the user explicitly needs more.

Numbered lists are allowed as well.

## Spoilers

```text
||hidden content||
```

Suitable for:

- solutions
- surprises
- deliberately hidden details
- optional additional information

Do not misuse as a general layout tool.

## Subtext

```text
-# Secondary information
```

Good for:

- source references
- side notes
- additional technical details
- low-priority status information

## Blockquotes

Single or bounded lines:

```text
> Note
```

Multiple bounded lines:

```text
> First line
> Second line
> Third line
```

**Important:** In Discord, `>>>` treats the entire remainder of the message as a multiline blockquote. Only use `>>>` if the **whole remaining message content** really should be quoted.

If normal text should follow afterwards, use `>` for each individual quote line.

---

# 5. Standard for short OLD → NEW comparisons: Native Compact List

This is the general standard for responsive value changes.

```text
- **Timeout:** `30 s` → `45 s`
- **Connections:** `100` → `150`
- **Status:** `Beta` → `Stable`
- **Sync:** `Manual` → `Automatic`
```

Advantages:

- responsive on mobile
- no fixed column width
- automatic, sensible line breaks
- labels stay distinct
- values are clearly highlighted
- easy to edit

Use this form unless a tabular monospace look genuinely adds value.

---

# 5.1 Strikethrough Change for a few direct replacements

If an old value should be visibly marked as replaced and there are only few to moderate changes, the old value may be struck through.

```text
- **Timeout:** ~~`30 s`~~ → `45 s`
- **Status:** ~~`Beta`~~ → `Stable`
```

Rules:

- Suitable for few to moderate replacements.
- With large change lists, a lot of strikethrough becomes visually restless; use a normal compact list then.
- This form fits particularly well under a changelog category `🔄 Changed`.
- The new value does not have to be bolded additionally; inline code is usually enough.

---

# 5.2 Change Card for 1–3 central changes

If individual changes are especially important, use a stacked change card instead of a long list.

```text
## ⭐ Key change

**Access control**

🔴 **Before**
`Individual user permissions`

🟢 **After**
`Role-based permissions`
```

Rules:

- Ideal for roughly **1–3 central changes**.
- Do not use for many small changes; the vertical height grows a lot.
- `Before` and `After` must stay spelled out; color/emoji is only additional semantics.
- With long values this form is particularly mobile-robust.

---

# 5.3 Changelog categories for release notes

For release notes and version updates, semantic categories are usually better than one large `diff` block.

Recommended base pattern:

```text
## ✨ Added
- New feature

## 🔄 Changed
- **Timeout:** ~~`30 s`~~ → `45 s`

## 🗑️ Removed
- Old feature

## 🐛 Fixed
- Bug description
```

Optional categories when they fit the content:

- `⚡ Performance`
- `🔒 Security`
- `⚠️ Breaking Change`
- `📚 Documentation`

Rules:

- Only emit categories that actually have content.
- Prefer changelog categories for public release notes.
- Still use `diff` when the exact technical difference is the main point.
- Under `🔄 Changed`, Strikethrough Change can be preferred.

## Category emoji contrast

Empirically tested on a mobile Discord client in the dark theme: `➕` and `➖`
render as thin, pale glyphs with very low contrast, while `🔄` and `🐛` carry
strong color. Using them makes *Added* and *Removed* — usually the two most
important categories — the visually weakest headings on the post.

Therefore prefer emoji with their own strong color for these two categories.
`✨` and `🗑️` are the defaults for that reason, not for decoration.

Avoid `🟢` / `🔴` here: those are already bound to OK/error semantics in status
lists (§ 9.1), and reusing them for added/removed conflicts with § 9.2.

---

# 6. Neutral code block for compact data

Use a code block **without a language tag** when short data benefits from fixed monospace alignment.

Example:

```text
Timeout         30 s → 45 s
Connections     100 → 150
Status          Beta → Stable
Sync            Manual → Auto
```

Rules:

1. Spaces only, no tabs.
2. Target width ≤ 36 characters.
3. About 40 characters only for a good reason.
4. No unnecessary table headers.
5. No vertical `|` as decorative column borders.
6. No complex Unicode frames.
7. If an entry does not reliably fit on one line, switch format.

## Table headers only when they genuinely help

A table header is allowed if it improves meaning:

```text
VALUE          CHANGE
----------------------------
Timeout        30 s → 45 s
Status         Beta → Stable
```

If `OLD`, `→` and `NEW` are already unambiguous from the content, a compact presentation without a header is often better.

---

# 7. No wide pseudo-tables as the default

Wide desktop tables are not robust on mobile. This section describes the `mobile-first` budget — it is **not** the default (§ 2 and § 2.1); apply it when a mobile-safe layout is requested or being produced.

Avoid when composing for mobile:

- 4 or more logical columns
- long labels plus separate `OLD` and `NEW` columns
- tables whose header already wraps
- rows where values fall onto the next visual line

## Column rule

- **2 logical columns:** preferred
- **3 columns:** only with very short values
- **4+ columns:** avoid; convert into a Record Layout

In `desktop-first` — the default — or with `codeblocks.allow_wide_tables: true`, a 4+ column table is a legitimate result and must not be restructured into records.

Example of a problematic structure:

```text
AREA           OLD        → NEW
Synchronization   Manual  → Automatic
```

Better:

```text
Synchronization
Manual → Automatic
```

or as a Native Compact List:

```text
- **Synchronization:** `Manual` → `Automatic`
```

---

# 8. Hanging Change for long changes

If the label or the values are too long for a compact line, use a stacked hanging layout.

Preferred pattern:

```text
Access control
  Individual permissions
  → Role-based permissions

Notifications
  Manual review
  → Automated daily review
```

Advantages:

- very mobile-robust
- clear semantic association
- no forced column layout
- less vertical space than many separate Markdown paragraphs

Prefer Hanging Change when a compact comparison line would likely become wider than about 36–40 characters.

---

# 9. Record Layout for objects with multiple properties

When several fields belong to one object, use records instead of wide tables.

Example:

```text
API
Status:   Online
Version:  3.2.1
Latency:  42 ms
Check:    15:30

Database
Status:   Online
Version:  14.6
Latency:  18 ms
Check:    15:35
```

Suitable for:

- services
- devices
- systems
- users
- products
- servers
- versions
- configurations

Rule: if a classic table layout would need 4+ columns, first check whether a Record Layout is better.

---

# 9.1 Status List for simple states

When multiple objects each have only a brief status and optionally one short measurement, a responsive status list is better than a table.

```text
🟢 **API** — Online · `42 ms`
🟢 **Database** — Online · `18 ms`
🟡 **Cache** — Degraded · `91 ms`
🔴 **Backup** — Failed
⚪ **Archive** — Not checked
```

Recommended semantics:

- 🟢 OK / online / successful
- 🟡 warning / degraded
- 🔴 error / offline
- ⚪ unknown / not checked
- 🔵 information / in progress

Rules:

- Always write the status as text as well; the emoji must never carry the meaning on its own.
- If multiple fields per object are needed, switch to Record Layout.

---

# 9.2 Callouts for information, warning, success and error

Build clear notice blocks with blockquotes, emoji and a bold label.

```text
> ℹ️ **Information**
> The new setting is applied automatically.

> ⚠️ **Warning**
> This change requires a restart.
```

Recommended labels:

- ℹ️ Information
- ⚠️ Warning / Caution
- ✅ Success
- ❌ Error
- 💡 Tip
- 🔒 Security / access

Rules:

- With multiple lines, start every line with `>`.
- Do not use `>>>` for bounded callouts.
- Do not duplicate the same semantic emoji immediately in both a heading **and** a callout. Example: not `## ⚠️ Installation` directly followed by `> ⚠️ **Warning**`; drop one of the two emojis.

---

# 9.3 Checklists

Discord does not render GitHub task syntax such as `- [x]` as a real checkbox. For visual checklists, use Unicode or emoji instead.

Preferred standard:

```text
☑ Backup created
☐ Install update
☐ Check services
```

More prominent variant:

```text
✅ Backup created
⬜ Install update
⬜ Check services
```

Rules:

- Unicode `☑ / ☐` is the compact standard.
- Only use emoji `✅ / ⬜` when stronger visual weight is wanted.
- Do not emit `- [x]` / `- [ ]` as checkbox UI.

---

# 10. diff for removed / added

Use `diff` when the semantics of **removed vs. added** are central.

```diff
- Old status display
- Manual retry

+ New dashboard
+ Automatic retry
```

Also for genuine before/after values:

```diff
- Timeout: 30 s
+ Timeout: 45 s
```

Mobile rule:

- Keep diff lines at ≤ 36–40 characters where possible.
- Long diff lines can wrap visually; the color is often preserved, but `+` or `-` only sits at the start of the logical line.
- With long text values, prefer Hanging Change instead.

The meaning must remain understandable without color.

---

# 11. ANSI: desktop-only / experimental

ANSI can produce colored code blocks on Discord desktop, but it is not reliable across platforms.

In tested mobile rendering, ANSI colors were **not** displayed.

Therefore:

- Never use ANSI as the default.
- Never choose ANSI automatically.
- Only use ANSI on the user's explicit request.
- Treat the output as **desktop-only / experimental**.
- Never convey information through ANSI color alone.

Recommended semantic mapping if used explicitly:

- red = error / removed / negative
- green = new / success / positive
- yellow = warning / attention
- blue = information
- cyan = hint
- magenta = special state

---

# 12. Syntax highlighting only for real syntax

Only use language tags when the content actually represents that language or data form.

Suitable:

```text
```json
{"status":"ok"}
```
```

```text
```yaml
status: active
```
```

```text
```diff
- old
+ new
```
```

Not allowed as an automatic styling hack:

- fake YAML just for colors
- fake INI just for colors
- fake CSS just for colors
- arbitrary highlighting languages that artificially colorize normal text

Reasons:

- Colors depend on client and theme.
- Syntax parsers interpret text by language rules rather than by content.
- Rendering can differ between desktop, web and mobile.
- Readability must be preserved without highlighting.

For real code, always prefer the technically correct language tag.

---

# 13. Hybrid post: the standard for complex content

A hybrid post combines native Discord hierarchy with specialized blocks.

Recommended pattern:

```text
# 🚀 SYSTEM UPDATE

Short introduction.

## Key changes

- **Timeout:** `30 s` → `45 s`
- **Status:** `Beta` → `Stable`

## Features

```diff
- Old status display
+ New dashboard
```

## Notes

> Existing settings are preserved.

**Restart:** required

-# Changes take full effect afterwards.
```

Format choice inside a hybrid post:

- explanation → normal Markdown
- short value changes → Native Compact List
- a few replaced values → Strikethrough Change
- 1–3 central changes → Change Card
- release notes → changelog categories
- many very short values → compact code block
- removed / added → diff
- long values → Hanging Change
- multiple fields per object → Record Layout
- simple states → Status List
- note / warning / error → callout
- tasks → Unicode checklist
- release/event data → Metadata Block + timestamp
- link with a desired name → Masked Link
- link without preview → `<URL>`
- additional info → subtext

---

# 13.1 Metadata Block for releases, events and status posts

Metadata is presented compactly as native Markdown lines.

```text
**Version:** `2.5.0`
**Status:** 🟢 Stable
**Environment:** `Production`
**Release:** <t:1787241600:f>
**Start:** <t:1787241600:R>
```

Rules:

- Keep labels short: `Version`, `Status`, `Environment`, `Release`, `Start`, `Build`, `Channel`.
- Avoid long labels such as `Deployment window` when they cause timestamp lines to wrap unnecessarily.
- Do not put metadata inside a code block when Discord timestamps or mentions should render natively.

## Discord timestamps

When an unambiguous point in time is known, prefer Discord timestamps so that Discord renders date/time locally for each user.

Important forms:

```text
<t:UNIX:t>  short time
<t:UNIX:d>  short date
<t:UNIX:f>  date + time
<t:UNIX:F>  verbose date + time
<t:UNIX:R>  relative time
```

Mobile-first defaults:

- `t` for pure times
- `d` for brief dates
- `f` for date + time
- `R` for countdown / relative statements
- `F` only when the verbose weekday genuinely adds value; `F` can become wide on mobile

Never invent Unix timestamps. If date, time or time zone are not unambiguously known and precision matters, clarify first or do not use timestamp syntax at all.

---

# 13.2 Links and link previews

Discord distinguishes between the visible link presentation and the preview/embed.

## Masked link

```text
📖 [Documentation](https://example.com)
```

Advantage: readable name.

Important: A masked link **can still produce a preview / embed**. It is not a no-preview mode.

## Plain raw link

```text
https://example.com
```

Can produce a preview / embed.

## Suppressing the preview deliberately

```text
<https://example.com>
```

In current tests this form suppressed the preview reliably. It is also documented in Discord's help as the method for suppressing a single embed.

Decision rule:

- readable link name and preview acceptable/wanted → `[Name](URL)`
- the raw URL itself is relevant → `https://...`
- compact post without a preview wanted → `<https://...>`

Note the trade-off: the compact `<URL>` form suppresses the embed but does not offer a freely named masked link.

---

# 14. Splitting long posts sensibly

A long Discord post may scroll. Height alone is not a defect.

By default, plan normal Discord messages to stay within the usual **2,000-character limit**, unless it is explicitly known that a higher limit applies to the user. If a longer paste would be treated as a file by Discord, that is usually not the desired outcome for directly readable posts.

But also split content into multiple Discord messages independently of the character ceiling when it clearly improves navigation.

Good split points:

- topic areas
- categories
- chapters
- feature groups
- main post + detail posts

Not in the middle of:

- a list
- a code block
- a record
- a change group
- a logically coherent section

Every message must remain understandable on its own.

In threads, a series of short, thematically separated posts may explicitly be preferred.

---

# 15. Writing style

1. Write concisely, factually and scannably.
2. Use the user's language unless something else is requested. This specification being written in English does not make English the output language.
3. Preserve names, numbers, units, signs and meanings exactly.
4. Do not invent missing values.
5. Do not silently correct factual content.
6. Remove unnecessary filler and repetition.
7. Prefer short sections over long blocks of running text.
8. Use emojis sparingly and functionally.
9. Use capital letters only for short labels or deliberately compact titles.
10. Avoid visual decoration without informational value.
11. Use spaces instead of tabs in code blocks.
12. Do not emit Markdown pipe tables — Discord does not render them as tables (§ 2.1). A table is always an aligned code block.
13. Never use color as the only layer of information.

---

# 16. Terminology and condensation

## Terminology

- Do not translate proper names unnecessarily.
- Only translate technical terms when the meaning stays unambiguous.
- Keep original terms when in doubt.
- Never alter numbers, signs, units and version numbers for stylistic reasons.
- Only use abbreviations when they are understandable in context.

## For summaries

1. Core statement first.
2. Changes before background information.
3. Preserve numbers and consequences.
4. Omit unchanged details unless relevant.
5. Shorten rationales when they are not necessary for understanding.
6. Never accidentally remove exceptions and limitations.
7. Mark warnings separately.

---

# 17. Decision matrix

Use this order before every output.

## 0. Which layout mode applies?

Establish this first — it changes the answers to F, G and H. Default `desktop-first`; `balanced` or `mobile-first` when the profile says so, when the user asked for a mobile-safe layout, or when they said the audience reads on phones (§ 2, § 2.1).

## A. Is it mostly normal text?

→ native Discord Markdown.

## B. Are there short `OLD → NEW` values?

→ Native Compact List.

## C. Should a few old values be visibly marked as replaced?

→ Strikethrough Change.

## D. Are there 1–3 particularly important changes?

→ Change Card.

## E. Is it release notes or a version changelog?

→ changelog categories.

## F. Are there very many short values, and does monospace alignment help?

→ compact neutral code block, target width ≤ 36 characters.

## G. Are the label or the values long?

→ Hanging Change or stacked native Markdown.

## H. Does an object have many properties?

→ Record Layout.

## I. Do many objects have only a simple state?

→ Status List.

## J. Is it semantically about removing/adding?

→ `diff`.

## K. Is it a note, a warning, a success or an error?

→ callout via `>`.

## L. Are these tasks with an open/done status?

→ Unicode checklist `☑ / ☐`; optionally `✅ / ⬜`.

## M. Is release/event metadata present?

→ compact Metadata Block.

## N. Is an exact point in time known?

→ Discord timestamp; usually `f`, `R`, `t` or `d`. Reserve `F` for cases where the verbose weekday earns its width.

## O. Does it contain a link?

→ `[Name](URL)` for readable links; `<URL>` when the preview should be suppressed.

## P. Does it contain real code or real configuration?

→ real language tag.

## Q. Is color explicitly requested?

→ ANSI only as desktop-only / experimental; never automatically.

## R. Is the overall post very long or approaching 2,000 characters?

→ distribute it logically across several Discord messages.

---

# 18. Regression test suite

After substantial changes to this skill, at least these reference cases should be re-checked.

## R1 – Normal Discord post

Must correctly support:

- `#` / `##`
- bold
- inline code
- blockquote
- `-#` subtext

Additional acceptance: on mobile, the main heading must not break unpleasantly mid-word because of an overly long single word; shorten or rephrase the title if needed.

## R2 – Short OLD → NEW list

Example:

```text
- **Timeout:** `30 s` → `45 s`
- **Status:** `Beta` → `Stable`
```

Acceptance: understandable without a forced column structure, and it reflows on any screen width.

## R3 – Long change

Example:

```text
Access control
  Individual permissions
  → Role-based permissions
```

Acceptance: no unclear association after a line break.

## R4 – Multi-dimensional data

Example:

```text
API
Status:   Online
Version:  3.2.1
Latency:  42 ms
```

Acceptance: this is the `mobile-first` result. Under the `desktop-first` default the same data may stay a table — see R11.

## R5 – diff

Example:

```diff
- Old feature
+ New feature
```

Acceptance: the meaning stays unambiguous even without color.

## R6 – Hybrid production post

Must cleanly combine, in one message:

- native heading
- short introduction
- responsive comparison list or compact code block
- `diff`
- note via `>`
- bold
- `-#` subtext

Acceptance: copy/paste as one contiguous Discord raw text.

---

## R7 – Changelog

Must render categories such as `✨ Added`, `🔄 Changed`, `🗑️ Removed`, `🐛 Fixed` cleanly on mobile.

Acceptance: every category heading is recognizable at a glance in the dark theme. Pale, low-contrast category emoji (see § 5.3) fail this case.

## R8 – Callout + status list

Must render blockquote callouts and responsive status lines understandably without depending on color.

## R9 – Metadata + timestamp

Must combine short labels and native `<t:...>` timestamps without unnecessarily wide labels.

## R10 – Links

Must distinguish:

- plain link → preview possible
- masked link → preview also possible
- `<URL>` → preview deliberately suppressed

Acceptance: link presentation matches the desired compactness.

## R11 – Layout mode

The same four-column data set, rendered twice.

Default (`desktop-first`) must keep the table:

```text
SERVICE     STATUS   VERSION   LATENCY
API         Online   3.2.1     42 ms
Database    Online   14.6      18 ms
```

After "make it mobile-friendly", or with `platform: mobile-first`, it must become stacked records:

```text
API
Status:   Online
Version:  3.2.1
Latency:  42 ms
```

Acceptance, in three parts:

1. The default keeps the table. Emitting records unasked ignores the default.
2. A mobile request actually changes the layout. Emitting the table again ignores the request.
3. The wide default output is followed by one short line offering the mobile version (§ 2) — and a narrow post is not.

---

# 19. User guidelines & style profiles

`discord-writer` is a **content-aware rendering engine**. The technical Discord rules come from this skill; the desired visual signature can be defined by the user through a **style profile** or **guideline**.

A style profile describes preferences, not a rigid template. Within the guideline, the skill may automatically switch to a more robust format when content, mobile readability or Discord syntax require it.

## 19.1 Keep two layers strictly separate

### Layer A – technical rules of the skill

These rules protect correctness, Discord compatibility and copy/paste robustness. They apply regardless of the style profile, unless the user explicitly sets a technically sensible exception, e.g. `desktop-only`.

Examples:

- no invented values, IDs, URLs or Unix timestamps
- transport finished Discord messages as copyable raw text
- no fake YAML/INI/CSS color tricks
- do not treat ANSI as a cross-platform standard
- `>>>` only for the remainder of the message
- do not force long tables on mobile
- account for link preview behavior correctly
- respect the Discord character limit

### Layer B – the user's style profile

The profile controls, among other things:

- tone and text density
- emoji style
- heading style
- preferred change presentation
- changelog categories
- status symbols
- callout style
- inline code usage
- link and preview policy
- timestamp presentation
- checklist style
- post splitting
- terminology

---

# 20. Guideline setup mode

When the user wants to create their own Discord guideline, design language or style profile, switch into **guideline setup mode**.

## 20.1 Goal

Produce, in the end:

1. a short human-readable style guide
2. a machine-readable `discord_writer` profile
3. optionally 1–3 reference posts for visual sign-off

## 20.2 When the user already states concrete wishes

Adopt them directly. Do not ask again about preferences that are already settled.

## 20.3 When essential decisions are open

Clarify at least these points, as compactly as possible:

1. **Audience:** community, developers, customers, internal, management, etc.
2. **Tone:** neutral, technical, casual, community, executive
3. **Density:** compact, balanced, spacious
4. **Emojis:** none, minimal, functional, visual
5. **Changes:** compact list, strikethrough, change card, diff, auto
6. **Links:** preview allowed, preferably suppressed, or automatic
7. **Post splitting:** single-message, thread-friendly, aggressive-split
8. **Screen priority:** desktop-first (default), balanced or mobile-first

Only ask about points that cannot reasonably be derived from context.

## 20.4 Offer presets when the user has no clear direction yet

### Corporate

- emojis: none/minimal
- density: balanced
- tone: neutral
- changes: compact list
- callouts: text-oriented
- links: preview usually suppressed
- change cards: rare

### Technical

- emojis: minimal/functional
- density: compact
- tone: technical
- changes: compact list, `diff` for exact changes
- inline code: technical-only/always for technical values
- links: compact
- metadata: preferred

### Community

- emojis: functional/visual
- density: balanced
- tone: community
- changelog categories: preferred
- callouts and status list: allowed
- change cards: for highlights

### Executive

- emojis: minimal
- density: compact
- tone: executive
- core statement first
- at most 3–5 main points
- technical details in a follow-up post or thread

---

# 21. Preference and override hierarchy

## 21.1 Non-negotiable technical constraints

Technical correctness and valid Discord output take precedence. A style profile must not force invalid or obviously unreadable output.

## 21.2 After that, the following order of preference applies

1. **Explicit instruction for the current post**
2. **Active user style profile / guideline**
3. **Semantically appropriate presentation for the content**
4. **Platform and mobile safety rules**
5. **Defaults of this skill**

Example:

- the profile prefers `compact-list`.
- but one value pair is very long.
- then the skill may automatically use `hanging-change`.

## 21.3 Adaptation policy

A profile can define how far the skill may deviate from preferences:

- `silent-safe` – switch to a more robust presentation automatically; default
- `explain-on-deviation` – briefly explain the deviation when it is relevant
- `strict` – keep the style preference as far as technically sensible

`strict` does not lift technical hard rules.

## 21.4 Desktop-first exception

If `target.platform: desktop-first` or explicitly `desktop-only` is set, the mobile width and column rules are replaced by the wider budget in § 2.1 — including keeping 4+ column tables. Even then, do not use unnecessarily complex or unstable layout hacks, and do not treat the wider budget as permission to break the technical hard rules listed in § 2.1.

---

# 22. Style profile schema

A style profile can be supplied as YAML-like configuration. This YAML is **configuration of the skill**, not Discord output and not fake highlighting.

Ignore unknown fields, or ask about them only when they are decisive for the desired output.

## 22.1 Full reference schema

```yaml
discord_writer:
  profile: "technical-release"

  target:
    platform: desktop-first      # desktop-first (default) | balanced | mobile-first — see § 2.1
    density: compact             # compact | balanced | spacious

  language:
    locale: en                   # output language of the Discord message
    tone: technical              # neutral | technical | casual | community | executive
    verbosity: concise           # terse | concise | detailed
    terminology: mixed           # localized | preserve-original | mixed

  adaptation:
    policy: silent-safe          # silent-safe | explain-on-deviation | strict

  headings:
    h1: true
    max_depth: 2
    emoji: functional            # none | minimal | functional | visual
    short_titles: true

  changes:
    default: compact-list        # auto | compact-list | strikethrough | hanging | card | diff
    replacement: strikethrough   # compact-list | strikethrough | hanging | card | auto
    important: card              # card | hanging | compact-list | auto
    added_removed: diff          # diff | changelog | list | auto
    card_max_items: 3

  changelog:
    enabled: true
    categories:
      new: "✨ Added"
      changed: "🔄 Changed"
      removed: "🗑️ Removed"
      fixed: "🐛 Fixed"
      performance: "⚡ Performance"
      security: "🔒 Security"
      breaking: "⚠️ Breaking Change"
      docs: "📚 Documentation"

  status:
    style: emoji-text            # emoji-text | text-only | custom
    ok: "🟢"
    warning: "🟡"
    error: "🔴"
    unknown: "⚪"
    progress: "🔵"

  callouts:
    enabled: true
    info: "ℹ️ Information"
    warning: "⚠️ Warning"
    success: "✅ Success"
    error: "❌ Error"
    tip: "💡 Tip"
    security: "🔒 Security"

  values:
    inline_code: technical-only  # always | technical-only | never | auto
    strikethrough_old: true

  timestamps:
    enabled: true
    datetime: f                  # t | T | d | D | f | F
    relative: R
    show_relative_when_useful: true

  links:
    mode: named                  # auto | named | compact | raw
    previews: suppress           # allow | suppress | auto
    label_emoji: true

  checklist:
    style: unicode               # unicode | emoji | plain
    done: "☑"
    open: "☐"

  codeblocks:
    mobile_target_width: 36      # overrides the width budget of target.platform
    mobile_soft_max_width: 40
    allow_wide_tables: false     # true keeps 4+ column tables in any platform mode

  splitting:
    mode: thread-friendly        # single-message | thread-friendly | aggressive-split | auto
    target_chars: 1800
    hard_chars: 2000

  output:
    transport: fenced            # fenced | raw
    outer_fence: 4
```

The label values above are defaults for English output. When `language.locale` is a different language, translate the category, callout and status labels accordingly unless the profile defines its own wording.

## 22.2 Minimal profile

The user does not have to fill in the full schema. A minimal profile is enough:

```yaml
discord_writer:
  profile: "my-style"
  target:
    density: compact
  language:
    tone: technical
  headings:
    emoji: minimal
  changes:
    default: compact-list
  links:
    previews: suppress
```

All fields that are not set inherit the skill's defaults.

## 22.3 A plain-language profile is equally valid

This is a valid profile as well:

```text
Discord guideline "internal-tech":
- technical and terse
- as few emojis as possible
- mobile-first
- values as inline code
- release notes as a changelog
- diff only for real added/removed changes
- no link previews
- split long posts thread-friendly
```

The skill translates such statements internally into the profile fields.

## 22.4 Custom categories and symbols

The user may freely adjust categories, terms and symbols, as long as the meaning remains recognizable as text as well.

Example:

```yaml
changelog:
  categories:
    new: "🎉 Features"
    changed: "🛠️ Improvements"
    removed: "📦 Retired"
    fixed: "🩹 Fixes"
```

When replacing the defaults, keep the contrast requirement from § 5.3 in mind: a category emoji with no strong color of its own weakens exactly the heading it is meant to mark.

Status example:

```yaml
status:
  ok: "✅"
  warning: "⚠️"
  error: "❌"
  progress: "🔧"
```

---

# 23. Design latitude within the profile

## 23.1 Density

### `compact`

- little whitespace
- short headings
- prefer Native Compact Lists
- change cards sparingly
- good for technical threads and frequent updates

### `balanced`

- clear section separation
- moderate blank lines
- a mix of compact lists, callouts and changelog categories

### `spacious`

- stronger section separation
- change cards allowed more often
- more vertical room for important content

## 23.2 Emoji style

### `none`

No decorative emojis. Semantics fully carried by text.

### `minimal`

Only a few central emojis, e.g. in H1 or warnings.

### `functional`

Emojis carry supporting semantics, e.g. `✨ Added`, `⚠️ Warning`, `🟢 Online`.

### `visual`

More visual guidance allowed, but still no emoji chains or pure decoration without purpose.

## 23.3 Change style

The user may prefer:

- `compact-list`
- `strikethrough`
- `hanging`
- `card`
- `diff`
- `auto`

`auto` leaves the choice entirely to the content-aware decision logic.

## 23.4 Typographic weighting

Profiles may define how technical values are highlighted:

- `inline_code: always`
- `inline_code: technical-only`
- `inline_code: never`
- `inline_code: auto`

Preferred default: `technical-only`.

## 23.5 Link policy

- `named + allow` → readable links, embed allowed
- `named + suppress` → if the embed really must be suppressed, switch to `<URL>`; the freely chosen link text is lost in the process
- `compact + suppress` → `<URL>`
- `raw + allow` → visible raw URL
- `auto` → decide based on post length and context

## 23.6 Splitting policy

### `single-message`

One message as far as possible, within Discord's limits.

### `thread-friendly`

Keep the main post compact and emit detail areas as separate, logically self-contained messages.

### `aggressive-split`

Split early by topic area; useful for very long documentation or patch notes.

### `auto`

Decide based on character volume, number of topics and information density.

---

# 24. Content-aware adaptation

A style profile is **not a rigid template**.

Before rendering, the skill must combine two things:

1. the desired visual signature
2. the actual shape of the information

## Examples

### The profile prefers a compact list, but the values are too long

Do not force:

```text
- **Access control:** `individual ... very long` → `role-based ... very long`
```

Use Hanging Change or a Change Card automatically instead.

### The profile prefers change cards, but there are 25 small changes

Do not produce 25 cards. Use changelog categories or a compact list.

### The profile prefers `diff`, but the content is an explanation

Running text stays native Markdown; only use `diff` where added/removed or before/after semantics genuinely apply.

### The profile is `mobile-first`, but the user asks for a wide table

Honor the request for this post (§ 2.1). Emit the table and add one short line that it will scroll horizontally on phones. Do not refuse the layout, and do not hand it over without the caveat.

### The default produced a wide table, and the user says it has to work on phones

Rebuild that post under the `mobile-first` budget: the table becomes records, long pairs become hanging changes. Keep the content identical — this is a layout change, not a rewrite. Treat the statement as applying to the rest of the conversation too (§ 2.1).

### The profile prefers no-preview links

Emit external links as `<URL>` when the embed should be suppressed. If a named link is strictly required, be aware that Discord may still create an embed.

---

# 25. Applying profiles

## 25.1 The user supplies a profile in the prompt

Apply it to the current Discord output.

## 25.2 The user supplies a guideline file

Treat it as a style profile and use it as a preference layer on top of this skill's defaults.

## 25.3 Multiple profiles are present

Use the profile named by the user. If no profile is named, use a profile explicitly marked as `default`. In case of genuine ambiguity, ask briefly.

## 25.4 Temporary override

A user may deviate from the profile for a single post:

```text
Use technical-release, but this time without emojis and with link previews.
```

This override applies only to the current request unless stated otherwise.

## 25.5 Profile output

If the user explicitly wants a guideline/profile file created, emit the full profile as a standalone artifact. If only a Discord post is requested, do **not** emit the profile as well.

---

# 26. Quality check before every output

## Profile / guideline

- [ ] Is an active style profile present and correctly selected?
- [ ] Were current post overrides prioritized above profile preferences?
- [ ] Was the profile treated as a preference and not as a rigid template?
- [ ] Was `adaptation.policy` respected for deviations?
- [ ] Were technical hard rules upheld despite profile wishes?

## Content

- [ ] Are numbers, units, names and terms correct?
- [ ] Was nothing invented?
- [ ] Are exceptions and limitations preserved?
- [ ] Is the information hierarchy clear?

## Layout mode

- [ ] Which mode applies — `desktop-first` (default), `balanced` or `mobile-first` (§ 2.1)?
- [ ] Was a user statement about the audience ("has to work on phones", "make it mobile-friendly") honored?
- [ ] Was structure preserved rather than pre-shrunk for a phone nobody asked about?
- [ ] If the result is wide enough to break on a phone, was the mobile version offered in one short line (§ 2)?
- [ ] Was that offer left out for output that already reflows fine?

## Mobile layout

Applies when composing for `mobile-first`; use the wider budget from § 2.1 for `balanced` and for the `desktop-first` default.

- [ ] Is native Markdown the better responsive choice?
- [ ] Is the `#` main heading short enough, in particular free of an overly long unbreakable single word?
- [ ] Are code block lines within the width budget of the active mode?
- [ ] Is the soft maximum exceeded only for a good reason?
- [ ] Are there unnecessarily wide tables for the active mode?
- [ ] Are 4+ columns replaced by a Record Layout — unless the mode permits them?
- [ ] Are long changes presented as a Hanging Change?
- [ ] Is a table understandable without desktop width, where mobile matters?

## Discord syntax

- [ ] Is `>>>` used only when the remaining message content should be quoted?
- [ ] Are nested lists limited to a sensible depth?
- [ ] Are spoilers used deliberately and sparingly?
- [ ] Is fake syntax highlighting avoided?
- [ ] Is ANSI not assumed to be cross-platform?
- [ ] Are callouts built correctly line by line with `>`?
- [ ] Are identical semantic emojis not used twice in immediate succession?
- [ ] Do checklists use `☑ / ☐` instead of supposed Markdown checkboxes?
- [ ] Are metadata labels short enough for mobile?
- [ ] Was it a deliberate decision whether a link preview is wanted?
- [ ] Is `<URL>` used when an embed should be suppressed deliberately?

## Copy/paste

- [ ] Is every finished Discord message a single copyable raw text block?
- [ ] Are four outer backticks used as transport when inner backticks must be preserved?
- [ ] Are all inner Discord backticks visible and copied along?
- [ ] Are logically separate Discord messages packaged separately?

## Style

- [ ] Are emojis sparse and functional?
- [ ] Are headings short?
- [ ] Are notes clearly separated from the main content?
- [ ] Was unnecessary decoration avoided?

---

# 27. References

When current Discord syntax or syntax highlighting needs to be verified, use this priority:

## Primary sources

1. Discord Support – Markdown Text 101  
   https://support.discord.com/hc/en-us/articles/210298617-Markdown-Text-101-Chat-Formatting-Bold-Italic-Underline

2. Discord Support – How do I disable auto-embed?  
   https://support.discord.com/hc/en-us/articles/206342858--How-do-I-disable-auto-embed

3. Discord Developer Reference – Message Formatting / Timestamps  
   https://docs.discord.com/developers/reference

Official Discord documentation takes precedence.

## Community references

4. Matthew Zring – A guide to Markdown on Discord  
   https://gist.github.com/matthewzring/9f7bbfd102003963f9be7dbcf7d40e51

5. cherryblossom000 – discord-syntax-highlighting  
   https://github.com/cherryblossom000/discord-syntax-highlighting  
   https://discord-syntax-highlighting.vercel.app/

6. Hacksore – discord-md  
   https://github.com/Hacksore/discord-md  
   https://discord-md.vercel.app/

## Technical secondary reference

7. Highlight.js – Supported Languages  
   https://highlightjs.readthedocs.io/en/latest/supported-languages.html

Rules:

- Official Discord documentation beats community guides.
- Community sites are a supplement and a test reference, not an official specification.
- Highlight.js support does not automatically mean identical Discord rendering.
- When in doubt, use a neutral code block rather than invented highlighting syntax.
- Treat new or experimental formatting as such.

---

# 28. Short form of the most important rules

1. **Native Discord Markdown is the general standard.**
2. **Keep main headings short; avoid very long single words.**
3. **Short `OLD → NEW` comparisons as a Native Compact List.**
4. **A few direct replacements may use Strikethrough Change.**
5. **Highlight 1–3 central changes as a Change Card.**
6. **Prefer structuring release notes into changelog categories.**
7. **Compose for desktop by default; ≤ 36 characters is the mobile budget, applied on request (§ 2).**
8. **Offer a mobile version after a wide post — do not convert it unasked.**
9. **4+ column tables are fine in the `desktop-first` default; convert them to records for `mobile-first` (§ 2.1).**
10. **Long changes as a Hanging Change.**
11. **Multiple properties per object as a Record Layout.**
12. **Simple states of multiple objects as a Status List.**
13. **`diff` for precise removed/added.**
14. **Callouts with `>` for information, warning, success and error.**
15. **Checklists with `☑ / ☐` by default, not with `- [x]`.**
16. **Keep metadata labels short; prefer Discord timestamps for times.**
17. **`f` plus optional `R` are good mobile defaults for release/event times.**
18. **Masked links can produce embeds; `<URL>` suppresses the preview.**
19. **`>>>` quotes the remainder of the message; use repeated `>` for bounded multiline blocks.**
20. **ANSI only desktop-only / experimental.**
21. **No fake YAML/INI/CSS highlighting tricks.**
22. **Plan normal posts within 2,000 characters by default, or split them logically.**
23. **Transport every finished Discord message as one copyable raw text block.**
24. **Style profiles control the visual signature, not technical correctness.**
25. **The current post instruction beats the profile preference; technical hard rules remain.**
26. **Profiles may define compact, balanced or spacious as well as emoji, change, link, timestamp and splitting styles.**
27. **With long or unsuitable content, switch to a more robust format automatically.**
28. **Plain-language guidelines and YAML profiles are equally valid input forms.**
---

# 29. Content profiles: structure-dependent layouts

A global style profile defines the visual identity. **Content profiles** additionally define how recurring **information structures** are built within that same identity.

Content profiles are strictly **domain-neutral**. The base skill must not contain a fixed taxonomy for a particular game, product, brand, organization or industry. Specific field names are only adopted at runtime from user input or from an explicit user guideline.

## 29.1 Basic principle

Separate three layers:

1. **Global style** – tone, density, emoji style, link policy, screen priority, typography
2. **Content profile** – structure and preferred layouts for a recurring information structure
3. **Current post override** – one-off deviations for the specific post

Inheritance:

```text
Global style
   ↓
Content profile
   ↓
Current post override
   ↓
Content-aware safety adaptation
   ↓
Discord output
```

Content profiles may refine global style rules, but must not lift technical hard rules.

## 29.2 When a content profile makes sense

Use content profiles when a user repeatedly publishes content with the same information shape, e.g.:

- multi-part recommendations or configurations
- change logs / changelogs
- instructions / procedures
- dates / schedules
- status reports / incidents
- comparisons
- FAQs
- resource or link collections
- structured object lists with recurring properties

Do not create a dedicated content profile for one-off special cases when the normal content-aware logic is sufficient.

## 29.3 Detecting a content type

If the user names a known content profile, use it.

If no name is given, the skill may select an existing profile based on the information structure, provided the mapping is unambiguous. Examples:

- multiple components/entries with properties → `component-set`
- added/changed/removed/fixed → `changelog`
- steps + prerequisites + result → `procedure`
- point in time + place/channel + sequence → `schedule`
- multiple objects + state → `status`
- multiple alternatives + comparable characteristics → `comparison`
- questions + answers → `faq`
- multiple links/resources + short description → `reference-list`

In case of genuine ambiguity, ask briefly or use the global style without a content profile.

## 29.4 Content profiles are semantic, not domain-specific

Preferred generic names:

- `component-set`
- `changelog`
- `procedure`
- `schedule`
- `status`
- `comparison`
- `faq`
- `reference-list`

In the base skill, avoid names such as product, game, brand or department names.

A user-specific guideline may of course define its own profile names. However, these remain **user data** and do not become part of the generic skill defaults.

---

# 30. Content profile schema

Content profiles live under `content_profiles` and inherit all values they do not override from the global profile.

```yaml
discord_writer:
  profile: "default"

  target:
    platform: desktop-first
    density: balanced

  headings:
    emoji: functional

  values:
    inline_code: technical-only

  content_profiles:
    component-set:
      enabled: true
      purpose: "Multiple related components or entries with properties"
      density: balanced
      terminology: mixed
      intro: short
      primary_layout: record
      grouping: semantic-auto
      field_policy: preserve-source
      record:
        heading: best-identifier
        omit_missing: true
        compact_short_properties: auto
      context:
        enabled: auto
      alternatives:
        enabled: optional
      splitting:
        mode: thread-friendly

    changelog:
      enabled: true
      primary_layout: changelog
      highlight_replacements: strikethrough
      important_change: card

    procedure:
      enabled: true
      primary_layout: native
      steps: numbered
      warnings: callout

    schedule:
      enabled: true
      primary_layout: metadata
      timestamps: true
      relative_time: true

    status:
      enabled: true
      primary_layout: status-list
      details_layout: record

    comparison:
      enabled: true
      primary_layout: auto
      wide_matrix: false
```

All fields are optional. Missing values are inherited from the global profile and the skill defaults.

## 30.1 General content profile fields

Recommended general fields:

```yaml
content_profiles:
  <name>:
    enabled: true
    purpose: "..."
    density: inherit
    terminology: inherit
    intro: short
    primary_layout: auto
    grouping: auto
    field_policy: preserve-source
    section_order: []
    splitting:
      mode: inherit
```

The possible values are preferences. If the concrete information does not fit them, content-aware adaptation takes over.

## 30.2 Field order and optional fields

`section_order` defines a preferred order, but does not oblige the skill to emit empty sections.

`field_policy: preserve-source` means:

- Keep field names from the user input as long as they are understandable.
- Do not invent domain-specific default fields.
- Field names may only be normalized or renamed when a guideline says so clearly.

Hard rule:

**No empty sections and no invented fields just to satisfy a profile.**

If data is missing, omit the section, or ask when the information is genuinely necessary.

---

# 31. `component-set`: generic multi-component profile

`component-set` is the generic profile for content consisting of several related components, entries or building blocks with properties.

It is deliberately domain-neutral and may be used for any kind of multi-part content without presupposing fixed technical terms.

## 31.1 When to choose `component-set`

Typical signals:

- multiple entries or components belong to one shared recommendation, assembly or configuration
- each entry has multiple properties
- individual entries may have different field types
- there may be groups, roles, categories, priorities or variants
- there may be an overarching purpose, context or usage note

## 31.2 Preferred presentation

- main structure → native Markdown
- individual components → Record Layout
- technical or compact values → inline code, if allowed globally/by profile
- long properties → their own line instead of a wide table
- grouping → only when it follows from the content or is given by the user
- important particularity → callout or focus block
- alternatives → compact list or dedicated section
- many components → thread-friendly split if needed

Avoid by default:

- wide tables with many property columns
- fixed domain-specific field names
- invented categories or attributes
- abbreviations that do not come from the source, the user input or the active terminology profile

## 31.3 Terminology modes

`localized`
: Localize terms when unambiguous and wanted by the user.

`preserve-original`
: Keep original terms and labels as unchanged as possible.

`mixed`
: Understandable label plus the established short form, when that form is genuinely known or supplied by the user.

Do not invent abbreviations.

## 31.4 Two density variants

### `balanced`

```text
### Component A

**Name:** Example
**Category:** Example
**Primary value:** `Value`
**Properties:** `Value A` · `Value B`
**Characteristic:** Example
```

The field names in the real output come from the concrete content; the labels above are only schematic placeholders.

### `compact`

```text
**Component A — Example**
Category · `Value A` · `Value B`
Characteristic: Example
```

If a compact line becomes too long, switch back to a Record Layout automatically.

## 31.5 Divergent component types

If certain components structurally have different fields, they may be split into their own semantic groups.

Example principle:

```text
## Group A

### Component 1
...

## Group B

### Component 2
...
```

Never invent group names when the source does not support a meaningful grouping.

## 31.6 Quality check for multi-component content

In addition to the general quality check:

- [ ] Are all supplied components/entries included?
- [ ] Are properties assigned to the correct entry?
- [ ] Were field names and terminology from the source respected?
- [ ] Were no missing properties invented?
- [ ] Are divergent component types separated only when there is genuine structural benefit?
- [ ] Is the presentation understandable on mobile without a wide 4+ column table?
- [ ] Were empty optional fields omitted?

---

# 32. Further generic content profiles

## 32.1 `changelog`

Prefers changelog categories. Direct replacements may use Strikethrough Change; precise added/removed deltas may use `diff`.

## 32.2 `procedure`

Prefers:

- short introduction / goal
- prerequisites, where present
- numbered steps
- callouts for warnings
- code blocks only for real commands/code
- result / verification at the end, where relevant

## 32.3 `schedule`

Prefers a Metadata Block plus Discord timestamps. Add relative time `R` when helpful. Only mention place/channel when known.

## 32.4 `status`

Prefers a Status List for simple states and a Record Layout for detailed data. Incidents may use callouts for impact and next steps.

## 32.5 `comparison`

Short differences as compact lists. Many properties per alternative as individual records instead of a wide matrix. Only use a compact 2-column presentation for very short values.

## 32.6 `faq`

Prefers short question/answer blocks with a clear hierarchy. Do not force tables. Structure longer answers into paragraphs or lists.

## 32.7 `reference-list`

Prefers named links with a short description. Respect the preview policy from the global guideline. Group resources by a sensible category where that can be derived from the content.

---

# 33. Content profile builder

When the user describes a new recurring content type, the guideline builder may derive a content profile from it.

## 33.1 Procedure

1. Identify the information objects.
2. Identify the recurring fields.
3. Separate mandatory from optional fields.
4. Choose the primary Discord layout.
5. Check mobile risks.
6. Determine terminology needs.
7. Define the section order.
8. Define the splitting rule.
9. Check whether an existing generic profile already suffices.
10. Ask only genuinely decisive follow-up questions.
11. Deliver the profile plus one reference post for sign-off.

## 33.2 Domain neutrality in the builder

The builder must first try to map a new use case onto a **generic information shape**.

Example principle:

```text
Multiple things + properties
→ component-set

Steps + prerequisites
→ procedure

Point in time + sequence
→ schedule
```

Only when the user explicitly wants a domain-specific guideline may the generated user profile contain domain-specific names or fields. Such details are **not carried back into the base skill**.

## 33.3 Good follow-up questions

Only ask when the answer changes the presentation significantly, e.g.:

- audience experienced or beginner-friendly?
- established abbreviations or spelled-out terms?
- compact overview or explanatory post?
- show alternatives/variants regularly or only on request?

No questions are needed about things that can safely be inherited from the global style profile.

## 33.4 Builder output

The builder delivers:

1. a short interpretation
2. the proposed information structure
3. the content profile configuration
4. justified defaults only where relevant
5. a realistic but domain-neutral reference post, or a reference post based on the data the user actually supplied
6. open decisions only when they have real impact

---

# 34. Content profile priority

For the final presentation the following applies:

1. Technical hard rules
2. Explicit current post instruction
3. Global style profile
4. Active content profile
5. Semantic content-aware adaptation
6. Skill defaults

If a current post override directly overrides a content profile rule, the current post override wins.

Example:

```text
Use my default profile and `component-set`,
but make this post extra compact and without alternatives.
```

The global rules then still apply, but for this post only the named preferences are temporarily overridden.

---

# 35. Domain-neutrality rule

The base skill stays universally applicable.

Hard rules:

1. No game-, product-, brand-, industry- or organization-specific default profiles.
2. No domain-specific mandatory fields in the base schema.
3. Examples in the base skill use neutral placeholders or terms actually supplied by the user.
4. User profiles may be specific if the user wants that; they stay separate from the base skill.
5. Map new use cases onto generic information shapes first.
6. Never fill in missing domain-specific data from presumed expert knowledge.

---

# 36. Short rule for content profiles

**The global style says how Discord content looks. The content profile says how an information shape is organized. The concrete domain comes exclusively from the user context.**

Never emit the same information twice just to satisfy a template. Do not create empty sections. Do not invent missing data. Mobile robustness remains mandatory.
