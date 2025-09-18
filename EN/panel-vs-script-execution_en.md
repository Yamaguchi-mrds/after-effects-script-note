# After Effects ScriptUI Panel and Script Execution Behavior

## Panel Initialization Timing

A ScriptUI panel in After Effects is loaded **the moment it is opened from the Window menu**.
Even without restarting AE, closing and reopening the panel reloads the script.
This allows developers to update code and simply reopen the panel to reflect the changes during debugging.

## Target Engine and Memory Persistence

By specifying `#targetengine`, variables and functions are retained within the named engine.

* Running scripts multiple times with the same engine name will reuse the same variables.
* Without an engine specification, each execution creates a new memory space.
* Quitting AE clears the engine and resets variables.

This enables persistent state between executions, but renamed or removed variables may leave behind “garbage” definitions. If behavior becomes inconsistent, restarting AE resets the engine cleanly.

## UI Handling: Panel Mode vs Script Execution Mode

UI construction differs between **Panel Mode** and **Script Execution Mode**:

* **Panel Mode**

  * AE provides an empty `Panel` object (`this`) when the panel is opened.
  * After adding UI elements, `win.layout.layout(true)` must be called to refresh the layout.
  * `win.show()` is not required.

* **Script Execution Mode**

  * The script must explicitly create a window with `new Window("dialog" …)`.
  * `win.show()` is required to display the window and trigger the initial layout pass.

Correctly branching between these two ensures the UI works both as a script and as a dockable panel.

## Mode Detection with a Flag

To differentiate between modes, use `thisObj instanceof Panel`.
For cleaner code, store the result in a flag (`isPanelMode`) and reuse it.

Example:

```jsx
var isPanelMode = (thisObj instanceof Panel);
var win = isPanelMode ? thisObj : new Window("dialog", "OneEase_v2");

// build UI …

if (isPanelMode) {
    win.layout.layout(true);
} else {
    win.show();
}
```

* The mode is determined once at the start.
* At the end, the flag is checked again to decide whether to re-layout or show the window.
* This simplifies branching and makes the flow more maintainable.

---

## Summary

1. Panels load scripts when opened, and reloading occurs each time the panel is reopened.
2. `#targetengine` allows persistent variables and state across runs; restarting AE clears memory.
3. In Panel Mode, use `layout.layout(true)`; in Script Mode, use `win.show()`.
4. Managing the mode with an `isPanelMode` flag keeps the implementation clean and easy to follow.