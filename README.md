![discord-writer — turning rough notes into mobile-first Discord messages](assets/banner.png)

# discord-writer

`discord-writer` is a domain-neutral skill for turning arbitrary content into copy-ready, mobile-first Discord messages.

You paste in rough notes, a changelog, a status dump or a config diff. You get back one block of Discord raw text that you can paste straight into a channel — structured for a narrow phone screen, not for a wide desktop table.

It focuses on information structure rather than a specific game, product, brand, community, or industry. Domain-specific terminology may come from the current user input or from a user-defined guideline/profile, but never from the base skill itself.

---

## See it in action

Every block below is real skill output, shown exactly as it arrives — and next to it, how Discord renders it.

### 1. One post, six formats

This is the flagship case. A release announcement that combines a heading, running text, a change list, a colored `diff`, a warning callout, a live timestamp and a subtext footer — in a **single** message that still holds together on a phone.

**You type:**

```text
2.5.0 is out, bulk export is live and sync is automatic now. timeout 30 to 45
seconds. we replaced the manual CSV import with a scheduled bulk export.
first run is tonight, might take an hour. rollout aug 20 19:00. old exports
stay for 30 days.
```

**You paste into Discord:**

````text
# 🚀 Release 2.5.0

Bulk export is live, and sync no longer needs a human.

## Changes

- **Timeout:** `30 s` → `45 s`
- **Sync:** `Manual` → `Automatic`

## Replaced

```diff
- Manual CSV import
+ Scheduled bulk export
```

> ⚠️ **Warning**
> The first run starts tonight and may take an hour.

**Rollout:** <t:1787241600:R>

-# Existing exports stay available for 30 days.
````

Six presentation formats in one message — native Markdown, a compact change list, a `diff`, a callout, a timestamped metadata line and a subtext footer. Each was chosen because of what the information *is*, not because it looked good.

### 2. The table trap

This is the single most common way Discord posts break on mobile. Four columns fit your monitor and destroy a phone screen:

```text
SERVICE     STATUS   VERSION   LATENCY
API         Online   3.2.1     42 ms
Database    Online   14.6      18 ms
```

The skill turns the same data into stacked records instead:

```text
API
Status:   Online
Version:  3.2.1
Latency:  42 ms

Database
Status:   Online
Version:  14.6
Latency:  18 ms
```

Code block lines are planned at **≤ 36 visible characters**, measured from actual Discord rendering tests on mobile.

### 3. Color that survives copy/paste

A `diff` block is the one place Discord gives you real red and green without hacks — and it still reads correctly for anyone who cannot see color, because `-` and `+` carry the meaning too.

````text
```diff
- Manual retry after failure
- Status page updated by hand

+ Automatic retry with backoff
+ Live status from health checks
```
````

ANSI color blocks exist, but they did **not** render on the tested mobile client. The skill treats them as desktop-only and never picks them automatically.

### 4. Before and after, at a glance

When one change matters more than the rest, it gets a stacked card instead of a bullet:

```text
## ⭐ Key change

**Access control**

🔴 **Before**
`Individual user permissions`

🟢 **After**
`Role-based permissions`
```

`Before` and `After` stay spelled out. The color is an accent, never the message.

### 5. Timestamps that adapt to the reader

This is the trick most Discord posts miss. Write the time once, and every reader sees it in **their own** time zone — with a live countdown that keeps updating:

```text
# 📅 Maintenance window

**Environment:** `Production`
**Starts:** <t:1787241600:f>
**Countdown:** <t:1787241600:R>
**Duration:** ~2 h

> ℹ️ **Information**
> Read access stays available throughout.
```

That timestamp resolves to 16:00 UTC. A reader in Berlin sees 18:00, one in New York sees 12:00, and both see the same live countdown next to it — from the exact same message text. No "18:00 CEST (12:00 EDT)" gymnastics, and nothing to correct when the date moves.

### 6. Release notes → changelog categories

**You type:**

```text
2.5.0 shipped. new bulk export and a dark theme. session timeout went
from 30 to 45 minutes. dropped the old CSV importer. fixed the crash
on empty search.
```

**You paste into Discord:**

```text
# 🚀 Release 2.5.0

## ✨ Added
- Bulk export
- Dark theme

## 🔄 Changed
- **Session timeout:** ~~`30 min`~~ → `45 min`

## 🗑️ Removed
- Legacy CSV importer

## 🐛 Fixed
- Crash on empty search
```

Only categories that actually have content are emitted — no empty `Removed` heading just because the template has one.

### 7. Live status without a table

```text
## Service status

🟢 **API** — Online · `42 ms`
🟢 **Database** — Online · `18 ms`
🟡 **Cache** — Degraded · `91 ms`
🔴 **Backup** — Failed
```

The state is always written as text too. The emoji never carries the meaning on its own.

### 8. Long changes stay unambiguous

A long `OLD → NEW` pair would blow past the width budget on one line, so it becomes a stacked hanging change:

````text
## Access control

```
Access control
  Individual permissions
  → Role-based permissions
```

> ⚠️ **Warning**
> This change requires a restart.
````

Note that examples 1, 3 and 8 are wrapped in **four** backticks, because the Discord message itself contains three. That is the transport rule (§ 3), and it is why the skill hands you output that way in a chat interface: the inner backticks survive copy/paste instead of being swallowed by ChatGPT or Claude.

