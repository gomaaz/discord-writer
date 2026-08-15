# Capture guide

Everything needed to produce the nine screenshots for the gallery, in the order
you should work through it.

Hand the finished PNGs over with the filenames below — they are wired into
`README.md`, so a matching name is all that is needed to place them.

## Before you start

1. Open a **quiet channel**: a private server or a DM with yourself. No other
   messages, no avatars in frame.
2. Set Discord to the **dark theme**. The contrast rules were tested there, and
   the header image matches it.
3. Post messages **1 to 9 below in order**, each as its own message.

Then capture: pass 1 on desktop, pass 2 on the phone. You post once and
photograph twice — there is no need to post anything a second time.

## What to capture where

| # | File | Device | Content |
|---|---|---|---|
| 1 | `01-hybrid-post.png` | 🖥️ desktop | message 1 |
| 2 | `02a-table-desktop.png` | 🖥️ desktop | message 2 |
| 3 | `02b-records-mobile.png` | 📱 **phone** | message 3 |
| 4 | `03-diff.png` | 🖥️ desktop | message 4 |
| 5 | `04-change-card.png` | 🖥️ desktop | message 5 |
| 6 | `05-timestamps.png` | 🖥️ desktop | message 6 |
| 7 | `06-changelog.png` | 🖥️ desktop | message 7 |
| 8 | `07-status-list.png` | 🖥️ desktop | message 8 |
| 9 | `08-hanging-callout.png` | 📱 phone | message 9 |

Only **one** shot needs a phone for its content: `02b`, where the narrow layout
is the entire point. `08` is on the phone because a hanging change is what a
narrow screen is for — a desktop shot works too if that is easier.

**Crop** to the message body: no channel list, no sidebar, no message input bar,
no username or avatar. Just the message.

## The messages to post

The fences below are transport containers (`SKILL.md` § 3). **Do not paste the
outermost backticks** — copy only what is inside them. Where a message contains
its own ``` lines, those *are* part of the message and must be pasted.

### Message 1 → `01-hybrid-post.png` 🖥️

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

### Message 2 → `02a-table-desktop.png` 🖥️

`````text
```
SERVICE     STATUS     VERSION   LATENCY
API         Online     3.2.1     42 ms
Database    Online     14.6      18 ms
Cache       Degraded   7.0.5     91 ms
```
`````

### Message 3 → `02b-records-mobile.png` 📱

The same data, narrow. Capture this one **on your phone** — side by side with
message 2 it is the before/after of the whole layout-mode idea.

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

### Message 4 → `03-diff.png` 🖥️

Check that red and green actually render before capturing.

`````text
```diff
- Manual retry after failure
- Status page updated by hand

+ Automatic retry with backoff
+ Live status from health checks
```
`````

### Message 5 → `04-change-card.png` 🖥️

````text
## ⭐ Key change

**Access control**

🔴 **Before**
`Individual user permissions`

🟢 **After**
`Role-based permissions`
````

### Message 6 → `05-timestamps.png` 🖥️

The timestamp is 2026-08-20 16:00 UTC. **If that date has passed**, the countdown
renders in the past and the screenshot loses its point — tell me and I will swap
in a future timestamp across the gallery and the README before you capture.

````text
# 📅 Maintenance window

**Environment:** `Production`
**Starts:** <t:1787241600:f>
**Countdown:** <t:1787241600:R>
**Duration:** ~2 h

> ℹ️ **Information**
> Read access stays available throughout.
````

### Message 7 → `06-changelog.png` 🖥️

Watch the category emoji. They were chosen because `➕` and `➖` washed out in the
dark theme (`SKILL.md` § 5.3). If `✨` or `🗑️` look pale here too, that is a
finding — tell me rather than working around it.

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

### Message 8 → `07-status-list.png` 🖥️

````text
## Service status

🟢 **API** — Online · `42 ms`
🟢 **Database** — Online · `18 ms`
🟡 **Cache** — Degraded · `91 ms`
🔴 **Backup** — Failed
````

### Message 9 → `08-hanging-callout.png` 📱

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

## Handing them over

Drop the PNGs into `showcases/images/` using exactly the filenames above. I will
place them in the gallery, add the thumbnails to the overview table and flip the
status markers.

If a capture contradicts what `SKILL.md` claims, that is a finding, not a bad
screenshot. The specification is built from rendering tests; if the render
disagrees, the specification is what changes.
