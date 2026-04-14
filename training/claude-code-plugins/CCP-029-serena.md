---
type: procedure
created: 2026-03-20
status: active
slug: ccp-029-serena
tags: [procedure, nexus]
---

# CCP-029 — Serena Semantic Code Analysis via LSP

## Problema
Claude Code understands code by reading files, but lacks deep semantic understanding — go-to-definition across files, find all references, rename symbol across codebase, understanding type hierarchies. Serena bridges this gap by integrating Language Server Protocol (LSP) capabilities into Claude Code, providing IDE-level code intelligence for refactoring and navigation.

## Procedura

### Prerequisites
- `uvx` available (from `uv`: `pip install uv` or `brew install uv`)
- Language server for the target language installed (see CCP-030 for LSP setup)
- Install: `/plugin install serena@claude-plugins-official`
- MCP type: stdio (runs via uvx, by Oraios)

### Steps

1. **Install the plugin**:
   ```
   /plugin install serena@claude-plugins-official
   ```

2. **MCP configuration** (stdio, runs via uvx from GitHub):
   ```json
   {
     "serena": {
       "command": "uvx",
       "args": ["--from", "git+https://github.com/oraios/serena", "serena", "start-mcp-server"]
     }
   }
   ```

3. **Install `uvx`** if not available:
   ```bash
   pip install uv    # or: brew install uv
   ```

4. **Capabilities via LSP integration**:
   - Go-to-definition across files
   - Find all references to a symbol
   - Symbol rename across entire codebase
   - Type hierarchy navigation
   - Code outline and structure inspection
   - Semantic refactoring suggestions
   - Cross-file dependency analysis

5. **Usage patterns**:
   ```
   "Find all references to the UserService class"
   "Rename the processPayment function to handlePayment everywhere"
   "Show me the type hierarchy for the BaseRepository class"
   "What functions call the authenticate() method?"
   "Outline the structure of the auth module"
   ```

6. **Combining with LSP plugins** (CCP-030): Serena uses whichever language server is installed and in PATH. Install the appropriate LSP plugin for your language first (typescript-lsp, pyright-lsp, etc.), then Serena provides the semantic analysis layer on top.

7. **Difference from text search**: Serena finds *semantic* references — it knows the difference between a variable named `user` in one scope and a different `user` in another. Regular grep/search doesn't have this understanding.

8. **Best for large codebases**: The semantic analysis is most valuable when a codebase is too large to manually trace references, when refactoring shared utilities used across many files, or when understanding complex inheritance hierarchies.

### Verification
- [ ] `uvx` is available in PATH
- [ ] Plugin installs without error
- [ ] Claude can find all references to a symbol across multiple files
- [ ] Symbol rename applies correctly in all affected files

## Cortex Logging
- collection: procedures
- tags: claude-code-plugins, training, serena, lsp, semantic-analysis, refactoring, v2

## Enforcement Loop
- WHERE: Large codebases where cross-file code intelligence matters
- WHEN: Renaming symbols, finding all usages, understanding type hierarchies, semantic refactoring
- HOW: Install serena + appropriate LSP plugin (CCP-030); use natural language to navigate and refactor
- CONNECT: CCP-030 (LSP setup required for Serena), CCP-014 (code-simplifier for simplification after refactoring)
