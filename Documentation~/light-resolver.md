
# Light Resolver

The **Light Resolver** is the central OptikaFX 2D component responsible for resolving how shadows should be generated, blended, washed out, textured, and applied across the scene.

![OptikaFX 2D Menu](./images/lighting-resolver.png)

It is created automatically by the OptikaFX 2D setup process and is added to the **OptikaFX 2D Manager** object.

```text
Scene
└── OptikaFX 2D Manager
├── Light Resolver component
├── Global Light
└── TimeManager
```

In most projects, you do not need to create this component manually. It is configured by the setup system and then adjusted when you want advanced control over project-wide shadow behavior.

---

## Created by Auto Setup

The Light Resolver is created when running:

```
OptikaFX 2D / 01. Run Full Setup
```

or when running the scene core setup step:

```
OptikaFX 2D / 02. Settings / 01. Setup by Steps / 04. Setup Scene Core Objects
```

During setup, OptikaFX:

- Creates or finds the `OptikaFX 2D Manager`.
- Creates the `Global Light` child object.
- Creates the `TimeManager` child object.
- Adds the `LightResolver` component to the manager.
- Removes legacy occlusion settings if they exist.
- Sets the initial global shadow pattern usage to disabled.

The Light Resolver is added directly to:

```
OptikaFX 2D Manager
```

not to a separate child object.

---

## Purpose

The Light Resolver decides how OptikaFX combines and interprets light information for shadows.

It controls:

- Caster shadow resolving.
- Global light contribution.
- Local light contribution.
- Multi-shadow behavior.
- Vector blended shadows.
- Dominant-light shadows.
- Occlusion shadow resolving.
- Environment darkness.
- Shadow washout.
- Shadow profile fallback values.
- Global shadow pattern textures.
- Advanced shadow visual blending.

It is one of the main components used to customize the final look of OptikaFX shadows.

---

## Caster Shadow Resolve Modes

The Light Resolver supports several caster shadow resolve modes.

| Mode | Description |
|---|---|
| `GlobalOnly` | Uses only the global light for caster shadows. |
| `DominantLightOnly` | Uses the strongest valid local light when available, otherwise falls back to the global light. |
| `VectorBlend` | Blends global and local lights into a single resolved shadow direction. |
| `MultiShadow` | Allows multiple shadows to be generated from global and local light sources. |

The default mode is:

```text
MultiShadow
```

---

## GlobalOnly

`GlobalOnly` uses the global light as the only source for caster shadows.

Use this when you want:

- Simple project-wide shadows.
- One consistent shadow direction.
- No local-light shadow contribution.
- Predictable stylized lighting.

---

## DominantLightOnly

`DominantLightOnly` searches for the strongest valid local light affecting the caster.

If a valid local light is found, it uses that light.

If not, it falls back to the global light.

Use this when you want:

- Only one shadow per caster.
- Local lights to override global lighting.
- Simpler results than multi-shadow.
- Cleaner visuals in scenes with many lights.

---

## VectorBlend

`VectorBlend` blends valid light influences into one combined shadow result.

It considers:

- Global light alpha.
- Local light alpha.
- Light direction.
- Projection length.
- Flattening.
- Width.
- Shadow color.
- Shadow visual settings.

Use this when you want:

- One final blended shadow.
- Smooth transitions between global and local lights.
- Reduced visual clutter.
- A softer combined lighting result.

---

## MultiShadow

`MultiShadow` allows multiple shadow results to exist at the same time.

This mode can include:

- One global shadow.
- One or more local light shadows.
- Washout between overlapping shadows.
- Separate visual data per light source.

Use this when you want:

- Richer lighting.
- Multiple shadow directions.
- Stronger local light presence.
- More advanced stylized scenes.
- Layered shadows from several light sources.

---

## MultiShadow Washout

The Light Resolver includes washout controls for MultiShadow behavior.

| Field | Description |
|---|---|
| `Local Washout Power` | Controls how much local shadows reduce or wash out other shadows. |
| `Global Washout Power` | Controls how much the global shadow affects local shadow visibility. |

These settings help prevent multi-shadow scenes from becoming too dark or visually noisy.

### Local Washout Power

Higher values make local lights reduce the visibility of other shadows more strongly.

Useful when:

- Local lights should dominate the scene.
- Global shadows feel too strong near local lights.
- Multiple local shadows overlap too heavily.

### Global Washout Power

Higher values make global lighting reduce the strength of local shadows more strongly.

Useful when:

- Global lighting should remain visually dominant.
- Local shadows should be subtle.
- You want smoother integration between global and local lighting.

---

## Global Source Settings

The Light Resolver can use the active `GlobalLight` as the base shadow source.

