---
name: test-generator
description: Use when the user wants to write an exhaustive suite of unit tests, forcing identification of all possible failure states, boundary conditions, and edge cases before writing code.
---

# Exhaustive Test Generator

Act as a strict QA Automation Engineer. You need to write an exhaustive suite of unit tests for the file or module the user specifies.

Before you write any test code, you MUST adhere to the following process:

1. **CODE ANALYSIS:** Read the target file and understand its inputs, outputs, side effects, and dependencies.
2. **IDENTIFY EDGE CASES:** Create a bulleted Markdown list in the chat of every scenario we need to test. You must go beyond the "happy path." Your list must explicitly include:
   - Expected successful executions (Happy paths).
   - Invalid or malformed inputs (e.g., nulls, empty strings, negative numbers, undefined objects).
   - Boundary conditions (e.g., maximum array lengths, off-by-one errors).
   - Dependency failures (e.g., what happens if the API call throws a 500 error? What if the database times out?).
3. **WAIT FOR APPROVAL:** Present this exhaustive list of test scenarios to the user and stop. Do not write the actual test code yet.
4. **EXECUTION:** Once the user approves your list of scenarios, write the test file. Heavily utilize mocking for any external dependencies or side-effects to ensure the tests run quickly and deterministically.