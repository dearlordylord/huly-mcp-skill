---
name: huly-mcp
description: Use the @firfi/huly-mcp Model Context Protocol server effectively for Huly project-management workflows. Trigger when a user wants to work with Huly issues, projects, documents, comments, milestones, custom fields, test management, activity, workspace data, or when they need concise setup direction for the Huly MCP server.
---

# Huly MCP

Use `@firfi/huly-mcp` as the source of truth for Huly MCP operations. Prefer its MCP tools over direct Huly API calls, browser automation, or ad hoc SDK code.

## Setup Pointer

Do not duplicate setup instructions unless the user asks for them. The canonical installation guide is:

https://github.com/dearlordylord/huly-mcp#installation

It includes Codex, Claude Code, Claude Desktop, VS Code, Cursor, Windsurf, and OpenCode setup. If the user is configuring a client, point them there and help adapt the listed config to their environment.

After setup, verify with read-only calls:

1. `list_projects`
2. `get_workspace_info` if workspace metadata is needed
3. `list_statuses` with a project identifier from `list_projects` before creating or updating issue statuses

## Operating Rules

- Prefer identifier-aware tools and let the MCP server resolve names/IDs where supported.
- Use `preview_deletion` before deleting supported tracker entities: issues, projects, components, and milestones. For other delete tools, inspect the target with the corresponding read tool and confirm the user wants deletion.
- Use `list_statuses`, `list_components`, or `list_milestones` with a project identifier to discover valid project-scoped IDs before writes.
- Use `list_custom_fields` for custom-field IDs. Use `get_custom_field_values` to inspect values already set on a target issue or card.
- Use `list_workspace_members` for workspace-member operations such as role updates. For issue assignment, use an assignee email address or display name, not a workspace member account ID.
- Keep Huly comments and descriptions in markdown; the MCP server handles Huly markup conversion.

## High-Value Caveats

- `list_issue_relations` returns `blockedBy` issues, bidirectional issue links, and linked documents. It does not return issues that the current issue blocks; call `list_issue_relations` on the target issue to inspect that direction.
- `list_activity` needs an internal `objectId` and `objectClass`, not an issue key like `PROJ-123`; use IDs/classes returned by read tools. Reaction and save tools take activity `messageId` values.
- Use `fulltext_search` when the target object type is unknown.

## References

Read `references/tool-selection.md` when choosing among similar Huly MCP tools or planning issue, document, dependency, deletion, search, or toolset-scoping work.

Read `references/troubleshooting.md` only when setup, auth, transport, or connection behavior is unclear.