| Field | Description |
|---|---|
| `Use Global Light As Base` | Uses the global light as the base source for shadow resolving. |
| `Require Real Global Light For Global Base` | Requires an active and valid global light before global shadows are used. |
| `Minimum Global Shadow Alpha` | Minimum global shadow alpha required for the global light to count as renderable. |
| `Minimum Global Light Intensity` | Minimum global light intensity required for global shadow resolving. |

These settings prevent invalid or nearly invisible global lights from contributing unwanted shadows.

---

## Occlusion Shadows

The Light Resolver also controls occlusion shadow resolving.

Occlusion shadows are handled separately from regular caster shadows, but they can use the same resolve mode if desired.

| Field | Description |
|---|---|
| `Enable Occlusion Shadows` | Enables or disables occlusion shadow resolving. |
| `Occlusion Use Same Resolve Mode As Casters` | Uses the caster shadow resolve mode for occlusion. |
| `Occlusion Resolve Mode` | Separate resolve mode used only when occlusion does not use the caster mode. |
| `Include Global Light In Occlusion` | Allows the global light to affect occlusion shadows. |
| `Include Local Lights In Occlusion` | Allows local lights to affect occlusion shadows. |
| `Max Local Lights Per Occluder` | Maximum number of local lights used per occluder. |
| `Local Light Search Range Multiplier` | Expands or reduces the range used to search local lights. |
| `Min Local Light Contribution` | Minimum local light contribution required to affect occlusion. |

---

## Occlusion Resolve Modes

Occlusion can use the same resolve modes as caster shadows:

```text
GlobalOnly
DominantLightOnly
VectorBlend
MultiShadow
```

If:

```text
Occlusion Use Same Resolve Mode As Casters
```

is enabled, occlusion shadows use the main caster shadow mode.

If disabled, the separate occlusion resolve mode is used.

---

## Global Occlusion

Global occlusion uses the Global Light and occlusion profile settings to produce scene-wide occlusion shadows.

Global occlusion depends on:

- Global light availability.
- Global light intensity.
- Shadow alpha.
- Occlusion profile source.
- Occlusion alpha multiplier.
- Occlusion projection length multiplier.

---

## Local Light Occlusion

Local lights can contribute to occlusion shadows if enabled.

The Light Resolver checks each active local light and validates:

- The light is active.
- The light is enabled.
- The light intensity is high enough.
- The occluder is inside range.
- The contribution is above the minimum threshold.
- Spot lights pass cone filtering if enabled.

Relevant settings:

| Field | Description |
|---|---|
| `Local Light Alpha Multiplier` | Extra alpha multiplier for local-light occlusion shadows. |
| `Local Light Projection Length Multiplier` | Extra projection length multiplier for local-light occlusion shadows. |
| `Filter Spot Local Lights For Occlusion` | Restricts spot lights to objects inside the cone. |

---

## Occlusion Light Requirement

The Light Resolver can require real lights before rendering occlusion shadows.

| Field | Description |
|---|---|
| `Require Real Light For Occlusion` | Prevents fallback values from counting as real lights. |
| `Manual Values Count As Light` | Allows manual occlusion values to render without a real light. |
| `Occlusion Minimum Light Intensity` | Minimum light intensity needed for occlusion. |
| `Occlusion Minimum Shadow Alpha` | Minimum shadow alpha needed for occlusion. |

This prevents occlusion from appearing when no meaningful lighting source exists.

---

## Occlusion Profile Source

Occlusion shadows can use different profile sources.

| Mode | Description |
|---|---|
| `ManualValues` | Uses manually configured occlusion projection and alpha. |
| `GlobalLightProfile` | Uses the active Global Light profile. |
| `ExplicitProfile` | Uses a profile assigned directly to the Light Resolver. |
| `ResolverDefaultProfile` | Uses the resolver default profile. |

The default source is:

```text
GlobalLightProfile
```

---

## Manual Occlusion Values

When using manual values, these fields define the occlusion result:

| Field | Description |
|---|---|
| `Manual Occlusion Projection Length` | Projection length used for manual occlusion. |
| `Manual Occlusion Alpha` | Alpha used for manual occlusion. |
| `Manual Occlusion Shadow Direction` | Direction used when manual direction is selected. |

Manual values are useful for stylized or fixed occlusion behavior.

---

## Occlusion Direction Source

The Light Resolver can determine occlusion direction from several sources.

| Source | Description |
|---|---|
| `Manual` | Uses the manual direction vector. |
| `GlobalLight` | Uses the active Global Light direction. |
| `TransformRight` | Uses the right vector of a transform. |
| `TransformUp` | Uses the up vector of a transform. |
| `TransformDown` | Uses the negative up vector of a transform. |

If no valid direction is found, the resolver falls back to a downward direction.

---

## Environment Darkness

