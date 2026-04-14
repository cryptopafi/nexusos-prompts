---
type: procedure
created: 2026-03-20
status: active
slug: ccp-026-firebase
tags: [procedure, nexus]
---

# CCP-026 — Google Firebase MCP

## Problema
Firebase projects span multiple services — Firestore, Authentication, Cloud Functions, Hosting, Storage — each with its own console. The Firebase MCP server (runs via `firebase-tools`) enables managing all Firebase services directly from Claude Code without switching to the Firebase console.

## Procedura

### Prerequisites
- Firebase CLI installed: `npm install -g firebase-tools`
- Firebase project with appropriate permissions
- Install: `/plugin install firebase@claude-plugins-official`
- MCP type: stdio (local process via npx)

### Steps

1. **Install the plugin**:
   ```
   /plugin install firebase@claude-plugins-official
   ```

2. **MCP configuration** (stdio, runs locally via npx):
   ```json
   {
     "firebase": {
       "command": "npx",
       "args": ["-y", "firebase-tools@latest", "mcp"]
     }
   }
   ```
   Launches the latest `firebase-tools` as an MCP server via stdio. Authentication uses your existing Firebase CLI credentials.

3. **Authenticate Firebase CLI** if not already done:
   ```bash
   npx firebase-tools login
   ```

4. **Capabilities**: Firestore database operations (read/write/query), Firebase Authentication management, Cloud Functions deployment and management, Firebase Hosting deployment, Cloud Storage bucket management, project configuration.

5. **Usage patterns**:
   ```
   "Query the users collection in Firestore"
   "Deploy the current Cloud Functions"
   "Check the Firebase Hosting deployment status"
   "List all authenticated users in this project"
   "Show the Firestore security rules"
   ```

6. **Local execution**: Unlike Supabase/Linear/Asana (HTTP-based), Firebase MCP runs as a local process via `npx`. It uses your Firebase CLI login state and project selection (`firebase use --project`).

7. **Project selection**: If you have multiple Firebase projects, set the active project first:
   ```bash
   npx firebase-tools use --project your-project-id
   ```

8. **Combine with feature-dev** (CCP-001): During codebase exploration, use Firebase MCP to inspect Firestore schema and security rules as part of understanding the data layer.

### Verification
- [ ] `npx firebase-tools login` completes successfully before plugin use
- [ ] Plugin installs without error
- [ ] Claude can list Firestore collections in the active project
- [ ] MCP server starts as a local process (check with `claude --debug`)

## Cortex Logging
- collection: procedures
- tags: claude-code-plugins, training, firebase, mcp, firestore, cloud-functions, v2

## Enforcement Loop
- WHERE: Projects using Google Firebase as backend
- WHEN: Firestore queries, Functions deployment, Auth management, security rule review
- HOW: Ensure `firebase-tools` is logged in; install plugin; use natural language to manage Firebase
- CONNECT: CCP-025 (supabase as alternative), CCP-029 (serena for code-level analysis alongside runtime Firebase data)
