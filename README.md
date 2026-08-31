# Agent Skills

A collection of agent skills that extend capabilities across planning, development, and knowledge.

All install commands are global (`-g`, user-level) and target only Claude Code (`-a claude-code`), which installs the skills into `~/.claude/skills/`.

## Install all skills

```
npx skills@latest add perhells/skills -g -s '*' -a claude-code -y
```

## Planning & Design

- **grill-me** — Interview the user relentlessly about a plan or design until reaching shared understanding, resolving each branch of the decision tree.

  ```
  npx skills@latest add perhells/skills/grill-me -g -a claude-code
  ```

- **create-a-plan** — Create a plan through user interview, codebase exploration, and module design, then submit it as a Linear issue or save it as a local markdown file. Delegates its interview to `grill-me`, so install that alongside.

  ```
  npx skills@latest add perhells/skills/create-a-plan -g -a claude-code
  ```

## Development

- **review** — Customizable multi-agent code review of the current diff or a PR/branch/path target. Effort level, per-role models, and all reviewer instructions are configured in the skill and its agent files.

  ```
  npx skills@latest add perhells/skills/review -g -a claude-code
  ```

  The skill spawns two custom agent types (finder, verifier) whose definitions ship in `review/agents/`. `npx skills` installs only the skill, so link the agent definitions afterwards:

  ```
  ln -s ~/.claude/skills/review/agents/review-*.md ~/.claude/agents/
  ```

- **implement-linear-issue** — Implement a Linear issue end-to-end: evaluate whether the issue is still worth implementing (grounded in code exploration and metrics, not questions to the user), resolve open questions via `grill-me`, create a feature branch, implement one commit at a time, run the project's tests, and optionally open a PR via `open-a-pr`.

  ```
  npx skills@latest add perhells/skills/implement-linear-issue -g -a claude-code
  ```

- **open-a-pr** — Open a clean pull request for the current branch. Grounds title and body in the actual diff, terse and exact, follows the repo template.

  ```
  npx skills@latest add perhells/skills/open-a-pr -g -a claude-code
  ```

## Knowledge

- **ubiquitous-language** — Extract a DDD-style ubiquitous language glossary from the current conversation, flagging ambiguities and proposing canonical terms.

  ```
  npx skills@latest add perhells/skills/ubiquitous-language -g -a claude-code
  ```
