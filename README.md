# InviteUp agent plugin

An [Agent Plugin](https://agent-plugins.org/) (spec v1.0.0) that teaches AI agents to work with InviteUp and connects them to the InviteUp MCP server.

## Install in Claude

This repository is also a Claude plugin marketplace, so you add the marketplace first and then install the `inviteup` plugin from it.

**Claude Code (terminal)**

```bash
claude plugin marketplace add benedya/inviteup-agent-plugin
claude plugin install inviteup@inviteup
```

Or inside a session: `/plugin marketplace add benedya/inviteup-agent-plugin`, then `/plugin install inviteup@inviteup`.

**Claude desktop app**

1. Open the plugin directory and choose **Add marketplace**.
2. Enter `benedya/inviteup-agent-plugin` and click **Sync**.
3. Install **inviteup** from the marketplace.

**Connect your InviteUp account**

The first time the InviteUp MCP server is used, Claude asks you to sign in. In Claude Code, run `/mcp`, pick `inviteup` and finish the Telegram login in your browser. Until then the InviteUp tools are unavailable.

**Update**

Auto-update is off by default for this marketplace. To get the latest version:

```bash
claude plugin update inviteup@inviteup
```

Or turn on auto-update in `/plugin` → **Marketplaces** → `inviteup` → **Enable auto-update**.

## Layout

```text
inviteup-agent-plugin/
├── .claude-plugin/
│   ├── marketplace.json              # Claude marketplace listing
│   └── plugin.json                   # Claude manifest
├── .mcp.json                         # Claude MCP server config
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

Claude reads `.claude-plugin/plugin.json` and `.mcp.json`; other agents read `plugin.json` and `mcp.json`. Keep each pair in sync, and bump `version` in both manifests on every release. Claude users stay on the installed version until it changes.
