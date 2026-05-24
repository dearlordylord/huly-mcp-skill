---
name: huly-mcp
description: Install, configure, verify, and use the @firfi/huly-mcp Model Context Protocol server for Huly workspaces. Use when a user wants to connect an agent to Huly, manage Huly issues/projects/documents/comments/milestones/custom fields/test management/workspace data, troubleshoot Huly MCP setup, or choose the right Huly MCP tool for a project-management workflow.
---

# Huly MCP

Use `@firfi/huly-mcp` as the source of truth for Huly operations. Prefer MCP tools over direct Huly API calls, shell scripts, browser automation, or ad hoc SDK code.

## Setup

First check whether a `huly` MCP server is already configured in the user's agent/client. If it is missing, add it with `npx -y @firfi/huly-mcp@latest`.

Standard MCP config:

```json
{
  "mcpServers": {
    "huly": {
      "command": "npx",
      "args": ["-y", "@firfi/huly-mcp@latest"],
      "env": {
        "HULY_URL": "https://huly.app",
        "HULY_EMAIL": "your@email.com",
        "HULY_PASSWORD": "yourpassword",
        "HULY_WORKSPACE": "yourworkspace"
      }
    }
  }
}
```

Claude Code:

```bash
claude mcp add huly \
  -e HULY_URL=https://huly.app \
  -e HULY_EMAIL=you@example.com \
  -e HULY_PASSWORD='...' \
  -e HULY_WORKSPACE=yourworkspace \
  -- npx -y @firfi/huly-mcp@latest
```

Use `HULY_TOKEN` instead of `HULY_EMAIL` + `HULY_PASSWORD` when the user has an API token. Never print real credentials back to the user, store them in repo files, or pass them through commands that will be committed.

## Environment

Required:

- `HULY_URL`: Huly instance URL, for example `https://huly.app`
- `HULY_WORKSPACE`: workspace identifier
- Auth: either `HULY_TOKEN`, or both `HULY_EMAIL` and `HULY_PASSWORD`

Optional:

- `HULY_CONNECTION_TIMEOUT`: connection timeout in milliseconds, default `30000`
- `MCP_TRANSPORT`: `stdio` by default, or `http`
- `MCP_HTTP_PORT`: HTTP port, default `3000`
- `MCP_HTTP_HOST`: HTTP host, default `127.0.0.1`
- `TOOLSETS`: comma-separated categories to expose

Available `TOOLSETS` categories: `projects`, `issues`, `comments`, `milestones`, `documents`, `storage`, `attachments`, `contacts`, `channels`, `calendar`, `time tracking`, `search`, `activity`, `notifications`, `workspace`, `cards`, `custom-fields`, `labels`, `leads`, `tag-categories`, `task-management`, `test-management`.

For HTTP transport:

```bash
HULY_URL=https://huly.app \
HULY_TOKEN=... \
HULY_WORKSPACE=yourworkspace \
MCP_TRANSPORT=http \
npx -y @firfi/huly-mcp@latest
```

The default HTTP endpoint is `http://127.0.0.1:3000/mcp`.

## Verify

After setup, call a read-only tool before making changes:

1. `list_projects` to confirm the workspace is reachable.
2. `get_workspace_info` if workspace metadata is needed.
3. `list_statuses` for a project before creating or updating issues with statuses.

If verification fails, inspect env var names first. Common mistakes are using `HULY_HOST` instead of `HULY_URL`, using a display workspace name instead of the workspace identifier, or providing both malformed token auth and password auth.

## Usage Rules

- Prefer identifier-aware tools and let the MCP server resolve names/IDs where supported.
- Use `preview_deletion` before any destructive delete.
- Use `list_statuses`, `list_components`, `list_milestones`, `list_custom_fields`, or `list_workspace_members` to discover valid IDs before writes.
- Use document tools for Huly documents; do not edit Huly document markup manually.
- Use `link_document_to_issue` / `unlink_document_from_issue` for issue-document relations.
- Use `list_issue_relations` to inspect blockers, related issues, and linked documents.
- Use `get_custom_field_values` before `set_custom_field` unless the desired field/value is already certain.
- Keep Huly comments and descriptions in markdown; the MCP server handles Huly markup conversion.

## Workflow Map

For issue work:

- Discover projects with `list_projects`.
- Discover statuses with `list_statuses`.
- Query issues with `list_issues`.
- Read details with `get_issue`.
- Create or modify with `create_issue`, `update_issue`, `move_issue`, labels, relations, components, and milestones.

For document work:

- Discover teamspaces with `list_teamspaces`.
- Use `list_documents`, `get_document`, `create_document`, and `edit_document`.
- Use `list_inline_comments` for document review threads.

For communication:

- Use `list_comments`, `add_comment`, `update_comment`, and `delete_comment` for issue comments.
- Use `list_activity`, `list_mentions`, saved-message tools, and reaction tools for activity workflows.
- Use channel/direct-message tools when the task is about Huly chat.

For planning and QA:

- Use time tracking tools for time reports, work slots, and timers.
- Use test-management tools for suites, cases, plans, runs, and results.
- Use `fulltext_search` when the target object is unknown.

## References

Read `references/tool-selection.md` when choosing among similar Huly MCP tools or when planning a multi-step workflow.

Read `references/troubleshooting.md` when setup, auth, transport, or connection behavior is unclear.
