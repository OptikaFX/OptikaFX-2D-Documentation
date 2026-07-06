
# Global Light

The `GlobalLight` component controls the main shadow direction, color, intensity and projection behavior for OptikaFX 2D.

It can work manually, through a fixed profile, or be driven by the `TimeManager`.

## Index

- [Overview](#overview)
- [Global Light Modes](#global-light-modes)
- [Manual Mode](#manual-mode)
- [TimeManager Mode](#timemanager-mode)
- [Profile Mode](#profile-mode)
- [Automatic TimeManager Sync](#automatic-timemanager-sync)
- [Main Fields](#main-fields)
- [Shadow Visuals and Softness](#shadow-visuals-and-softness)
- [Recommended Setup](#recommended-setup)
- [Useful C# Examples](#useful-c-examples)
  - [Get the GlobalLight instance](#get-the-globallight-instance)
  - [Set manual shadow angle](#set-manual-shadow-angle)
  - [Set shadow color](#set-shadow-color)
  - [Set intensity and projection length](#set-intensity-and-projection-length)
  - [Switch to Manual mode](#switch-to-manual-mode)
  - [Switch to TimeManager mode](#switch-to-timemanager-mode)
  - [Switch to Profile mode](#switch-to-profile-mode)
  - [Assign a ShadowProfile](#assign-a-shadowprofile)
  - [Read effective light direction](#read-effective-light-direction)
  - [Read effective shadow color](#read-effective-shadow-color)
- [Troubleshooting](#troubleshooting)

---

## Overview

`GlobalLight` is the main global shadow controller used by OptikaFX 2D.

![OptikaFX 2D Menu](./images/global-light.png)

It defines:

-   Shadow angle
-   Shadow direction
-   Shadow intensity
-   Shadow color
-   Projection length
-   Shadow softness
-   Optional profile-based shadow values

Usually there should be only one active `GlobalLight` in the scene.

---

## Global Light Modes

The `GlobalLight` has three modes:

---
| Mode | Description |
|---|---|
| `Manual` | Uses the values directly set in the GlobalLight inspector. |
| `TimeManager` | Values are driven by the active `TimeManager` and its `DayLightingPreset`. |
| `Profile` | Uses a fixed `ShadowProfile` instead of animated time values. |

---

## Manual Mode

Manual mode is used when you want full direct control over the global shadow.

Use this mode when:

- You do not need day/night transitions
- You want a static lighting direction
- You are testing shadow settings
- You are setting up a scene manually

In Manual mode, the main values are:

- `Light Angle`
- `Intensity`
- `Projection Length`
- `Flatten`
- `Shadow Color`

---

## TimeManager Mode

TimeManager mode is used when day/night lighting should control the global shadow.

When the `TimeManager` component is enabled, `GlobalLight` can automatically switch to:

```
TimeManager
```

In this mode, the `TimeManager Component` updates:

- Time of day
- Shadow angle
- Shadow intensity
- Projection length
- Flatten
- Shadow color
- Shadow softness and visual overrides

This mode is recommended for games with dynamic day/night cycles.

---

## Profile Mode

Profile mode uses a fixed `ShadowProfile`.

Use this mode when:

- You want reusable static shadow settings
- You want profile-based lighting without TimeManager
- You want to test a specific `ShadowProfile`

If no profile is assigned, the system can fallback to the default profile from `ProjectDefaults`.

---

## Automatic TimeManager Sync

If `Sync Mode With TimeManager State` is enabled:

-   When an active `TimeManager` exists, GlobalLight switches to  `TimeManager`  mode.
-   When no active `TimeManager` exists, GlobalLight returns to  `Manual`  mode.

If  `Restore Manual Values When TimeManager Is Inactive`  is enabled, previous manual values are restored after the TimeManager is disabled.

This prevents the GlobalLight from staying stuck with TimeManager-driven values after TimeManager is turned off.

---


## Main Fields

| Field | Description |
|---|---|
| `Mode` | Current GlobalLight mode. |
| `Sync Mode With TimeManager State` | Automatically switches between Manual and TimeManager modes. |
| `Restore Manual Values When TimeManager Is Inactive` | Restores previous manual values after TimeManager becomes inactive. |
| `Light Angle` | Direction of the global shadow. |
| `Intensity` | Overall global shadow intensity. |
| `Projection Length` | Length multiplier for projected shadows. |
| `Flatten` | Controls vertical flattening of projected shadows. |
| `Shadow Color` | Color used by global shadows. |
| `Profile` | Optional profile used in Profile mode. |

---

## Shadow Visuals and Softness

The GlobalLight can override visual shadow settings.

Useful fields:

---
| Field | Description |
|---|---|
| `Override Shadow Visuals` | Enables GlobalLight-specific visual overrides. |
| `Shadow Alpha Multiplier` | Multiplies shadow opacity. |
| `Shadow Softness` | Artist-friendly softness control. |
---



When override is disabled, the system uses the visual settings from the active `ShadowProfile`.

---

## Recommended Setup

For a simple scene:

1.  Run Full Setup.
2.  Select the  `Global Light`  object.
3.  Keep mode as  `Manual`.
4.  Adjust  `Light Angle`,  `Intensity`,  `Projection Length`  and  `Shadow Color`.

For day/night lighting:

1.  Select the  `TimeManager`  object.
2.  Enable the  `TimeManager`  component.
3.  Make sure a  `DayLightingPreset`  is assigned.
4.  GlobalLight should switch to  `TimeManager`  mode automatically.

---

## Useful C# Examples

---

## Get the GlobalLight instance

```csharp
using UnityEngine;
using OptikaFX;

public class GlobalLightExample : MonoBehaviour
{
    private void Start()
    {
        GlobalLight light = GlobalLight.Instance;

        if (light == null)
        {
            Debug.LogWarning("No GlobalLight found.");
            return;
        }

        Debug.Log("GlobalLight found: " + light.name);
    }
}

```
---

## Set manual shadow angle

```csharp
using UnityEngine;
using OptikaFX;

public class SetGlobalShadowAngle : MonoBehaviour
{
    public void SetEveningAngle()
    {
        if (GlobalLight.Instance == null)
            return;

        GlobalLight.Instance.mode =
            GlobalLight.GlobalLightMode.Manual;

        GlobalLight.Instance.lightAngle =
            135f;
    }
}
```
---

## Set shadow color
```csharp
using UnityEngine;
using OptikaFX;

public class SetGlobalShadowColor : MonoBehaviour
{
    public void SetBlueShadow()
    {
        if (GlobalLight.Instance == null)
            return;

        GlobalLight.Instance.mode =
            GlobalLight.GlobalLightMode.Manual;

        GlobalLight.Instance.shadowColor =
            new Color(0.1f, 0.15f, 0.35f, 0.8f);
    }
}
```
---

## Set intensity and projection length
```csharp
using UnityEngine;
using OptikaFX;

public class SetGlobalShadowStrength : MonoBehaviour
{
    public void ApplyStrongShadow()
    {
        if (GlobalLight.Instance == null)
            return;

        GlobalLight.Instance.mode =
            GlobalLight.GlobalLightMode.Manual;

        GlobalLight.Instance.intensity =
            0.85f;

        GlobalLight.Instance.projectionLength =
            1.4f;
    }
}
```
---

## Switch to Manual mode
```csharp
using UnityEngine;
using OptikaFX;

public class SwitchGlobalLightManual : MonoBehaviour
{
    public void SwitchToManual()
    {
        if (GlobalLight.Instance == null)
            return;

        GlobalLight.Instance.mode =
            GlobalLight.GlobalLightMode.Manual;
    }
}
```
---

## Switch to TimeManager mode
```csharp
using UnityEngine;
using OptikaFX;

public class SwitchGlobalLightTimeManager : MonoBehaviour
{
    public void SwitchToTimeManager()
    {
        if (GlobalLight.Instance == null)
            return;

        GlobalLight.Instance.mode =
            GlobalLight.GlobalLightMode.TimeManager;
    }
}
```
---

## Switch to Profile mode
```csharp
using UnityEngine;
using OptikaFX;

public class SwitchGlobalLightProfile : MonoBehaviour
{
    public void SwitchToProfile()
    {
        if (GlobalLight.Instance == null)
            return;

        GlobalLight.Instance.mode =
            GlobalLight.GlobalLightMode.Profile;
    }
}
```
---

## Assign a ShadowProfile
```csharp
using UnityEngine;
using OptikaFX;

public class AssignGlobalLightProfile : MonoBehaviour
{
    [SerializeField]
    private ShadowProfile profile;

    public void ApplyProfile()
    {
        if (GlobalLight.Instance == null)
            return;

        GlobalLight.Instance.profile =
            profile;

        GlobalLight.Instance.mode =
            GlobalLight.GlobalLightMode.Profile;
    }
}
```
---

## Read effective light direction
```csharp
using UnityEngine;
using OptikaFX;

public class ReadGlobalLightDirection : MonoBehaviour
{
    private void Update()
    {
        if (GlobalLight.Instance == null)
            return;

        Vector2 direction =
            GlobalLight.Instance.EffectiveLightDirection;

        Debug.DrawRay(
            Vector3.zero,
            direction,
            Color.yellow
        );
    }
}
```
---

## Read effective shadow color
```csharp
using UnityEngine;
using OptikaFX;

public class ReadGlobalShadowColor : MonoBehaviour
{
    private void Start()
    {
        if (GlobalLight.Instance == null)
            return;

        Color color =
            GlobalLight.Instance.EffectiveShadowColor;

        Debug.Log("Effective shadow color: " + color);
    }
}
```
---

## Troubleshooting

### Global Light does not follow TimeManager

Check:

- TimeManager component is enabled
- GlobalLight mode is `TimeManager`
- `Control OptikaFX Global Light` is enabled on TimeManager
- A `DayLightingPreset` is assigned

### Global Light stays in TimeManager mode after disabling TimeManager

Check:

- `Sync Mode With TimeManager State` is enabled
- There is no other active TimeManager in the scene
- The TimeManager component is disabled
- The TimeManager GameObject is not duplicated in another loaded scene

### Manual values are lost after disabling TimeManager

Enable:
```text
Restore Manual Values When TimeManager Is Inactive
```
This stores manual values before TimeManager takes control and restores them when TimeManager becomes inactive.

### Shadows are too dark or too strong

Adjust:

- `Intensity`
- `Shadow Color` alpha
- `Shadow Alpha Multiplier`
- `Projection Length`
- The active `ShadowProfile`

← [Back to Documentation Index](index.md)
