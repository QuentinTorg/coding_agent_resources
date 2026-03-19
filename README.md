# Coding Agent Resources

The goal of this repository is to provide a quick reference and onboarding guide to help you quickly become proficient with AI coding agents.

## Introduction: What is an AI Coding Agent?

An AI coding agent is an advanced AI model integrated directly into your development environment (like your terminal or IDE). Unlike a standard chatbot that only answers questions, an agent can read your files, execute terminal commands, run tests, and autonomously write or modify code.

While this guide focuses primarily on **Gemini CLI**, the principles and best practices apply to other powerful agents like **Claude Code** and **Codex**. We use cross-compatible conventions, such as `AGENTS.md` for context files, to ensure these resources are useful regardless of your specific tool.

You can use agents to:
*   **Investigate bugs** by having them trace data flows and run reproduction scripts.
*   **Plan features** by asking them to draft architectural design documents before writing code.
*   **Execute surgical refactors** across multiple files.
*   **Write exhaustive tests** for complex modules.

---

## Core Fundamentals (Before You Start)

If you are a senior engineer but a beginner to AI agents, your instincts might betray you. You cannot interact with an agent the same way you interact with a human colleague or a simple Google search.

<details>
<summary><strong>Read the Core Fundamentals</strong></summary>

1. **You are the Pilot:** The agent is a junior developer with infinite typing speed but zero architectural intuition. You must dictate the architecture, the workflow, and the boundaries. If you let the agent drive, it will write spaghetti code.
2. **Break Tasks Down (Atomic Steps):** Do not give the agent a massive prompt like *"Build the new perception pipeline."* Instead, break it down: *"Draft a design doc for the perception pipeline,"* followed by *"Implement the data ingestion node,"* followed by *"Write unit tests for the ingestion node."* Small, verifiable steps prevent compounding errors.
3. **Trust, but Verify:** Agents will confidently hallucinate nonexistent APIs or subtly break logic. Never blindly merge agent-written code. Always require the agent to write tests, or manually run your build/test suite after it finishes a task.
4. **Leverage Its Tools:** The agent is not just a text generator. It has access to your terminal. Instead of pasting code into the chat, tell the agent: *"Find where the `User` class is defined and explain its dependencies."*, and it will automatically use grep to find it. Let it do the legwork.
5. **Agents Are Always Guessing:** The agent lacks the implicit team knowledge and years of context you have. Every decision is a statistical guess based on its immediate context window. It is your job to minimize the guessing by providing necessary background, enforcing strict guardrails, and forcing structured workflows.
6. **Directives vs. Inquiries:** Understand the difference between asking a question and giving an order. An **Inquiry** (e.g., *"Why is this function crashing?"*) is a request for text analysis. A **Directive** (e.g., *"Fix the crash"*) is an order to execute a task. Enforce this boundary in your `user_level.AGENTS.md` so the agent waits for a Directive before taking action.
</details>

---

## Getting Started & Settings

To get the most out of your agent, you need to configure it correctly for your environment. Settings can typically be applied at both the user and workspace level.

<details>
<summary><strong>Settings & Configuration</strong></summary>

*   **User vs Workspace Settings:** Most agents allow you to define settings globally (in your home directory, e.g., `~/.gemini/config.json`) or locally per-workspace. Workspace settings generally override user settings, allowing you to tailor behavior for specific projects.
*   **Trusting Directories:** Agents like Gemini CLI operate within a security boundary. You may need to explicitly "trust" a directory before the agent can execute commands or modify files within it.
*   **Command Policies:** To protect your system, configure command policies. You can permanently allow safe, read-only commands (like `git status`, `ls`, `grep`) so the agent doesn't pause to ask for permission during its research phase. Destructive commands (like `rm` or `git push`) should always require explicit confirmation.
*   **Recommended Settings:** We recommend starting with strict command policies and a robust `.geminiignore` (or equivalent) to hide compiled assets, dependency folders (`node_modules/`, `venv/`), and sensitive credentials (`.env`, `.ssh/`).
</details>

---

## General Tips for Beginners

Working effectively with AI agents requires a shift in how you communicate and review code.

<details>
<summary><strong>General Tips & Best Practices</strong></summary>

