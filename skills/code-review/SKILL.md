---
name: code-review
description: Review code changes when the user explicitly asks for code review, says "review 一下", "帮我做 code review", or requests review of a PR, diff, patch, commit, or changed files. Focus on duplicate or redundant implementation, failure to reuse existing project capabilities, code structure clarity, type or logic errors, and potential boundary, performance, or UI issues.
---

# Code Review

Perform review as a high-signal engineering review task. Focus on concrete problems in implementation quality, structure, correctness, and user-facing behavior.

## Review Workflow

1. Establish scope from the user input, changed files, diff, or commit range.
2. Read the relevant code before forming conclusions.
3. Check whether the implementation reuses existing project capabilities instead of duplicating them.
4. Validate each finding against the actual code path, data flow, and runtime behavior.
5. Report findings in priority order with precise file and line references.

If the review target is unclear, infer the smallest reasonable scope from the available diff or modified files instead of asking broad questions.

## What To Prioritize

Prioritize findings that would matter before merge:

- Redundant or duplicate code that should reuse an existing function, component, hook, utility, or shared module
- Poor structure, weak separation of concerns, or implementations that make the behavior hard to understand
- Type errors, unsafe type assumptions, and logic errors
- Potential boundary-case, performance, or UI problems

Do not focus on style, naming, or formatting unless they point to one of the problems above.

## Evidence Standard

Only report issues you can support from the code or diff. Prefer high-confidence findings over broad advice.

For each finding:

- Identify the exact code location
- Explain the concrete engineering problem
- Describe when it would happen
- State the likely impact

Avoid vague concerns like "this might break" unless you can explain the path that breaks.

## Output Format

Present findings first. Start with the total count of errors or issues.

Use this structure:

1. `共 N 个错误` or `共 0 个错误`
2. A structured list of findings
3. Each item must include:

- File path
- Line reference if available
- Approximate error or issue summary
- Short explanation of why it is a problem

Prefer grouping by file path when one file has multiple issues.

Example shape:

```markdown
共 2 个错误

1. `src/foo.ts:23`
   文件路径：`src/foo.ts`
   问题：重复实现已有格式化逻辑，没有复用项目中的公共方法。

2. `src/bar.tsx:88`
   文件路径：`src/bar.tsx`
   问题：列表为空时没有兜底 UI，存在边界场景显示问题。
```

If there are no findings, state `共 0 个错误` and briefly mention any remaining testing or review limits if they matter.

## Review Heuristics

Use [references/checklist.md](references/checklist.md) when you need a deeper pass over likely defect classes or test gaps.

## Boundaries

- Do not invent project requirements that are not present in code, tests, or user instructions.
- Do not ask for large refactors unless the current structure directly causes a concrete problem.
- Prefer a smaller number of high-confidence findings over a long speculative list.
