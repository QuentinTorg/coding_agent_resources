# Coding Agent Resources

The goal of this repository is to provide a comprehensive getting started guide and background information to help you quickly become proficient with AI coding agents (like Gemini CLI, Claude Code, GitHub Copilot/Codex).

It acts as both an onboarding guide and a resource library containing templates, prompts, and best practices. These resources are designed to help you safely integrate an agent into your development workflow and immediately turn it into a productive, highly capable member of your engineering team.

---

## Introduction: What is an AI Coding Agent?

An AI coding agent is an advanced AI model integrated directly into your development environment (like your terminal or IDE). Unlike a standard chatbot that only answers questions, an agent can read your files, execute terminal commands, run tests, and autonomously write or modify code. Think of it as a highly capable but context-blind engineer—it has vast programming knowledge, but it requires your guidance, guardrails, and project-specific knowledge to succeed.

You can use agents to:
*   **Investigate bugs** by having them trace data flows and run reproduction scripts.
*   **Plan features** by asking them to draft architectural design documents before writing code.
*   **Execute surgical refactors** across multiple files.
*   **Write exhaustive tests** for complex modules.

---

## Core Fundamentals (Before You Start)

<details>
<summary><strong>Read the Core Fundamentals</strong></summary>

If you are a senior engineer but a beginner to AI agents, your instincts might betray you. You cannot interact with an agent the same way you interact with a human colleague or a simple Google search. Before diving into the technical setup, you must adopt the right mindset:

1. **You are the Pilot:** The agent is a junior developer with infinite typing speed but zero architectural intuition. You must dictate the architecture, the workflow, and the boundaries. If you let the agent drive, it will write spaghetti code.
2. **Break Tasks Down (Atomic Steps):** Do not give the agent a massive prompt like *"Build the new perception pipeline."* Instead, break it down: *"Draft a design doc for the perception pipeline,"* followed by *"Implement the data ingestion node,"* followed by *"Write unit tests for the ingestion node."* Small, verifiable steps prevent compounding errors.
3. **Trust, but Verify:** Agents will confidently hallucinate nonexistent APIs or subtly break logic. Never blindly merge agent-written code. Always require the agent to write tests, or manually run your build/test suite after it finishes a task.
4. **Leverage Its Tools:** The agent is not just a text generator. It has access to your terminal. Instead of pasting code into the chat, tell the agent: *"Use `grep` to find where the `User` class is defined and explain its dependencies."* Let it do the legwork.
5. **Agents Are Always Guessing:** The agent lacks the implicit team knowledge and years of context you have. Every decision is a statistical guess based on its immediate context window. It is your job to minimize the guessing by providing necessary background, enforcing strict guardrails, and forcing structured workflows.
6. **Directives vs. Inquiries:** Understand the difference between asking a question and giving an order. An **Inquiry** (e.g., *"Why is this function crashing?"*) is a request for text analysis. A **Directive** (e.g., *"Fix the crash"*) is an order to execute a task. Enforce this boundary in your `user_level.AGENTS.md` so the agent waits for a Directive before taking action.
</details>

---

## General Tips for Beginners

<details>
<summary><strong>General Tips & Best Practices</strong></summary>

*   **Adopt the "Pair Programmer" Mindset:** Treat the agent as a fast-typing junior developer. You remain the Senior Code Reviewer.
*   **The "Explain It Back" Rule:** Never accept code you cannot understand. Ask the agent to explain complex functions line-by-line before integrating them.
*   **Avoid "Vibe Coding":** Don't blindly accept code just because it looks correct at a glance. Always verify and run tests to catch subtle logic errors or API hallucinations.
*   **Provide Examples (Few-Shot Prompting):** Agents perform significantly better when given a template or an existing code snippet to match your project's established style.
*   **Model Selection:** Whenever possible, prefer using larger, more capable models for complex tasks. They save time and reduce "churn" because they are smarter and make fewer mistakes.
*   **Permissions & Safe Commands:** Configure tools to permanently allow safe, read-only commands (like `git status`, `ls`, `grep`) to speed up the agent's research phase so it doesn't have to pause and ask for permission constantly.

</details>

---

## The Concept of "Context"

To get the most out of your coding agent, you must understand how it perceives the world through **Context**. Setting up and managing context effectively is a critical skill for maintaining an agent's performance and accuracy. There are two distinct types of context you need to manage: **Working Context** (the agent's short-term memory during a session) and **Starting Context** (your configuration files).

<details>
<summary><strong>1. Working Context (Session Memory)</strong></summary>

The "Working Context" is the temporary memory the agent builds dynamically as you chat with it. Every file it reads, every command output it sees, and every message you type fills up its "context window."

**The Crucial Limitation:**
The agent has a finite amount of context it can hold. As this window fills up, the agent becomes slower, more expensive to run, and most importantly, **it loses performance and "forgets" earlier details**. The model performs at its absolute best when it only holds the strictly relevant content in its context window.

#### Managing Your Working Context

