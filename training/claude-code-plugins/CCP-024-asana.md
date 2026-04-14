---
type: procedure
created: 2026-03-20
status: active
slug: ccp-024-asana
tags: [procedure, nexus]
---

# CCP-024 — Asana Project Management MCP

## Problema
Development work tracked in Asana requires constant tab-switching to update task status, create tasks from identified work, and check project progress. The Asana MCP server (SSE-based) integrates Asana's work management platform directly into Claude Code sessions.

## Procedura

### Prerequisites
- Asana account with workspace access
- Install: `/plugin install asana@claude-plugins-official`
- MCP type: SSE (Server-Sent Events) to `https://mcp.asana.com/sse`

### Steps

1. **Install the plugin**:
   ```
   /plugin install asana@claude-plugins-official
   ```

2. **MCP configuration** (SSE, hosted service):
   ```json
   {
     "asana": {
       "type": "sse",
       "url": "https://mcp.asana.com/sse"
     }
   }
   ```

3. **Authentication**: Asana's MCP server handles authentication via SSE connection. Follow the authorization prompts to connect your Asana workspace.

4. **Capabilities**: Create and manage tasks, search projects, update task assignments and due dates, track progress, manage project sections, add comments and attachments, query workspace data.

5. **Usage patterns**:
   ```
   "Create an Asana task for implementing the OAuth feature"
   "What tasks are assigned to me this sprint?"
   "Mark the database migration task as complete"
   "Search for all tasks in the API Integration project"
   ```

6. **SSE vs HTTP**: Asana uses SSE (Server-Sent Events) transport — a persistent connection that enables real-time updates. This means the connection stays open during the Claude Code session.

7. **Combine with feature-dev** (CCP-001): After the `/feature-dev` workflow completes, create Asana tasks for the suggested next steps captured in Phase 7's summary.

### Verification
- [ ] Plugin installs without error
- [ ] SSE connection to `mcp.asana.com/sse` establishes successfully
- [ ] Claude can list tasks in a workspace project
- [ ] Claude can create new tasks with title and description

## Cortex Logging
- collection: procedures
- tags: claude-code-plugins, training, asana, mcp, sse, project-management, v2

## Enforcement Loop
- WHERE: Teams using Asana for project management
- WHEN: Converting code review findings to tasks, tracking feature work, reporting progress
- HOW: Install plugin, authenticate, use natural language to manage Asana tasks
- CONNECT: CCP-001 (feature-dev for identifying tasks), CCP-023 (linear as alternative tracker)
