# Huly MCP Skill

[![skills.sh](https://skills.sh/b/dearlordylord/huly-mcp-skill)](https://skills.sh/dearlordylord/huly-mcp-skill)
[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](LICENSE)

Agent Skill for installing, configuring, troubleshooting, and using [`@firfi/huly-mcp`](https://www.npmjs.com/package/@firfi/huly-mcp).

This repo contains a thin skill wrapper. It does not reimplement Huly access; it teaches agents how to install and use the MCP server correctly.

## Install

```bash
npx skills add dearlordylord/huly-mcp-skill
```

## What It Covers

- MCP installation for `@firfi/huly-mcp`
- Required Huly environment variables
- stdio and HTTP transport setup
- Safe verification with read-only tools
- Tool selection for Huly issues, documents, comments, custom fields, test management, workspace, and activity workflows
- Troubleshooting setup/auth/transport problems

## Skill

The skill lives at:

```text
skills/huly-mcp/SKILL.md
```

## License

MIT
