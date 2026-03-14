# Prompt: Generate Exhaustive Tests

**Goal:** Transform the agent into a rigorous QA Engineer. Instead of just writing a quick "happy path" test, this prompt forces the agent to identify and list all possible failure states, boundary conditions, and edge cases *before* it writes any test code.

**When to use this:** When you have completed a feature or fixed a bug and want to ensure the code is incredibly robust and resilient to unexpected inputs.

---

### Instructions for Use:
Copy the block below and paste it into your coding agent. Replace the bracketed text (`[LIKE THIS]`) with the path to the file you want to test.

```text
Act as a strict QA Automation Engineer. I need you to write an exhaustive suite of unit tests for the following file/module:
[PASTE FILE PATH OR MODULE NAME HERE]

Before you write any test code, you MUST adhere to the following process:

1. **CODE ANALYSIS:** Read the target file and understand its inputs, outputs, side effects, and dependencies.
2. **IDENTIFY EDGE CASES:** Create a bulleted Markdown list in the chat of every scenario we need to test. You must go beyond the "happy path." Your list must explicitly include:
   - Expected successful executions (Happy paths).
   - Invalid or malformed inputs (e.g., nulls, empty strings, negative numbers, undefined objects).
   - Boundary conditions (e.g., maximum array lengths, off-by-one errors).
   - Dependency failures (e.g., what happens if the API call throws a 500 error? What if the database times out?).
3. **WAIT FOR APPROVAL:** Present this exhaustive list of test scenarios to me and stop. Do not write the actual test code yet.
4. **EXECUTION:** Once I approve your list of scenarios, you will write the test file. You must heavily utilize mocking for any external dependencies or side-effects to ensure the tests run quickly and deterministically.
```
