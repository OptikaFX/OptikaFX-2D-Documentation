
# Troubleshooting

This page lists common issues and solutions when using OptikaFX 2D in Unity.

This guide is intended for users who installed the package through Unity and are using the included editor tools, inspectors and setup menus.

## Index

- [Setup Issues](#setup-issues)
- [Shadows Do Not Appear](#shadows-do-not-appear)
- [Receiver Issues](#receiver-issues)
- [Caster Issues](#caster-issues)
- [Wall Bending Issues](#wall-bending-issues)
- [Occluder Issues](#occluder-issues)
- [Camera and Shadow Rug Issues](#camera-and-shadow-rug-issues)
- [Global Light Issues](#global-light-issues)
- [Local Light Issues](#local-light-issues)
- [TimeManager Issues](#timemanager-issues)
- [Clock UI Issues](#clock-ui-issues)
- [Sorting and Layer Issues](#sorting-and-layer-issues)
- [Performance Issues](#performance-issues)
- [General Fix Checklist](#general-fix-checklist)
- [Resetting an Object](#resetting-an-object)

---

## Setup Issues

### Full Setup did not configure the scene correctly

Try running:

    OptikaFX 2D / 01. Run Full Setup

Then check that your scene contains:

- `OptikaFX 2D Manager`
- `Global Light`
- `TimeManager`
- A camera with `ShadowRenderQuad`

Also check that your project is using URP.

---

### URP is not configured

OptikaFX 2D requires Universal Render Pipeline.

Check:

- A URP Asset exists in the project.
- The URP Asset is assigned in Project Settings.
- Your camera uses a renderer that supports the required render features.
- You ran Full Setup after creating/importing the URP Asset.

Try:

    OptikaFX 2D / 01. Run Full Setup

---

### Required render features are missing

If shadows or occlusion are not rendering, the render features may be missing.

Try:

    OptikaFX 2D / 01. Run Full Setup

or:

    OptikaFX 2D / Settings / Setup by Steps / Setup URP

Then check your URP Renderer asset and confirm the OptikaFX render features were added.

---

## Shadows Do Not Appear

### No shadows are visible at all

Check:

- The scene has a `Global Light`.
- The camera has `ShadowRenderQuad`.
- At least one shadow rug is enabled.
- A `Caster` exists.
- A `Receiver` exists.
- The receiver material is assigned.
- The caster has a valid `Source Renderer`.
- The camera sorting layer/order allows shadows to be visible.
- Full Setup was run.

Recommended first fix:

    OptikaFX 2D / 01. Run Full Setup

---

### Shadows are generated but not visible

Check the active camera:

- It has `ShadowRenderQuad`.
- The `Shadow Rugs` list has at least one enabled rug.
- The rug has a valid Quad Material.
- The rug sorting layer/order is not hidden behind other renderers.

Also check:

- The receiver is visible.
- The receiver is enabled.
- The receiver area type matches the rug rules.

---

### Shadows appear in Scene view but not Game view

Check:

- The Game view camera is the same camera configured by OptikaFX.
- The active camera has `ShadowRenderQuad`.
- The camera is enabled.
- The camera is rendering the correct sorting layers.
- The URP renderer used by the camera has OptikaFX render features.

---

## Receiver Issues

### Receiver material is empty

Check:

    Assets/OptikaFX 2D/Settings/ProjectDefaults.asset

Make sure the Shadow Receiver Material is assigned.

Then select the receiver again or re-run:

    OptikaFX 2D / 01. Run Full Setup

If needed, remove and add the receiver again.

---

### Shadows do not appear on a receiver

Check:

- The object has `ShadowReceiver` or `TilemapShadowReceiver`.
- The receiver is enabled.
- The receiver material is assigned.
- The object has a `ShadowArea`.
- The ShadowArea type is correct.
- The camera has a shadow rug that allows that area.
- Elevation levels match.

---

### Shadows appear on the wrong surface

Check:

- ShadowArea type.
- Required Area on the camera shadow rug.
- Blocked By rules.
- Receiver elevation level.
- Caster elevation level.
- Shadow rug elevation level.

Example:

- Ground shadows need Ground area or `Anywhere`.
- Wall shadows need Wall area or a wall-compatible rug.
- Water/Hole areas can block shadows if configured.

---

### Water or holes do not block shadows

Check:

- The water/hole object has `ShadowArea`.
- The ShadowArea type is `Hole_Water_Blue`.
- The camera shadow rug uses `Blocked By: Holes And Water`.
- The material/shadow rug is assigned correctly.
- The object is rendered into the mask.

---

### Tilemap receiver does not update

Check:

- The object has `TilemapShadowReceiver`.
- The tilemap has tiles.
- The hidden shadow tilemap exists.
- The receiver is enabled.
- Auto sync is enabled if editing in the editor.
- Sorting layer/order matches the visible tilemap.

If needed, disable and re-enable the component.

---

## Caster Issues

### Caster does not create a shadow

Check:

- The object has a `Caster`.
- `Source Renderer` is assigned.
- The source SpriteRenderer has a valid sprite.
- The object has a visible sprite.
- A receiver exists in the scene.
- A Global Light exists.
- Shadow material/profile is assigned.

If the source renderer is empty, remove and add the caster again using the setup menu.

---

### Shadow is too long or too short

Adjust:

- Caster `Projection Length Multiplier`
- GlobalLight `Projection Length`
- TimeManager lighting preset projection length
- LocalLight projection length if using local lights

---

### Shadow is too dark

Adjust:

- Caster `Alpha Multiplier`
- GlobalLight intensity
- Shadow color alpha
- ShadowProfile settings
- LocalLight intensity
- Shadow rug material/settings

---

### Shadow is invisible or too transparent

Check:

- Caster `Alpha Multiplier` is not zero.
- GlobalLight intensity is not zero.
- Shadow color alpha is not zero.
- Receiver material is assigned.
- Sorting layer/order is correct.
- Required Area rules allow drawing.

---

### Shadow follows the wrong sprite

Check:

- `Source Renderer` points to the correct SpriteRenderer.
- It is not assigned to a generated shadow object.
- It is not assigned to a proxy object by mistake.
- The selected object is the actual character/prop source.

---

### Auto remap does not follow the character direction

Check:

- The Caster is using `Rotation` or `Mixed` mode.
- Caster Motion Mode is set to animated/dynamic.
- Animated direction remap is enabled.
- Source Animator is assigned.
- Animator movement parameters exist.
- Parameter names match exactly.
- Movement parameters are being updated at runtime.

If the character stops and the shadow snaps to the wrong direction, make sure last movement direction values are stored only when movement input is not zero.

See:

- [Animator Blend Tree Setup for Auto Remap](animator-blend-tree-setup.md)

---

### Shadow becomes too thin when moving horizontally

This can happen with side-facing sprites or horizontal auto remap.

For `Perspective` mode:

- Use a Horizontal Shadow Proxy.
- Increase proxy width or scale.
- Adjust projection and flatten settings.

For `Rotation` and `Mixed` modes:

- Use Animator Auto Remap.
- Check your Animator Blend Tree setup.
- Tune width/remap settings in the Caster inspector.

Important:

- Horizontal Proxy works only with `Perspective` mode.
- `Rotation` and `Mixed` modes do not use Horizontal Proxy.

For best results, see:

- [Animator Blend Tree Setup for Auto Remap](animator-blend-tree-setup.md)
- [Horizontal Shadow Proxy](horizontal-proxy.md)

---

### Blob shadow does not appear

Check:

- Caster mode is `TopDownBlob`, or blob fallback is enabled.
- Blob Preset is assigned.
- Blob sprites exist in the preset.
- Blob alpha is not zero.
- Blob size index is valid.
- Blob visibility settings allow it to show.

---

## Wall Bending Issues

### Wall bending does not work

Check:

- The object uses a `Caster`.
- The Caster uses `Perspective` or `Mixed` mode.
- Wall Bending is enabled.
- A wall receiver exists.
- The wall object has `ShadowReceiver` or `TilemapShadowReceiver`.
- The wall ShadowArea type is Wall.
- Detection settings are correct.
- The caster is close enough to the wall.
- The shadow angle allows wall bending.

---

### Shadow bends on the wrong object

Check:

- Wall layer settings.
- Detection mode.
- Ignore Own Hierarchy.
- Receiver overlap.
- Wall ID.
- Nearby wall receivers.

---

### Shadow is clipped too early

Adjust:

- Wall Shadow Offset
- Raycast Hit Offset
- Min Hit Distance Separation
- Min Angle From Horizontal
- Projection Length Multiplier

---

### Blob shadows do not bend

This is expected.

Blob shadows are contact shadows and are not designed for wall bending.

Use `Perspective` or `Mixed` mode for wall bending.

---

### Occluders do not bend on walls

This is expected.

Occluders are not compatible with Wall Bending.

Use a `Caster` if you need shadows to bend or interact with walls.

---

## Occluder Issues

### Object Occluder does not generate shadow

Check:

- The object has `ObjectOccluder`.
- The occluder is enabled.
- Source SpriteRenderer is assigned.
- The SpriteRenderer has a valid sprite.
- The sprite has visible alpha above the alpha threshold.
- Global or local lights exist.
- Occlusion render feature is installed.

---

### Tilemap Occluder does not work

Check:

- The object has `CompositeTilemapOccluder`.
- The object has a Tilemap.
- The tilemap has tiles.
- Required collider components exist.
- Topology was rebuilt.
- The occluder is enabled.

Try rebuilding the topology from the component if available.

---

### Occluder shape is too large or too small

Check:

- Sprite import settings.
- Sprite pivot.
- Transform scale.
- Composite shape scale.
- Projection length multiplier.
- Tilemap cell size.

---

### Occluder shadow is too weak or invisible

Adjust:

- Mask Alpha Multiplier
- Alpha Multiplier
- Alpha Threshold
- GlobalLight intensity
- LocalLight intensity
- Shadow color alpha

---

## Camera and Shadow Rug Issues

### Camera does not display shadows

Check:

- The active camera has `ShadowRenderQuad`.
- At least one shadow rug is enabled.
- Quad Material is assigned.
- Sorting Layer and Sorting Order are correct.
- The camera is the one used in Game view.

---

### Shadows render behind sprites

Adjust:

- Shadow rug Sorting Layer.
- Shadow rug Sorting Order.
- SpriteRenderer sorting settings.
- TilemapRenderer sorting settings.

---

### Shadows render over everything

Lower the shadow rug Sorting Order.

Example:

    Sorting Order: -100

Or use a sorting layer that renders below characters.

---

### Environment shadows are duplicated

Check if environment shadow is enabled on more than one shadow rug.

Usually only one main rug should include environment shadows.

---

### Shadows appear on wrong elevation

Check:

- Caster elevation level.
- Receiver elevation level.
- Shadow rug elevation level.
- Camera ShadowRenderQuad configuration.

---

## Global Light Issues

### Global Light does not affect shadows

Check:

- A `GlobalLight` exists in the scene.
- It is enabled.
- Intensity is above zero.
- Projection Length is above zero.
- Shadow color alpha is not zero.
- Caster uses global light direction.
- Receiver and camera shadow rug are configured.

---

### Global Light does not follow TimeManager

Check:

- TimeManager component is enabled.
- GlobalLight mode is `TimeManager`.
- TimeManager is allowed to control OptikaFX Global Light.
- A DayLightingPreset is assigned.

---

### Global Light still follows TimeManager after disabling it

Check:

- There is no other active TimeManager in the scene.
- The TimeManager component is disabled.
- GlobalLight automatic sync is enabled.
- GlobalLight has returned to Manual mode.

---

### Manual values changed after using TimeManager

If enabled, GlobalLight can restore manual values when TimeManager becomes inactive.

Check:

- Restore manual values option is enabled.
- Manual values were set before enabling TimeManager.
- No other script is changing the GlobalLight.

---

## Local Light Issues

### Local Light does not affect shadows

Check:

- The object has a `LocalLight`.
- LocalLight is enabled.
- Range is large enough.
- Caster is inside the range.
- Intensity is above zero.
- Priority is high enough if multiple lights overlap.
- Caster supports local light influence.

---

### Local Light is too strong

Adjust:

- Intensity
- Range
- Blend With Global
- Override Global
- Projection Length
- Shadow color alpha

---

### Spot light points in the wrong direction

Adjust:

- Rotation Angle
- Transform rotation
- Spot Angle
- Anchor position

---

### Local Light does not follow Unity Light2D

Run:

    OptikaFX 2D / Configure Lights

Then check that the LocalLight component was added to the same object as the Unity Light2D.

---

## TimeManager Issues

### Time does not advance

Check:

- TimeManager component is enabled.
- Time Speed Multiplier is above zero.
- Time is not paused.
- No external time blocker is active.
- Real Seconds Per In-Game Minute is above zero.

---

### TimeManager does not control lighting

Check:

- TimeManager component is enabled.
- GlobalLight exists.
- GlobalLight is in TimeManager mode.
- Control Unity Global Light is enabled if you want to affect Unity Light2D.
- Control OptikaFX Global Light is enabled if you want to affect GlobalLight.
- A DayLightingPreset is assigned.

---

### DayLightingPreset is missing

TimeManager can automatically assign:

    Assets/OptikaFX 2D/DayLightingPresets/SunnyDay.asset

If it is not assigned:

- Make sure the preset exists.
- Re-run Full Setup.
- Assign the preset manually.

---

### Sleep does not work as expected

Check:

- Normal wake time.
- Late sleep threshold.
- Late wake time.
- Forced sleep settings.
- Extended hours setting.

If using extended hours, values above 23 are valid.

Example:

    27:45 = 03:45 AM next day

---

## Clock UI Issues

### Clock UI does not appear

Check:

- TextMeshPro is installed.
- Clock UI Canvas exists.
- Canvas is enabled.
- TMP text objects are enabled.
- Canvas sorting order is visible.
- The UI was created from the TimeManager inspector.

---

### Clock UI module is missing

Check:

- TextMeshPro is installed.
- Unity has finished compiling.
- Try reopening the TimeManager inspector.
- Try creating the Clock UI again.

---

### Clock UI does not update

Check:

- TimeManager exists.
- TimeManager component is enabled.
- ClockUI has a TimeManager reference.
- Auto Find TimeManager is enabled.
- Date Text and Time Text are assigned.
- Refresh On Enable is enabled.

You can also use the component context menu:

    Refresh Clock UI

---

### Clock UI duplicates after changing scenes

Enable:

- Dont Destroy On Load
- Prevent Duplicates

Make sure only one persistent Clock UI exists in the first loaded scene.

---

## Sorting and Layer Issues

### Shadows appear in the wrong order

Check:

- Shadow rug Sorting Layer.
- Shadow rug Sorting Order.
- Receiver sorting layer/order.
- SpriteRenderer sorting layer/order.
- TilemapRenderer sorting layer/order.

---

### Shadows are hidden behind tilemaps

Increase or adjust the shadow rug Sorting Order.

Also check TilemapRenderer sorting settings.

---

### Shadows render above the character

Lower the shadow rug Sorting Order or use a sorting layer below the character.

---

## Performance Issues

### Shadows are expensive

Try:

- Reduce number of active casters.
- Reduce number of active local lights.
- Disable unnecessary occluders.
- Avoid rebuilding tilemap occluders every frame.
- Use Blob shadows for simple characters.
- Use fewer shadow rugs.
- Reduce expensive debug overlays in runtime.

---

### Tilemap occluder is slow

Check:

- Rebuild Every Frame is disabled unless needed.
- Runtime refresh is disabled unless needed.
- Tilemap topology is not rebuilt unnecessarily.
- Large tilemaps are split into smaller sections if needed.

---

### Too many local lights

Try:

- Reduce local light range.
- Lower max lights per occluder/caster if available.
- Disable lights when far from the camera/player.
- Use priority to control which lights matter.

---

## General Fix Checklist

If something is not working, try this order:

1. Run Full Setup.
2. Check the Console for errors.
3. Select the camera and verify `ShadowRenderQuad`.
4. Select the Global Light and verify it is enabled.
5. Select the caster and verify `Source Renderer`.
6. Select the receiver and verify material and area type.
7. Check sorting layer/order.
8. Check elevation levels.
9. Check TimeManager state if using day/night.
10. Re-add the component using OptikaFX setup menus.
11. Restart Unity if optional UI/debug modules were just installed.

---

## Resetting an Object

To remove OptikaFX components from selected objects, use:

    GameObject / OptikaFX 2D / Remove OptikaFX 2D Components from Selected Object

This removes OptikaFX components while keeping Unity native components such as:

- Transform
- SpriteRenderer
- Tilemap
- Camera
- Light2D

---

← [Back to Documentation Index](index.md)
