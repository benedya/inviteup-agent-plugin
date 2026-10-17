---
name: inviteup-poll-posts
description: Create and edit InviteUp poll posts, regular Telegram polls without a correct answer, for a Telegram channel. Use when the user wants to ask their channel's audience a question, run a vote or survey, or change an existing poll.
---

# InviteUp: poll posts

A poll is a Telegram poll: a question and answer options. There is no correct answer. For a question with a right answer, use the `inviteup-quiz-posts` skill instead.

## Tools

| Tool | Use it to |
|---|---|
| `create_post_poll` | Create a poll. |
| `edit_post_poll` | Change a poll that is not published yet. |
| `ai_assistant_poll_post` | Get an AI draft first; see the `inviteup-ai-assistant-posts` skill. |

## Creating a poll

`create_post_poll` takes:

| Field | Required | Notes |
|---|---|---|
| `channelId` | Yes | From `get_channels`. |
| `question` | Yes | Up to 300 characters. Plain text. |
| `options` | Yes | 2 to 10 items, each `{ "text": "..." }`, up to 100 characters. |
| `allowsMultipleAnswers` | No | `true` lets people pick more than one option. Default: one answer. |
| `publishAt` | No | ISO timestamp with offset. **Defaults to now**: the poll goes out right away. |

The poll is created as **scheduled**. To keep it waiting for approval, see the `inviteup-plan` skill.

Example:

```json
{
  "channelId": 123,
  "question": "What should we cover next week?",
  "options": [{ "text": "Pricing tips" }, { "text": "Case studies" }, { "text": "Tool reviews" }],
  "allowsMultipleAnswers": false,
  "publishAt": "2026-10-20T10:00:00+03:00"
}
```

## Editing a poll

`edit_post_poll` replaces the poll. Pass `postId`, `question`, the full `options` list and `publishAt` (required), plus `allowsMultipleAnswers` if it should be on. Read the current values from `get_channel_posts` first.

- It works only on polls. For a quiz it returns an error; use `edit_post_quiz`.
- A poll can't be turned into a quiz. Remove it and create a quiz instead, after confirming with the user.
- Edit only polls that are not published yet. Changing a published poll does not update it in Telegram.

## Writing good polls

- Keep the question short and neutral, so it doesn't push one answer.
- Make options distinct and similar in length. Add an "Other" or "Just show results" option when it helps.
- Use `allowsMultipleAnswers` for "which of these…" questions, not for "which one…" questions.

## Rules

- Confirm the publish time with the user before saving. Without one, the poll is published immediately.
- Show the question and options to the user before you create or edit the poll.
