# Coding Agent Resources

Welcome to the Coding Agent Resources repository! This project serves as a central hub for users to find helpful resources, templates, and best practices for setting up and collaborating with coding agents (like Gemini CLI, Claude Code, GitHub Copilot/Codex).

This repository aims to help you onboard coding agents into your development workflow effectively, ensuring they operate safely, follow good practices, and act as highly capable members of your team.

---

## Quick Start Guide

To get the most out of your coding agent, you need to configure its context properly. This context is fundamentally defined by markdown files—typically `AGENTS.md` and agent-specific files like `GEMINI.md`—which act as the initial starting point that guides your model's behavior. 

**Why is this so important?** 
Configuring these files well allows you to quickly dive in with a new agent without having to explain any background context. It transforms the experience from constantly onboarding a new senior developer every time you open your terminal, into collaborating with an experienced colleague who already knows you, knows your preferences, and has been consistently working in your workspace.

To maintain portability across different coding agents, **it is best practice to put your actual instructions in a generic `AGENTS.md` file.** You can then use the agent-specific file (e.g., `GEMINI.md`) simply to point towards the `AGENTS.md` file.

This context is divided into two levels: **System-wide (User) Context** and **Workspace (Project) Context**.

### 1. System-Wide (User) Context

The user context defines your agent's persona, its general behavior, and how you prefer it to interact with you across *all* projects. This file is installed in your user home directory (e.g., `~/.gemini/` or `~/.config/`).

*   **What it does:** Sets global guardrails, coding style preferences, and interaction rules (like whether the agent should ask before making changes).
*   **Where to start:** See our example [`/context_files/user_level.AGENTS.md`](./context_files/user_level.AGENTS.md). This example is catered towards breaking bad habits (like rushing to code before planning) and sets up a robust, consultative workflow.
*   **Action:** Copy the example into your home directory, create a `GEMINI.md` file next to it that simply says "Read AGENTS.md", and modify the `AGENTS.md` to suit your personal working style. Remember that this is a **living document**—as your preferences evolve, update it!

### 2. Workspace (Project) Context

The workspace context is defined where you run your agent (usually the root directory of your git repository). This context tells the agent how to operate *specifically* within that project.

*   **What it does:** Provides critical information about the build system, debugging loop, architecture, testing frameworks, and commit expectations. See our example [`/context_files/workspace_level.AGENTS.md`](./context_files/workspace_level.AGENTS.md) for a manual template.
*   **Keep it Lean:** The workspace `AGENTS.md` file should **not** contain duplicates of any information that exists in your project's `README.md`. The `README.md` is for high-level information used to onboard a human developer. If information isn't in the README, consider whether it belongs in `AGENTS.md` to help the agent without bloating its context window.
*   **Why it matters:** Providing useful context about how the repo is structured, how to test code, and where features belong significantly speeds up the agent's ability to work effectively and keeps its context window focused. This ensures that the agent doesn't need to load a massive amount of context just to figure out where to start investigating a bug or adding a feature.
*   **Auto-Generation:** Gemini itself is incredibly good at generating this context when dropped into an existing repository! Instead of writing it by hand, you can use our example prompt located at [`/prompts/generate_agent_md_existing_codebase.md`](./prompts/generate_agent_md_existing_codebase.md) to have the agent analyze your repo and build its own context file.
*   **Best Practices:** 
    *   **Living Document:** Like the user context, this is a living document. Update it as your project architecture, dependencies, or conventions change over time.
    *   **Version Control:** Unless an `AGENTS.md` file is highly customized for a specific one-off task or personal to a single developer's machine environment, you should **commit it to the repository**. This allows all developers (and their agents) on the team to benefit from the shared agent context.

### General Tips
*   **Model Selection:** Whenever possible, prefer using larger, more capable models. They save time and reduce "churn" because they are smarter and make fewer mistakes.
*   **Permissions:** You can often set up settings to permanently allow some trusted, non-destructive commands (like `git status`, `ls`, `grep`, `cat`) so the agent doesn't have to pause and ask for permission constantly.

---

## Day-to-Day Workflows for Beginners

Once your context is set up, you need to know how to effectively "steer" the agent. If you give an agent a massive, vague prompt, it will likely write spaghetti code or get lost in a loop. The key is to force the agent into structured workflows.

### 1. Starting a New Feature (The Planning Phase)
Never ask an agent to "build a new authentication system" and immediately let it write code. It will guess your architecture and likely get it wrong. 
*   **The Workflow:** Force the agent to act as a System Architect first. Ask it to read the codebase, find where similar features live, and write a Markdown design document detailing the files it plans to create and the functions it will write.
*   **The Prompt:** Use our **[`plan_new_feature.md`](./prompts/plan_new_feature.md)** power prompt. Do not let the agent write code until you approve the plan.

### 2. The Iterative Build Loop (The Execution Phase)
Once a feature is planned, do not ask the agent to implement the entire design document in one go. The context window will overflow, and mistakes will compound.
*   **The Workflow:** Break the plan down. Ask the agent to implement *one* specific file or component at a time.
*   **The Loop:** 
    1. Prompt the agent to build Component A.
    2. Prompt the agent to write the unit tests for Component A.
    3. Run the tests. If they fail, paste the error back to the agent to fix.
    4. Once tests pass, explicitly instruct the agent: *"Great, make a surgical commit for Component A."*
    5. Move on to Component B.

