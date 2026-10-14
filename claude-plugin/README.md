# PostPlanify for Claude

Schedule and manage social media posts from Claude. This plugin connects Claude to [PostPlanify](https://postplanify.com) and adds a skill that guides Claude through planning, scheduling and reviewing posts.

**Supported platforms:** Instagram, TikTok, LinkedIn, X (Twitter), Facebook, YouTube, Threads, Pinterest, Bluesky and Google Business Profile.

## What you can do

- Turn an idea, announcement or blog post into platform-ready posts and schedule them to every account in one go
- See what is scheduled and published, then edit, reschedule or cancel posts
- Ask which posts performed best and how followers and engagement are trending
- Read and reply to Instagram, Facebook and LinkedIn comments and Google Business reviews
- Work across multiple brands and client workspaces from one connection

## What's inside

| Component | What it does |
|---|---|
| PostPlanify connector (`.mcp.json`) | Connects to the hosted PostPlanify MCP server at `https://api.postplanify.com/api/mcp`. You sign in with your PostPlanify account through OAuth. No API key is stored in the plugin. |
| `schedule-social-posts` skill | Tells Claude how to find the right account, respect each platform's rules, confirm before anything is scheduled or published, and report results. |

## Requirements

A PostPlanify account with an active plan that includes API access, and at least one social account connected in PostPlanify. Sign up at [postplanify.com](https://postplanify.com).

## Setup

1. Install the plugin. In Claude Code:

   ```
   /plugin install postplanify --marketplace postplanifyapp/postplanify-mcp
   ```

   On claude.ai, add it from the plugin directory.
2. Connect the PostPlanify connector and sign in with your PostPlanify account when prompted.
3. Ask Claude something like "What do we have scheduled next week?"

## Data and privacy

The plugin runs no local code. All requests go to PostPlanify's own server (`api.postplanify.com`) over HTTPS, using the OAuth sign-in you approve. Posts are published to social networks by PostPlanify through each platform's official API, for the accounts you connected in PostPlanify. See the [PostPlanify privacy policy](https://postplanify.com/privacy-policy).

## Support

- Docs: https://postplanify.com/mcp/docs
- Contact: https://postplanify.com/contact

## License

MIT
