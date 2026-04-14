---
type: procedure
created: 2026-03-20
status: active
slug: ccp-020-github
tags: [procedure, nexus]
---

# CCP-020 — GitHub Official MCP Integration

## Problema
Managing GitHub — issues, pull requests, code review, repository search — normally requires context switching between the terminal and browser. The official GitHub MCP server exposes GitHub's full API as tools directly accessible to Claude Code, enabling repository management without leaving the development session.

## Procedura

### Prerequisites
- GitHub Personal Access Token with required scopes (repo, issues, pull_requests)
- Install: `/plugin install github@claude-plugins-official`
- MCP type: HTTP with Bearer token authentication

### Steps

1. **Install the plugin**:
   ```
   /plugin install github@claude-plugins-official
   ```

2. **Configure authentication**: The plugin requires `GITHUB_PERSONAL_ACCESS_TOKEN` set as an environment variable. The `.mcp.json` passes it as a Bearer header:
   ```json
   {
     "github": {
       "type": "http",
       "url": "https://api.githubcopilot.com/mcp/",
       "headers": {
         "Authorization": "Bearer ${GITHUB_PERSONAL_ACCESS_TOKEN}"
       }
     }
   }
   ```
   Set in shell: `export GITHUB_PERSONAL_ACCESS_TOKEN=ghp_...`

3. **Generate a PAT**: GitHub → Settings → Developer settings → Personal access tokens → Fine-grained tokens. Select scopes: `repo` (full), `issues` (read/write), `pull_requests` (read/write).

4. **Capabilities available via MCP tools**: Create and manage issues, create and review pull requests, search repositories and code, manage branches, interact with GitHub's full REST API, comment on issues and PRs.

5. **Usage patterns**:
   ```
   "Create an issue for the auth bug we found"
   "List all open PRs in this repository"
   "Search for uses of deprecated API in the codebase"
   "Add a comment to PR #42"
   ```

6. **Combine with commit-commands** (CCP-010): Use `/commit-push-pr` to create PRs via `gh` CLI, then use GitHub MCP for further PR management (comments, reviews, labels).

7. **Security note**: The PAT is passed in every MCP request header. Use fine-grained tokens scoped to specific repositories rather than classic tokens with broad access.

### Verification
- [ ] `GITHUB_PERSONAL_ACCESS_TOKEN` is set in shell environment
- [ ] Plugin installs without error
- [ ] Claude can list issues or PRs in the current repository
- [ ] MCP connection uses HTTPS to `api.githubcopilot.com/mcp/`

## Cortex Logging
- collection: procedures
- tags: claude-code-plugins, training, github, mcp, http, repository-management, v2

## Enforcement Loop
- WHERE: Any repository-management tasks during Claude Code sessions
- WHEN: Creating issues, reviewing PRs, searching code, managing branches
- HOW: Set PAT as env var, install plugin, use natural language to manage GitHub resources
- CONNECT: CCP-010 (commit-commands for git workflow), CCP-002 (code-review)
