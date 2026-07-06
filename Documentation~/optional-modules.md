
# Optional Modules

OptikaFX 2D keeps the core shadow system independent from optional Unity packages.

This means the main shadow framework can work without TextMeshPro or Input System.

Optional features become available only when their required Unity packages are installed.

## Index

- [Overview](#overview)
- [Why Optional Modules Exist](#why-optional-modules-exist)
- [Available Optional Modules](#available-optional-modules)
- [TextMeshPro Module](#textmeshpro-module)
- [Input System Module](#input-system-module)
- [Installing Optional Dependencies](#installing-optional-dependencies)
- [Recommended Setup](#recommended-setup)
- [Troubleshooting](#troubleshooting)

---

## Overview

Optional modules are features that depend on external Unity packages.

Currently, OptikaFX can use:

- TextMeshPro for Clock UI
- Input System for runtime debug input tools

These are not required for the main shadow system.

The core package still supports:

- Casters
- Receivers
- Occluders
- Global Light
- Local Lights
- TimeManager
- Render Features
- Editor setup tools

---

## Why Optional Modules Exist

Some Unity projects do not use TextMeshPro or the Input System.

If OptikaFX required them by default, every user would be forced to install extra packages even if they only wanted shadows.

By separating optional modules:

- The core package stays lighter.
- Projects avoid unnecessary dependencies.
- Users can enable only the features they need.
- Optional UI/debug tools remain available when dependencies are installed.

---

## Available Optional Modules

| Feature | Required Package | Used For |
|---|---|---|
| Clock UI | TextMeshPro | Time/date UI display |
| Runtime Debug Input Tools | Input System | Runtime debug shortcuts and toggles |

---

## TextMeshPro Module

TextMeshPro is required only for Clock UI.

Clock UI uses TextMeshPro to display:

- Time
- Day
- Season
- Custom date text

If TextMeshPro is not installed, the core OptikaFX shadow system still works.

When creating Clock UI from the TimeManager inspector, OptikaFX can ask for permission to install TextMeshPro automatically.

For more information, see:

- [Clock UI](clock-ui.md)

---

## Input System Module

Input System is required only for optional runtime debug input tools.

These tools can be used for:

- Debug shortcuts
- Runtime toggles
- Performance/debug overlays

If Input System is not installed, the core OptikaFX shadow system still works.

Only the optional runtime debug input tools are unavailable.

---

## Installing Optional Dependencies

### TextMeshPro

To use Clock UI, install TextMeshPro through one of these options:

1. Select the `TimeManager`.
2. Open the `Clock UI` section.
3. Click:

    Install TextMeshPro and Create Clock UI

Or install manually through Unity Package Manager:

    Window / Package Manager / TextMeshPro

After installation, wait for Unity to finish compiling.

---

### Input System

To use runtime debug input tools, install Input System manually through Unity Package Manager:

    Window / Package Manager / Input System

After installation, Unity may ask to enable the new Input System backend.

If you do not use the runtime debug tools, you do not need to install Input System.

---

## Recommended Setup

For most users:

1. Use OptikaFX core without optional dependencies.
2. Install TextMeshPro only if Clock UI is needed.
3. Install Input System only if runtime debug controls are needed.

Recommended:

| Need | Install |
|---|---|
| Shadows only | Nothing optional |
| Clock UI | TextMeshPro |
| Runtime debug shortcuts | Input System |
| Clock UI and debug tools | TextMeshPro and Input System |

---

## Troubleshooting

### Clock UI option asks for TextMeshPro

This is expected.

Clock UI requires TextMeshPro.

Click:

    Install TextMeshPro and Create Clock UI

or install TextMeshPro from Unity Package Manager.

---

### Clock UI is not available after installing TextMeshPro

Try:

- Wait for Unity to finish compiling.
- Reopen the TimeManager inspector.
- Click Create Clock UI again.
- Check the Console for errors.

---

### Runtime debug controls do not work

Check:

- Input System is installed.
- Unity has finished compiling.
- The debug tool object exists in the scene.
- The debug tool component is enabled.
- The correct keys/actions are configured.

---

### Unity asks to enable the new Input System backend

This can happen after installing Input System.

If you want to use runtime debug input tools, accept the prompt and allow Unity to restart.

If you do not need runtime debug input tools, you can keep using your current input setup and avoid installing Input System.

---

### Core shadows do not work after installing optional packages

Optional packages should not affect the core shadow system.

Check:

- The Console for errors.
- Render Features are installed.
- The camera has `ShadowRenderQuad`.
- Receivers and casters are configured.
- GlobalLight exists.

For general issues, see:

- [Troubleshooting](troubleshooting.md)

---

← [Back to Documentation Index](index.md)
