# Prompt: Plan New Feature (System Architecture)

**Goal:** Prevent the agent from immediately writing code that might not align with your system's architecture. Force it to act as a System Architect to map out the required file changes, data structures, and function signatures first.

**When to use this:** When you are asking the agent to build a multi-file feature, a new API endpoint, or any substantial addition to your codebase.

---

### Instructions for Use:
Copy the block below and paste it into your coding agent. Replace the bracketed text (`[LIKE THIS]`) with the details of the feature you want to build.

```text
Act as a Staff Software Engineer and System Architect. I want you to help me build a new feature in this repository.

Here are the requirements for the new feature:
[PASTE FEATURE DESCRIPTION AND REQUIREMENTS HERE]

[OPTIONAL: Insert any specific constraints, like "It must be a new ROS2 Node using rclcpp" or "It must be a POST endpoint in FastAPI"]

Before you write any production code, you MUST adhere to the following strict process:

1. **NO EAGER CODING:** Do not write the final implementation yet. Your first task is entirely planning and design.
2. **CONTEXT GATHERING:** Use search tools to locate where this feature should live based on existing repository patterns. Find similar features and understand the established conventions for routing, state management, or data access.
3. **DESIGN DOCUMENT:** Write a brief Markdown design document in the chat. The document must include:
   - **Target Files:** A list of files you intend to create or modify.
   - **Data Structures:** Any new interfaces, types, or database schemas required.
   - **Function Signatures:** The proposed signatures for the core logic functions or components.
   - **Testing Strategy:** How you plan to test this feature.
4. **WAIT FOR APPROVAL:** Once you have presented the design document, stop. Ask me to review and approve the architecture before you proceed to implementation.
```
