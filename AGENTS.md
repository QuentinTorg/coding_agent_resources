# 🛑 FOR HUMANS: DO NOT COPY THIS FILE
*This file contains meta-instructions strictly for AI agents operating inside THIS repository. For templates to use in your own projects, see `/context_files/`.*

---

## Constraints & "Landmines"
- **Link Integrity:** Every file change requires an audit of links in `README.md` and related docs. New resources **must** be indexed in the `README.md` Resource Index.
- **Scannability:** In `README.md`, wrap snippets/details in HTML `<details><summary>` tags. The user should be able to quickly read through the README at a high level and choose what information they want to investigate further by expanding.
- **Agnosticism:** Keep prompts and templates generic. The defined "Big Three" are Gemini CLI, Claude Code, and Codex, so ensure compatibility across these agents, not just Gemini-specific.
- **Execution Safety:** Do not "test" example prompts or configs against this repository unless explicitly asked.
- **Backlog:** Consult `ideas.md` for roadmap and task suggestions.

## Core Intent
- **Refine, Don't Copy:** Do not copy-paste raw user shorthand. Reinterpret, refine, and elevate their thoughts into professional, structured documentation.
- **Propose Structure:** Propose new directory structures to the user before creating them.
