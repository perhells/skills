---
name: create-a-plan
description: Create a plan through user interview, codebase exploration, and module design, split it into PR-sized plans when the change is large, then submit it as a Linear issue (new or updated, with sub-issues per slice) or save it as local markdown files. Use when user wants to create a plan, design a feature, write a PRD, or plan new work.
---

This skill will be invoked when the user wants to create a plan (or PRD) for a feature or change. You may skip steps if you don't consider them necessary.

1. Ask the user for a long, detailed description of the problem they want to solve and any potential ideas for solutions.

2. Explore the repo to verify their assertions and understand the current state of the codebase.

3. Resolve every open question and design decision by interviewing the user with the `grill-me` skill. If `grill-me` is unavailable, interview inline following the same rules: walk down each branch of the design tree resolving dependencies between decisions one-by-one, ask one question at a time, provide your recommended answer, and explore the codebase instead of asking whenever it can answer the question.

4. Sketch out the major modules you will need to build or modify to complete the implementation. Actively look for opportunities to extract deep modules that can be tested in isolation.

A deep module (as opposed to a shallow module) is one which encapsulates a lot of functionality in a simple, testable interface which rarely changes.

Check with the user that these modules match their expectations. Check with the user which modules they want tests written for.

5. Decide whether the work is one plan or several (see "Sizing and splitting" below). Each plan must map to one PR that a reviewer can read in a single sitting. If splitting, propose the slices, their order, and what each leaves working on main; confirm with the user before writing anything.

6. Ask the user where the plan should go:

   - **Linear issue** — requires a Linear MCP server; if none is available, say so and offer the local file instead. Ask whether to create a new issue or update an existing one (if updating, ask for the identifier, e.g. NEX-123). Also ask for:
     - **Team** (required for new issues)
     - **Project** (optional)
     - **Parent issue** (optional)
     - **Labels** (optional)
     - **Priority** (optional: 0=None, 1=Urgent, 2=High, 3=Normal, 4=Low)
   - **Local markdown file** — ask where to save it, and follow the naming convention of existing files at that location, if any.

7. Once you have a complete understanding of the problem and solution, use the plan template below to write the plan. For Linear, submit the plan title as the issue `title` and the rendered template as the issue `description` (markdown). For a local file, save the rendered template to the agreed path.

   If the work was split, write one parent plan with the parent template and one child plan per slice with the child template. The parent holds only what applies to every slice or to the overall objective: problem, solution, cross-cutting decisions, slice sequence, out of scope. Everything that belongs to one slice, including its user stories, lives in that slice's child plan and nowhere else. For Linear, create the parent issue first (or use the existing issue being updated as the parent), then each child as a sub-issue of the parent, in implementation order, and link back to the parent from each child. For local files, save the parent plus one numbered file per slice (e.g. `<name>.md`, `<name>-1-<slice>.md`, `<name>-2-<slice>.md`), following any existing convention at that location.

## Sizing and splitting

A plan is implemented as a single PR. Before writing, estimate the hand-written diff each plan implies from the modules sketched in step 4: new and modified source, tests, config, docs. Do not count generated code, lockfiles, vendored dependencies, fixtures, snapshots, recorded data, or scaffolding produced by a tool.

- Aim for a few hundred lines of hand-written diff per plan. Above roughly 500 lines, split. Above 1000, always split unless the size is inherent (see below).
- Size is inherent, and a large PR is acceptable, when the bulk is bootstrapping a new service or project, adding a dependency, vendoring, a mechanical rename or move, a large migration or generated file, or bulk data. Say so in the plan so the reviewer knows what to skim.
- Do not split to hit a number if the slices cannot stand on their own. A slice that leaves main broken, half-wired, or that only makes sense together with the next one is not a slice; merge it with its neighbour.

How to slice, in order of preference:

1. Along the module boundaries from step 4. A deep module with its tests, unused by production code, is an ideal first PR: fully reviewable and zero risk.
2. Bottom-up by dependency: schema and data model, then core modules, then adapters and wiring, then UI or API surface. Each slice depends only on slices before it.
3. Dark launch: land the new path behind a flag, config, or unused entry point, then switch over in a later slice, then remove the old path in a final slice.
4. By user story: a vertical slice that delivers one or a few stories end to end, when the stories are independent.

Each slice must be mergeable on its own, keep main working and tests green, and carry its own tests. Refactors that prepare the ground for the feature go in their own slice ahead of it.

When in doubt whether to split, ask the user; when the user has expressed no preference and the estimate is clearly above the threshold, split.

<plan-template>

## Problem Statement

The problem that the user is facing, from the user's perspective.

## Solution

The solution to the problem, from the user's perspective.

## User Stories

A numbered list of user stories. Cover every actor, including operators and support; every platform or surface; failure and edge cases such as offline use, concurrent updates and deletion; and operations such as metrics and rollout. Each user story should be in the format of:

1. As an <actor>, I want a <feature>, so that <benefit>

<user-story-example>
1. As a mobile bank customer, I want to see balance on my accounts, so that I can make better informed decisions about my spending
</user-story-example>

## Implementation Decisions

A list of implementation decisions that were made. This can include:

- The modules that will be built/modified
- The interfaces of those modules that will be modified
- Technical clarifications from the developer
- Architectural decisions
- Schema changes
- API contracts
- Specific interactions

Do NOT include specific file paths or code snippets. They may end up being outdated very quickly.

## Testing Decisions

A list of testing decisions that were made. Include:

- A description of what makes a good test (only test external behavior, not implementation details)
- Which modules will be tested
- Prior art for the tests (i.e. similar types of tests in the codebase)

## Out of Scope

A description of the things that are out of scope for this plan.

## Further Notes

Any further notes about the feature.

</plan-template>

<parent-plan-template>

> **Umbrella issue. Do not implement this issue directly.** The work is split into the sub-issues listed under Slices; implement them one at a time, in order, each as its own PR.

## Problem Statement

The problem that the user is facing, from the user's perspective.

## Solution

The solution to the problem, from the user's perspective, at the level of the whole feature. No per-slice detail.

## Slices

A numbered list of the slices in implementation order, one line each: what it delivers, what it depends on, and a rough size. Each slice links to its child plan once created.

## Cross-Cutting Decisions

Decisions that constrain every slice: architecture, module boundaries and their interfaces, naming, shared schema or API contracts, flags used for dark launch, testing philosophy. Anything that only one slice needs goes in that slice's plan instead. Same rules as the plan template: no file paths or code snippets.

## Out of Scope

Things out of scope for the whole feature. Deferred slices are not out of scope; they are listed under Slices.

## Further Notes

Any further notes about the feature as a whole.

</parent-plan-template>

<child-plan-template>

> **Slice N of M of <parent>. Implement only what is described here.** Sibling slices are separate issues and separate PRs.

## Parent

Link or identifier of the parent plan, and this slice's position in the sequence (e.g. 2 of 4).

## Depends On

The slices that must be merged before this one, or "none".

## Scope

What this slice delivers, from the user's perspective, and what it deliberately leaves for later slices.

## User Stories

A numbered list of the user stories this slice fulfils, in the same format as the plan template. Extensive for this slice only; stories belonging to other slices are not repeated here.

## State After Merge

What works on main once this is merged, and what is dormant (behind a flag, unused module, old path still active).

## Implementation Decisions

Same rules as the plan template, limited to this slice. Do not repeat cross-cutting decisions from the parent; reference them.

## Testing Decisions

Same rules as the plan template, limited to this slice.

## Estimated Size

Rough hand-written diff, and what is excluded from the count (generated, vendored, fixtures) if anything large is expected.

</child-plan-template>
