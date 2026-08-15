# Screenshot sources

Paste-ready source for the screenshots used in `README.md`. The numbering matches
the "See it in action" sections one to one.

Each block below is one Discord message. Post it into a quiet channel (a private
server or a DM with yourself works best — no avatars or unrelated messages in
frame), then capture it.

Blocks are fenced with **more** backticks than the message needs, because several
messages contain code blocks of their own. That outer fence is the transport
container from `SKILL.md` § 3 — **do not paste it**. Copy only what is inside,
including any inner ``` lines, which are part of the Discord message.

## Capture settings

| | |
|---|---|
| Theme | Dark (matches the header image, and it is where the contrast rules were tested) |
| Device | A real phone, default zoom, portrait |
| Crop | One message per shot — no channel list, no message bar, no usernames |
| Format | PNG |

Screenshots go into `assets/screenshots/` with exactly these names:

| File | README section | Note |
|---|---|---|
| `01-hybrid-post.png` | 1. One post, six formats | The flagship shot. Worth retaking until it is clean. |
| `02a-table-wide.png` | 2. The table trap | The wide table |
| `02b-table-record.png` | 2. The table trap | The record layout |
| `03-diff.png` | 3. Color that survives copy/paste | Red/green must be visible |
| `04-change-card.png` | 4. Before and after | |
| `05-timestamps.png` | 5. Timestamps that adapt | The countdown must still point into the future |
| `06-changelog.png` | 6. Release notes | |
| `07-status-list.png` | 7. Live status | |
| `08-hanging-callout.png` | 8. Long changes | |

The two `02*` shots are the important pair: they must come from the **same device
at the same width**, otherwise the comparison proves nothing.

---

## 01 — Hybrid post

The most valuable single screenshot in the README. It has to show all six formats
without scrolling if possible.

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

## 02a — The wide table (what breaks)

Post as its own message. The point of the screenshot is the wrapping.

`````text
```
SERVICE     STATUS   VERSION   LATENCY
API         Online   3.2.1     42 ms
Database    Online   14.6      18 ms
```
`````

## 02b — The record layout (what works)

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

## 03 — diff

Check that red and green actually render before capturing.

`````text
```diff
- Manual retry after failure
- Status page updated by hand

+ Automatic retry with backoff
+ Live status from health checks
```
`````

## 04 — Change card

````text
## ⭐ Key change

**Access control**

🔴 **Before**
`Individual user permissions`

🟢 **After**
`Role-based permissions`
````

## 05 — Timestamps

The timestamp resolves to 2026-08-20 16:00 UTC. Once that date has passed the
countdown renders in the past and the screenshot loses its point — substitute a
future Unix timestamp of your own and use the same value in the README example.

````text
# 📅 Maintenance window

**Environment:** `Production`
**Starts:** <t:1787241600:f>
**Countdown:** <t:1787241600:R>
**Duration:** ~2 h

> ℹ️ **Information**
> Read access stays available throughout.
````

## 06 — Changelog categories

The category emoji are chosen for contrast in the dark theme, see `SKILL.md`
§ 5.3. If a shot shows `✨` or `🗑️` washing out, that is a finding worth
reporting — the whole point of these defaults is that they stay legible.

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

## 07 — Status list

````text
## Service status

🟢 **API** — Online · `42 ms`
🟢 **Database** — Online · `18 ms`
🟡 **Cache** — Degraded · `91 ms`
🔴 **Backup** — Failed
````

## 08 — Hanging change + callout

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

## After capturing

Drop the PNGs into `assets/screenshots/` and the README examples can show real
rendered output next to the raw text. Keep the raw-text blocks either way — they
are what a reader copies, and they stay readable when images fail to load.

Any screenshot that contradicts `SKILL.md` is a finding, not a bad screenshot.
The specification is built from rendering tests; if the render disagrees, the
specification is what changes.
