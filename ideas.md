# Ideas for Future Repository Additions

This document serves as a backlog of ideas, templates, cleanup, and best practices that we plan to add to the `coding_agent_resources` repository.

## add example usage.
Considering this is an onboarding repo, I think it would make sense in the beginning to have some real examples of situations where the agent can be useful and some simple prompts that can be used to have it do certain tasks.
Example, simple prompt, opening up a pull request, making a commit, debugging a build issue. Open to other ideas.

## Consider removing other agent references since I don't know much about them yet. just focus on gemini for now
*Skipped: We decided to keep references to Claude Code and Codex as they are part of the "big three" alongside Gemini CLI. We did, however, remove references to Copilot.*

## address workflows section questions
* Does the workflows section next to sub-agents and agents really make sense? Workflows seems more like recommendations for how to use the tool versus features of the tool. The other two are more like features of the tool.

## Basic getting started missing pieces
*Skipped/Future items not yet added to the README:*
* Document how Codex handles command policies differently (the backend safety net that makes it smoother/more agentic without explicit allow-lists).

## skills
*Skipped/Future items not yet added to the README:*
* Add a section explaining that triggering skills is currently delicate, and users may need to use language specifically catered to triggering the skill since they don't always trigger when expected.

## subagents
*Skipped/Future items not yet added to the README:*
* Explain how users can create their own custom subagents.

## Update the day-to-day workflows for beginners.
*Skipped/Future items not yet added to the README:*
* Add comparative examples showing the day-to-day workflows both *with* and *without* the Superpowers plugin, to better illustrate its value.

## talk about slash commands
* what do they do
* what are they good for
* how do they work
* how to set them up
* put the slash commands in the section with skills and subagents

## address TODO's and "needs work" style comments in readme
