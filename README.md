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

### 🧠 Core Philosophy: Agents Are Always Guessing
When you interact with a coding agent, remember one fundamental truth: **The agent is always guessing.** It lacks the years of context, the implicit team knowledge, and the whiteboard architecture sessions that you have experienced. Every decision it makes is a statistical guess based on the limited text in its immediate context window. 

Therefore, it is your job as the pilot to provide the necessary background, enforce strict guardrails, and minimize the amount of guessing the agent has to do. If you give an agent a massive, vague prompt without a structured workflow, it will guess your architecture and likely get it wrong. The key to success is forcing the agent into structured, step-by-step workflows.

### 🧠 Directives vs. Inquiries
A common beginner mistake is asking a question and getting frustrated when the agent unexpectedly modifies code to "fix" it. To master an agent, you must understand the difference between asking a question and giving an order:
*   **Inquiries:** Requests for analysis, explanation, or opinion (e.g., *"Why is this function crashing?"* or *"What do you think of this architecture?"*). An agent should *only* respond with text.
*   **Directives:** Unambiguous orders to execute a task (e.g., *"Fix the crash in this function,"* or *"Implement the architecture we just discussed."*).
*   **The Habit:** If your agent constantly jumps the gun, update your `user_level.AGENTS.md` (as shown in our template) to explicitly enforce this boundary, telling it to wait for a Directive before taking action.

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

