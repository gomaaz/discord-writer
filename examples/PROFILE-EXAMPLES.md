# discord-writer profile examples

These profiles are examples, not hard-coded presets. Copy one, rename it, and adjust the values.

## Corporate

```yaml
discord_writer:
  profile: corporate
  target:
    platform: mobile-first
    density: balanced
  language:
    tone: neutral
    verbosity: concise
  headings:
    emoji: minimal
  changes:
    default: compact-list
    important: auto
  callouts:
    enabled: true
  links:
    mode: compact
    previews: suppress
  checklist:
    style: unicode
  splitting:
    mode: auto
```

## Technical

```yaml
discord_writer:
  profile: technical
  target:
    platform: mobile-first
    density: compact
  language:
    tone: technical
    terminology: preserve-original
  headings:
    emoji: minimal
  changes:
    default: compact-list
    added_removed: diff
  values:
    inline_code: always
  links:
    mode: compact
    previews: suppress
  splitting:
    mode: thread-friendly
```

## Community

```yaml
discord_writer:
  profile: community
  target:
    platform: mobile-first
    density: balanced
  language:
    tone: community
  headings:
    emoji: functional
  changes:
    default: auto
    important: card
  changelog:
    enabled: true
  status:
    style: emoji-text
  callouts:
    enabled: true
  links:
    mode: named
    previews: allow
```

## Executive

```yaml
discord_writer:
  profile: executive
  target:
    platform: mobile-first
    density: compact
  language:
    tone: executive
    verbosity: terse
  headings:
    emoji: minimal
    max_depth: 2
  changes:
    default: compact-list
    important: card
    card_max_items: 1
  links:
    mode: compact
    previews: suppress
  splitting:
    mode: thread-friendly
```

---

# Content Profile Example: Component Set

This example adds a reusable, domain-neutral information architecture for content made of multiple related entries or components with properties.

```yaml
discord_writer:
  profile: community-structured

  target:
    platform: mobile-first
    density: balanced

  language:
    tone: community
    terminology: mixed

  headings:
    emoji: functional

  values:
    inline_code: technical-only

  splitting:
    mode: thread-friendly

  content_profiles:
    component-set:
      enabled: true
      density: balanced
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
```

## Plain-language equivalent

```text
Content profile "component-set":
- mobile-first
- balanced and compact
- every entry as its own record
- keep the field names from the supplied content
- short technical values as inline code
- do not force a wide multi-column table
- derive sensible groups from the content only
- omit missing fields, never invent them
- show alternatives only when present or requested
- split very long content thread-friendly
```
