# Prompt: Review an Existing AGENTS.md File

*Use this prompt with your coding agent (like Gemini CLI) when you have an existing `AGENTS.md` (or `CLAUDE.md`, etc.) file in your repository. This will instruct the agent to analyze the file and prune it based on the latest research for minimal, high-signal instructions.*

---

**Copy and paste the following prompt to your coding agent:**

```text
Please thoroughly analyze the existing `AGENTS.md` (or equivalent context file) in this repository against the latest best practices for AI coding agent context files.

Your goal is to transform the existing file into a "hyper-minimal", "landmines-only" document by identifying and removing any low-signal information.

Please evaluate the file against these strict pruning criteria:

1. **Delete the Obvious:** Identify information that an agent could easily infer by reading the codebase or standard documentation (like the `README.md`). Flag these sections for immediate deletion.
2. **Remove Aspirational Rules:** Identify theoretical best practices or "clean code" rules that are not strictly enforced in the codebase today. Flag these for removal to ensure the agent anchors to reality, not aspirations.
3. **Identify Standard Commands:** Identify standard CLI commands (like `npm run dev` for a Next.js app). These must be removed. Only custom or non-standard commands that an agent would frequently get wrong should be included.
4. **Enforce Brevity:** The resulting file should be as short as possible. If a sentence can be shortened without losing its core instruction, flag it for pruning.

Please present a critique of the current file, explicitly listing exactly what should be removed and the technical justification for its removal. Then, provide a proposed, heavily pruned version of the file for me to review.

**Final Reminder:** Remember to keep any existing high-signal "gotchas", non-obvious quirks, or hard architectural constraints. If you have recently made a mistake in this repository because of a unique quirk, please remind me to add it to the file!
```