To mitigate this limitation and maintain high performance, you must proactively manage your session context:

**1. Prevent Premature Filling (The First Line of Defense)**
Before you even think about clearing your context, you should try to protect it from filling up with irrelevant information:
*   **Stay On Topic:** Do not ask the agent to perform tasks or answer questions that are not relevant to the current objective. Keep sessions focused on a single feature or bug.
*   **Guardrail Exploration:** Protect the model from exploring massive, irrelevant directories or files. Use tools like `.geminiignore` (or equivalent) to hide compiled assets, dependency folders, and other noise.
*   **Beware of High-Output Tools:** Be cautious when asking the agent to run commands that produce extremely long terminal outputs (like `tree` on a large directory, or printing the entirety of a massive log file). Suggest it uses `grep`, `head`/`tail`, or pagination instead.

**2. Prune the Context**
Once the context inevitably starts to fill up, you have two primary commands to clean it:
*   **Compressing/Compacting (`/compress` or `/compact`):** This command (available in many CLIs, including Gemini) tells the agent to summarize the older parts of the conversation, dropping massive file reads or lengthy command outputs, while keeping the high-level state of what you are working on. Use this after a long, messy debugging session where the agent read dozens of files or printed massive stack traces.
*   **Clearing (`/clear`):** This command completely wipes the session history, effectively giving the agent amnesia about everything you've done so far. Use this when you have successfully finished a task, made a commit, and are switching to an entirely new feature.

*Note: The exact commands and behavior vary by agent. Some tools (like GitHub Copilot/Codex) perform extremely efficient automatic compaction in the background. Tools with massive context windows (like Gemini) currently require more manual intervention from the user to purge unnecessary clutter and keep performance sharp.*

</details>

<details>
<summary><strong>2. Starting Context (Context Files)</strong></summary>

If you have to clear your session frequently to maintain performance, how do you prevent the agent from forgetting how your project works every time you run `/clear`? The answer is **Starting Context**.

Starting Context is the permanent baseline knowledge you provide the agent when you start a new session. It is defined by markdown files—typically named `AGENTS.md`—which act as the instruction manual for how the agent should behave.

This is the ultimate optimization for the Working Context. By reading these files upon startup, the agent immediately knows your build commands, project structure, and testing framework. This prevents it from wasting precious context window tokens repeatedly searching your filesystem, and prevents it from forgetting how your project works every time you clear your session.

*Tip: For portability across different agents, put your actual instructions in a generic `AGENTS.md` file, and use agent-specific files (e.g., `GEMINI.md`) simply to point towards it.*

This context is divided into two levels: **System-wide (User) Context** and **Workspace (Project) Context**. Both should be treated as **living documents** and updated as your preferences and architectures evolve.

#### System-Wide (User) Context
Installed in your user home directory (e.g., `~/.gemini/` or `~/.config/`), this file defines your agent's persona and general behavior across *all* projects.

*   **Purpose:** Sets global guardrails, coding style preferences, and interaction rules (like whether to ask before making changes).
*   **Action:** Copy our example [User Level Context File](./context_files/user_level.AGENTS.md) into your home directory, create a `GEMINI.md` file next to it that says "Read AGENTS.md", and modify it to suit your workflow.

#### Workspace (Project) Context
Located in the root directory of your repository, this tells the agent how to operate *specifically* within that project. Unless highly customized for local use, it should be **committed to version control** so your entire team benefits from the shared agent context.

*   **Purpose:** Provides critical, project-specific information (build systems, debugging loops, architecture, and testing frameworks).
*   **Keep it Lean:** Do not duplicate information from your `README.md`. The README is for humans; `AGENTS.md` is strictly for the agent.
*   **Auto-Generation:** Instead of writing it by hand, use our [Generate Context for Existing Codebase](./prompts/generate_agent_md_existing_codebase.md) prompt to have the agent analyze your repo and build its own context file automatically.

</details>

---

## Day-to-Day Workflows for Beginners

<details>
<summary><strong>1. Starting a New Feature (The Planning Phase)</strong></summary>

Never ask an agent to "build a new obstacle avoidance node" and immediately let it write code. It will guess your architecture and likely get it wrong.
*   **The Workflow:** Force the agent to act as a System Architect first. Ask it to read the codebase, find where similar features live, and write a Markdown design document detailing the files it plans to create and the functions it will write.
*   **The Prompt:**
    *   Use our **[`plan_new_feature.md`](./prompts/plan_new_feature.md)** power prompt. Do not let the agent write code until you approve the plan.

</details>

<details>
<summary><strong>2. The Iterative Build Loop (The Execution Phase)</strong></summary>

Once a feature is planned, do not ask the agent to implement the entire design document in one go. The context window will overflow, and mistakes will compound.
*   **The Workflow:** Break the plan down. Ask the agent to implement *one* specific file or component at a time.
*   **The Loop:**
    1. Prompt the agent to build Component A.
    2. Prompt the agent to write the unit tests for Component A.
    3. Run the tests. If they fail, paste the error back to the agent to fix.
    4. Once tests pass, explicitly instruct the agent: *"Great, make a surgical commit for Component A."*
    5. Move on to Component B.

