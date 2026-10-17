# InviteUp agent plugin

An [Agent Plugin](https://agent-plugins.org/) (spec v1.0.0) that teaches AI agents to work with InviteUp and connects them to the InviteUp MCP server.

## Layout

```text
inviteup-agent-plugin/
├── plugin.json                       # manifest
├── LICENSE                           # MIT
├── mcp.json                          # InviteUp MCP server (streamable HTTP)
└── skills/
    ├── inviteup-getting-started/     # what InviteUp is, connecting, choosing a post type
    │   └── references/tools.md
    ├── inviteup-plan/                # week-ahead Plan, scheduled vs suggested
    ├── inviteup-text-posts/          # text, HTML formatting, image, buttons
    ├── inviteup-poll-posts/          # polls without a correct answer
    ├── inviteup-quiz-posts/          # quizzes with one correct answer
    ├── inviteup-ai-assistant-posts/  # AI drafts: brief, review, save
    └── inviteup-recurring-posts/     # weekly AI-generated posts
```

## MCP server

`https://api2.inviteup.me/mcp`. The client handles authentication (OAuth with Telegram login); the plugin holds no credentials.

## Keeping it in sync

When an InviteUp MCP tool is added, renamed or removed, update the skills and `references/tools.md`.
