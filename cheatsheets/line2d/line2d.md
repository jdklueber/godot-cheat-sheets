# Line2D

*A polyline you draw by giving it points. No physics, no collision — pure visual. Pairs well with [RayCast2D](../raycast2d/raycast2d.md) for anything that needs a ray it can also see, like a laser.*

---

## Points are local space

`points` is a `PackedVector2Array`, relative to the Line2D node's own transform. So a Line2D parented under your ship inherits the ship's `rotation` automatically — you don't recompute point positions when the ship turns, same as RayCast2D's `target_position`. You only ever touch `points` to change the *shape* of the line (e.g. moving the endpoint of a laser), not to keep it aimed correctly.

```gdscript
points = [Vector2.ZERO, Vector2.RIGHT * 500]   # from the node, 500px "forward" in local space
```

---

## Core + glow recipe

Godot's 2D renderer has no built-in blur/bloom on a node, so a glowing beam is faked with **two overlapping Line2D nodes**:

1. **Glow layer** (add first, so it renders underneath): wide, low-alpha, same color family. `width` large (e.g. 3-5x the core), `default_color` with alpha around 0.15–0.3.
2. **Core layer** (add second, on top): narrow, opaque, bright/near-white. `width` small, `default_color` alpha near 1.0.

```gdscript
# glow (child added first / lower in tree = renders behind)
$Glow.width = 20.0
$Glow.default_color = Color(0.4, 0.9, 1.0, 0.25)

# core
$Core.width = 4.0
$Core.default_color = Color(0.9, 1.0, 1.0, 1.0)
```

Both nodes should share the same `points` — update them together (see recipe below). If one glow pass looks too weak or too harsh a step from the core, stack a second, even-wider, even-lower-alpha glow layer underneath rather than fighting one layer's numbers.

---

## Width control

- `width` — flat width in pixels, applied along the whole line.
- `width_curve` — a `Curve` resource that scales `width` along the line's length (0.0 at the start point, 1.0 at the end). Use this for a taper — e.g. a beam that narrows toward its far end, or pulses at the impact point.

---

## Color control

- `default_color` — flat color/alpha for the whole line, the common case for a laser.
- `gradient` — a `Gradient` resource for color/alpha that varies along the line's length instead. Useful for a beam that fades toward its tip, or shifts hue along its length. If `gradient` is set it overrides `default_color`.

---

## Joint and cap settings

`joint_mode`, `begin_cap_mode`, `end_cap_mode` control how corners and endpoints are drawn. For a straight 2-point laser (no interior corners) these barely matter — the one exception is `end_cap_mode`: set it to `LINE_CAP_ROUND` if you want the beam's tip to look rounded off rather than cut square at the impact point.

---

## Continuous laser recipe

Full frame-by-frame pattern combining a RayCast2D with the two Line2D layers above. All three nodes are siblings/children under the ship, so all of them inherit its rotation for free — only the *end point* needs updating each frame.

```gdscript
@onready var beam_cast: RayCast2D = $BeamCast
@onready var core: Line2D = $Core
@onready var glow: Line2D = $Glow

func _physics_process(_delta: float) -> void:
    beam_cast.force_raycast_update()

    var end_point: Vector2
    if beam_cast.is_colliding():
        end_point = beam_cast.to_local(beam_cast.get_collision_point())
    else:
        end_point = beam_cast.target_position

    core.points[1] = end_point
    glow.points[1] = end_point
```

See the [RayCast2D cheat sheet](../raycast2d/raycast2d.md) for how `target_position`, `force_raycast_update()`, and `get_collision_point()` behave.

---

## Texture mode (optional)

For a scrolling/animated beam instead of flat color: set `texture` to a strip texture, and `texture_mode` to `LINE_TEXTURE_TILE` (repeats along the length, good for a moving "energy flow" look) or `LINE_TEXTURE_STRETCH` (stretches one copy across the whole line). Combine with an `AnimatableBody2D`-style offset scroll by animating the texture's UV if you want motion.

---

## Quick-Reference Table

| Property | Notes |
|---|---|
| `points` | local-space `PackedVector2Array` |
| `width` | flat width in px |
| `width_curve` | tapers width along the line (0→1) |
| `default_color` | flat color/alpha |
| `gradient` | overrides `default_color`, varies along length |
| `joint_mode` | corner style (irrelevant for 2-point lines) |
| `begin_cap_mode` / `end_cap_mode` | endpoint style; round the tip with `LINE_CAP_ROUND` |
| `texture` / `texture_mode` | scrolling/tiled beam texture |

---

*[← back to index](../../README.md)*
