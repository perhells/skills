# Agent Skills

A collection of agent skills that extend capabilities across planning, development, and knowledge.

All install commands are global (`-g`, user-level); the CLI symlinks agent directories to its skill store by default.

## Install all skills

```
npx skills@latest add perhells/skills -g -s '*' -y
```

## Planning & Design

- **grill-me** — Interview the user relentlessly about a plan or design until reaching shared understanding, resolving each branch of the decision tree.

  ```
  npx skills@latest add perhells/skills/grill-me -g
  ```

- **create-a-plan** — Create a plan through user interview, codebase exploration, and module design, then submit it as a Linear issue or save it as a local markdown file. Delegates its interview to `grill-me`, so install that alongside.

  ```
  npx skills@latest add perhells/skills/create-a-plan -g
  ```

## Development

- **review** — Customizable multi-agent code review of the current diff or a PR/branch/path target. Effort level, per-role models, and all reviewer instructions are configured in the skill and its agent files.

  ```
  npx skills@latest add perhells/skills/review -g
  ```

  The skill spawns three custom agent types (scoper, finder, verifier) whose definitions ship in `review/agents/`. `npx skills` installs only the skill, so link the agent definitions afterwards:

  ```
  ln -s ~/.agents/skills/review/agents/review-*.md ~/.claude/agents/
  ```

- **implement-linear-issue** — Implement a Linear issue end-to-end: resolve open questions via `grill-me`, create a feature branch, implement one commit at a time, run the project's tests, and optionally open a PR via `open-a-pr`.

  ```
  npx skills@latest add perhells/skills/implement-linear-issue -g
  ```

- **open-a-pr** — Open a clean pull request for the current branch. Grounds title and body in the actual diff, terse and exact, follows the repo template.

  ```
  npx skills@latest add perhells/skills/open-a-pr -g
  ```

## Knowledge

- **ubiquitous-language** — Extract a DDD-style ubiquitous language glossary from the current conversation, flagging ambiguities and proposing canonical terms.

  ```
  npx skills@latest add perhells/skills/ubiquitous-language -g
  ```
