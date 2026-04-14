---
type: procedure
created: 2026-03-20
status: active
slug: ccs-016-obsidian-vault
tags: [procedure, nexus]
---

# CCS-016 — Obsidian Vault Management

## Problema
Obsidian vaults used as knowledge bases become disorganized when notes are created without consistent naming, linking, or index conventions. Without a flat structure with wikilinks and index notes, knowledge becomes siloed in folders that are hard to navigate and impossible to cross-reference. Without proper search workflows, existing notes are rediscovered too late (after being re-written) or not found at all.

## Procedura

### Prerequisites
- Obsidian vault accessible at the configured vault location (`/mnt/d/Obsidian Vault/AI Research/` by default — confirm with user)
- Filesystem access to vault directory (Bash/Glob/Grep tools or filesystem MCP)
- Note content ready (for creation workflow)

### Steps

1. **Confirm vault location.** Verify the vault path before any operations. The default configured location is `/mnt/d/Obsidian Vault/AI Research/`. Ask the user to confirm if working in a different vault or machine context. All subsequent file operations use this confirmed absolute path.

2. **Understand the vault structure conventions.** The vault is mostly flat at root level — no folders for organization. Organization is achieved through: wikilinks (`[[Note Title]]` syntax) connecting related notes; index notes that aggregate related topics (e.g., `Ralph Wiggum Index.md`, `Skills Index.md`); Title Case for all note names. Never create subfolders for organization — use links and index notes instead.

3. **Search by filename.** To find notes by name: use Glob with pattern `**/*.md` in the vault path and filter results, or use the filesystem search tool with a keyword. The flat structure means all notes are at root level, so direct filename searches are fast.

4. **Search by content.** To find notes containing specific content: use Grep on the vault directory with `--include="*.md"`. This is the primary way to find notes on a topic when the exact filename is unknown.

5. **Find backlinks (notes that reference a specific note).** To find all notes that link to a specific note, search for the wikilink pattern: grep for `\[\[Note Title\]\]` across the vault. Backlinks reveal which notes consider a given note as a dependency or related concept.

6. **Find index notes.** Index notes follow a naming pattern: `<Topic> Index.md`. To list all index notes, search filenames for the word "Index". Index notes are entry points into clusters of related knowledge.

7. **Create a new note — naming.** Use Title Case for the filename (e.g., `Transformer Architecture.md`, not `transformer-architecture.md` or `transformer architecture.md`). The filename IS the note's canonical name throughout the vault — it appears in all wikilinks referencing it.

8. **Create a new note — content.** Write content as a unit of learning: one concept per note, fully explained. At the bottom of the note, add `[[wikilinks]]` to related notes — dependencies (concepts this note builds on) and related notes (concepts this note connects to). Do not add links mid-text unless the note is explicitly referencing another note's content.

9. **Create a new note — numbered sequences.** If the note is part of a numbered learning sequence, use the hierarchical numbering scheme consistent with existing numbered notes in the vault. Check existing note filenames for the numbering convention before assigning a number.

10. **Update index notes after creating related notes.** When creating a note that belongs to an existing cluster, add a wikilink entry to the relevant index note. If no index note exists for the cluster and this is the third or more note on the same topic, create an index note.

11. **Never duplicate content between notes.** If two notes need to reference the same information, use a wikilink to the canonical note rather than copying text. Duplication creates maintenance burden and knowledge drift when one copy is updated without the other.

12. **Verify wikilinks point to real notes.** Before completing a note creation, verify that all `[[wikilinks]]` in the note correspond to actual files in the vault. A wikilink to a non-existent note is a dead end — either create the linked note or remove the link.

### Verification
- [ ] Vault location confirmed before any operations
- [ ] Note filename uses Title Case
- [ ] Note placed at vault root (no subfolders)
- [ ] Wikilinks added at bottom of note for dependencies and related notes
- [ ] Relevant index notes updated with new note entry
- [ ] All wikilinks verified to point to existing notes

## Cortex Logging
- collection: procedures
- tags: claude-code-skills, training, obsidian, knowledge-management, wikilinks, notes, v2

## Enforcement Loop
- WHERE: Obsidian vault at configured vault location
- WHEN: User says "find a note", "create a note in Obsidian", "add to the vault", or "organize notes"
- HOW: Always confirm vault location; enforce Title Case naming; enforce flat structure; maintain index notes
- CONNECT: CCS-015 (ubiquitous language — domain glossary can be linked into vault as an index note)
