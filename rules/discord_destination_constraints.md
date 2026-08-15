# Discord Destination Constraints

Version: 1.0  
Status: Skill rule set  
Purpose: Stable formatting and validation rules for generating Discord-ready content across text channels, forum posts, media channels, threads, announcement channels, embeds, and voice messages.

---

## 1. Scope

This rule set defines the output constraints a Discord-writing skill MUST apply before generating content for Discord.

The rules distinguish between:

1. **Discord API / Bot / Webhook hard limits**
2. **Discord client limits for human users**
3. **Server-configurable requirements**
4. **Recommended safe defaults**
5. **Fallback and splitting behavior**

The skill MUST NOT assume that all Discord destinations share the same limits.

---

## 2. Normative language

The keywords below are binding:

- **MUST**: mandatory rule
- **MUST NOT**: prohibited behavior
- **SHOULD**: preferred behavior unless a valid reason exists
- **SHOULD NOT**: discouraged behavior
- **MAY**: optional behavior

When a user requirement conflicts with a Discord hard limit, the Discord hard limit MUST take precedence.

---

## 3. Output context

Before generating Discord content, the skill SHOULD resolve the following context:

```yaml
discord_context:
  destination_type: text | forum | media | thread | announcement | embed | voice_message
  delivery_mode: manual | bot | webhook
  user_has_nitro: true | false | unknown
  require_tag: true | false | unknown
  use_embeds: true | false
  attachments_expected: true | false
```

If a value is unknown, the skill MUST use the safest compatible default.

Safe defaults:

```yaml
destination_type: text
delivery_mode: bot
user_has_nitro: false
require_tag: false
use_embeds: false
attachments_expected: false
```

---

# 4. Global message rules

## 4.1 Standard message content

For Bot/API/Webhook output:

```yaml
message:
  content_max_chars: 2000
```

The skill MUST treat **2,000 characters as the universal safe limit for normal Discord message content**.

If the generated content exceeds 2,000 characters, the skill MUST split it into multiple messages unless another compatible Discord structure is explicitly selected.

---

## 4.2 Manual Discord client

For manually posted messages:

```yaml
manual_message:
  standard_user_max_chars: 2000
  nitro_user_max_chars: 4000
```

The skill MAY use up to 4,000 characters only when all of the following are true:

1. `delivery_mode = manual`
2. `user_has_nitro = true`
3. the user explicitly wants Nitro-length output or longer individual messages

Otherwise, the skill MUST use the 2,000-character safe limit.

---

## 4.3 Character counting

Character limits MUST be calculated against the final rendered message text.

The count MUST include:

- headings
- Markdown syntax
- emoji characters
- URLs
- whitespace
- line breaks
- code fences
- list markers
- table syntax

The skill MUST validate message size after final formatting.

---

# 5. Text channels

## 5.1 Channel metadata

```yaml
text_channel:
  name_min_chars: 1
  name_max_chars: 100
  topic_max_chars: 1024
```

## 5.2 Messages

```yaml
text_channel_message:
  safe_content_max_chars: 2000
  media_required: false
```

Rules:

- A text message MUST NOT exceed 2,000 characters when generated for Bot/API/Webhook use.
- An image or other attachment MUST NOT be treated as mandatory.
- Long content MUST be split into multiple messages.

---

# 6. Forum channels

A Discord forum post is structurally a thread created inside a forum channel.

## 6.1 Forum channel metadata

```yaml
forum_channel:
  name_min_chars: 1
  name_max_chars: 100
  topic_max_chars: 4096
  available_tags_max: 20
  tag_name_max_chars: 20
  applied_tags_per_post_max: 5
```

---

## 6.2 Forum post title

```yaml
forum_post:
  title_required: true
  title_min_chars: 1
  title_max_chars: 100
```

Rules:

- Every generated forum post MUST have a title.
- The title MUST contain between 1 and 100 characters.
- The title SHOULD describe the subject directly.
- Decorative prefixes SHOULD NOT consume excessive title space.

---

## 6.3 Forum post body

```yaml
forum_post:
  initial_content_max_chars: 2000
  media_required: false
```

Rules:

- The initial forum message MUST remain within the normal message content limit.
- A forum post MUST NOT be assumed to require an image.
- Text-only forum posts are valid.
- If the content exceeds 2,000 characters, the skill SHOULD:
  1. place the overview in the initial post;
  2. continue the remaining content as replies inside the forum thread.

---

## 6.4 Forum tags

```yaml
forum_tags:
  available_tags_max: 20
  tag_name_max_chars: 20
  applied_tags_per_post_max: 5
```

A server MAY enforce that at least one tag is selected.

When:

```yaml
require_tag: true
```

the skill MUST ensure at least one valid tag is assigned or explicitly flag that a required tag still needs to be selected.

The skill MUST NOT invent server-specific tags unless tag options were provided.

---

# 7. Media channels

Discord Media Channels are media-oriented forum-style channels.

They MUST NOT be treated as technically identical to a mandatory-image input field.

