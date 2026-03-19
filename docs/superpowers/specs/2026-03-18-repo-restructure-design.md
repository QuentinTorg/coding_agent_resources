# Repository Restructuring Design Spec

This document outlines the plan to update the `coding_agent_resources` repository, transitioning its focus from static context files to dynamic Skills and Subagents, while biasing heavily towards the Gemini CLI.

## Phase 1: Research and Validation (Pre-Implementation)
Before writing any documentation, the assigned agents **must** perform empirical research to validate the facts they are adding.
*   **Context Gathering:** Agents must read `ideas.md` to get the necessary background context and understand the raw thoughts that led to this restructuring.
*   **External Verification:** Agents must verify the content and best practices using recent documentation from vendors (Gemini, Claude, Codex) and/or recent research papers from the internet regarding AI coding agents.
*   **Fact-Checking:** Verify settings locations (e.g., `~/.gemini/config.json`), command policy syntax, and the exact mechanics of user vs. workspace scoping for the respective tools. Ensure that any instructions provided in the README are accurate and currently supported.

## Phase 2: Repository Cleanup
*   **Remove Copilot:** Scrub all references to "GitHub Copilot" across the repository. The defined "Big Three" are Gemini CLI, Claude Code, and Codex.
*   **Delete Outdated Context Files:** Remove the following files from the `/context_files/` directory, as they no longer represent the "Landmines Only" best practices:
    *   `cpp_best_practices.AGENTS.md`
    *   `python_fastapi.AGENTS.md`
    *   `python_general_best_practices.AGENTS.md`
    *   `react_best_practices.AGENTS.md`
*   **Keep Core Context:** Ensure `user_level.AGENTS.md` and `workspace_level.AGENTS.md` remain as the primary examples of lean, human-curated context files.
*   **Skill Format Migration:** Migrate all existing skills that are currently single files (e.g., `bug-detective.skill`) into the standard directory format (`bug-detective/SKILL.md`). Remove the old standalone `.skill` files to ensure consistency across the repository.

## Phase 3: README Restructuring
The `README.md` will be rewritten to flow logically for a beginner, emphasizing rapid onboarding.
*   **Formatting Requirement:** The document must remain easy to parse. Use collapsible sections (`<details><summary>`) to hide the details of each section so users can easily digest the total context before diving deeper. When applicable, each section should still have a brief intro outside the collapsible block so the user understands what will be in that section.

1.  **Introduction & Intent:** Explicitly state the repo is for quick reference and rapid onboarding. Define AI agents, focusing on Gemini CLI, while acknowledging Claude Code and Codex (noting cross-compatibility like `AGENTS.md`).
2.  **Core Fundamentals:** Keep the foundational mindset for working with agents.
3.  **Getting Started (NEW):**
    *   Settings locations (e.g., `~/.gemini/config.json`).
    *   **User vs. Workspace Settings:** Explain that workspace settings override user-level settings.
    *   Trusting directories (security vs. prompt injection).
    *   Command policies (how Gemini restricts commands by default and how to safely allow them).
    *   Recommended settings (e.g., model indicators, preventing accidental downgrades).
4.  **General Tips for Beginners:**
    *   Add the **"Pink Elephant" problem**: Explain why speaking in the affirmative is critical (e.g., say "Commit files individually" instead of "Don't `git commit -am`").
5.  **The Concept of "Context":** Keep the current explanation of why minimal, human-curated context files are better.
6.  **Skills (EXPANDED):**
    *   What skills are and why they are superior to static context files (dynamic loading, progressive disclosure).
    *   User vs. Workspace scoped skills.
    *   How they are triggered (and the current delicate nature of triggering).
    *   **Ideas for Custom Skills:** A list of potential skills users might build (e.g., setting up new repositories with language-specific best practices, PR workflows, multi-step build systems).
    *   **Recommended Skills List:** Highlight the `superpowers` skill as the premier example of enforcing methodical workflows, serving as a foundation for a growing list.
7.  **Subagents (NEW):**
    *   What they are (delegating tasks, saving tokens/context, using smaller models).
    *   How to trigger them in Gemini.
8.  **Day-to-Day Workflows (UPDATED):** Revamp to show workflows using Skills and Subagents, rather than pasting raw prompts.

## Phase 4: Create the GitHub Workflow Skill
Create a new, comprehensive skill to replace the old PR review prompts.
*   **Format:** The skill must be created using the directory format: `skills/github-workflow/SKILL.md`.
*   **Capabilities:**
    *   **Setup Guidance:** Automatically check if the user is authenticated with the GitHub CLI (`gh auth status`) and guide them through setup if they are not.
    *   Read issues to understand tasks.
    *   Draft comprehensive PR descriptions.
    *   Check CI status.
    *   Review PR diffs and leave inline comments using the `gh api`.
    *   Reply to PR comments.

## Phase 5: Final Validation & `ideas.md` Cleanup
The final step in the execution plan.
*   **Audit:** The agent will read `ideas.md` and verify that every applicable idea has been successfully integrated into the repository according to this spec.
*   **Cleanup:** Remove the completed items from `ideas.md`.
*   **Documentation:** Any ideas that were discussed but ultimately excluded, or details that haven't been updated yet, must be clearly documented in `ideas.md` so they are not lost. This will serve as a final validation step to ensure no details are lost.