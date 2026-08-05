
# OptikaFX 2D Documentation

Welcome to the OptikaFX 2D documentation.

OptikaFX 2D is a dynamic 2D shadows and lighting framework for Unity URP. It provides tools for projected shadows, wall bending, blob shadows, occlusion, receivers, global/local lights, Light Resolver, TimeManager, optional Clock UI and editor setup workflows.

---

## Getting Started

Start here if this is your first time using OptikaFX 2D.

- [Quick Start](quick-start.md)

The Quick Start guide explains how to:

- Run Full Setup
- Add receivers
- Add casters
- Configure lighting
- Configure the camera
- Understand the OptikaFX 2D Manager
- Test shadows in Play Mode

---

## Core Setup

These pages explain the main systems required for shadows to appear correctly.

- [Camera](camera.md)
- [Receivers](receivers.md)
- [Casters](casters.md)
- [Global Light](global-light.md)
- [Light Resolver](light-resolver.md)

Recommended reading order:

1. [Quick Start](quick-start.md)
2. [Camera](camera.md)
3. [Receivers](receivers.md)
4. [Casters](casters.md)
5. [Global Light](global-light.md)
6. [Light Resolver](light-resolver.md)

---

## Shadows

These pages explain how OptikaFX shadow generation works.

- [Casters](casters.md)
- [Receivers](receivers.md)
- [Camera](camera.md)
- [Light Resolver](light-resolver.md)
- [Wall Bending](wall-bending.md)
- [Occluders](occluders.md)

Use these guides to understand:

- How sprites cast shadows
- How surfaces receive shadows
- How shadow mattes are configured on the camera
- How shadow resolving modes work
- How multi-shadow behavior is controlled
- How shadows bend onto walls
- How occluders create environment masks and silhouettes

---

## Light Resolver

The Light Resolver is created automatically by setup and lives on the **OptikaFX 2D Manager**.

- [Light Resolver](light-resolver.md)

Use this guide when:

- You want to understand how shadow resolve modes work
- You want to configure MultiShadow behavior
- You want to tune washout between global and local shadows
- You want to use global shadow pattern textures
- You want to configure occlusion shadow resolving
- You need advanced project-wide shadow customization

---

## Caster Advanced Setup

These pages are especially useful for animated characters and directional sprites.

- [Animator Blend Tree Setup](animator-blend-tree-setup.md)
- [Horizontal Proxy](horizontal-proxy.md)
- [Casters](casters.md)
- [Light Resolver](light-resolver.md)

Use these guides when:

- Your character uses a 2D Blend Tree
- You want auto remap to follow movement direction
- Shadows become too thin at horizontal angles
- You need better side-facing shadow silhouettes
- You need to tune how casters respond to global and local lights

---

## Receivers and Wall Interaction

These pages explain shadow receiving surfaces and wall behavior.

- [Receivers](receivers.md)
- [Wall Bending](wall-bending.md)
- [Camera](camera.md)
- [Light Resolver](light-resolver.md)

Use these guides when:

- Shadows do not appear on the correct surface
- You need ground and wall receivers
- Wall bending is not working
- Water or holes should block shadows
- Elevation levels need to be configured
- Shadow resolving or washout affects the final result

---

## Lighting

These pages explain scene-wide, local and resolved lighting behavior.

- [Global Light](global-light.md)
- [Local Light](local-light.md)
- [Light Resolver](light-resolver.md)
- [TimeManager](time-manager.md)

Use these guides when:

- You need basic global shadows
- You want day/night lighting
- You want torches, lamps or flashlights
- You want local light effects
- You want MultiShadow, VectorBlend or DominantLight behavior
- You want global shadow texture patterns
- Global Light is not following TimeManager correctly

---

## Time and UI

These pages explain time progression and optional UI.

- [TimeManager](time-manager.md)
- [Clock UI](clock-ui.md)
- [Optional Modules](optional-modules.md)

