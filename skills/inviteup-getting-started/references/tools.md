# InviteUp MCP tools: posts

## Channels

- `get_channels`: lists the user's channels with ID, name, description and username.
- `add_channel`: adds a channel. Accepts `@username`, a `t.me` link or a numeric chat ID. The bot must be an admin in the channel.
- `get_channel_settings` / `update_channel_settings`: content description, post tone and scheduling timezone. Update changes only the fields you pass.

## Plan

- `get_channel_posts`: all post slots for a channel, scheduled and suggested, with status, publish date and content. `recurringPost` is set when the slot comes from a recurring post.

## Text posts

- `create_post_text`: new text post with optional image, inline buttons and publish time.
- `edit_post_text`: change text, publish time, image or inline buttons.
- `generate_post_image`: generates an image from the post's text and replaces any existing image. Costs credits.

## Polls and quizzes

- `create_post_poll` / `edit_post_poll`: regular polls (question, options, multiple answers, publish time).
- `create_post_quiz` / `edit_post_quiz`: quizzes (question, options, correct answer, explanation, publish time).

## AI drafts (cost credits, save nothing)

- `ai_assistant_text_post`: text draft using the channel's tone and settings. Save with `create_post_text`.
- `ai_assistant_poll_post`: poll draft. Save with `create_post_poll`.
- `ai_assistant_quiz_post`: quiz draft. Save with `create_post_quiz`.

## Status changes

- `move_post_to_scheduled`: schedule a post (`postId`) or activate a campaign (`campaignUuid`).
- `move_post_to_suggested`: unapprove a post (`postId`) or move a campaign back to suggested (`campaignUuid`).
- `remove_post`: delete a post (`postId`) or a whole campaign (`campaignUuid`, when no post exists yet).
