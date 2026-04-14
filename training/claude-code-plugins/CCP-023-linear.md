---
type: procedure
created: 2026-03-20
status: active
slug: ccp-023-linear
tags: [procedure, nexus]
---

# CCP-023 — Linear Issue Tracking MCP

## Problema
Switching between Claude Code and the Linear browser interface to create issues, update statuses, and track work breaks development focus. The Linear MCP server exposes issue management directly in Claude Code, enabling issue creation and project tracking without context switching.

## Procedura

### Prerequisites
- Linear account with workspace access
- Install: `/plugin install linear@claude-plugins-official`
- MCP type: HTTP to `https://mcp.linear.app/mcp` (authentication via Linear's OAuth/API key)

### Steps

1. **Install the plugin**:
   ```
   /plugin install linear@claude-plugins-official
   ```

2. **MCP configuration** (HTTP, hosted service):
   ```json
   {
     "linear": {
       "type": "http",
       "url": "https://mcp.linear.app/mcp"
     }
   }
   ```

3. **Authentication**: Linear's MCP server handles authentication. On first use, Claude Code initiates the authorization flow. Follow the prompts to connect your Linear workspace.

4. **Capabilities**: Create issues, manage projects, update issue statuses, search across workspaces, assign issues, add comments, manage cycles and roadmaps.

5. **Usage patterns**:
   ```
   "Create a Linear issue for the auth bug we found"
   "What's the status of issues in the current sprint?"
   "Mark issue ENG-123 as in progress"
   "Search for all open bugs in the frontend project"
   ```

6. **Workflow integration**: Create issues directly from code review findings, link commits to issues, update status when starting/finishing work, search prior issues before creating duplicates.

7. **Combine with code-review** (CCP-002): After `/code-review` finds issues, create Linear issues for medium-priority findings that won't be fixed immediately.

### Verification
- [ ] Plugin installs without error
- [ ] Authentication to Linear workspace completes
- [ ] Claude can create a new issue in the workspace
- [ ] Claude can search and list existing issues

## Cortex Logging
- collection: procedures
- tags: claude-code-plugins, training, linear, mcp, issue-tracking, project-management, v2

## Enforcement Loop
- WHERE: Teams using Linear for issue tracking
- WHEN: Creating issues from code review findings, updating status during work, searching prior issues
- HOW: Install plugin, authenticate, use natural language to manage issues
- CONNECT: CCP-002 (code-review for issue creation source), CCP-020 (github for PR linking)
