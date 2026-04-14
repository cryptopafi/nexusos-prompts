---
type: procedure
created: 2026-03-20
status: active
slug: ccp-027-context7
tags: [procedure, nexus]
---

# CCP-027 — Context7 Library Documentation Lookup MCP

## Problema
Claude's training data has a knowledge cutoff. When working with rapidly evolving libraries — React, Next.js, Supabase, LangChain — Claude may suggest outdated APIs, deprecated patterns, or simply not know about features added after the cutoff. Context7 pulls version-specific documentation directly from source repositories into the LLM context.

## Procedura

### Prerequisites
- Node.js and npx available
- Install: `/plugin install context7@claude-plugins-official`
- MCP type: stdio (local process via npx, by Upstash)

### Steps

1. **Install the plugin**:
   ```
   /plugin install context7@claude-plugins-official
   ```

2. **MCP configuration** (stdio, runs locally):
   ```json
   {
     "context7": {
       "command": "npx",
       "args": ["-y", "@upstash/context7-mcp"]
     }
   }
   ```

3. **How it works**: Context7 maintains an index of library documentation pulled directly from source repositories. When Claude needs to reference a library's API, it queries context7 and receives version-specific, up-to-date documentation that gets injected into context.

4. **Activate explicitly for library questions**:
   ```
   "Use context7 to check the current Next.js App Router data fetching API"
   "Look up the latest Supabase JavaScript client authentication methods"
   "Get the current React 19 hooks documentation"
   ```

5. **When context7 adds the most value**:
   - Libraries that release frequently (React, Next.js, Shadcn/ui, Drizzle ORM)
   - Libraries added to the ecosystem after Claude's training cutoff
   - When Claude seems uncertain about API signatures or returns "I'm not sure about the latest version"
   - Before implementing integrations with external services that update their SDKs frequently

6. **Code examples**: Context7 injects actual code examples from the library's official docs, not Claude-generated examples. This makes the code more likely to be correct and idiomatic.

7. **Performance note**: The first call to a library may take a moment as Context7 fetches and indexes the documentation. Subsequent calls for the same library are faster.

### Verification
- [ ] Plugin installs without error
- [ ] Asking about a library API with "use context7" returns version-specific documentation
- [ ] Code examples match the library's official documentation style
- [ ] Outdated API patterns are not suggested when context7 is used

## Cortex Logging
- collection: procedures
- tags: claude-code-plugins, training, context7, mcp, documentation, library-docs, v2

## Enforcement Loop
- WHERE: Any Claude Code session working with external libraries
- WHEN: Implementing integrations, using libraries with frequent releases, Claude seems uncertain about API
- HOW: Explicitly ask Claude to "use context7" for library-specific questions; routine for post-cutoff libraries
- CONNECT: CCP-029 (serena for code-level analysis), CCP-028 (playwright whose MCP is also locally executed)
