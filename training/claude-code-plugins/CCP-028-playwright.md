---
type: procedure
created: 2026-03-20
status: active
slug: ccp-028-playwright
tags: [procedure, nexus]
---

# CCP-028 — Playwright Browser Automation MCP (Microsoft)

## Problema
Testing frontend behavior, scraping web content, and automating browser interactions normally requires a separate test file setup and manual script execution outside of Claude Code. The official Playwright MCP server by Microsoft gives Claude direct browser control — clicking, filling forms, taking screenshots, and running tests — within the development session.

## Procedura

### Prerequisites
- Node.js and npx available
- Install: `/plugin install playwright@claude-plugins-official`
- MCP type: stdio (local process via npx, by Microsoft)

### Steps

1. **Install the plugin**:
   ```
   /plugin install playwright@claude-plugins-official
   ```

2. **MCP configuration** (stdio, runs locally):
   ```json
   {
     "playwright": {
       "command": "npx",
       "args": ["@playwright/mcp@latest"]
     }
   }
   ```

3. **Capabilities — browser control tools**:
   - Navigate to URLs
   - Click elements (by selector, text, or coordinates)
   - Fill form inputs
   - Take screenshots
   - Read page content and text
   - Interact with dialogs
   - Execute JavaScript in page context
   - Wait for elements or conditions
   - Manage cookies and storage

4. **Usage patterns**:
   ```
   "Take a screenshot of localhost:3000 after login"
   "Click the submit button on the checkout form"
   "Fill in the registration form with test data"
   "Scrape the pricing table from the competitor's website"
   "Run the checkout flow and verify the success page"
   ```

5. **Frontend development integration**: After building a UI component with `frontend-design` (CCP-012), use Playwright to visually verify it renders correctly and interactive elements work as expected — without writing a separate test file.

6. **E2E testing workflow**: Describe the test scenario in natural language → Playwright MCP executes the steps → screenshot confirms the result → Claude can generate a Playwright test file based on the verified flow.

7. **Headless vs. headed mode**: By default runs headless. For visual debugging: use `playwright.config.ts` settings or pass launch arguments via the npx command.

8. **Local development testing**: Point at `localhost` development servers for interactive testing during development — no deployment needed.

### Verification
- [ ] Plugin installs without error
- [ ] Claude can navigate to a URL using Playwright tools
- [ ] Screenshot is captured and readable by Claude
- [ ] Form interactions (fill + submit) execute correctly

## Cortex Logging
- collection: procedures
- tags: claude-code-plugins, training, playwright, browser-automation, testing, mcp, v2

## Enforcement Loop
- WHERE: Frontend development and E2E testing workflows
- WHEN: Verifying UI behavior, running E2E flows, scraping content, automating browser interactions
- HOW: Install plugin; describe browser actions in natural language; use screenshots to verify visual output
- CONNECT: CCP-012 (frontend-design for what to visually test), CCP-029 (serena for code-level analysis)
