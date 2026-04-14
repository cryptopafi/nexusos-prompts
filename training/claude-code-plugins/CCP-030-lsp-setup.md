---
type: procedure
created: 2026-03-20
status: active
slug: ccp-030-lsp-setup
tags: [procedure, nexus]
---

# CCP-030 — LSP Plugin Setup Guide (12 Languages)

## Problema
Language Server Protocol (LSP) plugins provide IDE-level code intelligence to Claude Code — diagnostics, go-to-definition, find references, error checking — for any supported language. Each language has a separate plugin in the marketplace, each requiring its own language server binary installed and in PATH.

## Procedura

### Prerequisites
- Claude Code installed
- The appropriate language's toolchain installed (Node.js for TypeScript, Go for gopls, Rust for rust-analyzer, etc.)
- Install each plugin: `/plugin install {language}-lsp@claude-plugins-official`

### Steps

1. **Install the plugin for your language** using the marketplace command:
   ```
   /plugin install typescript-lsp@claude-plugins-official
   ```

2. **Install the language server binary** — each language requires its own server in PATH:

   **TypeScript/JavaScript** (`.ts`, `.tsx`, `.js`, `.jsx`, `.mts`, `.cts`, `.mjs`, `.cjs`)
   ```bash
   npm install -g typescript-language-server typescript
   # or: yarn global add typescript-language-server typescript
   ```
   More: [github.com/typescript-language-server/typescript-language-server](https://github.com/typescript-language-server/typescript-language-server)

   **Go** (`.go`)
   ```bash
   go install golang.org/x/tools/gopls@latest
   # Ensure $GOPATH/bin (or $HOME/go/bin) is in PATH
   ```
   More: [pkg.go.dev/golang.org/x/tools/gopls](https://pkg.go.dev/golang.org/x/tools/gopls)

   **Rust** (`.rs`)
   ```bash
   rustup component add rust-analyzer
   # macOS alternative: brew install rust-analyzer
   # Ubuntu: sudo apt install rust-analyzer
   ```
   More: [rust-analyzer.github.io](https://rust-analyzer.github.io/)

   **Python** (`.py`, `.pyi`)
   ```bash
   npm install -g pyright
   # or: pip install pyright
   # or: pipx install pyright  (recommended)
   ```
   More: [github.com/microsoft/pyright](https://github.com/microsoft/pyright)

   **Java** (`.java`) — requires Java 17+
   ```bash
   brew install jdtls           # macOS
   yay -S jdtls                 # Arch Linux (AUR)
   # Other: download from eclipse.org/jdtls/snapshots/, extract, create wrapper script
   ```
   More: [github.com/eclipse-jdtls/eclipse.jdt.ls](https://github.com/eclipse-jdtls/eclipse.jdt.ls)

   **Kotlin** (`.kt`, `.kts`)
   ```bash
   brew install JetBrains/utils/kotlin-lsp
   ```
   More: [github.com/Kotlin/kotlin-lsp](https://github.com/Kotlin/kotlin-lsp)

   **C/C++** (`.c`, `.h`, `.cpp`, `.cc`, `.cxx`, `.hpp`, `.hxx`, `.C`, `.H`)
   ```bash
   brew install llvm            # macOS (add /opt/homebrew/opt/llvm/bin to PATH)
   sudo apt install clangd      # Ubuntu/Debian
   sudo dnf install clang-tools-extra  # Fedora
   winget install LLVM.LLVM     # Windows
   ```
   More: [clangd.llvm.org](https://clangd.llvm.org/)

   **C#** (`.cs`) — requires .NET SDK 6.0+
   ```bash
   dotnet tool install --global csharp-ls
   # macOS: brew install csharp-ls
   ```
   More: [github.com/razzmatazz/csharp-language-server](https://github.com/razzmatazz/csharp-language-server)

   **Ruby** (`.rb`, `.rake`, `.gemspec`, `.ru`, `.erb`) — requires Ruby 3.0+
   ```bash
   gem install ruby-lsp
   # or add to Gemfile: gem 'ruby-lsp', group: :development
   ```
   More: [shopify.github.io/ruby-lsp](https://shopify.github.io/ruby-lsp/)

   **Swift** (`.swift`) — uses SourceKit-LSP (bundled with Swift toolchain)
   ```bash
   brew install swift           # macOS (or install Xcode)
   # Linux: download from swift.org/download/
   # After install, sourcekit-lsp is in PATH automatically
   ```
   More: [github.com/apple/sourcekit-lsp](https://github.com/apple/sourcekit-lsp)

   **Lua** (`.lua`)
   ```bash
   brew install lua-language-server     # macOS
   sudo snap install lua-language-server --classic  # Ubuntu
   sudo pacman -S lua-language-server   # Arch Linux
   # Windows: download from github.com/LuaLS/lua-language-server/releases
   ```
   More: [luals.github.io](https://luals.github.io/)

   **PHP** (`.php`) — uses Intelephense
   ```bash
   npm install -g intelephense
   # or: yarn global add intelephense
   ```
   More: [intelephense.com](https://intelephense.com/)

3. **Verify the server is in PATH** before testing:
   ```bash
   which typescript-language-server  # TypeScript
   which gopls                        # Go
   which rust-analyzer                # Rust
   which pyright                      # Python
   which jdtls                        # Java
   which clangd                       # C/C++
   which sourcekit-lsp                # Swift
   ```

4. **Multiple language projects**: Install multiple LSP plugins in Claude Code — each plugin handles its respective file extensions independently. A full-stack TypeScript + Python project can have both `typescript-lsp` and `pyright-lsp` active simultaneously.

5. **Serena integration** (CCP-029): Once an LSP server is installed and in PATH, Serena uses it automatically for semantic analysis. Install Serena alongside any LSP plugin for cross-file intelligence.

6. **Troubleshooting**: If LSP features don't activate, verify (a) the language server binary is in PATH, (b) the plugin is installed and enabled, (c) the file extension matches the language's supported extensions listed above.

### Verification
- [ ] Language server binary is installed and in PATH (`which <server>` returns a path)
- [ ] Plugin installs without error for the target language
- [ ] Claude receives diagnostic information when editing a file with errors
- [ ] For Java: Java 17+ is installed (`java -version`)

## Cortex Logging
- collection: procedures
- tags: claude-code-plugins, training, lsp, typescript, go, rust, python, java, kotlin, cpp, csharp, ruby, swift, lua, php, v2

## Enforcement Loop
- WHERE: All Claude Code projects — LSP adds value for any language
- WHEN: Setting up Claude Code on a new project; enabling code intelligence features
- HOW: Install the LSP plugin + corresponding server binary; verify with `which`; install Serena on top for cross-file analysis
- CONNECT: CCP-029 (serena uses LSP servers), CCP-006 (claude-code-setup recommends LSP based on codebase)
