---
name: generate-commit-message
description: Use when drafting Chinese Conventional Commits from diffs or git status.
---

# Generate Commit Message

## Overview

Generate accurate Chinese Git commit messages from real repository changes.
Inspect the actual diff or user-provided change summary before writing the
message, and follow the user's requested output contract exactly.

## Workflow

1. Read the user's rules first: language, template, allowed types, line length,
   whether the response must contain only the commit message, and whether body
   or footer are optional.
2. Inspect real changes instead of guessing. Prefer `git status --short`, then
   `git diff --staged` when staged changes exist; otherwise use `git diff`.
   If the user supplies a diff or summary, use it as the source of truth.
3. Choose the commit type from the allowed enum. Use a scope only when a clear
   module, package, feature area, or file group is affected.
4. Write the header as `<type>(<scope>): <subject>` or `<type>: <subject>` when
   no useful scope exists. Keep `type` in English and write the subject in
   Chinese unless the user asks otherwise.
5. Add a body only when it improves accuracy. Describe what changed and why,
   not implementation trivia. Use multiple short lines when needed.
6. Add a footer only for issue references, breaking changes, reverts, or other
   metadata the user or project convention requires.
7. Before final output, check that every line is within the requested width
   limit. Default to about 72 characters; accept up to 100 only when the user
   allows it.

## Type Reference

| Type | Use for |
| --- | --- |
| `feat` | New user-visible behavior or capability |
| `fix` | Bug fixes and behavioral corrections |
| `docs` | Documentation-only changes |
| `style` | Formatting that does not affect code execution |
| `refactor` | Code changes that are neither features nor fixes |
| `test` | Adding or changing tests |
| `chore` | Build process, tooling, config, or auxiliary updates |
| `revert` | Reverting a previous commit |

## Output Contract

- If the user says the response must only contain the commit message, output no
  explanations, labels, bullets about the process, or Markdown code fences.
- Preserve blank lines in the template:
  header, blank line, optional body, blank line, optional footer.
- Do not include an empty footer placeholder. If there is no footer, stop after
  the header or body.
- Keep the subject concise, informative, and aligned with the key change.
- Avoid vague subjects such as `更新代码`, `修复问题`, or `调整文件`.
- Do not mention files mechanically unless the filename is the meaningful
  product surface.

## Example

```text
feat(commit-message): 新增中文提交信息生成规范

根据真实 git 变更选择提交类型和范围，并按模板输出标题、
正文和页脚。
```