The Light Resolver also controls base environment darkness used by occlusion systems.

| Field | Description |
|---|---|
| `Base Darkness` | Base darkness amount applied to the environment. |
| `Global Light Amount` | Amount of global light reducing darkness. |
| `Base Darkness Uses Profile Shadow Alpha` | Multiplies darkness by profile shadow alpha. |
| `Base Darkness Alpha Multiplier` | Extra multiplier for environment darkness alpha. |

Effective base shadow alpha is calculated from darkness, global light amount, and optional profile alpha.

This is useful for scenes where ambient darkness should respond to lighting.

---

## Fallback Settings

Fallbacks are used when a valid profile or light source is missing.

| Field | Description |
|---|---|
| `Default Profile` | Default shadow profile used by the resolver. |
| `Fallback Normalized Time` | Time value used when evaluating fallback profile curves. |
| `Fallback Shadow Color` | Fallback color used when no profile color is available. |

Fallbacks help keep the system stable even if some references are missing.

---

## Global Shadow Pattern Texture

The Light Resolver can apply a global shadow pattern texture.

This can be used to add:

- Texture variation.
- Noise-like shadow breakup.
- Stylized shadow overlays.
- Project-wide shadow patterning.
- World-space or screen-space shadow texture behavior.

| Field | Description |
|---|---|
| `Use Global Shadow Pattern` | Enables global shadow pattern usage. |
| `Global Shadow Pattern Source` | Defines whether the pattern comes from custom texture, preset, or fallback order. |
| `Global Shadow Pattern Preset` | Preset asset used for pattern selection. |
| `Global Shadow Pattern Preset Index` | Index of the selected preset pattern. |
| `Global Shadow Pattern Texture` | Custom texture used as the pattern. |
| `Global Shadow Pattern Strength` | Intensity of the pattern effect. |
| `Global Shadow Pattern Scale` | Pattern tiling scale. |
| `Global Shadow Pattern Offset` | Pattern offset. |
| `Global Shadow Pattern Scroll` | Runtime scroll value. |
| `Use Screen UV` | Uses screen-space UVs. |
| `Use World UV` | Uses world-space UVs. |
| `Use Alpha` | Uses the pattern texture alpha channel. |

---

## Global Shadow Pattern Source

The pattern texture can be resolved using different source modes.

| Source | Description |
|---|---|
| `CustomOnly` | Uses only the custom texture field. |
| `PresetOnly` | Uses only the selected preset texture. |
| `CustomThenPreset` | Uses custom texture first, then preset fallback. |
| `PresetThenCustom` | Uses preset first, then custom fallback. |

This allows flexible project-wide texture configuration.

---

## Shadow Profiles and Visuals

The Light Resolver works with `ShadowProfile` data to resolve shadow visuals.

Resolved visual values include:

- Alpha multiplier
- Blur radius
- Edge feather
- Tip fade
- Alpha quantization steps

These values can come from:

- The active Global Light
- A Local Light
- An explicit occlusion profile
- The resolver default profile
- Project defaults

This allows shadows to share consistent visual rules while still reacting to different light sources.

---

## Caster Shadow Resolving

Caster shadow resolving happens through:

```text
ResolveShadows
```

The resolver considers:

- Caster position
- Effective caster shadow render mode
- Global light state
- Local light states
- Light zones
- Shadow profile overrides
- Resolve mode
- Washout controls
- Visual profile data

The result is one or more `ResolvedShadowData` entries used by the shadow rendering pipeline.

---

## Light Zones

The resolver can also react to active `LightZone` objects.

A light zone can:

- Override the shadow profile.
- Disable global light.
- Force local-light preference.
- Influence how shadows resolve in a specific area.

This makes it possible to change shadow behavior by region.

---

## Local Light Filtering

Local lights are only considered valid when they can affect the target position.

For caster shadows, a local light must:

- Be active.
- Be enabled.
- Affect the caster position.
- Support the relevant shadow curve set.
- Produce alpha above the minimum threshold.

For occlusion shadows, a local light must also pass range and contribution checks.

---

## Effective Occlusion Values

The Light Resolver exposes effective values for occlusion systems:

| Value | Description |
|---|---|
| `Effective Shadow Direction` | Final occlusion shadow direction. |
| `Effective Projection Length` | Final occlusion projection length. |
| `Effective Base Shadow Alpha` | Base darkness alpha. |
| `Effective Occlusion Alpha` | Final global occlusion alpha. |
| `Has Renderable Global Occlusion Light` | Whether global occlusion can render. |
| `Has Any Renderable Occlusion Light` | Whether any global or local occlusion light can render. |

These are used internally by OptikaFX systems to determine whether and how occlusion should appear.

---

## Auto Setup Defaults

When Auto Setup creates or configures the Light Resolver, it sets:

