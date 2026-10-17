---
name: inviteup-ai-assistant-posts
description: How to use the InviteUp AI assistant to draft text posts, polls and quizzes for a Telegram channel, then review and save them. Use when the user wants InviteUp to write, suggest or come up with a post, poll or quiz, or asks for content ideas on a topic for their channel.
---

# InviteUp: AI assistant

## The main idea

The AI assistant writes **drafts**. It does not save or publish anything.

1. **Draft**: call an `ai_assistant_*` tool with a short brief. It returns the content.
2. **Review**: show the draft to the user and apply their changes.
3. **Save**: pass the final content to the matching `create_post_*` tool.

Nothing reaches the channel until step 3, so the user always sees and approves the content first.

## Tools

| Tool | Returns | Save with |
|---|---|---|
| `ai_assistant_text_post` | `text` | `create_post_text` |
| `ai_assistant_poll_post` | `question`, `options` | `create_post_poll` |
| `ai_assistant_quiz_post` | `question`, `options`, `correctOptionIndex`, `explanation` | `create_post_quiz` |

All three take the same input:

- `channelId`: the channel to write for (from `get_channels`).
- `description`: the brief, **20 to 1000 characters**.

The output fields have the same names and shapes as the inputs of the matching `create_post_*` tool, so you can pass them on directly.

## Writing a good brief

The brief is what the post should be about. Include what the user told you:

- the topic and the key points or facts to mention;
- who it is for and the angle (news, tips, opinion, announcement);
- anything to avoid or a call to action.

Example: "Announce our Black Friday sale: 30% off all plans from Nov 28 to Dec 1. Short and upbeat, end with a link to the pricing page."

## Credits

Every draft costs credits, and a new draft costs credits again. If the user has too few credits, the tool returns an error. Tell the user and stop; don't retry.

- Say that a draft costs credits before the first call in a conversation.
- For small fixes (a word, a sentence, an option), edit the draft yourself instead of generating a new one.
- Generate a new draft only when the user wants a different direction.

## Before saving

Check the draft against the limits of the save tool:

- Text: at most 4096 characters, or 1024 if the post has an image.
- Poll or quiz question: at most 300 characters.
- Options: 2 to 10, each at most 100 characters.
- Quiz explanation: at most 200 characters.
- Quiz `correctOptionIndex` is zero-based. Update it if you reorder or remove options.

Saving creates a **scheduled** post. Pass a future `publishAt` unless the user wants it published now.

## Workflow

1. `get_channels` to get the `channelId`.
2. Agree on the topic and the format (text, poll or quiz) with the user.
3. Call the matching `ai_assistant_*` tool.
4. Show the draft in full and ask for changes.
5. Agree on the publish time, then save with `create_post_*`.
6. Optional, for text posts: `generate_post_image` adds an AI image (costs credits).
