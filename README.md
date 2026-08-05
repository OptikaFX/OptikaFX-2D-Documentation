# OptikaFX 2D

**OptikaFX 2D** is a dynamic 2D shadows and lighting framework for Unity URP.

It provides projected shadows, wall bending, blob shadows, occlusion, sprite/tilemap receivers, global/local light integration, Light Resolver shadow resolving, day/night lighting presets, editor setup tools and optional UI/debug modules.

---

## Features

- Dynamic 2D projected shadows
- Wall bending shadows
- 6 shadow modes
- Sprite and Tilemap receivers
- Shadow Areas and mask rules
- Global Light and Local Light integration with Unity 2D Lights
- MultiShadow, Washout and Shadow pattern textures
- TimeManager with day-night cycle presets
- Timeline editor for animated lighting presets
- Optional Clock UI using TextMeshPro
- Optional debug controls using Input System
- Interactive Tutorial scene with guided Editor window
- Interactive Documentation built into Unity
- Automatic URP Render Feature setup
- Editor tools for adding/removing OptikaFX componente


---

## Requirements

- **Unity 6000.0 or newer**
- Universal Render Pipeline 17.0.0 or newer
- Unity 2D Sprite package

### Optional

- TextMeshPro, required only for [Clock UI](Documentation~/clock-ui.md)
- Input System, required only for optional runtime debug input tools

See:

- [Optional Modules](Documentation~/optional-modules.md)

---

## Installation

Install this package as an embedded or local package:

```text
Packages/com.optikafx.2d
```

Or add it through Unity Package Manager using a local path.

After installation, open your project using URP and run the setup from the OptikaFX menu.

---

## Quick Start

1. Open your Unity project using URP.

2. Run Full Setup:

```text
Tools / OptikaFX 2D / 01. Run Full Setup
```

This creates and configures the core OptikaFX scene objects, including the manager, Global Light, TimeManager and Light Resolver.

3. Select your sprite objects and add casters:

```text
OptikaFX 2D / Add Casters and Receivers / Add Casters to Selected Game Objects
```

4. Select your ground, wall or tilemap objects and add receivers:

```text
OptikaFX 2D / Add Casters and Receivers / Add Receivers to Selected Game Objects or TileMaps
```

5. Configure your Global Light or enable the TimeManager.

6. Select your active camera and check that it has:

- Camera
- ShadowRenderQuad

7. Configure the `ShadowRenderQuad` shadow mattes so shadows draw in the correct sorting layer/order.

8. Remember to use the sprite pivot at the bottom center or right between the character's feet.

Full guide:

- [Quick Start](Documentation~/quick-start.md)

---

## Documentation

Main documentation files:

- [Documentation Index](Documentation/index.md)
- [Quick Start](Documentation/quick-start.md)
- [Scarecrow Tutorial](Documentation/scarecrow-tutorial.md)
- [Animator Blend Tree Setup](Documentation/animator-blend-tree-setup.md)
- [Camera](Documentation/camera.md)
- [Casters](Documentation/casters.md)
- [Clock UI](Documentation/clock-ui.md)
- [Global Light](Documentation/global-light.md)
- [Horizontal Proxy](Documentation/horizontal-proxy.md)
- [Light Resolver](Documentation/light-resolver.md)
- [Local Light](Documentation/local-light.md)
- [Occluders](Documentation/occluders.md)
- [Optional Modules](Documentation/optional-modules.md)
- [Receivers](Documentation/receivers.md)
- [TimeManager](Documentation/time-manager.md)
- [Troubleshooting](Documentation/troubleshooting.md)
- [Wall Bending](Documentation/wall-bending.md)

Recommended reading order:

1. [Quick Start](Documentation/quick-start.md)
2. [Scarecrow Tutorial](Documentation/scarecrow-tutorial.md)
3. [Camera](Documentation/camera.md)
4. [Receivers](Documentation/receivers.md)
5. [Casters](Documentation/casters.md)
6. [Global Light](Documentation/global-light.md)
7. [Light Resolver](Documentation/light-resolver.md)
8. [Local Light](Documentation/local-light.md)
9. [TimeManager](Documentation/time-manager.md)
. [Clock UI](Documentation/clock-ui.md)

Advanced topics:

- [Animator Blend Tree Setup](Documentation/animator-blend-tree-setup.md)
- [Horizontal Proxy](Documentation/horizontal-proxy.md)
- [Light Resolver](Documentation/light-resolver.md)
- [Occluders](Documentation/occluders.md)
- [Wall Bending](Documentation/wall-bending.md)
- [Optional Modules](Documentation/optional-modules.md)
- [Troubleshooting](Documentation/troubleshooting.md)

---

## Changelog

See:

- [Changelog](CHANGELOG.md)

---

## License

See:

- [License](LICENSE.md)
