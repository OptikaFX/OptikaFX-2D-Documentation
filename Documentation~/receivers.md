# Receivers and Shadow Areas

Receivers and Shadow Areas define where OptikaFX 2D shadows can appear.

A `Receiver` is the surface that displays shadows.

A `ShadowArea` defines what kind of area the object represents, such as ground, wall, water or hole.

## Index

- [Overview](#overview)
- [Receivers](#receivers)
- [Shadow Areas](#shadow-areas)
- [Receiver Types](#receiver-types)
- [Area Types](#area-types)
- [Ground Receivers](#ground-receivers)
- [Wall Receivers](#wall-receivers)
- [Water and Hole Areas](#water-and-hole-areas)
- [Tilemap Receivers](#tilemap-receivers)
- [Receiver Material](#receiver-material)
- [Wall ID](#wall-id)
- [Elevation Level](#elevation-level)
- [Shadow Alpha Mask](#shadow-alpha-mask)
- [Recommended Setup](#recommended-setup)
- [Useful C# Examples](#useful-c-examples)
  - [Get a ShadowReceiver](#get-a-shadowreceiver)
  - [Enable or disable a receiver](#enable-or-disable-a-receiver)
  - [Set receiver material](#set-receiver-material)
  - [Set elevation level](#set-elevation-level)
  - [Set ShadowArea type](#set-shadowarea-type)
- [Troubleshooting](#troubleshooting)

---

## Overview

OptikaFX uses receivers to display shadows on specific surfaces.

Examples:

- Ground
- Walls
- Tilemaps
- Floors
- Platforms
- Special shadow surfaces

Shadow Areas are used to classify these surfaces.

For example:

- Ground receives normal ground shadows.
- Walls receive wall bending/projected wall shadows.
- Water or holes can block or mask shadows.

---

## Receivers

![OptikaFX 2D Menu](./images/receiver.png)

Receivers are components that render shadow information onto an object.

Main receiver components:

| Component | Use |
|---|---|
| `ShadowReceiver` | Used for SpriteRenderer/MeshRenderer based objects. |
| `TilemapShadowReceiver` | Used for Tilemap based receivers. |

Receivers are usually added through the OptikaFX setup menu.

---

## Shadow Areas

`ShadowArea` defines what type of area an object represents.

It does not render the shadow by itself. It provides area information used by shadow mask rules.

ShadowArea types include:

| Type | Description |
|---|---|
| `Ground_Red` | Ground/floor area. |
| `Wall_Green` | Wall/vertical area. |
| `Hole_Water_Blue` | Blocks or masks shadows, usually holes or water. |
| `Special_Alpha` | Reserved for special/custom area behavior. |

---

## Receiver Types

OptikaFX supports:

| Receiver | Description |
|---|---|
| `ShadowReceiver` | Used for normal GameObjects with renderers. |
| `TilemapShadowReceiver` | Used for Unity Tilemaps. |

Use `ShadowReceiver` for:

- Sprite objects
- MeshRenderer objects
- Wall sprites
- Ground sprites
- Custom surface objects

Use `TilemapShadowReceiver` for:

- Ground tilemaps
- Wall tilemaps
- Floor tilemaps
- Large tile-based maps

---

## Area Types

Area types determine how shadows interact with the surface.

| Area | Color Channel | Common Use |
|---|---|---|
| Ground | Red | Floors, ground, walkable surfaces |
| Wall | Green | Walls, cliffs, vertical surfaces |
| Water/Hole | Blue | Holes, water, blocked areas |
| Special | Alpha | Custom rules or special masks |

These color associations are used internally by the mask system.

---

## Ground Receivers

Ground receivers are used for normal floor shadows.

Use them for:

- Terrain
- Floors
- Platforms
- Ground tilemaps
- Walkable surfaces

Setup:

    OptikaFX 2D / Add Receivers To Selected GameObject or Tilemap / Ground

This usually adds/configures:

- `ShadowArea`
- Area type as Ground
- Receiver rules for ground shadows

---

## Wall Receivers

Wall receivers are used for wall shadows and wall bending.

Use them for:

- Walls
- Cliffs
- Vertical surfaces
- Building sides
- Tilemap walls

Setup:

    OptikaFX 2D / Add Receivers To Selected GameObject or Tilemap / Wall

This usually adds/configures:

- `ShadowArea`
- `ShadowReceiver` or `TilemapShadowReceiver`
- Area type as Wall
- Unique Wall ID

Wall receivers are required for Caster Wall Bending.

---

## Water and Hole Areas

Water and hole areas are used to block or mask shadows.

Use them for:

- Water
- Pits
- Holes
- Void areas
- Shadow blocking masks

Setup:

    OptikaFX 2D / Add Receivers To Selected GameObject or Tilemap / Water or Hole

This usually adds/configures:

- `ShadowArea`
- Area type as Hole/Water
- Blue mask/channel behavior

These areas are commonly used with `BlockedBy` rules in shadow quads/materials.

---

## Tilemap Receivers

Tilemap receivers are used when shadows should appear on a tilemap.

Use `TilemapShadowReceiver` for wall tilemaps or tilemap surfaces that need receiver behavior.

Recommended for:

- Tilemap walls
- Tilemap floors
- Tilemap cliffs
- Large 2D levels

Tilemap receivers create a hidden tilemap used for shadow rendering.

---

## Receiver Material

Receivers need a material compatible with OptikaFX shadow rendering.

Usually this is assigned automatically from:

    Assets/OptikaFX 2D/Settings/ProjectDefaults.asset

Check that `Shadow Receiver Material` is assigned in ProjectDefaults.

If the receiver material is missing, shadows may not appear.

---

## Wall ID

Wall receivers use a unique Wall ID.

This helps the system identify wall ownership and receiver matching.

The Wall ID is generated automatically.

You usually do not need to edit it manually.

If duplicated Wall IDs are detected, the registry can assign new IDs automatically.

---

## Elevation Level

Receivers can use elevation levels.

Elevation is useful for:

- Multi-floor scenes
- Platforms
- Bridges
- Different height layers
- Sorting shadow levels

Caster and receiver elevation levels should match when needed.

If shadows appear on the wrong level, check:

- Caster elevation level
- Receiver elevation level
- ShadowRenderQuad elevation level

---

## Shadow Alpha Mask

Receivers can optionally use an alpha mask.

This allows parts of a receiver to ignore or weaken shadows.

Useful for:

- Grates
- Holes
- Transparent platforms
- Decorative alpha cutouts
- Partial shadow receiving surfaces

Important fields:

| Field | Description |
|---|---|
| `Shadow Alpha Mask` | Sprite used as alpha mask. |
| `Shadow Alpha Mask Scale` | Scale applied to the mask. |
| `Shadow Alpha Mask Strength` | Strength of the mask effect. |
| `Invert Shadow Alpha Mask` | Inverts the mask behavior. |
| `Shadow Alpha Mask Channel` | Selects Alpha, Red or Grayscale channel. |

---

## Recommended Setup

### Ground

1. Select ground object or ground tilemap.
2. Right-click in the Hierarchy.
3. Choose:

    OptikaFX 2D / Add Receivers To Selected GameObject or Tilemap / Ground

![OptikaFX 2D Menu](./images/add-receiver.png)

4. Check that `ShadowArea` was added.
5. Confirm area type is Ground.

### Wall

1. Select wall object or wall tilemap.
2. Right-click in the Hierarchy.
3. Choose:

    OptikaFX 2D / Add Receivers To Selected GameObject or Tilemap / Wall

4. Check that `ShadowArea` was added.
5. Check that `ShadowReceiver` or `TilemapShadowReceiver` was added.
6. Confirm area type is Wall.

### Water or Hole

1. Select water/hole object or tilemap.
2. Right-click in the Hierarchy.
3. Choose:

    OptikaFX 2D / Add Receivers To Selected GameObject or Tilemap / Water or Hole

4. Check that `ShadowArea` was added.
5. Confirm area type is Hole/Water.

---

## Useful C# Examples

---


## Get a ShadowReceiver
```csharp
using UnityEngine;
using OptikaFX;

public class ReceiverExample : MonoBehaviour
{
    private ShadowReceiver receiver;

    private void Awake()
    {
        receiver = GetComponent<ShadowReceiver>();
    }
}

```
---

## Enable or disable a receiver
```csharp
using UnityEngine;
using OptikaFX;

public class ToggleReceiverExample : MonoBehaviour
{
    [SerializeField]
    private ShadowReceiver receiver;

    public void SetReceiverEnabled(bool enabled)
    {
        if (receiver == null)
            return;

        receiver.isEnabled = enabled;
    }
}

```
---

## Set receiver material
```csharp
using UnityEngine;
using OptikaFX;

public class ReceiverMaterialExample : MonoBehaviour
{
    [SerializeField]
    private ShadowReceiver receiver;

    [SerializeField]
    private Material material;

    public void ApplyMaterial()
    {
        if (receiver == null)
            return;

        receiver.receiverMaterial = material;
    }
}

```
---

## Set elevation level
```csharp
using UnityEngine;
using OptikaFX;

public class ReceiverElevationExample : MonoBehaviour
{
    [SerializeField]
    private ShadowReceiver receiver;

    public void SetElevation(int elevation)
    {
        if (receiver == null)
            return;

        receiver.elevationLevel = Mathf.Clamp(elevation, 0, 5);
    }
}

```
---

## Set ShadowArea type
```csharp
using UnityEngine;
using OptikaFX;

public class ShadowAreaTypeExample : MonoBehaviour
{
    [SerializeField]
    private ShadowArea shadowArea;

    public void SetGround()
    {
        if (shadowArea == null)
            return;

        shadowArea.areaType = ShadowArea.AreaType.Ground_Red;
    }

    public void SetWall()
    {
        if (shadowArea == null)
            return;

        shadowArea.areaType = ShadowArea.AreaType.Wall_Green;
    }

    public void SetWaterOrHole()
    {
        if (shadowArea == null)
            return;

        shadowArea.areaType = ShadowArea.AreaType.Hole_Water_Blue;
    }
}

```
---

## Troubleshooting

### Shadows do not appear on the receiver

Check:

- Receiver component exists.
- Receiver is enabled.
- Receiver material is assigned.
- ShadowArea exists.
- Area type is correct.
- ShadowRenderQuad exists on the camera.
- GlobalLight exists.
- Caster exists and is generating shadows.

### Receiver material is empty

Check:

    Assets/OptikaFX 2D/Settings/ProjectDefaults.asset

Make sure `Shadow Receiver Material` is assigned.

Then re-run setup or add receiver again.

### Shadows appear on the wrong surface

Check:

- ShadowArea type.
- RequiredArea rules.
- BlockedBy rules.
- Elevation level.
- ShadowRenderQuad configuration.

### Wall bending does not work

Check:

- The object uses a `ShadowReceiver` or `TilemapShadowReceiver`.
- The ShadowArea type is Wall.
- The Caster has Wall Bending enabled.
- The Caster mode supports Wall Bending.
- The wall receiver has a valid Wall ID.
- Occluders are not compatible with Wall Bending.
- Check [Wall Bending Compatibility](wall-bending.md)



### Tilemap receiver does not update

Check:

- TilemapShadowReceiver exists.
- Hidden shadow tilemap exists.
- Auto sync is enabled.
- Tilemap has valid tiles.
- Sorting layer/order is correct.

### Water or holes do not block shadows

Check:

- ShadowArea type is Hole/Water.
- Shadow quad `BlockedBy` is set to Holes and Water.
- The mask material/shader is correctly assigned.
- The object is included in the shadow mask rendering path.

← [Back to Documentation Index](index.md)
