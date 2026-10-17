---
name: inviteup-getting-started
description: Explains what InviteUp is, how to connect to it, and which kind of post and skill fits a request. Use when the user mentions InviteUp, wants to manage their Telegram channel through InviteUp, asks which channels they have, wants to add a channel, wants to post something but the format is not decided, or wants to delete a post.
---

# InviteUp: getting started

InviteUp is a Telegram Mini App for growing and running Telegram channels: scheduling posts, polls and quizzes, AI-generated content, recurring posts, referral campaigns and contests.

The agent works with InviteUp through the `inviteup` MCP server that ships with this plugin. Parameter notes for all tools are in [references/tools.md](references/tools.md).

## Connecting

- The client signs in through OAuth, using Telegram login. No API key goes in the plugin.
- If a tool call fails with an authorization error, ask the user to finish the sign-in in their client.

## Core concepts

- **Channel**: a Telegram channel the user owns. The InviteUp bot must be an admin in it.
- **Post**: a text post, poll or quiz in a channel. A post is either *scheduled* (it will be published) or *suggested* (it waits for the user's approval in the Plan).
- **Plan**: the list of post slots for a channel, from `get_channel_posts`.
- **Credits**: AI features (drafts, images, recurring posts) cost credits.
- **Channel settings**: content description, post tone and timezone. They shape the AI-generated posts.

## First steps

1. Call `get_channels` to list the user's channels and get their IDs.
2. If the channel is missing, make sure the InviteUp bot is an admin there, then call `add_channel` with `@username`, a `t.me` link or the numeric chat ID.
3. Call `get_channel_settings` to read the channel's tone, description and timezone before creating content.
4. Call `get_channel_posts` to see the current Plan, so a new post doesn't clash with a planned one.

## Choosing the kind of post

| The user wants to… | Kind | Skill |
|---|---|---|
| Share news, an announcement, tips, a link | Text post | `inviteup-text-posts` |
| Ask the audience's opinion, run a vote or survey | Poll | `inviteup-poll-posts` |
| Test knowledge with a question that has one right answer | Quiz | `inviteup-quiz-posts` |
| Have the AI write any of these | AI draft | `inviteup-ai-assistant-posts` |
| Post on a fixed weekly routine | Recurring post | `inviteup-recurring-posts` |
| See, approve or unapprove what is planned | Plan | `inviteup-plan` |

If it is unclear, ask the user. A poll and a quiz can't be converted into each other later.

## Publish times

- `publishAt` is an ISO timestamp. Include the offset, and convert from the channel's timezone.
- All `create_post_*` tools create **scheduled** posts. Without `publishAt` the post is published right away.
- To keep a post waiting for approval, see the `inviteup-plan` skill.

## Removing a post

`remove_post` deletes a post (`postId`) or a whole campaign (`campaignUuid`, when no post exists yet).

- Confirm with the user first; it cannot be undone.
- To stop a post from publishing but keep it, unapprove it instead (see `inviteup-plan`).

## Rules

- Tell the user before calling a tool that costs credits (`ai_assistant_*`, `generate_post_image`, recurring posts).
- Show the final content and publish time to the user before saving.
