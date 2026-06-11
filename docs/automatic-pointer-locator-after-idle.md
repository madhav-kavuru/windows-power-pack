# Add an automatic pointer-location option after idle in Mouse Properties

## Description

Windows already includes a built-in option called **"Show location of pointer when I press the CTRL key"** in Mouse Properties under **Pointer Options**. This proposal is to add a second, closely related option directly beneath it: an automatic pointer-location trigger after idle.

Instead of requiring the user to press Ctrl, Windows would automatically show the same animated circle pointer locator when the pointer resumes movement after a user-configurable idle threshold. This reuses the existing visual behavior and a familiar accessibility-oriented feature, but makes it available automatically at the moment it is most useful.

For longer idle periods where the display has turned off or the user has signed back in, the animation timing and repetition could also be adjusted slightly so the circles are more likely to be noticed after the screen fully returns.

## Reproduction / current behavior

1. Open **Mouse Properties → Pointer Options**.
2. Turn on **"Show location of pointer when I press the CTRL key"**.
3. Use the PC, then step away or spend time reading a long document.
4. When you return, it is often hard to see where the pointer is.

Current behavior:

- The only way to use the animated circle pointer locator is to press Ctrl.
- There is no option to show those circles automatically after idle, exactly when users typically lose track of the pointer.
- When the display has turned off or the user has just signed back in, the first pointer movement may happen while the screen is still resuming, making it easy to miss the circles if they appear immediately.

## Expected behavior

Add a new setting directly under **"Show location of pointer when I press the CTRL key"** in **Mouse Properties → Pointer Options**:

- A checkbox such as **"Show pointer location automatically after idle"**.
- When enabled, allow the user to choose an idle time threshold, for example between 4 seconds and about 3 minutes, using minutes and seconds fields.
- If the pointer remains stationary for longer than the selected idle time, then on the **next mouse or touchpad movement**, Windows shows the same circle animation currently used for the Ctrl-key feature, once, on that first movement.
- The existing Ctrl-key behavior remains available and unchanged.

### Wake/lock refinement

- In scenarios where the display has turned off to save power or the user has had to sign back in after a longer idle period, Windows could delay showing the pointer-location circles by about 1–2 seconds after the first pointer movement and optionally play the animation twice in succession.
- This would help ensure the circles appear when the screen content is fully visible and the user is actually looking at the display.
- For example, this refinement would be most relevant after longer idle periods, such as when the display has turned off after roughly 5–10 minutes of inactivity or when the user returns to the desktop after a 10–15 minute lock period, so the circles appear clearly once the screen is fully visible again.
- This enhanced timing and repetition would be reserved for display-wake and unlock scenarios. In normal use where the screen remains on, the existing single, immediate animation would still be used.

This would provide a minimal, automatic way to locate the pointer right when the user resumes interaction, while still keeping the visual effect lightweight and familiar.

## Rationale / supporting info

This proposal is intended as a small, low-risk enhancement that **reuses the existing "Show location of pointer when I press the CTRL key" behavior** rather than introducing a new visual system. The main changes are adding an optional idle-based trigger and a simple timing adjustment for wake/lock scenarios, while keeping the current Ctrl-key option intact.

The goal is to get more value out of code and visuals that are already in Windows, not to build a large new feature from scratch.

- Users often lose track of the pointer after breaks, reading, or multi-monitor work and resort to "wiggling" the mouse to find it.
- The current Ctrl-key option is helpful but depends on a deliberate keyboard press and can be missed while the user is looking at the keyboard instead of the screen.
- Reusing the existing circle animation keeps the change small in scope: it mainly requires an idle timer, a new setting, and a call to the same logic used when Ctrl is pressed.
- Handling wake/lock scenarios with a short delay and optional second animation makes the feature more reliable in real-world power-saving situations without changing everyday short-idle behavior.

## Device / OS details

This is intended as a general feature for all supported x64 and ARM64 Windows 10 and Windows 11 systems, including laptops, desktops, and both single-monitor and multi-monitor setups.

[← Back to summary](../README.md)
```