### 3. Debugging (The Investigation Phase)
When an agent sees a stack trace, its instinct is to immediately write a code fix based on a guess. This often breaks things further.
*   **The Workflow:** Force the agent to be a detective. Tell it to read the stack trace, use `grep` to find the relevant files, and explain the execution path to you in plain English. Forbid it from writing a fix until it proves the bug by writing a failing test script.
*   **The Prompt:** Use our **[`deep_bug_investigation.md`](./prompts/deep_bug_investigation.md)** power prompt to enforce this behavior.

### 4. Code Review & GitHub Integration
If you have the GitHub CLI (`gh`) installed and authenticated, your agent can act as a fully automated code reviewer and PR author.

<details>
<summary><strong>Snippet: Draft a Pull Request</strong></summary>

*Use this prompt when you have finished your local feature branch and are ready to open a PR. The agent will read your diff and use the GitHub CLI to create the PR.*

```text
Please read my uncommitted changes and my recent git history for this branch. 
Draft a comprehensive Pull Request title and description explaining the "why" and "what" of these changes. 
Once I approve the text, use the `gh pr create` command to open the pull request.
```
</details>

<details>
<summary><strong>Snippet: Review an Open PR</strong></summary>

*Use this prompt to have the agent read a teammate's PR and leave inline comments.*

```text
Please use the GitHub CLI to view PR #[INSERT_PR_NUMBER]. 
Read the diff and review the code for logic errors, edge cases, or architectural issues. 
Do not comment on stylistic nitpicks. 
If you find issues, use the `gh api` to post inline code review comments. Prefix all your comments with "🤖 *Agent:*" so my team knows this is an automated review.
```
</details>

---

## Agent Settings

Different agents and models have specific configurations. Click to expand the settings for the model you are using.

<details>
<summary><strong>Gemini CLI</strong></summary>

### Gemini CLI Settings
*   **Authentication:** Ensure you have set up your API keys and authenticated correctly via the CLI (`gemini auth`).
*   **Configuration Files:** Gemini CLI looks for `GEMINI.md` files. You can point your `GEMINI.md` to read an `AGENTS.md` file to keep instructions cross-compatible with other agents.

<details>
<summary><strong>Snippet: Auto-Approve Safe Commands</strong></summary>

*Add the following to your `~/.gemini/config.json` to permanently allow the agent to run safe, read-only commands without pausing for your permission. This dramatically speeds up the agent's research phase.*

```json
{
  "tools": {
    "run_shell_command": {
      "autoApprove": [
        "git status",
        "git diff",
        "git log",
        "git show",
        "git branch",
        "cd",
        "pwd",
        "cat",
        "ls",
        "head",
        "tail",
        "grep",
        "rg",
        "find",
        "wc",
        "sleep",
        "timeout",
        "tree",
        "echo",
        "which",
        "jq"
      ]
    }
  }
}
```
</details>

<details>
<summary><strong>Snippet: Security & System Ignores (.geminiignore)</strong></summary>

*Create a `.geminiignore` file in your home directory (`~/.geminiignore`) or your project root to prevent the agent from reading sensitive files or wasting tokens on massive directories.*

```text
# Security & Credentials
.env
.env.*
.ssh/
*.pem
*.key
credentials.json

# Large/Unnecessary Directories
node_modules/
__pycache__/
venv/
dist/
build/
.git/
```
</details>

</details>

<details>
<summary><strong>Claude Code</strong></summary>

### Claude Code Settings
*(Details and snippets for configuring Claude Code will be added here)*

</details>

<details>
<summary><strong>Codex / Copilot</strong></summary>

### Codex / Copilot Settings
*(Details and snippets for configuring Codex and Copilot will be added here)*

</details>

---

## Resource Index

Below is an index of the resources available in this repository. These are samples and templates you can use to configure your own agents.

### Example Agent Contexts

> **💡 Note on Customization:** All of the templates and cheatsheets below are meant to be starting points. You are highly encouraged to modify them—or better yet, prompt your coding agent to rewrite them—so that the stated opinions and rules perfectly match your preferred tech stack and personal workflow!

*   **[`/context_files/user_level.AGENTS.md`](./context_files/user_level.AGENTS.md)**: An example template for your system-wide user context. Use this to define your agent's core persona and operational rules.
*   **[`/context_files/workspace_level.AGENTS.md`](./context_files/workspace_level.AGENTS.md)**: An example template for your workspace/project context. Use this to teach an agent about a specific repository's build system and contribution guidelines.
*   **[`/context_files/react_best_practices.AGENTS.md`](./context_files/react_best_practices.AGENTS.md)**: A highly opinionated snippet to establish modern React/TypeScript best practices (Vite, Tailwind, Zustand, React Query).
*   **[`/context_files/python_fastapi.AGENTS.md`](./context_files/python_fastapi.AGENTS.md)**: A highly opinionated snippet to establish scalable FastAPI best practices (Pydantic V2, SQLAlchemy 2.0 Async, Ruff).

### Example Prompts
*   **[`/prompts/generate_agent_md_existing_codebase.md`](./prompts/generate_agent_md_existing_codebase.md)**: A prompt designed to be pasted into a coding agent to make it automatically analyze an existing codebase and generate a high-quality `AGENTS.md` context file for that specific project.
*   **[`/prompts/deep_bug_investigation.md`](./prompts/deep_bug_investigation.md)**: Forces the agent into a methodical "detective" mindset to perform root-cause analysis without guessing or immediately writing code.
*   **[`/prompts/plan_new_feature.md`](./prompts/plan_new_feature.md)**: Instructs the agent to act as a System Architect, requiring it to write and get approval on a design document before implementing a large feature.
*   **[`/prompts/generate_exhaustive_tests.md`](./prompts/generate_exhaustive_tests.md)**: Turns the agent into a rigorous QA engineer, forcing it to list all boundary conditions, edge cases, and failure states before writing test code.
