---
name: inviteup-text-posts
description: Create and edit InviteUp text posts for a Telegram channel, with formatting, an image and inline link buttons. Use when the user wants to write, post, announce, change or add an image or button to a regular text post in their channel.
---

# InviteUp: text posts

A text post is a regular Telegram message: formatted text, an optional image, and optional inline buttons that open links.

## Tools

| Tool | Use it to |
|---|---|
| `create_post_text` | Create a post. |
| `edit_post_text` | Change a post that is not published yet. |
| `generate_post_image` | Add an AI image to a saved post (costs credits). |
| `ai_assistant_text_post` | Get an AI draft first; see the `inviteup-ai-assistant-posts` skill. |

## Creating a post

`create_post_text` takes:

| Field | Required | Notes |
|---|---|---|
| `channelId` | Yes | From `get_channels`. |
| `text` | Yes | Telegram HTML; see Formatting. |
| `publishAt` | No | ISO timestamp with offset. **Defaults to now**: the post goes out right away. |
| `imgUrl` | No | A public image URL. |
| `buttons` | No | List of `{ "label": "...", "url": "https://..." }`. |

It returns the new post's `id`. The post is created as **scheduled**. To keep it waiting for approval, see the `inviteup-plan` skill.

## Formatting

Text is sent as Telegram HTML. Only these tags are kept; anything else is removed:

`<b>`, `<strong>`, `<i>`, `<em>`, `<u>`, `<ins>`, `<s>`, `<strike>`, `<a href="...">`, `<code>`, `<pre>`, `<blockquote>`

- Use plain line breaks for new lines, not `<p>` or `<br>`.
- Don't use Markdown (`**bold**`); it is shown as typed.

## Length

Length is counted on the text as shown, without tags:

- Without an image: up to **4096** characters.
- With an image: up to **1024** characters (Telegram's caption limit).

If a long post needs an image, shorten the text or post it without the image.

## Editing a post

`edit_post_text` **replaces** the post's content. Pass the full new state, not just what changes:

- `postId`, `text` and `publishAt` are required. Read the current values from `get_channel_posts` and send them back if they should stay.
- `imgUrl`: **leaving it out removes the image.** Pass the current `post.imgUrl` to keep it.
- `buttons`: leaving it out keeps the current buttons. Pass `[]` to remove them all.

Edit only posts that are not published yet. Changing a published post does not update the message in Telegram.

## AI image

`generate_post_image` writes an image from the post's text and attaches it, replacing any image already there. It works only on saved text posts that are scheduled or waiting for approval. Remember the 1024-character limit once the post has an image.

## Rules

- Confirm the publish time with the user before saving. Without one, the post is published immediately.
- Show the final text to the user before you create or edit the post.
- Say that `generate_post_image` costs credits before calling it.
