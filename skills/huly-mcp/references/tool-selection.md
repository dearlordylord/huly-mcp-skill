# Tool Selection

Use this reference when choosing Huly MCP tools for a user workflow.

## Discovery First

Before writes, discover valid IDs and names:

- Project: `list_projects`, then `get_project` if details/statuses are needed.
- Status: `list_statuses`.
- Assignee/member: `list_workspace_members`.
- Component: `list_components`.
- Milestone: `list_milestones`.
- Labels: `list_labels`.
- Custom fields: `list_custom_fields`, then `get_custom_field_values`.
- Documents: `list_teamspaces`, then `list_documents`.

## Common Workflows

Create a tracked task:

1. `list_projects`
2. `list_statuses`
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
3. For linked docs, use `get_document`

Delete safely:

1. `preview_deletion`
2. Explain affected entities and warnings
3. Only call the delete tool if the user explicitly wants deletion

## Toolset Narrowing

Use `TOOLSETS` only when startup/tool-list size is a problem. Prefer all tools for general agents. For focused contexts:

- Issue tracker: `issues,projects,comments,milestones,labels,custom-fields,search`
- Docs: `documents,comments,search`
- QA: `test-management,issues,projects,documents`
- Chat/activity: `channels,activity,notifications,contacts`
