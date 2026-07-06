
# Horizontal Proxy

The Horizontal Proxy is an optional helper used by the `Caster` component to improve horizontal or side-facing shadows in `Perspective` mode.

It is useful when a sprite becomes too thin, distorted or visually incorrect when the light direction is close to horizontal.

> Important: Horizontal Proxy works with `Perspective` mode.  
> In `Rotation` and `Mixed` modes, use Animator Auto Remap only.

## Index

- [Overview](#overview)
- [When to Use Horizontal Proxy](#when-to-use-horizontal-proxy)
- [Perspective Mode Only](#perspective-mode-only)
- [How It Works](#how-it-works)
- [Common Use Cases](#common-use-cases)
- [Caster Setup](#caster-setup)
- [Animator and Blend Tree Usage](#animator-and-blend-tree-usage)
- [Recommended Settings](#recommended-settings)
- [Useful C# Examples](#useful-c-examples)
  - [Enable Horizontal Proxy](#enable-horizontal-proxy)
  - [Assign Horizontal Proxy](#assign-horizontal-proxy)
  - [Toggle Horizontal Proxy at Runtime](#toggle-horizontal-proxy-at-runtime)
- [Troubleshooting](#troubleshooting)

---

## Overview

Some sprites do not create good projected shadows when viewed or projected from a horizontal direction.

For example, a character walking left or right can generate a very thin shadow because the side-facing sprite has less visible width.

The Horizontal Proxy solves this by allowing the Caster to use an alternative proxy shape for horizontal shadow cases.

This proxy can be:

- A wider sprite
- A simplified silhouette
- A custom shadow shape
- A hidden helper object
- A manually adjusted SpriteRenderer

---

## When to Use Horizontal Proxy

Use Horizontal Proxy when:

- The Caster is using `Perspective` mode.
- Shadows become too thin when projected left or right.
- The original sprite is too narrow from the side.
- You want a custom silhouette only for horizontal projections.
- You need a better shape for horizontal projected shadows.

This is especially useful for:

- Characters
- Houses
- Towers
- Rocks
- Trees
- Large props
- Directional sprites in Perspective mode

---

## Perspective Mode Only

Horizontal Proxy is only used by `Perspective` mode.

| Caster Mode | Horizontal Proxy |
|---|---|
| `Perspective` | Supported |
| `Rotation` | Not supported |
| `Mixed` | Not supported |
| `TopDownBlob` | Not supported |

For `Rotation` and `Mixed` modes, use Animator Auto Remap instead.

See:

- [Animator Blend Tree Setup for Auto Remap](animator-blend-tree-setup.md)
- [Casters](casters.md)

---

## How It Works

The Caster normally uses the `Source Renderer` as the shadow source.

When Horizontal Proxy is enabled in `Perspective` mode, the Caster can use a different proxy renderer or proxy shape when the shadow projection is near horizontal.

This helps preserve better shadow width and silhouette.

Typical flow:

1. Light direction approaches a horizontal angle.
2. Perspective projection would make the original sprite shadow too thin.
3. The Caster uses the Horizontal Proxy source.
4. The shadow keeps a better silhouette instead of becoming too narrow.

---

## Common Use Cases

Use Horizontal Proxy for:

- Side-facing character shadows in Perspective mode
- Buildings that need a wider horizontal shadow
- Trees with thin side silhouettes
- Rocks or props that look too narrow from horizontal projection
- Custom projected silhouettes
- Stylized shadow shapes

Do not use Horizontal Proxy for:

- Rotation mode remap
- Mixed mode remap
- Blob shadows
- Wall bending-specific fixes
- Occluder silhouettes

---

## Caster Setup

Select the object with the `Caster` component.

Make sure the Caster uses:

    Perspective

Open:

    Optional Components

Enable:

    Use Horizontal Proxy

Then configure:

| Field | Description |
|---|---|
| `Use Horizontal Proxy` | Enables horizontal proxy support. |
| `Horizontal Proxy Mode` | Controls how the proxy is used. |
| `Horizontal Proxy` | Reference to the proxy object or renderer. |

The exact fields may vary depending on inspector version.

---

## Creating a Horizontal Proxy

Recommended setup:

1. Create a child GameObject under the caster object.
2. Name it:

    _HorizontalShadowProxy

3. Add a `SpriteRenderer`.
4. Assign a sprite or silhouette suitable for horizontal projected shadows.
5. Adjust its local position, scale and pivot.
6. Assign it to the `Horizontal Proxy` field in the Caster.
7. Hide it visually if needed.

Example hierarchy:

    Player
    ├── SpriteRenderer
    ├── Caster
    └── _HorizontalShadowProxy
        └── SpriteRenderer

The proxy should usually be wider or better shaped than the original side-facing sprite.

---

## Animator and Blend Tree Usage

Horizontal Proxy is not a replacement for Animator Auto Remap.

Use Horizontal Proxy when:

- The Caster is in `Perspective` mode.
- The projection becomes too thin at horizontal light angles.

Use Animator Auto Remap when:

- The Caster is in `Rotation` mode.
- The Caster is in `Mixed` mode.
- You need shadows to follow character movement direction.
- Your character uses a 2D Blend Tree.

Recommended Animator parameters for Auto Remap:

| Parameter | Type | Description |
|---|---|---|
| `MoveX` | Float | Horizontal movement direction. |
| `MoveY` | Float | Vertical movement direction. |
| `LastMoveX` | Float | Last horizontal direction. |
| `LastMoveY` | Float | Last vertical direction. |

For more information, see:

- [Animator Blend Tree Setup for Auto Remap](animator-blend-tree-setup.md)

---

## Recommended Settings

For Perspective casters using Horizontal Proxy:

| Field | Suggested Value |
|---|---:|
| `Shadow Mode` | `Perspective` |
| `Use Horizontal Proxy` | Enabled |
| `Horizontal Proxy` | Assigned |
| `Projection Length Multiplier` | Adjust per asset |
| `Flatten Multiplier` | Adjust per asset |
| `Alpha Multiplier` | Adjust per asset |

If the shadow is still too thin, increase:

- Horizontal proxy sprite width
- Proxy local scale
- Projection Length Multiplier
- Flatten Multiplier
- Source/proxy silhouette width

For animated characters using Rotation or Mixed mode, do not use Horizontal Proxy. Use Auto Remap instead.

---

## Useful C# Examples

---

## Enable Horizontal Proxy

```csharp
using UnityEngine;
using OptikaFX;

public class EnableHorizontalProxyExample : MonoBehaviour
{
    [SerializeField]
    private Caster caster;

    public void EnableProxy()
    {
        if (caster == null)
            return;

        caster.useHorizontalProxy = true;

        caster.RefreshShadowNow();
    }
}

```

---

## Assign Horizontal Proxy

```csharp
using UnityEngine;
using OptikaFX;

public class AssignHorizontalProxyExample : MonoBehaviour
{
    [SerializeField]
    private Caster caster;

    [SerializeField]
    private SpriteRenderer horizontalProxy;

    public void AssignProxy()
    {
        if (caster == null)
            return;

        caster.useHorizontalProxy = true;

        caster.horizontalProxy = horizontalProxy;

        caster.RefreshShadowNow();
    }
}

```

---

## Toggle Horizontal Proxy at Runtime

```csharp
using UnityEngine;
using OptikaFX;

public class ToggleHorizontalProxyExample : MonoBehaviour
{
    [SerializeField]
    private Caster caster;

    public void SetHorizontalProxy(bool enabled)
    {
        if (caster == null)
            return;

        caster.useHorizontalProxy = enabled;

        caster.RefreshShadowNow();
    }
}

```

---

## Troubleshooting

### Horizontal Proxy is not working

Check:

- The Caster is in `Perspective` mode.
- `Use Horizontal Proxy` is enabled.
- `Horizontal Proxy` is assigned.
- The proxy SpriteRenderer has a valid sprite.
- The light projection is near a horizontal angle.
- The Caster was refreshed after changing settings.

Horizontal Proxy does not work in `Rotation`, `Mixed` or `TopDownBlob` mode.

---

### Shadow is too thin when projected horizontally

Enable:

    Use Horizontal Proxy

Then check:

- Horizontal Proxy is assigned.
- Proxy sprite is wider than the original source sprite.
- Proxy scale is large enough.
- Projection and flatten settings are not too extreme.

If using `Rotation` or `Mixed` mode, use Auto Remap instead:

- [Animator Blend Tree Setup for Auto Remap](animator-blend-tree-setup.md)

---

### Horizontal Proxy appears visually in the scene

If the proxy SpriteRenderer is visible when it should not be:

- Put it on a hidden/debug sorting layer.
- Disable visible rendering if your setup supports it.
- Use a material that does not render visibly.
- Keep the proxy object hidden if the Caster can still read it.
- Make sure it is not being used as a normal character sprite.

---

### Proxy has wrong position

Check:

- Proxy local position.
- Proxy pivot.
- Shadow Anchor position.
- Character scale.
- SpriteRenderer flip settings.
- Mirror settings in the Caster.

---

### Proxy does not match left/right flip

Check:

- SpriteRenderer `Flip X`.
- Proxy local scale.
- Proxy sprite orientation.
- Mirror settings in the Caster.
- Character art orientation.

If your character art faces left by default, you may need to adjust mirror settings or invert horizontal input values.

---

### Rotation or Mixed mode still has thin horizontal shadows

Horizontal Proxy is not used in these modes.

For `Rotation` and `Mixed` mode:

- Enable Animator Auto Remap.
- Configure `MoveX`, `MoveY`, `LastMoveX` and `LastMoveY`.
- Tune the width/remap settings in the Caster inspector.
- Check the Animator Blend Tree setup.

See:

- [Animator Blend Tree Setup for Auto Remap](animator-blend-tree-setup.md)
- [Casters](casters.md)

---

← [Back to Documentation Index](index.md)
