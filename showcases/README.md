# Showcase gallery

What `discord-writer` output actually looks like in Discord.

Each showcase is one message: the rough input, the raw text the skill produces,
and a screenshot of how Discord renders it. Everything here is real output, not
a mockup.

> **Screenshots in progress.** Entries marked ⬜ below have their source ready but
> no capture yet. See [Adding a showcase](#adding-a-showcase) at the end.

## Overview

| | Showcase | Shows | Shot |
|---|---|---|---|
| 01 | [One post, six formats](#01--one-post-six-formats) | The flagship: heading, prose, change list, diff, callout, timestamp, subtext in a single message | ⬜ |
| 02 | [One layout, two screens](#02--one-layout-two-screens) | The same data as a desktop table and as mobile records | ⬜ |
| 03 | [Screen priority](#03--screen-priority) | Composing for desktop by default, and the offer to narrow it | ⬜ |
| 04 | [Color that survives copy/paste](#04--color-that-survives-copypaste) | `diff` with real red and green, readable without color too | ⬜ |
| 05 | [Before and after](#05--before-and-after) | Change card for the one change that matters | ⬜ |
| 06 | [Timestamps that adapt](#06--timestamps-that-adapt-to-the-reader) | One text, every reader sees their own time zone | ⬜ |
| 07 | [Release notes](#07--release-notes) | Changelog categories, only the ones with content | ⬜ |
| 08 | [Live status](#08--live-status) | Status list with color *and* text | ✅ |
| 09 | [Long changes](#09--long-changes) | Hanging change plus a warning callout | ✅ |

---

## 01 — One post, six formats

The flagship case. Native Markdown, a compact change list, a `diff`, a callout, a
timestamped metadata line and a subtext footer — in **one** message.

**Input:**

```text
2.5.0 is out, bulk export is live and sync is automatic now. timeout 30 to 45
seconds. we replaced the manual CSV import with a scheduled bulk export.
first run is tonight, might take an hour. rollout aug 20 19:00. old exports
stay for 30 days.
```

**Output:**

`````text
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
`````

Each format was chosen because of what the information *is* — not because it
looked good.

---

## 02 — One layout, two screens

By default the skill composes for desktop, so a four-column table stays a table:

`````text
```
SERVICE     STATUS   VERSION   LATENCY
API         Online   3.2.1     42 ms
Database    Online   14.6      18 ms
```
`````

Ask for a mobile-safe layout and the same data becomes stacked records, because
four columns fit a monitor and destroy a narrow screen:

`````text
```
API
Status:   Online
Version:  3.2.1
Latency:  42 ms

Database
Status:   Online
Version:  14.6
Latency:  18 ms
```
`````

Capture the table on desktop and the records on a phone — that is the pair the
default and the mobile request actually produce. Keep the width consistent within
each half, otherwise the comparison proves nothing.

---

## 03 — Screen priority

Desktop is the default because composing happens at a keyboard (`SKILL.md` § 2).
A wide result comes with an offer to narrow it — and only a wide one.

**Input:**

```text
service overview: api online 3.2.1 42ms, database online 14.6 18ms,
cache degraded 7.0.5 91ms
```

**Output (default `desktop-first`):**

`````text
## Service overview

```
SERVICE     STATUS     VERSION   LATENCY
API         Online     3.2.1     42 ms
Database    Online     14.6      18 ms
Cache       Degraded   7.0.5     91 ms
```

`````

Followed by one short line:

```text
This uses a wide table. Want a mobile-friendly version?
```

Say yes and it becomes the stacked records from showcase 02. The screenshot pair
is the point: the structure is offered, never taken away unasked. A compact list
or a callout gets no such offer — it already reflows.

---

## 04 — Color that survives copy/paste

A `diff` block is the one place Discord gives real red and green without hacks —
and it still reads correctly without color, because `-` and `+` carry the meaning
too.

`````text
```diff
- Manual retry after failure
- Status page updated by hand

+ Automatic retry with backoff
+ Live status from health checks
```
`````

ANSI color blocks exist but did **not** render on the tested mobile client. The
skill treats them as desktop-only and never picks them automatically.

---

## 05 — Before and after

When one change matters more than the rest, it gets a stacked card instead of a
bullet.

````text
## ⭐ Key change

**Access control**

🔴 **Before**
`Individual user permissions`

🟢 **After**
`Role-based permissions`
````

`Before` and `After` stay spelled out. The color is an accent, never the message.

---

## 06 — Timestamps that adapt to the reader

Write the time once; every reader sees it in their own time zone, with a live
countdown.

````text
# 📅 Maintenance window

**Environment:** `Production`
**Starts:** <t:1787241600:f>
**Countdown:** <t:1787241600:R>
**Duration:** ~2 h

> ℹ️ **Information**
> Read access stays available throughout.
````

That timestamp is 2026-08-20 16:00 UTC: Berlin sees 18:00, New York sees 12:00.
Once the date has passed, substitute a future one before capturing.

---

## 07 — Release notes

````text
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
````

Only categories with actual content are emitted. The category emoji are chosen
for contrast in the dark theme (`SKILL.md` § 5.3) — if a capture shows `✨` or
`🗑️` washing out, that is a finding worth reporting.

---

## 08 — Live status

````text
## Service status

🟢 **API** — Online · `42 ms`
🟢 **Database** — Online · `18 ms`
🟡 **Cache** — Degraded · `91 ms`
🔴 **Backup** — Failed
````

The state is always written as text too. The emoji never carries the meaning on
its own.

---

## 09 — Long changes

A long `OLD → NEW` pair is hard to follow on one line, so it becomes a stacked
hanging change — and it stays unambiguous at any width.

`````text
## Access control

```
Access control
  Individual permissions
  → Role-based permissions
```

> ⚠️ **Warning**
> This change requires a restart.
`````

---

## Adding a showcase

### Capturing

| | |
|---|---|
| Theme | Dark — it is where the contrast rules were tested |
| Device | Desktop for default output; a real phone for anything showing the `mobile-first` layout. Showcases 02 and 03 need both, at a consistent width. |
| Crop | One message per shot — no channel list, no message bar, no usernames |
| Format | PNG into `images/`, named after the showcase, e.g. `01-hybrid-post.png` |

Post into a quiet channel — a private server or a DM with yourself — so no
avatars or unrelated messages end up in frame.

The blocks above are fenced with **more** backticks than the message needs,
because several contain code blocks of their own. That outer fence is the
transport container from `SKILL.md` § 3 — **do not paste it**. Copy only what is
inside, including any inner ``` lines, which *are* part of the Discord message.

### Writing a new entry

1. Add a row to the overview table.
2. Add a section with input (where useful), output, and one or two sentences on
   *why* this layout — not just what it looks like.
3. Drop the screenshot into `images/` and reference it in the section.
4. If the showcase demonstrates a rule, cite the section number from `SKILL.md`.

Keep entries domain-neutral: no game, product, brand or company specifics. The
examples use generic services, releases and settings for that reason.

A showcase that contradicts `SKILL.md` is a finding, not a bad showcase. The
specification is built from rendering tests; if the render disagrees, the
specification is what changes.