## 7.1 Media post

```yaml
media_post:
  title_required: true
  title_min_chars: 1
  title_max_chars: 100
  content_max_chars: 2000
  media_required: false
  available_tags_max: 20
  tag_name_max_chars: 20
  applied_tags_per_post_max: 5
```

Rules:

- A Media post title MUST contain between 1 and 100 characters.
- Normal text content MUST remain within 2,000 characters for Bot/API use.
- The skill MUST NOT globally enforce `image_required = true`.
- Media MAY be recommended where it improves the post.
- Server-specific UI or future Discord changes MAY introduce additional behavior; therefore media requirements SHOULD remain configurable.

---

# 8. Threads

## 8.1 Thread metadata

```yaml
thread:
  title_min_chars: 1
  title_max_chars: 100
```

## 8.2 Thread messages

```yaml
thread_message:
  safe_content_max_chars: 2000
  media_required: false
```

Rules:

- Thread messages use normal Discord message limits.
- Long structured content SHOULD be distributed across multiple thread replies rather than compressed into unreadable blocks.
- The skill SHOULD preserve section boundaries when splitting.

---

# 9. Announcement channels

```yaml
announcement_channel:
  safe_content_max_chars: 2000
  media_required: false
  crosspost_rate_limit_per_hour: 10
```

Rules:

- Announcement content MUST follow standard message-length limits.
- Attachments are optional.
- The skill SHOULD NOT assume that every announcement will be crossposted.
- If automatic crossposting is part of a workflow, the implementation MUST respect Discord's crosspost rate limitations.

---

# 10. Embeds

Embeds use a separate set of limits from normal message content.

```yaml
embed:
  embeds_per_message_max: 10
  total_embed_chars_per_message_max: 6000
  title_max_chars: 256
  description_max_chars: 4096
  fields_max: 25
  field_name_max_chars: 256
  field_value_max_chars: 1024
  footer_max_chars: 2048
  author_name_max_chars: 256
```

---

## 10.1 Total embed character budget

The 6,000-character maximum applies to the **combined textual content of all embeds in one message**.

The skill MUST NOT interpret the limit as 6,000 characters per individual embed.

Before output, the skill MUST validate:

```text
sum(
  embed.title
  + embed.description
  + all field names
  + all field values
  + embed.footer
  + embed.author_name
) <= 6000
```

across all embeds contained in the same Discord message.

---

## 10.2 Embed fallback

If the embed structure exceeds any limit, the skill MUST apply this fallback order:

1. shorten decorative or redundant text;
2. split large sections into multiple fields;
3. split the content across multiple embeds;
4. split the embeds across multiple Discord messages.

The skill MUST NOT silently truncate substantive information unless the user requested shortening.

---

# 11. Voice messages

Voice messages are a special case.

```yaml
voice_message:
  audio_required: true
  audio_attachment_count: 1
  normal_text_allowed: false
```

Rules:

- A voice message MUST contain the required audio attachment.
- Voice-message output MUST NOT be treated as a standard text message with optional audio.
- The skill SHOULD only select this destination when voice-message output was explicitly requested.

---

# 12. Attachments and file size

Discord upload limits may vary by account, subscription, experiment, client, and Discord product changes.

Therefore:

```yaml
attachments:
  hardcode_end_user_upload_limit: false
```

The skill MUST NOT use one permanent account-level upload size as a universal Discord rule.

If file-size validation is required, the implementation SHOULD:

1. obtain the current limit from the active Discord environment where possible;
2. otherwise warn that upload size is environment-dependent;
3. avoid claiming one account upload limit as universally valid.

For Bot/API integrations, transport/request limits SHOULD be validated against the current Discord Developer Documentation by the implementing application.

---

# 13. Splitting algorithm

When content exceeds the destination limit, the skill MUST split content deliberately.

## 13.1 Split priority

Preferred split boundaries, highest to lowest priority:

1. major section boundary
2. subsection boundary
3. paragraph boundary
4. list-item boundary
5. sentence boundary
6. hard character boundary only as a last resort

---

## 13.2 Structures that SHOULD remain intact

The skill SHOULD NOT split in the middle of:

- Markdown links
- code fences
- inline code
- tables
- headings
- numbered list items
- bullet items
- quoted blocks
- spoiler syntax
- custom emoji syntax
- URLs

If a code block must continue in another message, the skill SHOULD close the code fence in the first message and reopen it in the next.

---

## 13.3 Continuation labels

Continuation labels MAY be used when they improve orientation.

Examples:

```text
Part 1/3
Part 2/3
Part 3/3
```

or:

```text
Overview
Details
Additional Notes
```

Continuation markers count toward the character limit and MUST be included during validation.

---

# 14. Readability rules

A technically valid Discord message can still be poorly formatted.

The skill SHOULD optimize for scanability.

Recommended behavior:

- use concise headings;
- keep paragraphs short;
- prefer bullets for discrete attributes;
- avoid unnecessary horizontal decoration;
- avoid excessive emoji;
- avoid deeply nested lists;
- keep related information together;
- split large content before it becomes visually dense.

