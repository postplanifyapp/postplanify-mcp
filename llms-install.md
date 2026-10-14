# Installing the PostPlanify MCP server

Instructions for AI agents setting up this server for a user.

PostPlanify is a **remote** MCP server. Do not clone this repository, install packages or build anything. The only step is adding the endpoint to the MCP client configuration.

- **Endpoint:** `https://api.postplanify.com/api/mcp`
- **Transport:** Streamable HTTP
- **Auth:** OAuth 2.1, or an API key sent as `Authorization: Bearer sk_live_...`

## Step 1: Get an API key from the user

Ask the user for their PostPlanify API key. If they do not have one:

1. They sign in at https://postplanify.com (an active plan with API access is required).
2. They create a key at https://postplanify.com/dashboard/api-keys. Keys start with `sk_live_`.

Never print the key back in full, commit it to a repository or write it anywhere except the MCP settings file.

If the client supports OAuth for remote servers, you can skip the key: add the server without the `headers` block and the client will open a browser window for the user to sign in.

## Step 2: Add the server to the MCP settings

### Any client with remote server support

Add this entry inside `mcpServers` in the client's MCP settings file (keep any existing servers):

```json
"postplanify": {
  "url": "https://api.postplanify.com/api/mcp",
  "headers": {
    "Authorization": "Bearer USER_API_KEY"
  }
}
```

Some clients need the transport named explicitly. Use the value the client documents for Streamable HTTP, for example `"type": "http"` or `"type": "streamableHttp"`.

### Claude Code

```bash
claude mcp add --transport http postplanify https://api.postplanify.com/api/mcp --header "Authorization: Bearer USER_API_KEY"
```

### Cursor

Add to `~/.cursor/mcp.json` inside `mcpServers`:

```json
"postplanify": {
  "url": "https://api.postplanify.com/api/mcp",
  "headers": {
    "Authorization": "Bearer USER_API_KEY"
  }
}
```

### Cline

Same entry as above in `cline_mcp_settings.json`, with `"type": "streamableHttp"` added.

### Clients without remote (HTTP) server support

Use the `mcp-remote` bridge (requires Node.js 18+):

```json
"postplanify": {
  "command": "npx",
  "args": [
    "mcp-remote",
    "https://api.postplanify.com/api/mcp",
    "--header",
    "Authorization: Bearer USER_API_KEY"
  ]
}
```

Replace `USER_API_KEY` with the key from step 1.

## Step 3: Verify

Call the `get_current_user` tool. It returns the user's name and email when the connection works. Then call `list_workspaces` to show the user their workspaces and connected social accounts.

| Result | Meaning | Fix |
|---|---|---|
| User profile returned | Connected | Done |
| 401 Unauthorized | Key missing, mistyped or revoked | Re-check the key or create a new one |
| Tools return an upgrade or plan message | The account has no plan with API access | The user needs an active plan with API access at https://postplanify.com |

## Using the server safely

- Show the user the account, caption, media and time (in their timezone) and get a yes before calling `create_post`. Posts publish automatically at the scheduled time.
- `reply_to_comment` posts publicly right away. Get approval for the exact text first.
- `delete_post` and `delete_media` are permanent. Confirm each one.
- All times are UTC (ISO 8601). Convert from the user's local time.

Full tool reference: https://postplanify.com/mcp/docs