Use these guides when:

- You want in-game time
- You want seasons, days and sleep rules
- You want day/night lighting presets
- You want a clock/date UI
- You need TextMeshPro only for Clock UI

---

## Occlusion

These pages explain environment occlusion and silhouettes.

- [Occluders](occluders.md)
- [Camera](camera.md)
- [Global Light](global-light.md)
- [Local Light](local-light.md)
- [Light Resolver](light-resolver.md)

Use these guides when:

- You need environment shadow masks
- You need tilemap occlusion
- You need object silhouettes
- You need static obstacle/environment shadows
- You want occlusion to react to global or local lights
- You want to configure occlusion resolve modes

Important:

- Occluders are not compatible with Wall Bending.
- Use a `Caster` if you need shadows to bend onto walls.
- Occlusion resolving is configured through the Light Resolver.

See:

- [Wall Bending](wall-bending.md)
- [Light Resolver](light-resolver.md)

---

## Optional Features

These pages explain features that require optional Unity packages.

- [Clock UI](clock-ui.md)
- [Optional Modules](optional-modules.md)

Optional dependencies:

| Feature | Required Package |
|---|---|
| Clock UI | TextMeshPro |
| Runtime Debug Input Tools | Input System |

The core OptikaFX shadow system does not require these optional packages.

---

## Troubleshooting

If something is not working, start here:

- [Troubleshooting](troubleshooting.md)

The troubleshooting guide covers:

- Setup issues
- Missing shadows
- Receiver problems
- Caster problems
- Wall bending issues
- Occluder issues
- Camera and shadow matte issues
- Global Light issues
- Local Light issues
- Light Resolver issues
- TimeManager issues
- Clock UI issues
- Sorting and layer issues
- Performance issues

---

## Documentation Files

All documentation pages:

- [Animator Blend Tree Setup](animator-blend-tree-setup.md)
- [Camera](camera.md)
- [Casters](casters.md)
- [Clock UI](clock-ui.md)
- [Global Light](global-light.md)
- [Horizontal Proxy](horizontal-proxy.md)
- [Light Resolver](light-resolver.md)
- [Local Light](local-light.md)
- [Occluders](occluders.md)
- [Optional Modules](optional-modules.md)
- [Quick Start](quick-start.md)
- [Receivers](receivers.md)
- [TimeManager](time-manager.md)
- [Troubleshooting](troubleshooting.md)
- [Wall Bending](wall-bending.md)

---

## Recommended Learning Path

If you are new to OptikaFX 2D, follow this order:

1. [Quick Start](quick-start.md)
2. [Camera](camera.md)
3. [Receivers](receivers.md)
4. [Casters](casters.md)
5. [Global Light](global-light.md)
6. [Light Resolver](light-resolver.md)
7. [Local Light](local-light.md)
8. [TimeManager](time-manager.md)
9. [Clock UI](clock-ui.md)

Then continue with advanced topics:

1. [Animator Blend Tree Setup](animator-blend-tree-setup.md)
2. [Horizontal Proxy](horizontal-proxy.md)
3. [Wall Bending](wall-bending.md)
4. [Occluders](occluders.md)
5. [Optional Modules](optional-modules.md)
6. [Troubleshooting](troubleshooting.md)

---

## Common First Steps

For most scenes:

1. Run Full Setup.
2. Confirm the OptikaFX 2D Manager was created.
3. Configure the camera shadow mattes.
4. Add receivers to ground and walls.
5. Add casters to sprites.
6. Adjust Global Light.
7. Review Light Resolver settings if you need advanced shadow behavior.
8. Add Local Lights if needed.
9. Enable TimeManager if you want day/night lighting.
10. Create Clock UI if needed.
11. Test in Play Mode.

See:

- [Quick Start](quick-start.md)
- [Light Resolver](light-resolver.md)
- [Troubleshooting](troubleshooting.md)