```text
Use Global Shadow Pattern: Disabled
```

This means global pattern textures are not enabled by default.

You can enable and configure them manually if your project needs textured shadows.

---

## Recommended Workflow

Use this workflow when tuning the Light Resolver:

1. Run **OptikaFX 2D / 01. Run Full Setup**.
2. Select the **OptikaFX 2D Manager**.
3. Locate the **Light Resolver** component.
4. Confirm the caster shadow resolve mode.
5. Tune MultiShadow washout if using multiple lights.
6. Configure occlusion shadow settings.
7. Adjust environment darkness.
8. Enable global shadow pattern only if needed.
9. Test with real casters, receivers, occluders, global light, and local lights.

---

## Common Setups

### Simple Global Shadows

Use:

```text
Resolve Mode: GlobalOnly
Use Global Light As Base: Enabled
```

Best for:

- Simple scenes
- One light direction
- Predictable shadow behavior
- Minimal configuration

---

### Local Light Priority

Use:

```text
Resolve Mode: DominantLightOnly
Use Global Light As Base: Enabled
```

Best for:

- One shadow per caster
- Local lights overriding global light
- Cleaner scenes with fewer overlapping shadows

---

### Smooth Blended Shadows

Use:

```text
Resolve Mode: VectorBlend
Use Global Light As Base: Enabled
```

Best for:

- Blending global and local light influence
- Avoiding multiple visible shadows
- Smooth transitions between lights

---

### Rich Multi-Shadow Lighting

Use:

```text
Resolve Mode: MultiShadow
Use Global Light As Base: Enabled
```

Best for:

- Advanced lighting
- Multiple shadow directions
- Stylized scenes
- Local and global light layering

---

### Textured Shadows

Use:

```text
Use Global Shadow Pattern: Enabled
Global Shadow Pattern Strength: Above 0
```

Then configure either:

```text
Global Shadow Pattern Texture
```

or:

```text
Global Shadow Pattern Preset
```

Best for:

- Stylized shadows
- Patterned shadows
- Noisy or organic shadow detail
- Project-wide shadow texture effects

---

## Troubleshooting

### Light Resolver is missing

Run:

```text
OptikaFX 2D / 01. Run Full Setup
```

or:

```text
OptikaFX 2D / 02. Settings / 01. Setup by Steps / 04. Setup Scene Core Objects
```

The component should be added to:

```text
OptikaFX 2D Manager
```

---

### Shadows are not appearing

Check:

- The Global Light exists.
- The Global Light is active.
- The Global Light intensity is above minimum.
- The shadow alpha is above minimum.
- The caster or occluder has a valid shadow mode.
- The resolver is enabled.
- The selected resolve mode allows the expected light source.

---

### Local lights do not affect shadows

Check:

- The local light is active and enabled.
- The local light can affect the object position.
- The local light intensity is high enough.
- The local light range reaches the caster or occluder.
- `Include Local Lights In Occlusion` is enabled for occlusion shadows.
- The selected resolve mode supports local lights.

---

### MultiShadow looks too dark

Increase washout:

```text
Local Washout Power
Global Washout Power

```

This reduces stacked shadow darkness.

---

### MultiShadow looks too weak

Reduce washout values.

Also check:

- Local light intensity
- Global light intensity
- Shadow alpha
- Shadow profile alpha
- Local light blend settings

---

### Occlusion shadows do not render

Check:

- `Enable Occlusion Shadows` is enabled.
- A valid global or local light exists.
- `Require Real Light For Occlusion` is not preventing fallback values.
- Alpha and projection length are above minimum thresholds.
- The occluder is inside local light range if using local lights.

---

### Shadow pattern is not visible

Check:

- `Use Global Shadow Pattern` is enabled.
- Pattern strength is above zero.
- A valid custom texture or preset texture is resolved.
- The selected source mode allows that texture.
- Scale is not too small or too large.
- UV mode is configured correctly.

---

## Best Practices

- Keep the Light Resolver on the **OptikaFX 2D Manager**.
- Use Auto Setup to create it.
- Start with `MultiShadow` only if your scene needs multiple light contributions.
- Use `GlobalOnly` for simpler projects.
- Tune washout after global and local lights are working.
- Avoid enabling pattern textures until basic shadows are correct.
- Use occlusion settings only after occluders are configured.
- Keep fallback values reasonable.
- Use real lights for final gameplay behavior.
- Use manual occlusion values only for controlled stylized setups.

---

## Related Pages

- [Quick Start](quick-start.md)
- [Global Light](global-light.md)
- [Local Light](local-light.md)
- [Casters](casters.md)
- [Receivers](receivers.md)
- [Occluders](occluders.md)
- [Time Manager](time-manager.md)
- [Troubleshooting](troubleshooting.md)