*   **Adopt the "Pair Programmer" Mindset:** Treat the agent as a fast-typing junior developer. You remain the Senior Code Reviewer.
*   **The "Pink Elephant" Problem (Speak in the Affirmative):** If you tell someone "Don't think of a pink elephant," they immediately think of one. LLMs work similarly. Instead of saying "Do not use `var`", say "Always use `const` or `let`". Provide positive instructions and clear examples of what you *want*, rather than a long list of what you *don't want*.
*   **The "Explain It Back" Rule:** Never accept code you cannot understand. Ask the agent to explain complex functions line-by-line before integrating them.
*   **Avoid "Vibe Coding":** Don't blindly accept code just because it looks correct at a glance. Always verify and run tests to catch subtle logic errors or API hallucinations.
*   **Provide Examples (Few-Shot Prompting):** Agents perform significantly better when given a template or an existing code snippet to match your project's established style.
*   **Model Selection:** Whenever possible, prefer using larger, more capable models for complex tasks. They save time and reduce "churn" because they are smarter and make fewer mistakes.
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
*   **Beware of High-Output Tools:** Be cautious when asking the agent to run commands that produce extremely long terminal outputs. Suggest it uses `grep`, `head`/`tail`, or pagination instead.

**2. Prune the Context**
Once the context inevitably starts to fill up, you have two primary commands to clean it:
*   **Compressing/Compacting:** This command tells the agent to summarize the older parts of the conversation, dropping massive file reads or lengthy command outputs, while keeping the high-level state of what you are working on.
*   **Clearing:** This command completely wipes the session history, effectively giving the agent amnesia about everything you've done so far. Use this when switching to an entirely new feature.
</details>

<details>
<summary><strong>2. Starting Context (Context Files)</strong></summary>

If you have to clear your session frequently to maintain performance, how do you prevent the agent from forgetting how your project works every time? The answer is **Starting Context**.

Starting Context is the permanent baseline knowledge you provide the agent when you start a new session. It is defined by markdown files—typically named `AGENTS.md` (or `CLAUDE.md`, `.cursorrules`, etc.)—which act as the "landmine detector" for the agent.

