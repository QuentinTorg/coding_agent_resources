# Ideas for Future Repository Additions

This document serves as a backlog of ideas, templates, and best practices that we plan to add to the `coding_agent_resources` repository.

## expand the getting started for specific agent setup.
* Add a short paragraph at the very bottom saying something along the lines of "Opinionated setup recommendations are below if you are interested", then show an expandable "Gemini Setup" section header.
  * In the future we can add other model's setup info, but for now, stick to gemini.
* The Gemini section should be a series of setup actions that basically read like a setup tutorial.
* install. installation information can be found here: https://geminicli.com/docs/get-started/installation/
* Explain that by default Gemini is quite conservative with tool use. It will ask for permission for most tools and you can even force it to be in read-only mode by default in the settings if you desire. I believe by default it will only use its own internal tools that are useful for exploring the codebase. it will prompt for basically any bash tool. You can see the settings below for how to set up the ability to accept all future instances of using a tool if you desire.
* settings: They should provide basic snippets of the setting, explain which file needs to be modified for that setting, and explain what the setting does and why it's helpful. I recommend that the user goes through the settings themselves by typing the `/settings` command in gemini to see other customizations they may want to make
here is my full settings file. i think we should only pull the example snippets described below. each section of the settings file can be described independently. snippets should include a ... to make it clear that there may be other stuff in that section
User settings: ~/.gemini/settings.json
Workspace settings: your-project/.gemini/settings.json

```json
{
  "security": {
    "enablePermanentToolApproval": true,
    "environmentVariableRedaction": {
      "enabled": true
    }
  },
  "general": {
    "plan": {
      "modelRouting": false,
    }
  },
  "ui": {
    "footer": {
      "hideContextPercentage": false
    },
    "showMemoryUsage": true,
    "showModelInfoInChat": true
  },
  "experimental": {
    "plan": true,
    "modelSteering": true
  },
  "model": {
    "name": "gemini-3.1-pro-preview"
  }
}
```

  * security
    * enable permanent tool approval setting true if you want to be able to permanently allow tools like grep, ls, find when prompted to "approve". useful for generally safe read-only tools like `ls`, `grep`, etc.
  * general
    * plan mode:
        model routing turned off if you don't want it to automatically fall back from gemini 3.1 pro to 3 flash when it exits plan mode. This seems to break some settings about which model you're using, even if you don't use plan mode. I started seeing it fall back to Gemini 2.5 flash which is basically useless.
  * ui
    * footer
      * hide context percentage: false. this shows how much of your current context you are using. It's good to keep a close eye on this, considering its importance.
    * showModelInfoInChat: true. This shows in line exactly which model is set for every turn. This is important because I have seen bugs where Gemini falls back to a smaller model even though in the footer it says you are using Gemini 3.1 Pro. This was the symptom that I saw when the plan mode model routing was turned on.
  * Model: gemini-3.1-pro-preview. lock your model to always use pro preview. you can switch to automatic to save tokens during a small/simple task, but pro preview is much better with making complex changes and debugging.
  * experimental
    * plan mode: true. Allow read-only plan mode. plan mode allows you to make a plan with the agent without it immediately jumping into building. You can make a plan with the AI bouncing ideas off of it until you're happy with the plan that it generates. It will explore your code and give you feedback based on your ideas. I think very soon this will no longer be an experimental feature. We'll see.
    * model steering: true. This is a very helpful feature that allows you to inject commands while the agent is in a chain of thought. This means if you see the agent start to proceed down a wrong path instead of killing the process and sending a new prompt you can just add a comment to help the agent through a step. Small nudges like "look out for this Pitfall" or "I prefer to use this build command." "The current test was failing before you added the new feature."
* If you're interested in trying the superpowers extension, you can install it with this command. I recommend starting without it for a few example tasks/projects So you understand how the plugin is modifying the agent's behavior..
  * `gemini extensions install https://github.com/obra/superpowers`
* Don't make a user level gemini.md file until you have an understanding of how the model behaves and what you want to change.
* Explain that it's possible to start Gemini in sandbox mode, which runs it inside of a Docker container, somewhat opaque to the user. The downside is this Docker container will not have any of the tools that you've installed on your system, so it will be starting with a fresh context every time and force the agent to install the tools that it thinks are useful. This could potentially be beneficial though because you can specify the specific container you want it to enter and the Docker arms you want it to launch with. This means if you already have a Docker build environment you can just have Gemini working directly inside of that Docker Container. This is not necessary though, as Gemiini will also happily build your Docker images and enter the images and run tests for you if it needs to. It's up to you how interested you are in making sure that Gemini is sandboxed.. give commands for launching in docker sandbox and passing docker args

## Consider removing other agent references since I don't know much about them yet. just focus on gemini for now
*Skipped: We decided to keep references to Claude Code and Codex as they are part of the "big three" alongside Gemini CLI. We did, however, remove references to Copilot.*

## Basic getting started missing pieces
*Skipped/Future items not yet added to the README:*
* Document how Codex handles command policies differently (the backend safety net that makes it smoother/more agentic without explicit allow-lists).
* Document the specific setting that allows agents to accidentally jump around to dumber/smaller models.
* settings for showing the model its currently using
* add some more detail about how policies work

## skills
*Skipped/Future items not yet added to the README:*
* Add a section explaining that triggering skills is currently delicate, and users may need to use language specifically catered to triggering the skill since they don't always trigger when expected.

## 4. subagents
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


