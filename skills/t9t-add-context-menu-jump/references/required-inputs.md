# Required inputs for a t9t context-menu jump

The request should provide, or allow you to determine from existing code:

- the source screen ZUL path and the source grid ID
- the target screen ZUL path
- the new context-menu option ID (the suffix after `<gridId>.ctx.`)
- the user-facing menu label text to translate
- the selected-row field or derived key used to filter the target screen
- whether a simple single-field equality filter is enough, or a custom `SearchFilter` is required
- the back-navigation screen path to pass into `JumpTool`
- any conditions that should disable the menu entry for some rows

If any of the assumptions below would require guessing, stop and ask:

- target screen location
- source grid ID or option ID naming
- target filter field or compound-filter logic
- translated menu label text
- whether rows with missing related data should hide / disable the action
