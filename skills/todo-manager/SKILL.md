---
name: todo-manager
description: Create or update a Markdown todo file when the user asks to organize pending work, unfinished feature modules, unfixed code issues, uncommitted changes, or other development follow-up items. Use when Codex needs to classify and track outstanding work in a TODO.md-style file.
---

# Todo Manager

Create a structured Markdown todo file for development follow-up work.

## Default Behavior

- Create or update a Markdown file to track pending work.
- Use `TODO.md` as the default file name unless the user specifies another path.
- Prefer updating an existing todo file if one already exists in the requested scope.
- Keep the todo file concise, categorized, and easy to scan.

## Core Categories

When organizing pending work, classify items into these groups first:

1. 未实现功能模块
2. 待修复代码问题
3. 未提交代码内容
4. 其他待跟进事项

If a category has no items, omit it unless the user explicitly wants a fixed full template.

## Workflow

1. Determine whether the user wants to create a new todo file or update an existing one.
2. Collect pending items from the user request, current changes, code comments, review findings, or project context.
3. Normalize overlapping items so the same task does not appear in multiple categories unless that duplication is intentionally useful.
4. Write the items into a Markdown file using a stable structure.
5. Keep each item short, actionable, and specific.

## Item Writing Rules

- Use checklist items like `- [ ] ...`.
- Describe the concrete pending work, not vague reminders.
- Prefer one actionable item per line.
- Include file paths, modules, or feature names when they materially help locate the work.
- If the work came from uncommitted changes, note that explicitly in the “未提交代码内容” section.
- If the work came from a bug or review finding, summarize the issue rather than copying a long analysis block.

## Output Structure

Use [assets/todo-template.md](assets/todo-template.md) as the default template.

Keep this section order unless the user asks otherwise:

1. 标题
2. 概览
3. 未实现功能模块
4. 待修复代码问题
5. 未提交代码内容
6. 其他待跟进事项

## File Management Rules

- If `TODO.md` already exists, merge or update it instead of blindly overwriting it.
- Preserve useful existing items unless the user asks to reset the file.
- Remove duplicate items when updating.
- Do not invent tasks that are not supported by the request or current project state.

## Boundaries

- Do not mark items as done unless the user or current project state clearly confirms completion.
- Do not create a long project plan when the user only wants a concise todo tracker.
- Do not mix implementation details and status notes into the same line unless it improves clarity.
