# Repository Restructure Implementation Plan

> **For agentic workers:** REQUIRED SUB-SKILL: Use superpowers:subagent-driven-development (recommended) or superpowers:executing-plans to implement this plan task-by-task. Steps use checkbox (`- [ ]`) syntax for tracking.

**Goal:** Execute the repository restructure according to `2026-03-18-repo-restructure-design.md`, migrating to dynamic skills and simplifying the context configurations.

**Architecture:** This is primarily a documentation update and repository file organization exercise. The core outputs are a rewritten `README.md`, pruned `context_files/`, reformatted `skills/`, and a new `github-workflow` skill.

**Tech Stack:** Markdown, Gemini CLI Tools, GitHub CLI.

---

### Task 1: Research and Validation (Pre-Implementation)

**Files:** None modified directly.

- [ ] **Step 1: Read the background context**
Run: `cat ideas.md`
Expected: You read the contents and understand the original user intent.

- [ ] **Step 2: External verification of Gemini settings**
Search the internet or utilize your built-in knowledge to verify the current file locations for Gemini config (e.g. `~/.gemini/config.json`) and the latest best practices for context management.
Expected: You have confirmed the settings paths to use in the README.

- [ ] **Step 3: External verification of command policies and scoping**
Verify command policy syntax (e.g., how the new policy model replaced `autoApprove`), and the exact mechanics of user vs. workspace scoping for the tools.
Expected: You have confirmed the exact mechanics so you can document them accurately.

### Task 2: Repository Cleanup

**Files:**
- Modify: `context_files/` directory
- Modify: Any files containing "GitHub Copilot"

- [ ] **Step 1: Identify context files to remove**
Run: `ls context_files/`
Expected: Lists the files to be removed.

- [ ] **Step 2: Remove outdated context files**
Run: `git rm context_files/cpp_best_practices.AGENTS.md context_files/python_fastapi.AGENTS.md context_files/python_general_best_practices.AGENTS.md context_files/react_best_practices.AGENTS.md`
Expected: Only `user_level.AGENTS.md` and `workspace_level.AGENTS.md` remain.

- [ ] **Step 3: Search for "Copilot" references**
Run: `grep -r -i "copilot" . --exclude-dir=.git --exclude-dir=.gemini`
Expected: Identifies any files containing references to Copilot.

- [ ] **Step 4: Scrub Copilot references from all files**
Use your file editing tools to remove all references found in Step 3.
Expected: Re-running the grep from Step 3 yields no results.

- [ ] **Step 5: Commit**
Run: `git commit -am "chore: remove outdated context files and scrub Copilot references"`
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

- [ ] **Step 1: Draft Introduction & Core Fundamentals**
Use your file writing tools to replace the content of `README.md` up to the Core Fundamentals section, strictly adhering to the design spec.
Expected: The file is updated with the first two sections.

- [ ] **Step 2: Draft Getting Started & Tips**
Append the "Getting Started" section (detailing settings, user vs workspace, trusting directories, command policies) and the "General Tips for Beginners" section (including the "Pink Elephant" problem). Use `<details><summary>` blocks with brief intros outside of them.
Expected: The file contains the new sections.

- [ ] **Step 3: Draft Skills, Subagents, and Workflows**
Append the expanded Skills section, the new Subagents section, and the updated Day-to-Day Workflows section.
Expected: The file contains the final instructional sections.

- [ ] **Step 4: Update the Resource Index**
Append the updated Resource Index to reflect the deleted context files and updated skill formats.
Expected: The README rewrite is complete.

- [ ] **Step 5: Verify formatting and links**
Inspect the written `README.md` to ensure `<details>` tags are closed and paths in the Resource Index match the repository state.
Expected: All links and formatting are valid.

- [ ] **Step 6: Commit**
Run: `git add README.md && git commit -m "docs: restructure README and update best practices"`
Expected: Success

### Task 5: Create the GitHub Workflow Skill

**Files:**
- Create: `skills/github-workflow/SKILL.md`

- [ ] **Step 1: Create the skill directory**
Run: `mkdir -p skills/github-workflow`

- [ ] **Step 2: Draft the SKILL.md file**
Write the skill file containing the YAML frontmatter (name: `github-workflow`) and markdown instructions covering all specified capabilities:
- Setup Guidance (check `gh auth status`, guide if needed)
- Read issues to understand tasks
- Draft comprehensive PR descriptions
- Check CI status
- Review PR diffs and leave inline comments using the `gh api`
- Reply to PR comments
Save this to `skills/github-workflow/SKILL.md`.

- [ ] **Step 3: Verify the skill file**
Run: `cat skills/github-workflow/SKILL.md`
Expected: The file contains all required capabilities and formatting.

- [ ] **Step 4: Commit**
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