# Showcase gallery

What `discord-writer` output actually looks like in Discord.

Each showcase is one message: the rough input, the raw text the skill produces,
and a screenshot of how Discord renders it. Everything here is real output, not
a mockup.

Every screenshot below is a real Discord client rendering the raw text shown
above it — dark theme, desktop unless marked otherwise. To reproduce one, copy
the matching file from [`messages/`](messages/) and paste it into a channel;
[`CAPTURE-GUIDE.md`](CAPTURE-GUIDE.md) describes the setup.

## Overview

| | Showcase | Shows |
|---|---|---|
| 01 | [One post, six formats](#01--one-post-six-formats) | Heading, prose, change list, diff, callout, timestamp and subtext in a single message |
| 02 | [Screen priority](#02--screen-priority) | The same data as a desktop table and as mobile records |
| 03 | [Color that survives copy/paste](#03--color-that-survives-copypaste) | `diff` with real red and green, readable without color too |
| 04 | [Before and after](#04--before-and-after) | Change card for the one change that matters |
| 05 | [Timestamps that adapt](#05--timestamps-that-adapt-to-the-reader) | One text, every reader sees their own time zone *and language* |
| 06 | [Release notes](#06--release-notes) | Changelog categories, only the ones with content |
| 07 | [Live status](#07--live-status) | Status list with color *and* text |
| 08 | [Long changes](#08--long-changes) | Hanging change plus a warning callout |

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

![Hybrid post rendered in Discord](images/01-hybrid-post.png)

Each format was chosen because of what the information *is* — not because it
looked good.

---

## 02 — Screen priority

Composing happens at a keyboard, so the default keeps structure: a four-column
table stays a table (`SKILL.md` § 2).

`````text
```
SERVICE     STATUS     VERSION   LATENCY
API         Online     3.2.1     42 ms
Database    Online     14.6      18 ms
Cache       Degraded   7.0.5     91 ms
```
`````

![Four-column table on desktop](images/02a-table-desktop.png)

Because that is wide enough to break on a phone, the reply ends with one short
line — and only then. A compact list or a callout reflows fine and gets no such
offer:

```text
This uses a wide table. Want a mobile-friendly version?
```

Say yes, and the same data comes back as stacked records under the `mobile-first`
budget. The content is identical; only the layout changed:

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

Cache
Status:   Degraded
Version:  7.0.5
Latency:  91 ms
```
`````

![The same data as stacked records on a phone](images/02b-records-mobile.png)

The pair is the showcase — the first shot is a desktop client, the second a
phone. The structure is offered, never taken away unasked.

---

## 03 — Color that survives copy/paste

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

![diff block rendering red and green](images/03-diff.png)

ANSI color blocks exist but did **not** render on the tested mobile client. The
skill treats them as desktop-only and never picks them automatically.

---

## 04 — Before and after

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

![Change card with before and after](images/04-change-card.png)

`Before` and `After` stay spelled out. The color is an accent, never the message.

---

## 05 — Timestamps that adapt to the reader

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

![Metadata block with rendered timestamps](images/05-timestamps.png)

That timestamp is 2026-08-20 16:00 UTC, and the capture was taken from a Berlin
client — which is why it reads 18:00. A New York reader sees 12:00 from the same
message text.

Note the countdown: **"in 5 Tagen"**. Discord localizes the *language* too, not
just the offset, and the surrounding message stays English. One more reason to
write times as `<t:...>` rather than spelling them out.

---

## 06 — Release notes

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

![Changelog categories rendered in Discord](images/06-changelog.png)

Only categories with actual content are emitted. The category emoji are chosen
for contrast in the dark theme (`SKILL.md` § 5.3): this capture is the check, and
`✨` and `🗑️` hold up where `➕` and `➖` washed out.

---

## 07 — Live status

````text
## Service status

🟢 **API** — Online · `42 ms`
🟢 **Database** — Online · `18 ms`
🟡 **Cache** — Degraded · `91 ms`
🔴 **Backup** — Failed
````

![Status list with colored dots and text](images/07-status-list.png)

The state is always written as text too. The emoji never carries the meaning on
its own.

---

## 08 — Long changes

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

![Hanging change and warning callout on a phone](images/08-hanging-callout.png)

---

## Adding a showcase

### Capturing

| | |
|---|---|
| Theme | Dark — it is where the contrast rules were tested |
| Device | Desktop for default output; a real phone for anything showing the `mobile-first` layout. Showcases 02 and 03 need both, at a consistent width. |
| Crop | One message per shot — no channel list, no message bar, no usernames |
| Format | PNG into `images/`, named as listed in [`CAPTURE-GUIDE.md`](CAPTURE-GUIDE.md) |

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
