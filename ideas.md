# Ideas for Future Repository Additions

This document serves as a backlog of ideas, templates, and best practices that we plan to add to the `coding_agent_resources` repository.

## 1. Workflows & External Tools (`/workflows/`)
*   **`github_cli_integration.md`**: A dedicated guide explaining how to authenticate the agent with the GitHub CLI (`gh`) and providing exact prompts for things like:
    *   "Review my uncommitted changes and draft a PR description."
    *   "Read PR #42 and leave inline comments using the `gh api`."

## 2.
* add some basic concepts for getting starte for each agent
* gemini
    * trusted directories. controlling which directories are trusted. best practices for enabling
        * it will prompt if you should trust it
    * model will want to know which commands it is allowed to run by default. gemini starts out basically not allowing anything, you have to approve all commands
    * codex handles it a little bit differently, seems to allow most commands as long as they don't reach out to the internet or do certain things to your system. there is some back end safety net that is more hiddne, but makes it smoother to use and a little more agentic
    * recommended settings
        * showing model it is using to run each command
        * make sure we document the setting that allows it to jump around to dumber models accientally

## 3. setting up tools? I'm not sure if this is a thing
        using tools instead of recommended prompts. we can tell users how to convert the prompts into tools or subagents potentially.
## 4. subagents
        what are they useful for. using smaller models for menial tasks, reducing context window of main agent. how to configure them, per agent type

## 5. remove references to codex
