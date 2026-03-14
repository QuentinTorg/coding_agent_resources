# Prompt: Generate AGENTS.md for an Existing Codebase

*Use this prompt with your coding agent (like Gemini CLI) when you open it in an existing, established repository for the first time. This will instruct the agent to analyze the repository and generate the optimal context files so that it, and any future agents, can work efficiently.*

---

**Copy and paste the following prompt to your coding agent:**

```text
Please thoroughly analyze this repository and generate an `AGENTS.md` file, as well as a `GEMINI.md` file that points to it. 

Your goal is to extract and document the critical, mechanical context required for an AI coding agent to operate effectively within this codebase. 

Please adhere strictly to the following best practices for the `AGENTS.md` file:
1. **No Redundancy:** First, please read the existing `README.md` file in this repository. Ensure that the `AGENTS.md` you generate does NOT duplicate any high-level, human-centric onboarding information that already exists there. Instead, explicitly instruct all future agents to read the `README.md` file upon entering the repository.
2. **Project Structure:** Provide a brief architectural map. Where do different types of files live (e.g., components, tests, API wrappers, state management)? This prevents the agent from having to aggressively search the filesystem.
3. **Execution Commands:** Discover and clearly list the exact terminal commands required to:
   - Install dependencies.
   - Build the project.
   - Run the local development server.
   - Run the linting and formatting tools.
4. **Testing Guidelines:** Identify the testing framework used. Explain the exact commands to run tests, and outline any established patterns (e.g., "always mock external APIs using X", "require X% coverage", "snapshot tests are discouraged").
5. **Coding Conventions:** Identify strict codebase rules. For example: Do we use absolute or relative imports? How is error handling managed? Are there strict typing rules (e.g., "never use `any` in TypeScript")?
6. **Version Control:** Note any existing commit message conventions (e.g., Conventional Commits) and PR template requirements.

Once you have analyzed the repo and drafted the `AGENTS.md` content, please also create a `GEMINI.md` file in the root directory. The `GEMINI.md` file should simply contain the following to ensure portability across different tools:

# Gemini CLI Configuration
Please read `AGENTS.md` to understand how to operate within and contribute to this repository.

Please present your proposed `AGENTS.md` content to me for review before writing the files to the disk.
```
