# Tool Selection

Use this reference when choosing Huly MCP tools for a user workflow.

## Discovery First

Before writes, discover valid IDs and names:

- Project: `list_projects`, then `get_project` if details/statuses are needed.
- Status: `list_statuses` with a project identifier.
- Workspace member role changes: `list_workspace_members`. For issue assignees, use email address or display name, not the workspace member account ID.
- Component: `list_components` with a project identifier.
- Milestone: `list_milestones` with a project identifier.
- Labels: `list_labels`.
- Custom fields: `list_custom_fields` for field definitions; `get_custom_field_values` for values already set on a target issue or card.
- Documents: `list_teamspaces`, then `list_documents`.

## Common Workflows

Create a tracked task:

1. `list_projects`
2. `list_statuses` with the selected project identifier
3. `create_issue`
4. Optionally `add_comment`, `add_issue_label`, `set_issue_component`, or `set_issue_milestone`

Update an existing issue:

1. `list_issues` or `fulltext_search`
2. `get_issue`
3. `update_issue`
4. Add a comment explaining the change when the user asks for communication or audit trail

Create a spec or note:

1. `list_teamspaces`
2. `create_document`
3. `link_document_to_issue` if it belongs with a tracker item

Inspect dependencies:

1. `get_issue`
2. `list_issue_relations`
3. Remember `list_issue_relations` returns `blockedBy`, bidirectional links, and documents; it does not list issues that the current issue blocks
4. For linked docs, use `get_document`

Delete safely:

1. Use `preview_deletion` for supported tracker entities: issues, projects, components, and milestones
2. Explain affected entities and warnings
3. For other delete tools, inspect the target with the corresponding read/list tool
4. Only call the delete tool if the user explicitly wants deletion

## Toolset Narrowing

Use `TOOLSETS` only when startup/tool-list size is a problem. Prefer all tools for general agents. For focused contexts:

- Issue tracker: `issues,projects,comments,milestones,labels,custom-fields,search`
- Docs: `documents,search`; add `issues` too when linking documents to issues
- QA: `test-management,issues,projects,documents`
- Chat/activity: `channels,activity,notifications,contacts`