The skill SHOULD prefer multiple readable Discord messages over one message that technically fits but is difficult to scan.

---

# 15. Tables

Markdown tables are not always an ideal Discord presentation format.

The skill SHOULD use tables only when:

- the information is genuinely comparative;
- the number of columns is small;
- values remain readable on narrow screens;
- the table stays within the message limit.

For mobile-heavy audiences, the skill SHOULD prefer labeled blocks or bullet groups over wide Markdown tables.

Example preferred structure:

```text
**Item A**
Type: ...
Value: ...
Status: ...

**Item B**
Type: ...
Value: ...
Status: ...
```

instead of a wide multi-column table.

---

# 16. Formatting selection

The skill SHOULD select output format based on information structure rather than subject matter.

Suggested mapping:

```yaml
presentation_strategy:
  short_update: compact_message
  simple_list: bullets
  ranked_list: numbered_list
  structured_entity: labeled_block
  multiple_entities: repeated_blocks
  direct_comparison: compact_table
  long_reference_content: multi_message_sections
  forum_guide: forum_initial_post_plus_replies
  dense_metadata: embed_fields
```

The skill MUST remain domain-agnostic.

It MUST NOT contain game-specific, product-specific, community-specific, or industry-specific formatting rules unless they are supplied separately by the user.

---

# 17. Destination-aware rendering

The skill SHOULD make rendering decisions in this order:

```text
1. Identify destination type.
2. Identify delivery mode.
3. Apply destination hard limits.
4. Apply server-specific requirements if known.
5. Choose the most readable representation.
6. Generate content.
7. Count final characters.
8. Validate titles, fields, tags, and attachment requirements.
9. Split content if necessary.
10. Return Discord-ready output.
```

Validation MUST occur after formatting, not only before formatting.

---

# 18. Recommended internal constraint model

```yaml
discord_constraints:

  global:
    safe_message_chars: 2000
    manual_nitro_message_chars: 4000

  text:
    channel_name:
      min: 1
      max: 100
    topic:
      max: 1024
    message:
      max: 2000
    media_required: false

  forum:
    channel_name:
      min: 1
      max: 100
    topic:
      max: 4096
    post_title:
      required: true
      min: 1
      max: 100
    initial_message:
      max: 2000
    tags:
      available_max: 20
      name_max: 20
      applied_max: 5
      required: configurable
    media_required: false

  media:
    post_title:
      required: true
      min: 1
      max: 100
    initial_message:
      max: 2000
    tags:
      available_max: 20
      name_max: 20
      applied_max: 5
      required: configurable
    media_required: false

  thread:
    title:
      min: 1
      max: 100
    message:
      max: 2000
    media_required: false

  announcement:
    message:
      max: 2000
    media_required: false
    crossposts_per_hour_max: 10

  embed:
    count_per_message_max: 10
    total_chars_per_message_max: 6000
    title_max: 256
    description_max: 4096
    fields_max: 25
    field_name_max: 256
    field_value_max: 1024
    footer_max: 2048
    author_name_max: 256

  voice_message:
    audio_required: true
    audio_attachment_count: 1
    normal_text_allowed: false

  attachments:
    universal_account_upload_limit: null
    runtime_validation_recommended: true
```

---

# 19. Validation result

The skill SHOULD be able to evaluate an output against the constraints before returning it.

Suggested internal result:

```yaml
validation:
  valid: true | false
  destination: forum
  violations: []
  warnings: []
  generated_parts: 1
```

Example failure:

```yaml
validation:
  valid: false
  destination: forum
  violations:
    - "Forum title exceeds 100 characters."
    - "Initial message exceeds 2000 characters."
  warnings:
    - "Server may require at least one forum tag."
  generated_parts: 3
```

---

# 20. Automatic repair behavior

When output violates a known limit, the skill SHOULD repair it automatically whenever repair does not change the user's intended meaning.

Automatic repair is allowed for:

- message splitting;
- title shortening;
- section redistribution;
- embed field redistribution;
- changing a wide table to stacked blocks;
- moving secondary content into follow-up messages.

Automatic repair MUST NOT:

- delete substantive user-requested information;
- invent missing tags;
- invent attachments;
- remove required distinctions;
- alter factual content to save space.

---

# 21. Unknown or changing Discord behavior

Discord can change limits, subscriptions, experiments, and client behavior.

Therefore the skill MUST classify constraints as one of:

```yaml
constraint_stability:
  hard_api_limit
  client_limit
  configurable_server_rule
  environment_dependent
  recommendation
```

The skill MUST NOT represent environment-dependent or configurable behavior as a universal hard limit.

---

# 22. Core invariant

The skill MUST always follow this invariant:

> Generate for the actual Discord destination, validate against the destination's technical constraints, and optimize the final result for readability without changing the user's intended content.

When in doubt:

```yaml
fallback:
  message_max_chars: 2000
  media_required: false
  use_multiple_messages: true
  preserve_information: true
```
