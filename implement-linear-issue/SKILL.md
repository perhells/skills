---
name: implement-linear-issue
description: Implement a Linear issue end-to-end — evaluate whether the issue is still worth implementing (grounded in code and metrics), resolve open questions, create a feature branch, implement one commit at a time, run the project's tests, and optionally open a PR. Use when the user wants to implement or work on a Linear issue.
---

## Prerequisites

A Linear MCP server MUST be available — this skill cannot run without it. If no Linear MCP tools are present, notify the user and abort instead of guessing or proceeding.

## Steps

1. Fetch the Linear issue and read all of it, including its description, comments, and any linked PRDs, sub-issues, or attachments.

2. Evaluate whether the issue still makes sense to implement (see "Evaluating the issue" below). On a no-go, report the evidence to the user and stop.

3. If there are any outstanding questions or decisions to take, resolve them using the `grill-me` skill.

4. Check that you're currently on an up-to-date main branch. If not, ask the user if it's their intention to work on an already existing feature branch, or base a new feature branch on something other than main.

5. Create the required feature branch using the Linear default format if it doesn't already exist, for example: `perhells/nex-1235-migrate-v2compositesuggested_episode-preserves-404-path`

6. Implement according to the issue's description/PRD and the decisions reached in step 3, committing one change at a time.

7. Run all tests/checks for the project and fix any problems.

8. Ask the user whether they want to create a PR. If so, use the `open-a-pr` skill.

You MUST:

- Always develop on a feature branch, never directly on the default branch.
- A feature branch may be based on the project's default branch or another feature branch.

## Evaluating the issue

Before writing any code, verify that the issue's premise still holds:

- Does the problem or gap the issue describes actually exist in the current code? Explore the codebase to confirm — it may already be fixed, partially implemented, or superseded by later changes.
- Is the proposed approach still compatible with how the code works today, or has the surrounding code changed in a way that invalidates it?
- If the issue makes claims about runtime behaviour (error rates, traffic, usage, performance), check available metrics, dashboards, and logs to confirm the problem is actually observed.

Answer these questions yourself by exploring the code and querying metrics where available — do NOT ask the user anything the code or metrics can answer. Only bring a question to the user if it's a genuine product or priority decision that evidence cannot settle.

Conclude with a short go/no-go assessment grounded in what you found. A no-go (already done, premise no longer holds, problem not observed in practice) ends the skill: present the evidence and stop instead of implementing.
