# Code Review Checklist

Use this checklist to deepen the review when the change is non-trivial.

## Reuse And Redundancy

- Check whether the same logic already exists elsewhere in the project.
- Look for duplicated request handling, formatting, validation, state logic, or UI fragments.
- Verify whether an existing shared utility, hook, component, service, or helper should be reused.

## Structure And Clarity

- Check whether responsibilities are split cleanly.
- Look for deeply nested flows, mixed concerns, or hard-to-follow control paths.
- Verify whether naming, file boundaries, and abstractions make the feature understandable.

## Types And Logic

- Check type compatibility, nullability, and unsafe casts or assumptions.
- Compare old and new control flow for logic regressions.
- Check error handling, fallback behavior, and return values.

## Boundary Cases

- Check empty, null, loading, timeout, and partial-failure states.
- Verify behavior for unexpected input sizes, missing fields, or repeated actions.
- Review whether default values and conditional branches cover edge cases.

## Performance

- Look for repeated heavy work, redundant requests, or unnecessary re-renders.
- Check hot-path loops, large-list rendering, and avoidable recomputation.
- Verify whether the implementation adds overhead compared with existing project patterns.

## UI

- Check loading, empty, error, and success states for visible inconsistencies.
- Review interaction feedback, disabled states, and obvious accessibility regressions.
- Verify whether the UI structure matches existing app patterns and avoids duplicated presentation logic.
