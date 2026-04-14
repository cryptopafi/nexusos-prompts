---
type: procedure
created: 2026-03-20
status: active
slug: ccp-025-supabase
tags: [procedure, nexus]
---

# CCP-025 — Supabase Database/Auth/Storage MCP

## Problema
Managing Supabase projects requires switching between the dashboard, SQL editor, and code editor to run queries, check table structure, manage auth configuration, and handle storage. The Supabase MCP server exposes database operations, authentication, and storage management directly in Claude Code.

## Procedura

### Prerequisites
- Supabase account with active project
- Install: `/plugin install supabase@claude-plugins-official`
- MCP type: HTTP to `https://mcp.supabase.com/mcp`

### Steps

1. **Install the plugin**:
   ```
   /plugin install supabase@claude-plugins-official
   ```

2. **MCP configuration** (HTTP, hosted service):
   ```json
   {
     "supabase": {
       "type": "http",
       "url": "https://mcp.supabase.com/mcp"
     }
   }
   ```

3. **Authentication**: Supabase's MCP server handles authentication. Follow the authorization flow to connect your Supabase projects.

4. **Capabilities**: Database operations (SQL queries, table management, schema inspection), authentication configuration (users, policies, providers), storage bucket management, real-time subscriptions, Edge Functions management, project configuration.

5. **Usage patterns**:
   ```
   "Show me the schema for the users table"
   "Run a query to find all users created this week"
   "Check the RLS policies on the orders table"
   "Create a new storage bucket for user avatars"
   "Show me all Edge Functions in this project"
   ```

6. **RLS (Row Level Security)**: When building features, ask Claude to verify RLS policies are in place before writing code that queries the database — security by default.

7. **Combine with feature-dev** (CCP-001): During Phase 2 (codebase exploration), ask Claude to use Supabase MCP to inspect the database schema as part of understanding the existing data model.

8. **Key Supabase concepts accessible via MCP**: Projects, organizations, database tables/views/functions, auth users and policies, storage buckets/objects, Edge Functions, project secrets.

### Verification
- [ ] Plugin installs without error
- [ ] Authentication to Supabase project completes
- [ ] Claude can inspect table schema in the connected project
- [ ] Claude can run read queries against the database

## Cortex Logging
- collection: procedures
- tags: claude-code-plugins, training, supabase, mcp, database, auth, storage, v2

## Enforcement Loop
- WHERE: Projects using Supabase as backend
- WHEN: Database schema inspection, query debugging, auth policy review, storage management
- HOW: Install plugin, authenticate, use natural language to interact with Supabase project
- CONNECT: CCP-026 (firebase as alternative backend), CCP-022 (stripe for payment data storage)
