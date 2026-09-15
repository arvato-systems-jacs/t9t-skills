# t9t context-menu jump implementation checklist

This checklist is based on upstream `t9t` ZK UI examples using `gridContext`, `IGridContextMenu`, and `JumpTool`.

1. Inspect the source screen ZUL and confirm which component owns the results grid:
   - `grid28`
   - `twosections28`
   - `threesections28`
2. Add or extend the `gridContext` attribute so it includes the new option ID. `gridContext` is a comma-separated option list.
3. Remember the effective context-menu ID becomes `<gridId>.ctx`, and each option becomes a menu item ID `<gridId>.ctx.<option>`.
4. Add a Java handler implementing `IGridContextMenu<YourDto>` in the appropriate ZK UI module.
5. Annotate the handler with `@Singleton` and `@Named("<gridId>.ctx.<option>")`.
6. In `selected(...)`, get the selected DTO from `dwt.getData()` and build the jump:
   - simple many-to-one / one-to-many filter:
     - `JumpTool.jump("screens/.../target28.zul", "targetField", dto.getSomething(), "screens/.../source28.zul");`
   - compound or derived filter:
     - create a `SearchFilter`
     - call `JumpTool.jump("screens/.../target28.zul", searchFilter, "screens/.../source28.zul");`
7. If not every row can jump, implement `isEnabled(...)` and return `false` for rows without a valid target.
8. Add the translation entry `@.<gridId>.ctx.<option>=Your menu label` in the appropriate `ui_*.properties` files used by the module.
9. Validate the three names stay aligned:
   - `gridContext="...,<option>,..."`
   - `@Named("<gridId>.ctx.<option>")`
   - `@.<gridId>.ctx.<option>=...`
10. Reuse upstream patterns where possible:
   - `bpmnStatus.ctx.toBpmnDef` jumps with a single equality filter to `processDefinition28.zul`
   - `requests.ctx.toSession` jumps from request rows to session rows by `sessionRef`
   - `requests.ctx.toChilds` jumps from a request to related child requests by `invokingProcessRef`
   - `dataChangeRequestExtended.ctx.toOriginal` shows the complex case where a custom `SearchFilter` must be derived before calling `JumpTool.jump(...)`
11. Validate the result manually:
   - open the source screen
   - select a row with related data
   - trigger the context menu entry
   - confirm the target screen opens and shows the expected filtered records
