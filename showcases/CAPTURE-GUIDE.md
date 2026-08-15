# Capture guide

Everything needed to produce the nine screenshots for the gallery.

The messages are **files**, not code blocks on this page: open the file, select
all, copy, paste into Discord. Nothing to trim, nothing to accidentally include.

That matters more than it sounds. Several of these messages contain ``` code
blocks of their own, so a copy that picks up one stray backtick too many turns
the whole post into a code block instead of a rendered message. Files avoid the
problem entirely — which is the same reason `SKILL.md` § 3 wraps finished output
in four backticks when it comes out of a chat interface.

## Before you start

1. Open a **quiet channel**: a private server or a DM with yourself. No other
   messages, no avatars in frame.
2. Set Discord to the **dark theme**. The contrast rules were tested there, and
   the header image matches it.
3. Post the nine messages from [`messages/`](messages/) in order, each as its own
   message. Open file → select all → copy → paste → send.

Then capture: pass 1 on desktop, pass 2 on the phone. You post once and
photograph twice — there is no need to post anything a second time.

## The nine captures

| # | Paste this file | Save the screenshot as | Device |
|---|---|---|---|
| 1 | [`messages/01-hybrid-post.txt`](messages/01-hybrid-post.txt) | `01-hybrid-post.png` | 🖥️ desktop |
| 2 | [`messages/02a-table-desktop.txt`](messages/02a-table-desktop.txt) | `02a-table-desktop.png` | 🖥️ desktop |
| 3 | [`messages/02b-records-mobile.txt`](messages/02b-records-mobile.txt) | `02b-records-mobile.png` | 📱 **phone** |
| 4 | [`messages/03-diff.txt`](messages/03-diff.txt) | `03-diff.png` | 🖥️ desktop |
| 5 | [`messages/04-change-card.txt`](messages/04-change-card.txt) | `04-change-card.png` | 🖥️ desktop |
| 6 | [`messages/05-timestamps.txt`](messages/05-timestamps.txt) | `05-timestamps.png` | 🖥️ desktop |
| 7 | [`messages/06-changelog.txt`](messages/06-changelog.txt) | `06-changelog.png` | 🖥️ desktop |
| 8 | [`messages/07-status-list.txt`](messages/07-status-list.txt) | `07-status-list.png` | 🖥️ desktop |
| 9 | [`messages/08-hanging-callout.txt`](messages/08-hanging-callout.txt) | `08-hanging-callout.png` | 📱 phone |

Only **one** shot truly needs a phone: `02b`, where the narrow layout is the
entire point and, next to `02a`, forms the before/after of the layout mode. `08`
is on the phone because a hanging change is what narrow screens are for — a
desktop shot works too if that is easier.

Desktop is the default everywhere else because that is what the `desktop-first`
default produces (`SKILL.md` § 2).

**Crop** to the message body: no channel list, no sidebar, no message input bar,
no username or avatar. Just the message.

## Two things to watch while capturing

**Message 6, timestamps.** The value resolves to 2026-08-20 16:00 UTC. If that
date has passed, the countdown renders in the past and the screenshot loses its
point — say so and I will swap a future timestamp into the gallery, the README
and the message file before you capture.

**Message 7, changelog.** Look closely at `✨` and `🗑️`. They are the defaults
because `➕` and `➖` washed out in the dark theme (`SKILL.md` § 5.3). If the new
ones look pale too, that is a finding worth reporting rather than working around.

## If a message renders wrong

Check first whether the paste picked up something it should not have — a leading
```` ```text ```` line means extra characters came along. The files contain
exactly the message and nothing else, so a clean select-all cannot go wrong.

If the render is genuinely off, that is a finding, not a bad screenshot. The
specification is built from rendering tests; if the render disagrees with
`SKILL.md`, the specification is what changes.

## Handing them over

Drop the PNGs into [`images/`](images/) using exactly the filenames in the table.
I will place them in the gallery, add thumbnails to the overview and update the
status.
