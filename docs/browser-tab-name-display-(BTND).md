# Browser Tab Name Display (BTND)

Browser Tab Name Display (BTND) is a proposed PowerToys utility for Windows 11 that displays the title of the currently active browser tab in a compact, taskbar-adjacent strip so that it remains readable even when tab captions become too small to be useful. This initial proposal is limited to Google Chrome, Mozilla Firefox, and Microsoft Edge.

## Overview

BTND is intended to be universal at the platform level in how it detects the active foreground window, but in practical day-to-day use it is focused on maximized browser workflows where many tabs are open in a single visible window. The first implementation should rely on standard Windows APIs to read the title of the current foreground window so that the utility remains lightweight, broadly compatible, and appropriately scoped for PowerToys.

BTND combines two core functions: it displays the active tab title in a readable taskbar-adjacent strip, and it provides left and right arrow controls in that same taskbar area so the user can step through tabs while immediately seeing the title of the newly active tab. The utility is therefore not just a passive display mechanism; it is a taskbar-based tab identification and navigation tool.

## Problem

When many tabs are open in a browser, the visible text on those tabs becomes too small to read comfortably, and the user may know only roughly where the desired tab is located. This creates both a readability problem and a selection problem, because aiming at tiny tab headers also increases the chance of accidentally clicking a nearby close button or the wrong tab.

Existing shortcuts such as Ctrl+Tab, Ctrl+Shift+Tab, Ctrl+PageDown, and Ctrl+PageUp can move between tabs, but they do not solve the identification problem because they switch focus directly into tab content rather than presenting a stable readable label first. For example, a user with 40 tabs open in Edge could use BTND's taskbar arrows to step through tabs while reading each full tab title as it becomes active, instead of hunting for tiny compressed tab labels.

## Proposed behavior

BTND would present a small collapsible title strip immediately above the Windows taskbar near the Start area instead of using a floating overlay that must be manually positioned. The strip would occupy a narrow reserved area, approximately 2 inches wide and about 0.4 inches high, with the right edge of that area aligned with the left edge of the Windows Start icon, and the displayed title would wrap into two lines in a tooltip-like font style for readability.

The strip should be non-obscuring by design and should appear only in reserved or safe space so that it never covers important desktop content, shell information, application status bars, or specialized software interface elements. The expand/collapse control should appear in the taskbar near the Start button, with left and right arrow controls beside it as a core part of the BTND interface for tab stepping.

## Activation and reset

BTND should work only when a supported browser window is maximized and is the foreground window currently visible to the user. When the user clicks the up arrow, the feature turns on for that active window, but the strip initially appears as a blank or inactive shaded strip until the user initializes it by clicking a tab in that same window.

After that initialization click, the title of the selected tab should appear in the strip, and subsequent tab changes within that same window should update the displayed title. The feature should automatically deactivate whenever the user minimizes the window, restores it to a non-maximized size, or switches focus to another supported browser window, at which point the user must reinitialize the feature for the newly focused window if they want BTND active there.

## Visibility rules

Visibility follows the activation rules described above: only the current maximized foreground supported browser window can show a title in the strip. If the active window is resized, restored, minimized, or otherwise no longer the visible foreground maximized window, the title strip should remain hidden or inactive.

In a desktop containing a mix of maximized and resized windows, the strip should respond only to the window currently in front, and a maximized window behind another foreground window must not cause any title to be shown. This keeps the behavior aligned with the intended active-session workflow inside one visible browser window at a time.

## Tab stepping

A core part of BTND is the ability to move left and right through tabs from the taskbar while reading each active tab title in the strip. Once the feature has been initialized for the current window, the user should be able to move left or right from tab to tab by using the taskbar arrows, with the name of the newly selected tab displayed in the strip after each move.

This feature is not meant merely to duplicate existing keyboard shortcuts. Avoiding awkward shortcut combinations is part of the benefit, but the main benefit is that the user can step through tabs while seeing each newly active tab name immediately displayed in the BTND strip, creating a more precise and readable tab-browsing workflow and reducing the need to aim repeatedly at very small tab headers.

The left and right arrow icons should always be present in the interface. When the user clicks the left arrow, the utility should first attempt Ctrl+PageUp, and when the user clicks the right arrow, it should first attempt Ctrl+PageDown; if the tracked title remains unchanged after approximately 200 ms, the utility should then fall back to Ctrl+Shift+Tab for left movement or Ctrl+Tab for right movement.

## Supported tab-stepping scope

To keep implementation cost and maintenance burden proportionate to user benefit, the left/right tab-stepping feature should initially be limited to Google Chrome, Microsoft Edge, and Mozilla Firefox. These three browsers represent the most important and most stable use cases for the feature, align most closely with the proposal's core high-tab workflow, and provide the largest practical share of the utility's value.

Restricting early support to these browsers would reduce the need for application-specific exception handling, compatibility testing, and ongoing maintenance across a wider range of less consistent tabbed applications. It would also make the feature easier to explain, validate, and support within PowerToys.

## Target applications

The primary and only in-scope targets for this initial proposal are Google Chrome, Mozilla Firefox, and Microsoft Edge. Tabbed PDF viewers, XPS-style viewers, Adobe Acrobat, Acrobat Reader, Microsoft Office applications, and other tab-oriented desktop applications are out of scope for this proposal.

## Technical approach

The preferred first implementation is a lightweight native Windows desktop utility using the Win32 API, built for x64 and ARM64 Windows 11 systems, that detects the active foreground window, reads its caption text, determines whether that window is maximized, and displays the title in a passive taskbar-adjacent surface.

Recommended first-prototype choices include:

- Window tracking via `GetForegroundWindow`, `GetWindowTextW`, and `IsZoomed`.
- Overlay display via a small custom-drawn Win32 popup using `WM_PAINT` and `DrawTextW`.
- A 200 ms timer-driven update loop.
- Taskbar-aware positioning using `SHAppBarMessage(ABM_GETTASKBARPOS)`.
- Non-activating behavior using styles such as `WS_EX_NOACTIVATE`.

## Staged implementation

- **Stage 1:** Foreground window caption display and left/right tab stepping for the active maximized supported browser window, with explicit user activation and per-window initialization behavior.
- **Stage 2:** Application-aware shortcut refinement where supported browser behavior benefits from browser-specific routing.
- **Stage 3:** Optional browser-specific title integration only if ordinary browser titles prove insufficient.

## Estimated implementation effort

As currently scoped, BTND appears to be a modest but non-trivial PowerToys utility rather than a very small feature. A realistic estimate for a credible first implementation is roughly 300 to 450 hours of engineering work, or about 6 to 12 developer-weeks, assuming the proposal includes both the taskbar-adjacent title strip and the required left/right tab-stepping behavior for Chrome, Firefox, and Edge.

## Why PowerToys fits

BTND fits PowerToys because it is a lightweight productivity enhancement for Windows users rather than a change to any single application. It also aligns with the PowerToys pattern of providing practical shell-adjacent utilities that improve workflow without unsupported modifications to the Windows shell itself.

## Mockup

![BTND mockup](../images/tab-name-display-mockup.png)

[← Back to summary](../README.md)
```
