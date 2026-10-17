---
name: inviteup-plan
description: Explains the InviteUp Plan, the week-ahead view of a channel's upcoming posts, and the difference between scheduled and suggested items, and how to move items between them. Use when the user asks what is planned for their channel, wants to approve, unapprove, schedule, unschedule or skip a post, or asks why something did or did not publish.
---

# InviteUp: the Plan

The Plan is the week ahead for one channel: today and the next six days. `get_channel_posts` returns it as a flat list of **slots**, sorted by day and time.

## Scheduled vs suggested

Every slot has a `status`:

| `status` | Meaning | What happens at the slot time |
|---|---|---|
| `scheduled` | Approved by the user. | It is published automatically. |
| `suggested` | Waiting for the user's approval. | Nothing. It is never published unless it is moved to scheduled. |

Suggested items are proposals. If the user does not approve one before its time, it is skipped and drops off the Plan.

## What a slot can be

Use the slot fields to tell them apart:

| Kind | How to recognise it | ID to use when moving it |
|---|---|---|
| **Post** (text, poll, quiz) | `hasFullPost: true`, `post` is set | `post.id` as `postId` |
| **Recurring post slot** | `recurringPost` is set | `campaignUuid` for the whole recurring post, or `post.id` for one generated post |
| **Contest** | `hasFullPost: false`, `campaignUuid` set; it is a campaign, not a post | `campaignUuid` |
| **Published today** | `post.status: "PUBLISHED"` | None; it is already out |

`post.status` is the post's own state: `SCHEDULED`, `UNAPPROVED` (shown as suggested), `PUBLISHED` or `PUBLISH_ERROR`.

### How recurring slots become posts

About an hour before a **scheduled** recurring slot, the AI writes the post (this costs credits) and it is published at the slot time. A **suggested** recurring post gets no text, costs nothing and is skipped. To see or edit the text before it goes out, write the post now with the `inviteup-text-posts` skill instead.

## Moving between scheduled and suggested

| Goal | Item has a post | Item has no post (recurring post, contest) |
|---|---|---|
| Approve: suggested → scheduled | `move_post_to_scheduled` with `postId` | `move_post_to_scheduled` with `campaignUuid` |
| Unapprove: scheduled → suggested | `move_post_to_suggested` with `postId` | `move_post_to_suggested` with `campaignUuid` |
| Drop it entirely | `remove_post` with `postId` | `remove_post` with `campaignUuid` |

Pass only one of `postId` or `campaignUuid`. If both a post and a `campaignUuid` exist, prefer `postId`: it affects only that post.

## Workflows

### Review the Plan with the user

1. `get_channels` to pick the channel, then `get_channel_posts`.
2. Group the slots by day. Show time, title or text preview, and status, and list suggested items separately.
3. For each suggested item, ask whether to approve, change or drop it.

## Rules

- Confirm with the user before approving anything that costs credits (recurring slots) or publishes soon.
- Confirm before `remove_post`; it cannot be undone.
- `publishDateTime` and `publishAt` are ISO timestamps. Show times to the user in the channel's timezone from `get_channel_settings`.
