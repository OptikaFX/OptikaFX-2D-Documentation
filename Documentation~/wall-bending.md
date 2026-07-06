# Wall Bending Compatibility

Wall Bending is a Caster feature used to make projected shadows bend, clip or interact with wall receivers.

Not every OptikaFX component supports Wall Bending.

## Index

- [Overview](#overview)
- [Compatible Components](#compatible-components)
- [Not Compatible Components](#not-compatible-components)
- [When to Use Wall Bending](#when-to-use-wall-bending)
- [When Not to Use Wall Bending](#when-not-to-use-wall-bending)
- [Recommended Setup](#recommended-setup)
- [Occluders and Wall Bending](#occluders-and-wall-bending)
- [Troubleshooting](#troubleshooting)

---

## Overview

![OptikaFX 2D Menu](./images/caster-wall-bending.png)

Wall Bending allows projected shadows to interact with walls.

It is mainly used when a shadow cast on the ground should continue, bend or clip when it reaches a wall receiver.

This is useful for:

- Characters near walls
- Props casting shadows onto wall surfaces
- Perspective shadows that should interact with vertical receivers
- Hybrid top-down/isometric scenes

Wall Bending is not a general occlusion feature. It is specifically tied to compatible Caster shadows and receiver setups.

---

## Compatible Components

Wall Bending is compatible with:

| Component | Compatibility |
|---|---|
| `Caster` | Supported |
| `ShadowReceiver` | Supported as wall receiver |
| `TilemapShadowReceiver` | Supported as wall receiver |
| `ShadowArea` | Used to define wall/ground areas |

Recommended caster modes:

| Caster Mode | Wall Bending Support |
|---|---|
| `Perspective` | Supported |
| `Mixed` | Supported |
| `Rotation` | Limited or setup-dependent |
| `TopDownBlob` | Not recommended |

For best results, use `Perspective` or `Mixed` mode.

---

## Not Compatible Components

Wall Bending is not compatible with:

| Component | Reason |
|---|---|
| `ObjectOccluder` | Occluders use occlusion/projection masks, not Caster wall bending logic. |
| `CompositeTilemapOccluder` | Tilemap occluders generate occlusion silhouettes, not bendable Caster shadows. |
| Blob-only shadows | Blob shadows are contact shadows and are not designed to bend onto walls. |

If you need wall bending, use a `Caster`, not an occluder.

---

## When to Use Wall Bending

Use Wall Bending when:

- A character shadow should interact with walls.
- A prop shadow should bend from ground to wall.
- Your scene has both ground and wall receivers.
- You use perspective-style shadows.
- Shadows should visually respect vertical surfaces.

Good examples:

- Player standing near a wall
- Tree casting a shadow onto a cliff wall
- Object shadow crossing from ground to wall
- Character in a top-down/isometric room

---

## When Not to Use Wall Bending

Do not use Wall Bending when:

- You only need simple contact shadows.
- You use blob-only shadows.
- You are using `ObjectOccluder`.
- You are using `CompositeTilemapOccluder`.
- You only need environment occlusion silhouettes.
- There are no wall receivers in the scene.

For environment silhouettes or large static blockers, use occluders instead.

---

## Recommended Setup

### 1. Add a Caster

Select the object that should cast a bendable shadow.


Use:

    OptikaFX 2D / Add Casters To Selected GameObjects / Perspective

or:

    OptikaFX 2D / Add Casters To Selected GameObjects / Mixed

![OptikaFX 2D Menu](./images/add-caster-object.png)

### 2. Add a Wall Receiver

Select the wall object or wall tilemap.


Use:

    OptikaFX 2D / Add Receivers To Selected GameObject or Tilemap / Wall


![OptikaFX 2D Menu](./images/add-receiver.png)


### 3. Enable Wall Bending

Select the object with the `Caster`.

Open:

    Wall Bending / Receiver Clipping

Enable:

    Wall Bending

### 4. Configure Detection

Depending on your project, configure:

- Detection Mode
- Wall Layer
- Hit Triggers
- Ignore Own Hierarchy
- Max Receiver Hits
- Use Multi Hit
- Min Angle From Horizontal

### 5. Test the Shadow

Move the caster close to the wall and check if the projected shadow interacts with the wall receiver.

---

## Occluders and Wall Bending

Occluders are not compatible with Wall Bending.

This is expected.

Occluders are designed for:

- Environment occlusion
- Silhouette masks
- Tilemap occlusion
- Static obstacle shadows
- Directional mask projection

Wall Bending is designed for:

- Caster-projected shadows
- Ground-to-wall interaction
- Receiver-based bending/clipping

Use this rule:

| Need | Use |
|---|---|
| Character or prop shadow bending on walls | `Caster` |
| Dynamic sprite shadow with wall interaction | `Caster` |
| Environment occlusion silhouette | `ObjectOccluder` |
| Tilemap occlusion silhouette | `CompositeTilemapOccluder` |
| Static obstacle/environment shadow | `ObjectOccluder` or `CompositeTilemapOccluder` |

If a shadow needs Wall Bending, use a `Caster`.

If an object only needs to contribute to environment occlusion, use an occluder.

---

## Troubleshooting

### Occluder does not bend on walls

This is expected.

Occluders are not compatible with Wall Bending.

Use a `Caster` if you need wall bending.

---

### Wall Bending does not work

Check:

- The object has a `Caster`.
- The Caster uses `Perspective` or `Mixed` mode.
- Wall Bending is enabled.
- A wall receiver exists.
- The wall object has `ShadowReceiver` or `TilemapShadowReceiver`.
- The wall area is configured as wall.
- Detection settings are correct.
- The caster is close enough to the wall.
- The shadow angle allows wall bending.

---

### Shadow bends on the wrong object

Check:

- Wall Layer settings.
- Detection Mode.
- Ignore Own Hierarchy.
- Receiver Wall ID.
- Whether other receivers overlap the intended wall.

---

### Shadow is clipped too early

Adjust:

- Wall Shadow Offset
- Raycast Hit Offset
- Min Hit Distance Separation
- Min Angle From Horizontal

---

### Shadow does not reach the wall

Adjust:

- Projection Length Multiplier on the Caster
- Projection Length on GlobalLight
- TimeManager preset projection length
- LocalLight projection length if using local lights

---

### Blob shadows do not bend

This is expected.

Blob shadows are contact shadows and are not intended to bend onto walls.

Use `Perspective` or `Mixed` Caster mode for wall bending.

← [Back to Documentation Index](index.md)
