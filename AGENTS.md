# 🛑 FOR HUMANS: DO NOT COPY THIS FILE
*If you are a human user looking for an `AGENTS.md` template to copy into your own project, **this is the wrong file**. Please look inside the `/context_files/` directory for usable templates. This file contains meta-instructions strictly for AI agents operating inside THIS specific repository (`coding_agent_resources`).*

---

# Agent Instructions: `coding_agent_resources` Repository

Welcome, AI Agent! You are currently operating within the `coding_agent_resources` repository. This file serves as your mechanical guide for how to behave and contribute to this specific project.

## Repository Nature & Architecture
- **Type:** This is a documentation and resource repository. There is no application code to compile, no build system, and no automated test suite. 
- **Focus:** Your primary actions here will involve writing, editing, and formatting Markdown files, as well as organizing directory structures.
- **Context:** Please read the `README.md` file to fully understand the high-level purpose of this repository, its target audience, and the resources provided here. 

## Contribution Guidelines

When assisting a user with modifying or adding to this repository, adhere strictly to the following rules:

1. **Link Maintenance (CRITICAL):** This repository acts as an index of resources. Every time a file is added, removed, or renamed, you MUST ensure that all links across the repository remain up-to-date.
   - Any newly added file must be appropriately categorized and linked in the `README.md` Resource Index.
   - Verify that no links within the body of the `README.md` or any other Markdown files are broken by your changes.
   - As the repository grows to include additional documentation files, ensure links stay synchronized across all of them.
2. **Centralized Documentation:** When adding new, digestible information (like short snippets or settings), add it directly to the `README.md` using HTML `<details>` and `<summary>` tags (collapsible sections). This keeps all resources conveniently in one place for the user without overwhelming them. Keep top-level information exposed so users know what each section contains before expanding it.
3. **Consult the Backlog:** Always read `ideas.md` when looking for future plans, backlog items, or suggestions for what to build next. It serves as our living roadmap.
4. **Template Quality:** When generating new templates (e.g., in `/context_files/` or `/prompts/`), ensure they are strictly formatted, professional, and do not contain conversational filler or hallucinated application code.
5. **Generalization:** While the primary focus is the Gemini CLI, keep concepts, prompts, and practices as generic as possible so they provide value to users of all AI coding agents (e.g., Claude Code, Copilot).
6. **No Direct Execution:** Do not "test" example prompts or configurations by executing them against this repository unless specifically asked to do so by the user to validate a workflow.
7. **Interpret and Improve:** The user often provides shorthand ideas or raw thoughts in their prompts. Unless explicitly told otherwise, do not just copy and paste their raw text into the repository. Your goal is to reinterpret, refine, and elevate their wording, converting their shorthand into organized, professional, and well-structured documentation. Actively offer improvements and help refine their ideas.

## Structure Maintenance
When adding new resources, organize them logically into the established directories:
- `/context_files/` - Example `AGENTS.md` templates for user and workspace levels.
- `/prompts/` - Useful prompt examples for users to paste into their agents.
- `/settings/` - Example JSON snippets and configuration guides (e.g., `.geminiignore`).

If a necessary directory structure doesn't exist yet for a new type of resource, propose the new structure to the user before creating it.
