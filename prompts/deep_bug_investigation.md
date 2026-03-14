# Prompt: Deep Bug Investigation

**Goal:** Stop the AI agent from immediately guessing and writing incorrect code. Force it into a methodical, "detective" mindset where it must trace the execution path and empirically prove the failure state before attempting a fix.

**When to use this:** When you have a complex bug, a confusing stack trace, or an issue that spans multiple files and you need the agent to perform root-cause analysis.

---

### Instructions for Use:
Copy the block below and paste it into your coding agent. Replace the bracketed text (`[LIKE THIS]`) with the specific details of your bug.

```text
Act as a Senior Debugging Expert and System Architect. I have a bug in this repository that I need you to investigate. 

Here is the description of the bug:
[PASTE BUG DESCRIPTION HERE]

Here is the stack trace, error log, or incorrect output:
[PASTE LOGS/OUTPUT HERE]

I want you to perform a deep, methodical root-cause analysis. You MUST adhere to the following strict rules during your investigation:

1. **NO GUESSING:** Do not immediately propose a code fix. Do not write any production code yet.
2. **TRACE THE PATH:** Use targeted search commands (`grep`, `rg`, or file searches) to find the files mentioned in the stack trace or related to the described behavior. Read the relevant functions to understand the execution flow.
3. **EXPLAIN YOUR FINDINGS:** Once you have mapped out the relevant code, explain the execution path to me in plain English. Point out exactly where the logic diverges from the expected behavior.
4. **PROVE IT:** Before we write a fix, you must empirically prove the failure state. Write a failing unit test, a temporary reproduction script, or provide the exact command required to trigger the bug consistently.

Please begin your investigation now and stop when you have a hypothesis for the root cause and a proposed method to reproduce it. Do not attempt to fix the bug until I approve your hypothesis.
```
