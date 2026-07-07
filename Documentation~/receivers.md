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
- [Wall Bending Raycasts](#wall-bending-raycasts)
- [Large Objects and Wide Caster Sampling](#large-objects-and-wide-caster-sampling)
- [Occluders and Wall Bending](#occluders-and-wall-bending)
- [Troubleshooting](#troubleshooting)

---

## Overview

Wall Bending allows projected shadows to interact with walls.
![OptikaFX 2D Menu](./images/caster-wall-bending.png)

It is mainly used when a shadow cast on the ground should continue, bend or clip when it reaches a wall receiver.

This is useful for:

- Characters near walls
- Props casting shadows onto wall surfaces
- Perspective shadows that should interact with vertical receivers
- Hybrid top-down/isometric scenes
- Large objects whose shadow may touch a wall only from one side

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
| `Rotation` | Not recommended |
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
- A wide caster, such as a tree or large prop, needs more reliable wall detection.

Good examples:

- Player standing near a wall
- Tree casting a shadow onto a cliff wall
- Object shadow crossing from ground to wall
- Character in a top-down/isometric room
- Large sprite whose left or right side reaches a wall before the center does

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

### 2. Add a Wall Receiver

Select the wall object or wall tilemap.

Use:

    OptikaFX 2D / Add Receivers To Selected GameObject or Tilemap / Wall

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
- Max Component Hits
- Use Multi Hit
- Max Receiver Hits
- Min Hit Distance Separation
- Min Angle From Horizontal
- Horizontal Ray Samples
- Horizontal Sample Width

### 5. Test the Shadow

Move the caster close to the wall and check if the projected shadow interacts with the wall receiver.

For small or medium objects, the default single center ray is usually enough.

For large sprites, trees, wide props or big characters, increase `Horizontal Ray Samples`.

---

## Wall Bending Raycasts

Wall Bending detects walls using 2D raycasts.

A ray is cast from the caster hinge/origin toward the projected shadow tip. If the ray hits a valid wall receiver, the Caster can bend or clip the shadow at that wall.

The system can detect wall receivers in different ways:

| Detection Mode | Description |
|---|---|
| `LayerMask` | Uses the configured wall physics layer. |
| `ReceiverComponents` | Uses `ShadowReceiver` or `TilemapShadowReceiver` components. This does not require wall-specific physics layers. |
| `LayerMaskAndReceiverComponents` | Uses physics layers and also validates receiver components. |

By default, component-based detection is recommended because it is easier to set up and less dependent on project layer configuration.

### Valid Wall Hits

A raycast hit is considered valid when:

- The hit collider belongs to a valid `ShadowReceiver` or `TilemapShadowReceiver`, depending on detection mode.
- The receiver is enabled.
- The receiver has `blockWallBending` enabled.
- The receiver has a valid `WallID`.
- The hit comes from the expected direction.
- The hit normal is suitable for wall bending.

Wall Bending only accepts wall projection hits that represent the shadow reaching the collider from bottom to top.

Internally, renderable wall hits require the collider normal to point downward enough:

    hit.normal.y < -0.05

This prevents Wall Bending from activating on the wrong side of a collider.

### Triggers

If `Hit Triggers` is enabled, Wall Bending raycasts can hit trigger colliders.

Use this only when your wall receiver colliders are configured as triggers or when your project intentionally uses trigger-based wall detection.

### Ignore Own Hierarchy

If `Ignore Own Hierarchy` is enabled, Wall Bending ignores colliders that belong to the same hierarchy as the caster.

This prevents a character or prop from detecting its own colliders as walls.

---

## Large Objects and Wide Caster Sampling

Large sprites may fail to detect walls correctly when only a single center ray is used.

This can happen when:

- The object is very wide.
- The wall touches only one side of the projected shadow.
- The caster hinge is centered, but the left or right side reaches the wall first.
- A large tree, prop or character overlaps wall space horizontally.
- The shadow projection is diagonal and the center ray misses the receiver.

To solve this, Wall Bending supports horizontal ray sampling.

Instead of casting only one ray from the center, the Caster can cast multiple horizontal rays across the sprite width.

### Horizontal Ray Samples

Controls how many horizontal rays are used for Wall Bending detection.

Inspector field:

    Wall Bending / Wide Caster Sampling / Horizontal Ray Samples

| Value | Use |
|---|---|
| `1` | Center ray only. Best performance. Recommended for small objects. |
| `3` | Good default for wide characters and medium props. |
| `5` | Recommended for trees, large props and wide sprites. |
| `7` to `9` | Use only for very large or irregular objects. Higher cost. |

The system prefers odd sample counts so the center ray is always included.

For example, if an even value is selected, the system may internally resolve it to the next odd amount.

Recommended values:

| Object Type | Recommended Samples |
|---|---|
| Small character | `1` |
| Medium character | `1` to `3` |
| Large character | `3` |
| Tree | `3` to `5` |
| Wide prop | `3` to `5` |
| Very large object | `5` to `9` |

### Horizontal Sample Width

Controls how much of the sprite width is sampled by the horizontal rays.

Inspector field:

    Wall Bending / Wide Caster Sampling / Horizontal Sample Width

| Value | Meaning |
|---|---|
| `1.0` | Samples the full sprite width. |
| `0.5` | Samples half of the sprite width around the hinge/center. |
| `0.25` | Samples a narrow area near the hinge/center. |
| `0` | Effectively collapses sampling to the center. |

Use `1.0` when the whole sprite should be considered for wall detection.

Use smaller values when the visual shadow should only react near the center or hinge.

### Example: Large Tree

For a large tree casting a perspective shadow onto a cliff wall:

Recommended settings:

| Setting | Value |
|---|---|
| Caster Mode | `Perspective` or `Mixed` |
| Wall Bending | Enabled |
| Detection Mode | `ReceiverComponents` |
| Horizontal Ray Samples | `5` |
| Horizontal Sample Width | `1` |
| Use Multi Hit | Enabled |
| Max Receiver Hits | `2` or `3` |

This lets the tree detect the wall even if only one side of the shadow reaches it.

### Example: Large Character

For a wide boss character or large enemy:

Recommended settings:

| Setting | Value |
|---|---|
| Caster Mode | `Mixed` |
| Wall Bending | Enabled |
| Horizontal Ray Samples | `3` |
| Horizontal Sample Width | `0.75` to `1` |

Use `3` samples first. Increase to `5` only if the side rays still miss important wall receivers.

### Performance Notes

Each horizontal sample performs a raycast.

Higher values improve detection reliability but increase physics query cost.

Use the lowest value that gives stable results.

Recommended workflow:

1. Start with `1`.
2. If the object is wide and misses walls, try `3`.
3. If side detection is still unreliable, try `5`.
4. Use `7` or `9` only for very large objects.

Also keep `Max Component Hits` reasonable. The default value is usually enough for most scenes.

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
- Raycast-based wall detection from the Caster projection

Use this rule:

| Need | Use |
|---|---|
| Character or prop shadow bending on walls | `Caster` |
| Dynamic sprite shadow with wall interaction | `Caster` |
| Large sprite shadow detecting wall contact across its width | `Caster` with multiple horizontal ray samples |
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
- The wall receiver has `blockWallBending` enabled.
- The wall receiver has a valid `WallID`.

---

### Large object misses the wall

If a large object does not trigger Wall Bending, the center ray may be missing the wall.

Adjust:

- Increase `Horizontal Ray Samples` to `3`.
- For very wide objects, increase to `5`.
- Set `Horizontal Sample Width` closer to `1`.
- Make sure the wall receiver collider covers the visible wall area.
- Make sure the shadow projection is long enough to reach the wall.

Recommended starting point for large objects:

    Horizontal Ray Samples: 3
    Horizontal Sample Width: 1

For trees or very wide props:

    Horizontal Ray Samples: 5
    Horizontal Sample Width: 1

---

### Shadow bends on the wrong object

Check:

- Wall Layer settings.
- Detection Mode.
- Ignore Own Hierarchy.
- Receiver Wall ID.
- Whether other receivers overlap the intended wall.
- Whether trigger colliders are being hit unexpectedly.
- Whether `Hit Triggers` should be disabled.

---

### Shadow is clipped too early

Adjust:

- Wall Shadow Offset
- Raycast Hit Offset
- Min Hit Distance Separation
- Min Angle From Horizontal

If multiple colliders are very close together, increase `Min Hit Distance Separation` slightly to avoid duplicate wall hits.

---

### Shadow does not reach the wall

Adjust:

- Projection Length Multiplier on the Caster
- Projection Length on GlobalLight
- TimeManager preset projection length
- LocalLight projection length if using local lights

For elevated casters, also check whether the projected distance is enough after elevation offsets are applied.

---

### Multi Hit does not detect multiple walls

Check:

- `Use Multi Hit` is enabled.
- `Max Receiver Hits` is greater than `1`.
- The ray actually crosses multiple valid wall receivers.
- The receivers have different hit distances.
- `Min Hit Distance Separation` is not too large.

Multi Hit is useful for:

- Grates
- Holes
- Windows
- Stacked walls
- Layered wall receivers

---

### Blob shadows do not bend

This is expected.

Blob shadows are contact shadows and are not intended to bend onto walls.

Use `Perspective` or `Mixed` Caster mode for wall bending.

← [Back to Documentation Index](index.md)
