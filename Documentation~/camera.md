# Camera and Shadow Mattes

The camera setup is responsible for rendering the final OptikaFX 2D shadow layer.

OptikaFX uses a `ShadowRenderQuad` component on the camera. Each entry in this component is a shadow matte, also called a shadow quad.

A shadow matte is a full-screen or camera-aligned quad that displays the resolved shadow mask using specific rules such as shadow layer, area type, blocking and sorting.

## Index

- [Overview](#overview)
- [Camera Setup](#camera-setup)
- [ShadowRenderQuad](#shadowrenderquad)
- [What Are Shadow Mattes](#what-are-shadow-mattes)
- [Main Fields](#main-fields)
- [Shadow Matte Fields](#shadow-matte-fields)
- [Area Rules](#area-rules)
- [Blocked By Rules](#blocked-by-rules)
- [Shadow Layer](#shadow-layer)
- [Environment Shadow](#environment-shadow)
- [Sorting Layer and Order](#sorting-layer-and-order)
- [Multiple Shadow Mattes](#multiple-shadow-mattes)
- [Recommended Setup](#recommended-setup)
- [Useful C# Examples](#useful-c-examples)
  - [Get ShadowRenderQuad from camera](#get-shadowrenderquad-from-camera)
  - [Enable or disable first shadow matte](#enable-or-disable-first-shadow-matte)
  - [Set shadow matte material](#set-shadow-matte-material)
  - [Set sorting order](#set-sorting-order)
- [Troubleshooting](#troubleshooting)

---

## Overview

OptikaFX renders shadows through render textures and mask passes.

The final visible shadow layer is drawn by the camera using `ShadowRenderQuad`.

The `ShadowRenderQuad` contains one or more shadow mattes.

Each shadow matte defines:

- Which shadow layer it renders
- Where shadows are allowed to appear
- What blocks the shadows
- Whether environment shadows are included
- Which material is used
- Sorting layer and sorting order

---

## Camera Setup

Full Setup automatically configures the active camera.

Menu:

    Tools / OptikaFX 2D / 01. Run Full Setup

After setup, your main camera should have:

- `Camera`
- `ShadowRenderQuad`

The `ShadowRenderQuad` component is required for final shadow display.

If the camera does not have this component, shadows may be generated internally but not visible on screen.

---

## ShadowRenderQuad

![OptikaFX 2D Menu](./images/configure-camera.png)

`ShadowRenderQuad` is attached to the camera.

It creates and updates shadow matte quads as camera children.

These quads are usually hidden/internal generated objects.

The component contains a list:

    Shadow Quads

Each element in this list represents one shadow matte.

---

## What Are Shadow Mattes

A shadow matte is a camera-aligned quad that receives and displays the resolved shadow mask.

Think of it as a visual layer where shadows are drawn.

You can use multiple mattes for:

- Different shadow layer
- Different sorting layers
- Ground-only shadows
- Wall-only shadows
- Environment shadows
- Special rendering passes

---

## Main Fields

Common ShadowRenderQuad fields include:

| Field | Description |
|---|---|
| `Shadow mattes` | List of shadow matte configurations. |
| `Final Shadow Color` | Controls how final shadow tint is resolved. |
| `Use Day Night Shadow Tint` | Uses GlobalLight or TimeManager shadow color when available. |
| `Fallback Shadow Tint` | Color used if no GlobalLight/TimeManager color is available. |

Names may vary depending on inspector version.

---

## Shadow Matte Fields

Each shadow matte entry can contain:

| Field | Description |
|---|---|
| `Is Enabled` | Enables or disables this shadow matte. |
| `Floor Name` | Display name for the matte. |
| `Shadow Layer` | Shadow layer rendered by this matte. |
| `Required Area` | Defines where this matte can draw shadows. |
| `Blocked By` | Defines what blocks this matte. |
| `Use Environment Shadow` | Includes environment/occluder shadows. |
| `Environment Shadow Strength` | Strength of environment shadows. |
| `Environment Shadow Ignores Area Rules` | Allows environment shadows to ignore Required Area. |
| `Environment Shadow Ignores Blocked By` | Allows environment shadows to ignore Blocked By rules. |
| `Sorting Layer Name` | Sorting layer used by the matte renderer. |
| `Sorting Order` | Sorting order used by the matte renderer. |
| `Quad Material` | Material used to render this matte. |

---

## Area Rules

`Required Area` controls where the matte can draw shadows.

Common options:

| Required Area | Description |
|---|---|
| `Anywhere` | Shadows can appear on any valid area. |
| `Only On Ground` | Shadows appear only on ground/floor areas. |
| `Only On Wall` | Shadows appear only on wall areas. |

Use this to separate ground shadows from wall shadows.

Example:

- Ground matte uses `Only On Ground`
- Wall matte uses `Only On Wall`

---

## Blocked By Rules

`Blocked By` controls what can block shadows.

Common options:

| Blocked By | Description |
|---|---|
| `Nothing` | Nothing blocks this matte. |
| `Holes And Water` | Hole/water areas block shadows. |

Use `Holes And Water` when shadows should not appear over holes, water or void areas.

---

## Shadow Layer

Shadow layer lets you render shadows for different height layers.

Use shadow layer for:

- Platforms
- Bridges
- Multi-floor scenes
- Raised terrain
- Separate vertical layers
- Cloud or tree shadows

Caster, receiver and Shadow Layer should match when needed.

Example:

| Object | Shadow Layer |
|---|---:|
| Ground receiver | `0` |
| Player caster | `0` |
| Shadow matte | `0` |

For a raised platform:

| Object | Shadow Layer |
|---|---:|
| Platform receiver | `1` |
| Platform caster | `1` |
| Shadow matte | `1` |

---

## Environment Shadow

Environment shadows usually come from occluders or environmental shadow passes.

Use:

    Use Environment Shadow

when this matte should include environment/occluder shadows.

Useful settings:

| Field | Description |
|---|---|
| `Environment Shadow Strength` | Controls environment shadow opacity. |
| `Environment Shadow Ignores Area Rules` | Environment shadows draw even if area rules would block them. |
| `Environment Shadow Ignores Blocked By` | Environment shadows ignore hole/water blocking rules. |

Recommended:

- Enable environment shadow on only one main matte unless you need multiple environment layers.
- Avoid duplicating environment shadow on many mattes unless intentional.

---

## Sorting Layer and Order

The matte is rendered using Unity sorting.

Important fields:

| Field | Description |
|---|---|
| `Sorting Layer Name` | Unity sorting layer. |
| `Sorting Order` | Draw order inside the sorting layer. |

If shadows appear behind or in front of the wrong objects, adjust sorting.

Common setup:

| Use | Sorting Layer | Sorting Order |
|---|---|---:|
| Ground shadows | Default | `-100` |
| Wall shadows | Default | `-90` |
| Special overlay shadows | Custom layer | Depends on project |

Sorting depends heavily on your project rendering order.

---

## Multiple Shadow Mattes

You can use multiple shadow mattes.

Examples:

### One simple matte

Use one matte for all basic shadows.

Recommended for simple projects.

### Ground and wall mattes

Use two mattes:

| Matte | Required Area | Purpose |
|---|---|---|
| Ground Matte | Only On Ground | Ground shadows |
| Wall Matte | Only On Wall | Wall shadows |

### Shadow layer

Use one matte per Shadow Layer:

| Matte | Shadow Layer |
|---|---:|
| Ground Level Matte | 0 |
| Platform Matte | 1 |
| Upper Platform Matte | 2 |

### Environment matte

Use one matte with environment shadow enabled.

This allows occluders/environment masks to be shown.

---

## Recommended Setup

For most projects:

1. Run Full Setup.
2. Select the Main Camera.
3. Confirm `ShadowRenderQuad` exists.
4. Open `Shadow Mattes`.
5. Make sure at least one matte is enabled.
6. Confirm `Quad Material` is assigned.
7. Set `Required Area` to `Anywhere` or `Only On Ground`.
8. Set `Blocked By` to `Holes And Water` if needed.
9. Set Sorting Layer and Order to match your scene.

Simple recommended matte:

| Field | Value |
|---|---|
| `Is Enabled` | Enabled |
| `Shadow Layer` | `0` |
| `Required Area` | `Anywhere` |
| `Blocked By` | `Holes And Water` |
| `Use Environment Shadow` | Enabled |
| `Sorting Layer Name` | `Default` |
| `Sorting Order` | `-100` |
| `Quad Material` | Default ShadowMaskSortedQuad material |

---

## Useful C# Examples

---

## Get ShadowRenderQuad from camera
```csharp
using UnityEngine;
using OptikaFX;

public class ShadowRenderQuadExample : MonoBehaviour
{
    private ShadowRenderQuad shadowRenderQuad;

    private void Awake()
    {
        Camera cam = Camera.main;

        if (cam == null)
            return;

        shadowRenderQuad = cam.GetComponent<ShadowRenderQuad>();
    }
}

```
---

## Enable or disable first shadow matte
```csharp
using UnityEngine;
using OptikaFX;

public class ToggleFirstShadowMatte : MonoBehaviour
{
    [SerializeField]
    private ShadowRenderQuad shadowRenderQuad;

    public void SetFirstMatteEnabled(bool enabled)
    {
        if (shadowRenderQuad == null)
            return;

        if (shadowRenderQuad.shadowQuads == null || shadowRenderQuad.shadowQuads.Count == 0)
        {
            return;
        }

        shadowRenderQuad.shadowQuads[0].isEnabled = enabled;
    }
}

```
---

## Set shadow matte material
```csharp
using UnityEngine;
using OptikaFX;

public class ShadowMatteMaterialExample : MonoBehaviour
{
    [SerializeField]
    private ShadowRenderQuad shadowRenderQuad;

    [SerializeField]
    private Material matteMaterial;

    public void ApplyMaterialToFirstMatte()
    {
        if (shadowRenderQuad == null)
            return;

        if (shadowRenderQuad.shadowQuads == null || shadowRenderQuad.shadowQuads.Count == 0)
        {
            return;
        }

        shadowRenderQuad.shadowQuads[0].quadMaterial = matteMaterial;
    }
}

```
---

## Set sorting order
```csharp
using UnityEngine;
using OptikaFX;

public class ShadowMatteSortingExample : MonoBehaviour
{
    [SerializeField]
    private ShadowRenderQuad shadowRenderQuad;

    public void SetFirstMatteSortingOrder(int order)
    {
        if (shadowRenderQuad == null)
            return;

        if (shadowRenderQuad.shadowQuads == null || shadowRenderQuad.shadowQuads.Count == 0)
        {
            return;
        }

        shadowRenderQuad.shadowQuads[0].sortingOrder = order;
    }
}

```
---

## Troubleshooting

### Shadows are not visible

Check:

- The camera has a `ShadowRenderQuad` component.
- At least one shadow matte is enabled.
- `Quad Material` is assigned.
- Sorting Layer and Sorting Order are correct.
- GlobalLight exists.
- Caster exists.
- Receiver exists.
- Render Features are installed.

### Shadows render behind sprites

Adjust:

- Shadow matte `Sorting Layer Name`
- Shadow matte `Sorting Order`
- SpriteRenderer sorting settings
- TilemapRenderer sorting settings

### Shadows render over everything

Lower the shadow matte sorting order.

Example:

    Sorting Order: -100

Or use a sorting layer that renders below characters.

### Shadows appear on wrong area

Check:

- `Required Area`
- `Blocked By`
- ShadowArea type
- Receiver type
- Shadow Layer

### Environment shadows are duplicated

Check if `Use Environment Shadow` is enabled on more than one matte.

Usually only one main matte should include environment shadows.

### Shadows appear on water or holes

Set:

    Blocked By: Holes And Water

Also make sure water/hole objects have the correct `ShadowArea` type.

### Shadows are missing on walls

Check:

- Wall receiver exists.
- ShadowArea type is Wall.
- A matte exists for wall shadows.
- `Required Area` allows wall rendering.
- Sorting order places wall shadows correctly.

← [Back to Documentation Index](index.md)
