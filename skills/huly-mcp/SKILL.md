---
name: huly-mcp
description: Install, configure, verify, and use the @firfi/huly-mcp Model Context Protocol server for Huly workspaces. Use when a user wants to connect an agent to Huly, manage Huly issues/projects/documents/comments/milestones/custom fields/test management/workspace data, troubleshoot Huly MCP setup, or choose the right Huly MCP tool for a project-management workflow.
---

# Huly MCP

Use `@firfi/huly-mcp` as the source of truth for Huly MCP operations. Prefer its MCP tools over direct Huly API calls, browser automation, or ad hoc SDK code.

## Setup

When the active client exposes MCP configuration, first check whether a `huly` server is already configured. If it is missing, add `@firfi/huly-mcp` with `npx -y @firfi/huly-mcp@latest`.

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

Use `HULY_TOKEN` instead of `HULY_EMAIL` + `HULY_PASSWORD` when the user has an API token. Never print real credentials back to the user or store them in repository files.

## Environment

Required:

- `HULY_URL`: Huly instance URL, for example `https://huly.app`
- `HULY_WORKSPACE`: workspace identifier
- Auth: either `HULY_TOKEN`, or both `HULY_EMAIL` and `HULY_PASSWORD`

Additional server settings:

- `HULY_CONNECTION_TIMEOUT`: connection timeout in milliseconds, default `30000`
- `MCP_TRANSPORT`: `stdio` by default, or `http`
- `MCP_HTTP_PORT`: HTTP port, default `3000`
- `MCP_HTTP_HOST`: HTTP host, default `127.0.0.1`

`TOOLSETS` is an advanced filter. Leave it unset for the full tool surface. If a user explicitly wants fewer exposed tools, set it to comma-separated categories such as `issues,projects,search`. Available categories: `projects`, `issues`, `comments`, `milestones`, `documents`, `storage`, `attachments`, `contacts`, `channels`, `calendar`, `time tracking`, `search`, `activity`, `notifications`, `workspace`, `cards`, `custom-fields`, `labels`, `leads`, `tag-categories`, `task-management`, `test-management`.

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
3. `list_statuses` with a selected project identifier before creating or updating issues with statuses.

If verification fails, inspect env var names first. Common mistakes are using `HULY_HOST` instead of `HULY_URL`, using a display workspace name instead of the workspace identifier, or supplying stale credentials.

## Usage Rules

- Prefer identifier-aware tools and let the MCP server resolve names/IDs where supported.
- Use `preview_deletion` before deleting supported tracker entities: issues, projects, components, and milestones. For other delete tools, inspect the target with the corresponding read tool and confirm the user wants deletion.
- Use `list_statuses`, `list_components`, or `list_milestones` with a project identifier to discover valid project-scoped IDs before writes. Use `list_custom_fields` for custom-field IDs. Use `list_workspace_members` for workspace-member operations such as role updates; for issue assignment, use an assignee email address or display name, not a workspace member account ID.
- Use document tools for Huly documents; do not edit Huly document markup manually.
- Use `link_document_to_issue` / `unlink_document_from_issue` for issue-document relations.
- Use `list_issue_relations` to inspect `blockedBy` issues, bidirectional issue links, and linked documents. It does not return issues that the current issue blocks; call `list_issue_relations` on the target issue to inspect that direction.
- Use `list_custom_fields` to discover field IDs before `set_custom_field`. Use `get_custom_field_values` to inspect the current custom-field values on the target issue or card.
- Keep Huly comments and descriptions in markdown; the MCP server handles Huly markup conversion.

## Workflow Map

For issue work:

- Discover projects with `list_projects`.
- Discover statuses with `list_statuses` and the selected project identifier.
- Query issues with `list_issues`.
- Read details with `get_issue`.
- Create or modify with `create_issue`, `update_issue`, `move_issue`, labels, relations, components, and milestones.

For document work:

- Discover teamspaces with `list_teamspaces`.
- Use `list_documents`, `get_document`, `create_document`, and `edit_document`.
- Use `list_inline_comments` for document review threads.

For communication:

- Use `list_comments`, `add_comment`, `update_comment`, and `delete_comment` for issue comments.
- Use `list_activity`, `list_mentions`, `save_message`, `unsave_message`, `list_saved_messages`, `add_reaction`, `remove_reaction`, and `list_reactions` for activity workflows. `list_activity` needs an internal `objectId` and `objectClass`, not an issue key like `PROJ-123`; use IDs/classes returned by read tools. Reaction and save tools take activity `messageId` values.
- Use channel/direct-message tools when the task is about Huly chat.

For planning and QA:

- Use `log_time`, `get_time_report`, `list_time_spend_reports`, `get_detailed_time_report`, `list_work_slots`, `create_work_slot`, `start_timer`, and `stop_timer` for time tracking.
- Use `list_test_projects`, `list_test_suites`, `list_test_cases`, `list_test_plans`, `list_test_runs`, and `list_test_results` to discover test-management entities before creating or updating suites, cases, plans, runs, and results.
- Use `fulltext_search` when the target object is unknown.

## References

Read `references/tool-selection.md` when choosing among similar Huly MCP tools or when planning a multi-step workflow.

Read `references/troubleshooting.md` when setup, auth, transport, or connection behavior is unclear.
