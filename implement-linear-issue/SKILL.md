---
name: implement-linear-issue
description: Implement a Linear issue end-to-end — evaluate whether the issue is still worth implementing (grounded in code and metrics), resolve open questions, create a feature branch, implement one commit at a time, run the project's tests, and optionally open a PR. Use when the user wants to implement or work on a Linear issue.
---

## Prerequisites

A Linear MCP server MUST be available — this skill cannot run without it. If no Linear MCP tools are present, notify the user and abort instead of guessing or proceeding.

## Steps

1. Fetch the Linear issue and read all of it, including its description, comments, and any linked PRDs, sub-issues, or attachments.

2. Determine the issue's scope (see "Scoping the issue" below). An issue with sub-issues is never implemented as a whole; a sub-issue is implemented alone.

3. Evaluate whether the issue still makes sense to implement (see "Evaluating the issue" below). If the evidence casts doubt on the issue, present it and ask the user whether to implement.

4. If there are any outstanding questions or decisions to take, resolve them using the `grill-me` skill.

5. Check that you're currently on an up-to-date main branch. If not, ask the user if it's their intention to work on an already existing feature branch, or base a new feature branch on something other than main.

6. Create the required feature branch using the Linear default format if it doesn't already exist, for example: `perhells/nex-1235-migrate-v2compositesuggested_episode-preserves-404-path`

7. Implement according to the issue's description/PRD and the decisions reached in step 4, committing one change at a time. Stay inside the scope determined in step 2.

8. Run all tests/checks for the project and fix any problems.

9. Ask the user whether they want to create a PR. If so, use the `open-a-pr` skill.

Branching: work on a feature branch, based on the default branch or another feature branch, never on the default branch itself. One Linear issue per branch and PR.

## Scoping the issue

Plans created with `create-a-plan` may be split into a parent (umbrella) issue and one sub-issue per slice, each slice meant to be its own PR. Before evaluating anything, classify the issue:

- **Has sub-issues, or is marked as an umbrella/parent plan**: do not implement it. List the sub-issues with their status and dependency order, propose the first one that is not done and whose dependencies are done, and ask the user which sub-issue to implement. Then restart this skill on that sub-issue. The parent's description is context, not a task list.
- **Is a sub-issue (has a parent, or is marked as a slice)**: fetch the parent and read it for cross-cutting decisions and the slice sequence. Implement only what this sub-issue's own description covers. Check that the slices it depends on are merged; if not, ask the user whether to proceed on top of their branch or stop. If the parent's description or sibling issues describe work this issue does not, that work is out of scope, even when it would be quick, even when the code you are touching seems to invite it.
- **Standalone**: implement as described.

Wherever the issue text says "do not implement directly" or "implement only what is described here", obey it literally.

## Evaluating the issue

Before writing any code, verify that the issue's premise still holds:

- Does the problem or gap the issue describes actually exist in the current code? Explore the codebase to confirm — it may already be fixed, partially implemented, or superseded by later changes.
- Is the proposed approach still compatible with how the code works today, or has the surrounding code changed in a way that invalidates it?
- If the issue makes claims about runtime behaviour (error rates, traffic, usage, performance), check available metrics, dashboards, and logs to confirm the problem is actually observed.

Answer these questions yourself by exploring the code and querying metrics where available — do NOT ask the user anything the code or metrics can answer. Only bring a question to the user if it's a genuine product or priority decision that evidence cannot settle.

Conclude with a short assessment grounded in what you found. If the premise clearly holds, continue with the next step. If the evidence casts doubt on it (already done, premise no longer holds, problem not observed in practice), present the findings and ask the user whether to implement anyway, adjust the issue first, or abandon it — the decision is the user's, not the skill's.
