---
name: t9t-add-context-menu-jump
description: "Guide for adding a ZK grid context-menu jump between CRUD / search screens in t9t or a t9t-based application. Use when asked to add a context menu entry that opens another screen with a preset related-record filter."
license: Apache-2.0
---

# t9t: add a context-menu jump

Use this skill when the task is to add a right-click grid context-menu entry which jumps from one CRUD / overview screen to another related screen in `t9t` or a `t9t`-based application.

## Required input

Before writing code, verify the task provides all required inputs listed in [`references/required-inputs.md`](references/required-inputs.md).

If critical inputs are missing, ask for them first. If the request is too incomplete to implement safely, reject the task instead of guessing.

## Workflow

Copy and track this checklist:

```text
- [ ] Verify the required inputs and identify the source / target ZK modules
- [ ] Inspect an existing handler using `IGridContextMenu` and `JumpTool` in the same codebase or in upstream `t9t`
- [ ] Add or update the source screen so the grid exposes the requested `gridContext` option
- [ ] Implement the `IGridContextMenu` handler with the matching `@Named("<gridId>.ctx.<option>")` qualifier
- [ ] Build the preset filter using the simplest valid `JumpTool.jump(...)` overload or a custom `SearchFilter`
- [ ] Add or update the translation key for the context-menu label
- [ ] If needed, gate availability with `isEnabled(...)` so the entry is disabled for invalid rows
- [ ] Run targeted validation and confirm the jump opens the target screen with the intended filtered result set
```

## Implementation rules

1. Start by reading [`references/implementation-checklist.md`](references/implementation-checklist.md).
2. Preserve existing package names, screen paths, grid IDs, translation naming, and menu placement patterns.
3. Prefer the simple `JumpTool.jump(targetZul, fieldName, refOrId, backNaviLink)` overload when the target screen can be filtered by one field.
4. Use `JumpTool.jump(targetZul, searchFilter, backNaviLink)` when the jump requires a compound filter, a derived key, or target-specific lookup logic.
5. The handler qualifier must match the generated menu item ID pattern: `<gridId>.ctx.<option>`.
6. The visible menu label comes from translations for the same key pattern; do not hardcode user-facing text in Java.
7. Only override `isEnabled(...)` when some rows cannot produce a valid jump target.
8. Do not guess the target screen path, filter field name, or back-navigation screen. Inspect analogous screens or ask when unclear.

## Output contract

Before finishing, confirm that:

1. The source grid exposes the requested context-menu option.
2. The handler qualifier, translation key, and `gridContext` option use the same `<gridId>.ctx.<option>` naming.
3. The target screen opens with the expected preset search filter for the selected row.
4. Any row-level enablement restrictions are intentional.
5. The changed area was validated with targeted checks or manual verification appropriate for the repository.