**The Golden Rule:**
A [recent study by ETH Zurich](https://arxiv.org/pdf/2602.11988) found that **standard, auto-generated context files actually make coding agents worse.** Human-curated files are the only ones that boost performance, and *only* when they are concise.

#### Why Auto-Generated Files Fail:
*   **Attention Dilution:** Large, verbose files distract the agent with irrelevant information.
*   **Inference Costs:** They increase token costs while reducing task success rates.
*   **Reality Gap:** Agents "anchor" to aspirational rules that don't match the reality of the existing code.
*   **Maintenance Burden:** Auto-generated files frequently include too much information, becoming quickly outdated.

To be effective, your context file must be **human-curated and hyper-minimal**. It is NOT an onboarding manual for the project—that is what your `README.md` is for. Instead, it is a list of "gotchas" that prevent the agent from making mistakes due to unique aspects of your specific codebase.

#### Best Practices for Your Context File:
*   **The "Landmines-Only" Method:** Only include non-obvious quirks of your codebase, strict architectural constraints, or specific CLI commands the agent frequently messes up.
*   **Focus on the Invisible:** If the information can be inferred by reading the code or the `README.md`, **leave it out**. Only document what the code *cannot* tell the agent.
*   **Avoid Aspirational Rules:** Do not document "theoretical best practices" that aren't actually enforced.
*   **Institutional Memory:** Use the file to capture "lessons learned."

#### What NOT to include (Delete these!):
*   **Standard CLI Commands:** Don't include `npm install` or `npm run dev` if they do exactly what you'd expect.
*   **High-Level Architecture:** If your folder structure is standard, the agent will find it.
*   **Boilerplate Documentation:** Avoid copy-pasting your project's history or basic setup guides from the README.
*   **General Language Rules:** The agent already knows how to write clean code.

This context is divided into two levels: **System-wide (User) Context** and **Workspace (Project) Context**. Both should be treated as **living documents**.

#### System-Wide (User) Context
Installed in your user home directory, this file defines your agent's persona and general behavior across *all* projects.

#### Workspace (Project) Context
Located in the root directory of your repository, this tells the agent how to operate *specifically* within that project. It should be **committed to version control**.
</details>

---

## Skills, Subagents, and Workflows

To elevate your agent's capabilities beyond simple prompt-and-response, you should leverage Skills and Subagents. These tools enable complex, multi-step workflows with high reliability.

<details>
<summary><strong>Skills</strong></summary>

### What are Skills?
Skills are modular, self-contained packages that extend an agent's capabilities by providing specialized knowledge, strict procedural workflows, and bundled resources. They transform the agent from a general-purpose AI into an expert with procedural memory for a specific task.

### How do they work?
*   **Dynamic Loading:** Instead of dumping a massive prompt into your session every time (which eats up your token limit), a Skill uses **Progressive Disclosure**. The agent only loads the skill's instructions into context *after* it decides the skill is relevant or you explicitly activate it.
*   **User vs Workspace Scoping:** Skills can be installed globally for a user (available in all projects) or locally to a specific workspace. Workspace skills are excellent for standardizing team workflows.

### Ideas for Custom Skills
*   **Repo Setup:** A skill that automatically checks dependencies, runs initialization scripts, and verifies the environment is ready for development.
*   **PR Workflows:** A skill that runs linters, generates a comprehensive PR description based on git diffs, and submits the PR via the GitHub CLI.
*   **Deployment:** A skill that builds the project, runs deployment scripts, and verifies the deployment succeeded.

### Recommended Skills List
We highly recommend exploring the `superpowers` extension for Gemini CLI, which includes a suite of powerful skills such as:
*   `brainstorming`
*   `writing-plans`
*   `test-driven-development`
*   `systematic-debugging`
*   `requesting-code-review`
</details>

<details>
<summary><strong>Subagents</strong></summary>

### What are Subagents?
Subagents are specialized, independent instances of the agent that can be delegated specific tasks. Instead of the main agent doing all the work (and bloating its context window), it can spawn a subagent to handle a focused job and report back the results.

### How to trigger them
Subagents are typically exposed as tools to the main agent. You can explicitly ask your agent to "delegate this task to the `codebase_investigator` subagent" or "use the `generalist` subagent to run these tests." The main agent will then orchestrate the execution and incorporate the subagent's summary into your session.
</details>

<details>
<summary><strong>Day-to-Day Workflows</strong></summary>

By combining Skills and Subagents, you can establish powerful day-to-day workflows:

1.  **Planning:** Activate a planning skill (like `writing-plans`) to draft a comprehensive design document before writing code.
2.  **Implementation:** Use a subagent-driven workflow (like the `subagent-driven-development` skill) to delegate independent components to parallel subagents.
3.  **Debugging:** When a test fails, activate a debugging skill (like `systematic-debugging`) or delegate to a specialized investigator subagent to find the root cause without polluting your main session context.
4.  **Review:** Before merging, use a review skill (like `requesting-code-review`) to ensure your changes meet all project standards and conventions.
</details>

---

## Resource Index

Below is an index of the resources available in this repository. These are samples and templates you can use to configure your own agents.

### Example Agent Contexts

*   **[User Level Context File](./context_files/user_level.AGENTS.md)**: An example template for your system-wide user context.
*   **[Workspace Level Context File](./context_files/workspace_level.AGENTS.md)**: An example template for your workspace/project context.

### Example Prompts

*   **[Codebase Tour Guide](./prompts/codebase_tour_guide.md)**: Map out and explain an unfamiliar subsystem.
*   **[Deep Bug Investigation](./prompts/deep_bug_investigation.md)**: Perform methodical root-cause analysis.
*   **[Devil's Advocate PR Review](./prompts/devils_advocate_pr_review.md)**: Find edge cases before opening a PR.
*   **[Generate Exhaustive Tests](./prompts/generate_exhaustive_tests.md)**: List boundary conditions before writing test code.
*   **[Plan New Feature](./prompts/plan_new_feature.md)**: Draft a design document before implementing a feature.
*   **[Review an Existing AGENTS.md File](./prompts/review_agent_md_best_practices.md)**: Analyze an existing context file and suggest improvements.

### Example Skills

These skills can be found in the `skills/` directory. Each skill contains a `SKILL.md` file defining its behavior.

*   **[Bug Detective](./skills/bug-detective/)**
*   **[Codebase Tour](./skills/codebase-tour/)**
*   **[Context File Reviewer](./skills/context-file-reviewer/)**
*   **[Devil's Advocate](./skills/devils-advocate/)**
*   **[Feature Architect](./skills/feature-architect/)**
*   **[Test Generator](./skills/test-generator/)**