---

## Using the skill

The skill is a single file: [`SKILL.md`](SKILL.md). Any assistant that can read it can follow it.

### Claude Code

Install it as a personal skill so it loads automatically when a task looks like Discord formatting:

```bash
git clone https://github.com/gomaaz/discord-writer.git
mkdir -p ~/.claude/skills/discord-writer
cp discord-writer/SKILL.md ~/.claude/skills/discord-writer/SKILL.md
```

For a single project instead of your whole account, use `.claude/skills/discord-writer/SKILL.md` inside that project.

Then just ask:

```text
Format this as a Discord announcement: <your notes>
```

### Claude (claude.ai / Desktop)

Create a Project and upload `SKILL.md` to the project knowledge, then put a short line in the project instructions:

```text
When I ask for Discord output, follow SKILL.md from the project knowledge.
```

The specification is ~57 KB, which is why it belongs in project knowledge rather than pasted into an instructions field.

For a one-off message, attaching `SKILL.md` to a single chat works too.

### ChatGPT

**As a custom GPT:** upload `SKILL.md` under **Knowledge** — it is far too long for the instructions field — and put this in **Instructions**:

```text
You format content as Discord messages by following the attached
discord-writer specification (SKILL.md).

Before writing any Discord message, consult SKILL.md: choose the layout
from the shape of the information, keep it mobile-first, and wrap every
finished Discord message in four backticks so the inner Discord backticks
survive copy/paste.

Never invent values, field names, IDs or Unix timestamps that were not
supplied.
```

**In a project or a single chat:** attach `SKILL.md` and open with:

```text
Follow the attached SKILL.md when formatting Discord messages.
Format this as a Discord post: <your notes>
```

### Any other assistant

`SKILL.md` is deliberately vendor-neutral — no Claude-specific or OpenAI-specific requirements. Give the assistant the file and tell it to follow the skill when preparing Discord content.

### Verifying it works

Send this and check the answer:

```text
Format as a Discord post: timeout 30 to 45 seconds, status beta to stable
```

You should get a compact list wrapped in four outer backticks — not a Markdown table, and not a fenced block that has eaten its own backticks.

---

## Configuring the output

The skill has a default look, but you steer it with a **style profile**. Both of these are valid, equivalent input:

```yaml
discord_writer:
  target:
    density: compact
  headings:
    emoji: minimal
  links:
    previews: suppress
```

```text
Discord guideline "internal-tech": technical and terse, few emojis,
mobile-first, values as inline code, no link previews.
```

Profiles control density, emoji style, headings, change styles, links, timestamps, splitting behavior and terminology. Technical correctness is not negotiable by a profile — if a preference would produce unreadable output, the skill switches to a safer layout.

Ready-made starting points: [`templates/GUIDELINE-TEMPLATE.yaml`](templates/GUIDELINE-TEMPLATE.yaml) and [`examples/PROFILE-EXAMPLES.md`](examples/PROFILE-EXAMPLES.md) (Corporate, Technical, Community, Executive).

To have the skill build one for you, ask: `Help me create a Discord style guideline.`

---

## What it does

- native Discord Markdown for normal posts
- mobile-first `OLD → NEW` / before-after layouts
- changelog categories
- callouts and status lists
- record layouts for multi-field objects
- `diff` blocks for added/removed content
- Discord timestamps
- masked links and preview suppression
- Unicode checklists
- style profiles / guidelines
- generic content profiles based on information shape
- transport rules that preserve literal Discord backticks when output is generated by an AI chat interface

## Core design principle

The base skill stays generic. It selects a layout from the *shape of the information*:

- normal prose → native Discord Markdown
- short replacements → compact change list
- long replacements → hanging change
- a set of objects with multiple fields → record/component layout
- added/removed items → `diff`
- release changes → changelog categories
- warnings/info/errors → callouts
- status summaries → status list

## Repository layout

```text
discord-writer/
├── SKILL.md
├── README.md
├── CLAUDE.md
├── PROJECT_HANDOFF.md
├── CHANGELOG.md
├── GENERICITY.md
├── LICENSE
├── VERSION
├── assets/
│   └── banner.png
├── templates/
│   ├── GUIDELINE-TEMPLATE.yaml
│   └── CONTENT-PROFILE-TEMPLATE.yaml
└── examples/
    ├── PROFILE-EXAMPLES.md
    ├── COMPONENT-SET-PROFILE-EXAMPLE.yaml
    └── SCREENSHOT-SOURCES.md
```

## Working on this repository

This is the contributor path, not the usage path. Open the repository directory in Claude Code and ask Claude to read `CLAUDE.md`, `PROJECT_HANDOFF.md`, and `SKILL.md` before making changes. A suggested first instruction is included in `PROJECT_HANDOFF.md`.

`SKILL.md` is the canonical source of behavior. There is no build step and no test runner; § 18 of `SKILL.md` holds the regression cases R1–R10 that should be re-checked after substantial changes.

## Language

The specification is written in English. That does not determine the output language: Discord messages are written in the user's language. The default labels (changelog categories, callout labels, status wording) are English defaults and should be translated when `language.locale` is a different language, or replaced through a profile.

## Version

Current version: **1.5**

## License

MIT — see [LICENSE](LICENSE).
