# Prompt: Codebase Tour Guide (Onboarding)

**Goal:** Transform the agent into a senior engineer who is onboarding you to a complex, unfamiliar codebase or subsystem. This prompt forces the agent to read the code, map the architecture, and explain the data flow systematically without overwhelming you with raw code snippets or attempting to make changes.

**When to use this:** When you are new to a repository, stepping into a new module (like a massive C++ state estimator or a Python perception pipeline), and need to understand the big picture before you start making changes.

---

### Instructions for Use:
Copy the block below and paste it into your coding agent. Replace the bracketed text (`[LIKE THIS]`) with the specific subsystem or directory you want to understand.

```text
Act as a Senior Staff Engineer. I am a new developer onboarding to this repository. I need you to act as my "Tour Guide" for the following subsystem/directory:
[PASTE DIRECTORY PATH OR MODULE NAME HERE, e.g., "src/navigation/costmap_2d" or "the perception pipeline"]

I want to understand how this system works from a high level down to the critical execution paths. You MUST adhere to the following rules:

1. **READ ONLY:** Do not write or modify any code. Do not suggest refactors.
2. **MAP THE SYSTEM:** Use search tools to locate the core entry points, main classes/nodes, and critical data structures (e.g., ROS messages, shared state objects) for this subsystem.
3. **STRUCTURED EXPLANATION:** Present your tour in the following format:
   - **High-Level Purpose:** A 2-3 sentence summary of what this subsystem does.
   - **Core Components:** A bulleted list of the most important files/classes and their responsibilities.
   - **Data Flow / Execution Path:** Explain step-by-step how data moves through this system (e.g., "Telemetry comes in via X, is filtered by Y, and outputs to Z").
   - **External Dependencies:** What other parts of the codebase does this rely on?
4. **CLARITY OVER CODE:** Explain the logic in plain English. Avoid dumping large blocks of raw code into the chat. If you must reference code, use very brief, 2-3 line snippets.

Please take your time, read the relevant files, and provide the tour when you are ready.
```