</details>

<details>
<summary><strong>3. Debugging (The Investigation Phase)</strong></summary>

When an agent sees a stack trace, its instinct is to immediately write a code fix based on a guess. This often breaks things further.
*   **The Workflow:** Force the agent to be a detective. Tell it to read the stack trace, use `grep` to find the relevant files, and explain the execution path to you in plain English. Forbid it from writing a fix until it proves the bug by writing a failing test script.
*   **The Prompt:**
    *   Use our **[`deep_bug_investigation.md`](./prompts/deep_bug_investigation.md)** power prompt to enforce this behavior.

</details>

<details>
<summary><strong>4. Codebase Onboarding (The "Tour Guide" Workflow)</strong></summary>

Agents are not just for writing code; they are incredible tools for rapidly onboarding yourself onto a massive, unfamiliar codebase. Instead of spending hours tracing function calls manually, you can ask the agent to act as a senior developer giving you a tour.
*   **The Workflow:** Forbid the agent from modifying code, and ask it to trace a specific data flow or explain a complex module step-by-step.
*   **The Prompts:**
    *   **For a deep dive:** Use our **[`codebase_tour_guide.md`](./prompts/codebase_tour_guide.md)** power prompt.
    *   **For a quick inline question:** Use something like: *"I am new to this codebase. Please read the `src/navigation/` directory and explain to me step-by-step how the sensor telemetry data is ingested and filtered in the state estimator. Do not write or modify any code."*

</details>

<details>
<summary><strong>5. Targeted Refactoring</strong></summary>

If you give an agent a vague prompt like *"Clean up this file and make it better,"* it will likely rewrite the entire file, change your formatting, and potentially break subtle edge cases.
*   **The Workflow:** Give specific, architectural directives rather than vague cleanup requests.
*   **The Prompts:**
    *   *"Please extract all the direct database queries in `user.py` into a separate `user_repository.py` file."*
    *   *"Convert this C++ class to use smart pointers (`std::shared_ptr` and `std::unique_ptr`) instead of raw pointers to prevent memory leaks."*

</details>

<details>
<summary><strong>6. Code Review & GitHub Integration</strong></summary>

If you have the GitHub CLI (`gh`) installed and authenticated, your agent can act as a fully automated code reviewer and PR author.

*Use this prompt when you have finished your local feature branch and are ready to open a PR. The agent will read your diff and use the GitHub CLI to create the PR.*

```text
Please read my uncommitted changes and my recent git history for this branch.
Draft a comprehensive Pull Request title and description explaining the "why" and "what" of these changes.
Once I approve the text, use the `gh pr create` command to open the pull request.
```

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

*   **[User Level Context File](./context_files/user_level.AGENTS.md)**: An example template for your system-wide user context. Use this to define your agent's core persona and operational rules.
*   **[Workspace Level Context File](./context_files/workspace_level.AGENTS.md)**: An example template for your workspace/project context. Use this to teach an agent about a specific repository's build system and contribution guidelines.
*   **[React Best Practices](./context_files/react_best_practices.AGENTS.md)**: A highly opinionated snippet to establish modern React/TypeScript best practices (Vite, Tailwind, Zustand, React Query).
*   **[C++ Best Practices](./context_files/cpp_best_practices.AGENTS.md)**: A snippet enforcing modern C++17/20 standards (smart pointers, Rule of Zero, `<algorithm>`).
*   **[Python Best Practices](./context_files/python_general_best_practices.AGENTS.md)**: A snippet establishing strict rules for general Python development (type hinting, Ruff, path manipulation).

### Example Prompts
*   **[Generate Context for Existing Codebase](./prompts/generate_agent_md_existing_codebase.md)**: A prompt designed to be pasted into a coding agent to make it automatically analyze an existing codebase and generate a high-quality `AGENTS.md` context file for that specific project.
*   **[Codebase Tour Guide](./prompts/codebase_tour_guide.md)**: Transforms the agent into a senior engineer to map out and explain an unfamiliar subsystem or architecture without writing any code.
*   **[Deep Bug Investigation](./prompts/deep_bug_investigation.md)**: Forces the agent into a methodical "detective" mindset to perform root-cause analysis without guessing or immediately writing code.
*   **[Devil's Advocate PR Review](./prompts/devils_advocate_pr_review.md)**: Forces the agent to act as a highly skeptical reviewer tasked with breaking your code and finding edge cases, memory leaks, or race conditions before you open a PR.
*   **[Plan New Feature](./prompts/plan_new_feature.md)**: Instructs the agent to act as a System Architect, requiring it to write and get approval on a design document before implementing a large feature.
*   **[Generate Exhaustive Tests](./prompts/generate_exhaustive_tests.md)**: Turns the agent into a rigorous QA engineer, forcing it to list all boundary conditions, edge cases, and failure states before writing test code.