To get the most out of your coding agent, you must understand how it perceives the world through **Context**. There are two distinct types of context you need to manage: **Working Context** (the agent's short-term memory during a session) and **Starting Context** (your configuration files).

<details>
<summary><strong>1. Working Context (Session Memory)</strong></summary>

The "Working Context" is the temporary memory the agent builds dynamically as you chat with it. Every file it reads, every command output it sees, and every message you type fills up its "context window."

**The Crucial Limitation:** 
The agent has a finite amount of context it can hold. As this window fills up, the agent becomes slower, more expensive to run, and most importantly, **it loses performance and "forgets" earlier details**. The model performs at its absolute best when it only holds the strictly relevant content in its context window.

#### Managing Your Working Context

To mitigate this limitation and maintain high performance, you must proactively manage your session context:
*   **Compressing/Compacting (`/compress` or `/compact`):** This command (available in many CLIs, including Gemini) tells the agent to summarize the older parts of the conversation, dropping massive file reads or lengthy command outputs, while keeping the high-level state of what you are working on. Use this after a long, messy debugging session where the agent read dozens of files or printed massive stack traces.
*   **Clearing (`/clear`):** This command completely wipes the session history, effectively giving the agent amnesia about everything you've done so far. Use this when you have successfully finished a task, made a commit, and are switching to an entirely new feature.

*Note: The exact commands and behavior vary by agent. Some tools (like GitHub Copilot/Codex) perform extremely efficient automatic compaction in the background. Tools with massive context windows (like Gemini) currently require more manual intervention from the user to purge unnecessary clutter and keep performance sharp.*

</details>

<details>
<summary><strong>2. Starting Context (Context Files)</strong></summary>

If you have to clear your session frequently to maintain performance, how do you prevent the agent from forgetting how your project works every time you run `/clear`? The answer is **Starting Context**. 

This is the permanent baseline knowledge you provide the agent the moment you start a new session. It is defined by markdown files—typically `AGENTS.md` (often referred to generically as a "Context File")—which act as the instruction manual for how the agent should behave.

**Why are Context Files so important?** 
Configuring your `AGENTS.md` files allows you to quickly start a new session without having to manually re-explain your repository's architecture or your coding preferences. It transforms the experience from constantly onboarding a new developer every time you open your terminal, into collaborating with an experienced colleague who already knows your stack. 

Crucially, **Context Files are the ultimate super-user optimization for the Working Context.** Because the agent already knows the build commands, the project structure, and the testing framework from reading the static file upon startup, it doesn't have to waste time (and precious context window tokens) aggressively searching your filesystem to figure out how your project works.

To maintain portability across different coding agents, **it is best practice to put your actual instructions in a generic `AGENTS.md` file.** You can then use agent-specific files (e.g., `GEMINI.md`) simply to point towards the generic `AGENTS.md` file.

This starting context is typically divided into two levels: **System-wide (User) Context** and **Workspace (Project) Context**.

#### System-Wide (User) Context

The user context defines your agent's persona, its general behavior, and how you prefer it to interact with you across *all* projects. This file is installed in your user home directory (e.g., `~/.gemini/` or `~/.config/`).

*   **What it does:** Sets global guardrails, coding style preferences, and interaction rules (like whether the agent should ask before making changes).
*   **Where to start:** See our example [`/context_files/user_level.AGENTS.md`](./context_files/user_level.AGENTS.md). This example is catered towards breaking bad habits (like rushing to code before planning) and sets up a robust, consultative workflow.
*   **Action:** Copy the example into your home directory, create a `GEMINI.md` file next to it that simply says "Read AGENTS.md", and modify the `AGENTS.md` to suit your personal working style. Remember that this is a **living document**—as your preferences evolve, update it!

#### Workspace (Project) Context

The workspace context is defined where you run your agent (usually the root directory of your git repository). This context tells the agent how to operate *specifically* within that project.

*   **What it does:** Provides critical information about the build system, debugging loop, architecture, testing frameworks, and commit expectations. See our example [`/context_files/workspace_level.AGENTS.md`](./context_files/workspace_level.AGENTS.md) for a manual template.
*   **Keep it Lean:** The workspace `AGENTS.md` file should **not** contain duplicates of any information that exists in your project's `README.md`. The `README.md` is for high-level information used to onboard a human developer. If information isn't in the README, consider whether it belongs in `AGENTS.md` to help the agent without bloating its context window.
*   **Why it matters:** Providing useful context about how the repo is structured, how to test code, and where features belong significantly speeds up the agent's ability to work effectively and keeps its context window focused. This ensures that the agent doesn't need to load a massive amount of context just to figure out where to start investigating a bug or adding a feature.
*   **Auto-Generation:** Gemini itself is incredibly good at generating this context when dropped into an existing repository! Instead of writing it by hand, you can use our example prompt located at [`/prompts/generate_agent_md_existing_codebase.md`](./prompts/generate_agent_md_existing_codebase.md) to have the agent analyze your repo and build its own context file.
*   **Best Practices:** 
    *   **Living Document:** Like the user context, this is a living document. Update it as your project architecture, dependencies, or conventions change over time.
    *   **Version Control:** Unless an `AGENTS.md` file is highly customized for a specific one-off task or personal to a single developer's machine environment, you should **commit it to the repository**. This allows all developers (and their agents) on the team to benefit from the shared agent context.

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

*   **[`/context_files/user_level.AGENTS.md`](./context_files/user_level.AGENTS.md)**: An example template for your system-wide user context. Use this to define your agent's core persona and operational rules.
*   **[`/context_files/workspace_level.AGENTS.md`](./context_files/workspace_level.AGENTS.md)**: An example template for your workspace/project context. Use this to teach an agent about a specific repository's build system and contribution guidelines.
*   **[`/context_files/react_best_practices.AGENTS.md`](./context_files/react_best_practices.AGENTS.md)**: A highly opinionated snippet to establish modern React/TypeScript best practices (Vite, Tailwind, Zustand, React Query).
*   **[`/context_files/cpp_best_practices.AGENTS.md`](./context_files/cpp_best_practices.AGENTS.md)**: A snippet enforcing modern C++17/20 standards (smart pointers, Rule of Zero, `<algorithm>`).
*   **[`/context_files/python_general_best_practices.AGENTS.md`](./context_files/python_general_best_practices.AGENTS.md)**: A snippet establishing strict rules for general Python development (type hinting, Ruff, path manipulation).

### Example Prompts
*   **[`/prompts/generate_agent_md_existing_codebase.md`](./prompts/generate_agent_md_existing_codebase.md)**: A prompt designed to be pasted into a coding agent to make it automatically analyze an existing codebase and generate a high-quality `AGENTS.md` context file for that specific project.
*   **[`/prompts/codebase_tour_guide.md`](./prompts/codebase_tour_guide.md)**: Transforms the agent into a senior engineer to map out and explain an unfamiliar subsystem or architecture without writing any code.
*   **[`/prompts/deep_bug_investigation.md`](./prompts/deep_bug_investigation.md)**: Forces the agent into a methodical "detective" mindset to perform root-cause analysis without guessing or immediately writing code.
*   **[`/prompts/devils_advocate_pr_review.md`](./prompts/devils_advocate_pr_review.md)**: Forces the agent to act as a highly skeptical reviewer tasked with breaking your code and finding edge cases, memory leaks, or race conditions before you open a PR.
*   **[`/prompts/plan_new_feature.md`](./prompts/plan_new_feature.md)**: Instructs the agent to act as a System Architect, requiring it to write and get approval on a design document before implementing a large feature.
*   **[`/prompts/generate_exhaustive_tests.md`](./prompts/generate_exhaustive_tests.md)**: Turns the agent into a rigorous QA engineer, forcing it to list all boundary conditions, edge cases, and failure states before writing test code.