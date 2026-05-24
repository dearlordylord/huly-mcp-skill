# Troubleshooting

## MCP Server Does Not Start

Check:

- Node.js can run `npx -y @firfi/huly-mcp@latest`.
- Env var names are exact: `HULY_URL`, `HULY_WORKSPACE`, and either `HULY_TOKEN` or `HULY_EMAIL` + `HULY_PASSWORD`.
- The client config uses `"command": "npx"` and `"args": ["-y", "@firfi/huly-mcp@latest"]`.
- Restart the MCP client after config changes.

## Authentication Fails

Check:

- `HULY_URL` points to the instance root, not a workspace page.
- `HULY_WORKSPACE` is the workspace identifier expected by Huly.
- Token auth and password auth are not mixed accidentally.
- Passwords with shell-special characters are quoted if configured through a shell command.

## No Tools Appear

Check:

- `TOOLSETS` is unset or uses exact category names.
- The MCP client was restarted after changing config.
- The server process is not pinned to an old cached package. Re-run with `npx -y @firfi/huly-mcp@latest`.

## HTTP Transport

Set:

```bash
MCP_TRANSPORT=http
MCP_HTTP_PORT=3000
MCP_HTTP_HOST=127.0.0.1
```

Use `MCP_HTTP_HOST=0.0.0.0` only when the endpoint must be reachable from another host or container, and protect that deployment appropriately because the server has Huly credentials.

## Safe Verification

Use read-only calls first:

- `list_projects`
- `get_workspace_info`
- `list_statuses`

Do not test setup by creating, updating, or deleting Huly objects unless the user asks for that.
