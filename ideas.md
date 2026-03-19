# Ideas for Future Repository Additions

This document serves as a backlog of ideas, templates, and best practices that we plan to add to the `coding_agent_resources` repository.

## Consider removing other agent references since I don't know much about them yet. just focus on gemini for now

## make sure the intent of this repo and readme is well documented for readers.
* The goal is to use this as a quick reference and rapid onboarding for new users of agents.

## rework the context files
* move the workflows documented into skills, document the skills in the skills section below
* Consider if it even makes sense to have some of these other Example context files.

## update the user/system context sectino of the readme.
* Make sure we point to skills when discussing workflows. Make sure The recommendations match the most recent best practices that we have just documented for the Workspace level context files.

## add explanation of the pink elephant problem wherever we think it belongs.
* I think probably in the general tips
* The pink elephant problem outlines the issue of if you tell someone not to think about a pink elephant, the first thing they'll think of is a pink elephant. The idea behind this is if you, without reason, tell an LLM not to do something, it's far more likely to do it in the future. The exception to this is if an LLM is doing something that you don't want it to do, then it is acceptable to tell it not to do that thing. In general, the best way to have an LLM avoid something is to not mention it. It's much more effective to speak in the affirmative to tell it what you would prefer it do, instead of outlining something you would prefer that it not do, hoping that it figures out what you expect it to do. for example, if you don't want it to `git commit -am`, then it may be better to request instead that it individual adds files to stage before committing. There is probably a much more concise way to state all of this so that it fits better in this section. Alternatively, it could belong in another section, such as the context files section.

## Basic getting started
* Where are your settings files. how does it load initial context. Which settings do I recommend you change right away
* add some basic concepts for getting started for each agent
* basic commands they will need in the cli
* opinionated things I do when i set it up, they do not need to do all of these
* gemini
    * trusted directories. explain it will ask if it should trust directory when started. you should accept. avoid running in your home directory. usually you will run on individual repos. the Fear is that if you run Gemini inside of a workspace where you don't have full control over the files then it's possible that somebody could do prompt injection on Gemini and have it hacked in a way that you don't expect.
    * model will want to know which commands it is allowed to run by default. gemini starts out basically not allowing anything, you have to approve all commands. Update to talk about the new policy model instead of the list in the settings.json
    * codex handles it a little bit differently, seems to allow most commands as long as they don't reach out to the internet or do certain things to your system. there is some back end safety net that is more hiddne, but makes it smoother to use and a little more agentic
    * recommended settings
        * showing model it is using to run each command
        * make sure we document the setting that allows it to jump around to dumber models accientally

## skills
* add section about skills to readme
* Do some research about what skills actually are before filling out the document. Make sure that the points below are well formatted and follow the online results for what skills are. Make sure that this section remains approachable for a new user.
* point out how skills can be used to make agents behave a certain way in a certain scenario. can be used to manipulate the behavior of Gemini.
* Skills can also be used to teach the agent how to use a specific workflow.
  * such as your preferred Git workflow.
  * PR review workflow.
  * How to use a multi-step build system
  * How to run tests or integration tests.
  * how to push build and push code to a robot then run tests on it
* Skills can be scoped at the user level or at the workspace level.
* point out that skills are much better than putting workflows in your agents.md because they are dynamically loaded. If the entire workflow gets put in the agents.md, then it will always be in the context, even in scenarios where the agent doesn't need to know it. Alternatively, a skill is a special file format that uses a single description at the top to tell the agent when the skill is applicable, and then a body that gives the agent the specific knowledge to use that skill. Skills can also go further by having supporting scripts that the agent will know to execute and when. When the agent starts up, it only Loads the skill descriptions into the context so that the agent knows when to use this skill. It does not load any of the knowledge until the applicable time when the skill gets triggered.
* Making sure that skills get triggered properly is delicate, and this is one of the biggest problems with skills today. Right now, the skills will not always trigger when expected. This means it's helpful for you to know which skills your agent has, and while skills are in their infancy, you may have to use language catered towards triggering the skill.
* For this readme, it's only important to explain roughly how skills work and why they're so important, but we will not get into building our own skills yet. Instead, we will recommend a few skills that we have experience with
* superpowers skill
  * provide instructions on how to install
  * Explain briefly what the superpower skill is, maybe pull some reference information from the repo. This should be one to two lines explaining in general how the superpower's skill manipulates the behavior of Gemini and why it's powerful. It should not be an exhaustive explanation for how to use it.
  * Superpowers skill forces the agent to work methodically. It operates in a brainstorm, plan, then execute workflow. It performs an interactive brainstorming step getting your input to create a comprehensive design document for your new feature. It then uses this design document to create an execution plan. The execution plan is a detailed roadmap that is provided to agents when writing the code. Basically, superpowers is a skill that helps you force Gemini to follow the core fundamentals we discussed further up in this document. The extremely detailed execution plan also allows the agent to do large chunks of work without significant user input while keeping the agent on the proper guardrails. It enforces a test-driven approach where features are added one at a time and verified before they're committed.
* We should add an example skill that explains our preferred workflow for PR reviews. the information currently in the user level agents.md about using the GitHub CLI should be moved into this skill. We should make this skill slightly more comprehensive where we have different skills all centered around the GitHub CLI. creating a pull request, reviewing a pull request, and replying to a pull request review. Reading through issues. Using an issue as a basis for a new task. Opening issues. Checking the status of CI.

## 4. subagents
* what are they useful for.
  * Delegating tasks so that the current context doesn't get cluttered.
  * It allows you to use different models for certain tasks to save tokens, or get extra thinking.
    * This could be using a small agent to run build commands that are clearly documented or using a larger agent to explore a repository while looking for a bug and report back what it has found.
* Explain how to trigger them in Gemini.
* Explain how to create your own agents.

## Update the day-to-day workflows for beginners.
* Now that we have all of this extra information, including skills like superpowers, some of these workflows might not make as much sense. Or maybe we should rework the workflows to explain how to use superpowers to do these things. Maybe it makes sense to have an example with and without the superpowers plugin.

## 5. remove references to copilot. the big three are gemini cli, claude code, and codex. codex and cobpilot are not interchangeable. i would rather not discuss copilot in this repo

## send a deep research job to investigate if my readme follows recent best practices. use the latest docs from the major venodors, but mostly ground the information in test-backed research.
