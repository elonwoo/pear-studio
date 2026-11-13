# Workflow

项目的协作与行为准则。动手之前读这一份；分支规则另见 [../../docs/en/guides/branching-strategy.md](../en/guides/branching-strategy.md)。

## Guiding Principles (MUST FOLLOW)

- **Keep it clear**: Write code that is easy to read, maintain, and explain.
- **Match the house style**: Reuse existing patterns, naming, and conventions.
- **Search smart**: Prefer `ast-grep` for semantic queries; fall back to `rg`/`grep` when needed.
- **Log centrally**: Route all logging through `loggerService` with the right context—no `console.log`.
- **Research via subagent**: Lean on `subagent` for external docs, APIs, news, and references.
- **Always propose before executing**: Before making any changes, clearly explain your planned approach and wait for explicit user approval to ensure alignment and prevent unwanted modifications.
- **Lint, test, and format before completion**: Coding tasks are only complete after running `pnpm lint`, `pnpm test`, and `pnpm format` successfully.
- **Write conventional commits**: Commit small, focused changes using Conventional Commit messages (e.g., `feat:`, `fix:`, `refactor:`, `docs:`).
- **Sign commits**: Use `git commit --signoff` as required by contributor guidelines.

## Pull Request Workflow (CRITICAL)

When creating a Pull Request, you MUST use the `gh-create-pr` skill.
If the skill is unavailable, directly read `.agents/skills/gh-create-pr/SKILL.md` and follow it manually.

## Review Workflow

When reviewing a Pull Request, do NOT run `pnpm lint`, `pnpm test`, or `pnpm format` locally.
Instead, check CI status directly using GitHub CLI:

- **Check CI status**: `gh pr checks <PR_NUMBER>` - View all CI check results for the PR
- **Check PR details**: `gh pr view <PR_NUMBER>` - View PR status, reviews, and merge readiness
- **View failed logs**: `gh run view <RUN_ID> --log-failed` - Inspect logs for failed CI runs

Only investigate CI failures by reading the logs, not by re-running checks locally.

## Issue Workflow

When creating an Issue, you MUST use the `gh-create-issue` skill.
If the skill is unavailable, directly read `.agents/skills/gh-create-issue/SKILL.md` and follow it manually.

## Branch Strategy (Effective April 3, 2026)

> **IMPORTANT**: The `main` branch is now under **code freeze**. Only critical bug fixes submitted via `hotfix/*` branches are accepted. Fix PRs must be minimal in scope and must not include any refactoring code.
>
> All new features, refactoring, and optimizations should be developed on the **`v2` branch**. We welcome every developer to actively participate in v2 development!
>
> The `v2` branch will only accept new feature submissions after all current features have been fully refactored.

## V2 Refactoring in Progress

The `main` branch is under code freeze. All development has moved to the `v2` branch.

- **`main` branch**: Only accepts critical bug fixes via `hotfix/*` branches. Minimal changes, no refactoring.
- **`v2` branch**: All new features, refactoring, and optimizations go here.

Files marked with the following header are **blocked for feature changes**:

```typescript
/**
 * @deprecated Scheduled for removal in v2.0.0
 * ⚠️ NOTICE: V2 DATA&UI REFACTORING
 * STOP: Feature PRs affecting this file are currently BLOCKED.
 */
```

Do not introduce new features to these files. Bug fixes only.

## 其他

- 提交前跑 `pnpm build:check`，命令清单见 [commands.md](commands.md)。
- 测试要求见 [testing.md](testing.md)，代码规范见 [conventions.md](conventions.md)。
