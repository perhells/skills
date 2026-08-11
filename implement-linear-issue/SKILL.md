---
name: implement-linear-issue
description: Implement a Linear issue end-to-end — resolve open questions, create a feature branch, implement one commit at a time, run the project's tests, and optionally open a PR. Use when the user wants to implement or work on a Linear issue.
---

## Prerequisites

A Linear MCP server MUST be available — this skill cannot run without it. If no Linear MCP tools are present, notify the user and abort instead of guessing or proceeding.

## Steps

1. Fetch the Linear issue and read all of it, including its description, comments, and any linked PRDs, sub-issues, or attachments.

2. If there are any outstanding questions or decisions to take, resolve them using the `grill-me` skill.

3. Check that you're currently on an up-to-date main branch. If not, ask the user if it's their intention to work on an already existing feature branch, or base a new feature branch on something other than main.

4. Create the required feature branch using the Linear default format if it doesn't already exist, for example: `perhells/nex-1235-migrate-v2compositesuggested_episode-preserves-404-path`

5. Implement according to the issue's description/PRD and the decisions reached in step 2, committing one change at a time.

6. Run all tests/checks for the project and fix any problems.

7. Ask the user whether they want to create a PR. If so, use the `open-a-pr` skill.

You MUST:

- Always develop on a feature branch, never directly on the default branch.
- A feature branch may be based on the project's default branch or another feature branch.
