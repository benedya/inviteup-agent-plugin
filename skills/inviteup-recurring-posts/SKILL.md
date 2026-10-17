---
name: inviteup-recurring-posts
description: Set up and change InviteUp recurring posts, where the AI writes a fresh post every week on a fixed schedule. Use when the user wants regular weekly content, a posting routine, or to change or pause an existing recurring post.
---

# InviteUp: recurring posts

A recurring post is a description of what to post, plus a weekly schedule. Each week the AI generates a new post for every scheduled slot. Each generated post costs credits.

## Tools

- `create_recurring_post`: create one. The schedule is weekly days and times in **UTC**.
- `edit_recurring_post`: change it. Only the fields you pass change. A passed `schedule` replaces the whole weekly schedule.
- `get_channel_posts`: shows generated slots; `recurringPost` holds the recurring post settings.

## Approval mode

- `postWithoutApproval: true`: the recurring post is scheduled. About an hour before each slot the AI writes the post, and it is published.
- `postWithoutApproval: false` (default): the recurring post waits in the Suggested section of the Plan. Nothing is generated or published until it is approved.

Approving it later is the same as `move_post_to_scheduled` with its `campaignUuid`; see the `inviteup-plan` skill.

Ask the user which mode they want. Default to approval if they are unsure.

## Workflow

1. Get the channel ID (`get_channels`) and its timezone (`get_channel_settings`).
2. Agree on the topic, days and local times with the user.
3. Convert local times to UTC before calling `create_recurring_post`.
4. Tell the user the weekly credit cost.
5. Create the recurring post and confirm the first slots with `get_channel_posts`.
