# Repository Restructure Implementation Plan

> **For agentic workers:** REQUIRED SUB-SKILL: Use superpowers:subagent-driven-development (recommended) or superpowers:executing-plans to implement this plan task-by-task. Steps use checkbox (`- [ ]`) syntax for tracking.

**Goal:** Execute the repository restructure according to `2026-03-18-repo-restructure-design.md`, migrating to dynamic skills and simplifying the context configurations.

**Architecture:** This is primarily a documentation update and repository file organization exercise. The core outputs are a rewritten `README.md`, pruned `context_files/`, reformatted `skills/`, and a new `github-workflow` skill.

**Tech Stack:** Markdown, Gemini CLI Tools, GitHub CLI.

---

### Task 1: Research and Validation (Pre-Implementation)

**Files:** None modified directly.

- [ ] **Step 1: Read the background context**
Run: the appropriate read tool to inspect `ideas.md` completely.
Expected: You understand the original user intent.

- [ ] **Step 2: External verification**
Search the internet or utilize your built-in knowledge to verify the current file locations for Gemini config (e.g. `~/.gemini/config.json`) and the latest best practices for context management.
Expected: You have confirmed the settings paths to use in the README.

### Task 2: Repository Cleanup

**Files:**
- Modify: `context_files/` directory

- [ ] **Step 1: Identify context files to remove**
Run: `ls context_files/`
Expected: `cpp_best_practices.AGENTS.md`, `python_fastapi.AGENTS.md`, `python_general_best_practices.AGENTS.md`, `react_best_practices.AGENTS.md`, `user_level.AGENTS.md`, `workspace_level.AGENTS.md`

- [ ] **Step 2: Remove outdated context files**
Run: `git rm context_files/cpp_best_practices.AGENTS.md context_files/python_fastapi.AGENTS.md context_files/python_general_best_practices.AGENTS.md context_files/react_best_practices.AGENTS.md`
Expected: Only `user_level.AGENTS.md` and `workspace_level.AGENTS.md` remain.

- [ ] **Step 3: Commit**
Run: `git commit -m "chore: remove outdated language-specific context files"`
Expected: Success

### Task 3: Skill Format Migration

**Files:**
- Modify: `skills/` directory

- [ ] **Step 1: Create directories for standalone skills**
Run: `mkdir -p skills/bug-detective skills/codebase-tour skills/context-file-reviewer skills/devils-advocate skills/feature-architect skills/test-generator`
Expected: Directories exist.

- [ ] **Step 2: Move standalone `.skill` files to directory format**
Run: 
`git mv skills/bug-detective.skill skills/bug-detective/SKILL.md`
`git mv skills/codebase-tour.skill skills/codebase-tour/SKILL.md`
`git mv skills/context-file-reviewer.skill skills/context-file-reviewer/SKILL.md`
`git mv skills/devils-advocate.skill skills/devils-advocate/SKILL.md`
`git mv skills/feature-architect.skill skills/feature-architect/SKILL.md`
`git mv skills/test-generator.skill skills/test-generator/SKILL.md`
Expected: Old standalone files are gone, `SKILL.md` files exist in directories.

- [ ] **Step 3: Commit**
Run: `git commit -m "chore: migrate skills to directory format"`
Expected: Success

### Task 4: Rewrite the README.md

**Files:**
- Modify: `README.md`

- [ ] **Step 1: Draft the new README**
Create the new `README.md` structure strictly adhering to the design spec in Phase 3. 
- Ensure "Copilot" mentions are scrubbed. 
- Ensure "Pink Elephant" is in "General Tips".
- Ensure the new "Getting Started" section outlines the settings.
- Use `<details><summary>` blocks with brief intros outside of them.
- Update the Resource Index to reflect the deleted context files and updated skill formats.
Use your file writing tools to replace the content of `README.md`.

- [ ] **Step 2: Verify formatting and links**
Inspect the written `README.md` to ensure `<details>` tags are closed and paths in the Resource Index match the repository state.

- [ ] **Step 3: Commit**
Run: `git add README.md && git commit -m "docs: restructure README and update best practices"`
Expected: Success

### Task 5: Create the GitHub Workflow Skill

**Files:**
- Create: `skills/github-workflow/SKILL.md`

- [ ] **Step 1: Create the skill directory**
Run: `mkdir -p skills/github-workflow`

- [ ] **Step 2: Draft the SKILL.md file**
Write the skill file containing the YAML frontmatter (name: `github-workflow`) and markdown instructions covering:
- Setup Guidance (check `gh auth status`, guide if needed)
- PR review, PR drafting, Issue reading, and CI checking capabilities.
Save this to `skills/github-workflow/SKILL.md`.

- [ ] **Step 3: Commit**
Run: `git add skills/github-workflow/SKILL.md && git commit -m "feat: add github-workflow skill"`
Expected: Success

### Task 6: Final Validation & ideas.md Cleanup

**Files:**
- Modify: `ideas.md`

- [ ] **Step 1: Audit implemented ideas**
Read `ideas.md` and check off everything that was covered in the README and repo changes.

- [ ] **Step 2: Cleanup ideas.md**
Delete the sections from `ideas.md` that have been addressed. Keep only those that were excluded or left for the future, documenting briefly why they were skipped if necessary.

- [ ] **Step 3: Commit**
Run: `git add ideas.md && git commit -m "docs: clean up implemented items from ideas.md"`
Expected: Success
