---
name: inviteup-quiz-posts
description: Create and edit InviteUp quiz posts, Telegram quizzes with one correct answer and an optional explanation, for a Telegram channel. Use when the user wants a quiz, a trivia or knowledge question, a test for their audience, or to change an existing quiz.
---

# InviteUp: quiz posts

A quiz is a Telegram poll with **one correct answer**. After someone answers, Telegram shows whether they were right, and the explanation if there is one. For a question without a right answer, use the `inviteup-poll-posts` skill instead.

## Tools

| Tool | Use it to |
|---|---|
| `create_post_quiz` | Create a quiz. |
| `edit_post_quiz` | Change a quiz that is not published yet. |
| `ai_assistant_quiz_post` | Get an AI draft first; see the `inviteup-ai-assistant-posts` skill. |

## Creating a quiz

`create_post_quiz` takes:

| Field | Required | Notes |
|---|---|---|
| `channelId` | Yes | From `get_channels`. |
| `question` | Yes | Up to 300 characters. Plain text. |
| `options` | Yes | 2 to 10 items, each `{ "text": "..." }`, up to 100 characters. |
| `correctOptionIndex` | Yes | **Zero-based** position of the right answer in `options`. |
| `explanation` | No | Shown after answering. Up to 200 characters. |
| `publishAt` | No | ISO timestamp with offset. **Defaults to now**: the quiz goes out right away. |

The quiz is created as **scheduled**. To keep it waiting for approval, see the `inviteup-plan` skill.

Example: the right answer is "Mercury", the first option, so `correctOptionIndex` is `0`.

```json
{
  "channelId": 123,
  "question": "Which planet is closest to the Sun?",
  "options": [{ "text": "Mercury" }, { "text": "Venus" }, { "text": "Mars" }],
  "correctOptionIndex": 0,
  "explanation": "Mercury orbits about 58 million km from the Sun.",
  "publishAt": "2026-10-20T10:00:00+03:00"
}
```

## The correct answer

- `correctOptionIndex` must point to one of the options, or the tool returns an error.
- Whenever you add, remove or reorder options, recompute it.
- Before saving, read the correct option back to the user by its text, not its number.
- Don't always put the right answer first. Vary its position.

## Editing a quiz

`edit_post_quiz` replaces the quiz. Pass `postId`, `question`, the full `options` list, `correctOptionIndex` and `publishAt` (required), plus `explanation` if it should stay. Read the current values from `get_channel_posts` first.

- It works only on quizzes. For a regular poll, use `edit_post_poll`.
- A quiz can't be turned into a poll. Remove it and create a poll instead, after confirming with the user.
- Edit only quizzes that are not published yet. Changing a published quiz does not update it in Telegram.

## Writing good quizzes

- Ask one clear question with exactly one defensible right answer.
- Make the wrong options plausible, similar in length and style to the right one.
- Use the explanation to teach something: why the answer is right, or a short fact.

## Rules

- Confirm the publish time with the user before saving. Without one, the quiz is published immediately.
- Show the question, options and correct answer to the user before you create or edit the quiz.
