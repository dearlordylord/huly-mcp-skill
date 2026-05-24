# Troubleshooting

For full client-specific setup, use the canonical MCP server README:
https://github.com/dearlordylord/huly-mcp#installation

## MCP Server Does Not Start

Check:

- Node.js can run `npx -y @firfi/huly-mcp@latest`.
- Env var names are exact: `HULY_URL`, `HULY_WORKSPACE`, and either `HULY_TOKEN` or `HULY_EMAIL` + `HULY_PASSWORD`.
- The client config runs `npx -y @firfi/huly-mcp@latest`. Some clients split that into `"command": "npx"` plus `"args"`, while OpenCode uses a single command array.
- Restart the MCP client after config changes.

## Authentication Fails

Check:

- `HULY_URL` points to the instance root, not a workspace page.
- `HULY_WORKSPACE` is the workspace identifier expected by Huly.
- The configured token or password is current.
- If a password contains shell-special characters such as `*`, `%`, `!`, or `#`, edit the MCP config file directly instead of passing the password through CLI flags like `-e` or `--env`. Use `~/.codex/config.toml` for Codex, Claude JSON config for Claude clients, `~/.config/opencode/opencode.json` for OpenCode, the canonical README install section for VS Code or Cursor paths, and the configured MCP file for Windsurf or other clients. Avoid committing workspace config files that contain real credentials. `HULY_TOKEN` also avoids password quoting issues.
- On self-hosted Huly, repeated bad password attempts can return `PasswordLoginLocked`. Stop retrying password auth. Reset `global_account.account.failed_login_attempts` to `0` for the account in CockroachDB, or use `HULY_TOKEN` if available. CockroachDB credentials are usually in the Huly `compose.yml` or container environment.

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
- `list_statuses` with a project identifier from `list_projects`

Do not test setup by creating, updating, or deleting Huly objects unless the user asks for that.
