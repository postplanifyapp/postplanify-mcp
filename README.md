<p align="center">
  <img src="logo.png" alt="PostPlanify" width="96" height="96">
</p>

<h1 align="center">PostPlanify MCP Server</h1>

<p align="center">
  Schedule social media posts, plan your content calendar and track analytics from Claude, ChatGPT, Cursor, Cline or any MCP client.
</p>

<p align="center">
  <a href="https://postplanify.com">Website</a> ·
  <a href="https://postplanify.com/mcp/docs">MCP docs</a> ·
  <a href="https://postplanify.com/docs">API docs</a>
</p>

---

PostPlanify MCP is a hosted (remote) MCP server for social media scheduling and management. Connect it to your AI assistant and manage every social account from one conversation: turn an idea or a blog post into platform-ready posts, schedule them, see what is performing and reply to comments.

**Supported platforms:** Instagram, TikTok, LinkedIn, X (Twitter), Facebook, YouTube, Threads, Pinterest, Bluesky and Google Business Profile.

There is nothing to install or run locally. The server lives at:

```
https://api.postplanify.com/api/mcp
```

## What you can do

- **Schedule posts across platforms.** Write once, adapt per platform and schedule to every account in one go. Images, videos and carousels supported.
- **Plan your content calendar.** See what is scheduled and published, reschedule, edit or cancel posts.
- **Know what is working.** Ask which posts performed best, how followers are growing and which accounts get the most engagement.
- **Handle comments and reviews.** Read and reply to Instagram, Facebook and LinkedIn comments and Google Business reviews.
- **Manage multiple brands.** Agencies and teams work across every workspace and client account from a single connection.

## Example prompts

- "Schedule this announcement to our LinkedIn, X and Threads for Tuesday at 9am."
- "Turn this blog post into an Instagram caption and a TikTok caption and schedule both for Friday evening."
- "What do we have scheduled next week across all accounts?"
- "Which of our posts got the most views in the last 30 days?"
- "Show me unread Instagram comments and draft replies for me to approve."

## Requirements

An active PostPlanify plan with API access. Sign up at [postplanify.com](https://postplanify.com).

## Setup

| | |
|---|---|
| Endpoint | `https://api.postplanify.com/api/mcp` |
| Transport | Streamable HTTP |
| Auth | OAuth 2.1 (dynamic client registration), or an API key as `Authorization: Bearer sk_live_...` |

Clients that support OAuth open a browser window to sign in with your PostPlanify account. No key needed. For clients that do not, create an API key at [postplanify.com/dashboard/api-keys](https://postplanify.com/dashboard/api-keys).

### Claude (claude.ai and Claude Desktop)

Settings → Connectors → **Add custom connector**, then paste `https://api.postplanify.com/api/mcp` and sign in.

### ChatGPT

Search for **PostPlanify** in the ChatGPT apps/plugins directory, click install and sign in with your PostPlanify account.

### Claude Code

```bash
claude mcp add --transport http postplanify https://api.postplanify.com/api/mcp
```

Then run `/mcp` inside Claude Code to sign in.

### Cursor

Add to `~/.cursor/mcp.json`:

```json
{
  "mcpServers": {
    "postplanify": {
      "url": "https://api.postplanify.com/api/mcp"
    }
  }
}
```

### Cline

Add to `cline_mcp_settings.json` (MCP Servers → Configure):

```json
{
  "mcpServers": {
    "postplanify": {
      "type": "streamableHttp",
      "url": "https://api.postplanify.com/api/mcp",
      "headers": {
        "Authorization": "Bearer sk_live_your_api_key_here"
      }
    }
  }
}
```

### VS Code, Windsurf and other clients

Any client that supports remote MCP servers works with the endpoint above. For clients that only support local (stdio) servers, use the `mcp-remote` bridge (requires Node.js):

```json
{
  "mcpServers": {
    "postplanify": {
      "command": "npx",
      "args": [
        "mcp-remote",
        "https://api.postplanify.com/api/mcp",
        "--header",
        "Authorization: Bearer sk_live_your_api_key_here"
      ]
    }
  }
}
```

## Tools

22 tools, discovered automatically once connected. Read-only and destructive tools carry MCP annotations, so clients can ask before anything is published or deleted.

| Category | Tools |
|---|---|
| Posts | `list_posts`, `create_post`, `get_post`, `edit_post`, `cancel_post`, `delete_post` |
| Media | `upload_media_from_url`, `upload_media_from_base64`, `list_media`, `delete_media` |
| Analytics | `get_brand_overview`, `get_account_analytics`, `get_analytics_trends` |
| Comments | `list_comments`, `list_comment_replies`, `reply_to_comment` |
| Accounts | `get_current_user`, `list_workspaces`, `get_workspace`, `list_social_accounts`, `get_social_account`, `list_pinterest_boards` |

Full parameter reference: [postplanify.com/mcp/docs](https://postplanify.com/mcp/docs).

## Agent skill

Prefer plain HTTP over MCP? The [`skills/postplanify`](skills/postplanify) folder is an agent skill that teaches any coding agent to use the PostPlanify REST API with `curl`.

```bash
npx skills add postplanifyapp/postplanify-mcp
```

It is also on ClawHub: `openclaw skills install @hasancagli/postplanify`.

## Support

Questions or problems: open an issue here or reach us at [postplanify.com/contact](https://postplanify.com/contact).

## License

The documentation and skill in this repository are MIT licensed. The PostPlanify service is subject to the [PostPlanify terms](https://postplanify.com/terms).
