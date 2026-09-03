# RayCast2D

*A ray that reports what it hits — not a physics body, not drawn on screen. Pairs well with [Line2D](../line2d/line2d.md) for anything that needs to visualize the ray, like a laser.*

---

## The core gotcha: everything is local space

`target_position` is **relative to the RayCast2D node's own position**, not global. The ray goes from the node's `position` to `position + target_position`, then that whole segment gets transformed by the node's parent chain like anything else in the scene tree.

```gdscript
# RayCast2D is a child of the ship, at local position (0, 0)
target_position = Vector2.RIGHT * 500   # "500 px in front of me," in local space
```

Because of this, if the RayCast2D is parented under your ship and you never touch its own `rotation`, it automatically points wherever the ship is facing — you don't need to recompute `target_position` from the ship's global rotation by hand. This is the same "points/positions are local, rotation is inherited for free" behavior Line2D has.

If instead you want the raycast to point in a *fixed global direction* regardless of parent rotation, set the RayCast2D's own `global_rotation = 0` (or whatever fixed angle you need) each frame, or do the trig yourself and feed a pre-rotated vector into `target_position`.

---

## Enabling and updating

- `enabled = true` — a disabled RayCast2D never collides with anything, `is_colliding()` always returns `false`.
- Normally it updates once per physics frame, right before `_physics_process`. If you change `target_position` or move the ship and then need a result **in the same frame** — e.g. before physics has ticked again — call:

```gdscript
force_raycast_update()
```

This is the one you'll want every frame for a continuously-updating laser: move/rotate the ship, update `target_position` if needed, force the update, then read the result.

---

## Reading a hit

All of these are only meaningful when `is_colliding()` is `true`:

| Method | Returns |
|---|---|
| `is_colliding()` | `bool` — did the ray hit something |
| `get_collider()` | the `Object` (usually a body) that was hit |
| `get_collision_point()` | hit point, in **global** coordinates |
| `get_collision_normal()` | surface normal at the hit point, in global direction |

Note the asymmetry: `target_position` is local, but `get_collision_point()` comes back global. If you're feeding the collision point into a Line2D whose points are local, you need to convert:

```gdscript
var local_hit := to_local(raycast.get_collision_point())
```

---

## Filtering what you hit

- `collision_mask` — which physics layers this ray can hit. Set this like any other physics node's mask.
- `exclude_parent` — `true` by default; stops the ray from hitting the body it's attached to.
- `add_exception(node)` / `add_exception_rid(rid)` — exclude specific other bodies (e.g. a ship excluding its own weapon hardpoints, or a friendly-fire exclusion list). `remove_exception(...)` / `clear_exceptions()` to undo.

---

## Continuous laser recipe

See the [Line2D cheat sheet](../line2d/line2d.md#continuous-laser-recipe) for the full frame-by-frame recipe that combines a RayCast2D with two Line2D nodes for the beam visual. Short version of this node's part in it:

```gdscript
func _physics_process(_delta: float) -> void:
    force_raycast_update()
    var end_point := target_position if not is_colliding() else to_local(get_collision_point())
    # ...hand end_point to the Line2D nodes...
```

---

## Quick-Reference Table

| Property/Method | Notes |
|---|---|
| `enabled` | must be `true` to collide at all |
| `target_position` | local-space end point of the ray |
| `force_raycast_update()` | recompute immediately, same frame |
| `is_colliding()` | `bool` |
| `get_collider()` | the hit `Object` |
| `get_collision_point()` | **global** coordinates |
| `get_collision_normal()` | global direction |
| `collision_mask` | which layers to hit |
| `exclude_parent` | default `true` |
| `add_exception(node)` | exclude a specific body |

---

*[← back to index](../../README.md)*
