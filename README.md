# Cheat Sheets

A growing collection of reference sheets — concepts distilled to the minimum needed to read and write code, not full courses.

## Game Dev / Math

- [Vector Math for Hand-Rolled 2D Physics](cheatsheets/vector-math-2d-physics/vector-math-2d-physics.md) — subtraction as "to minus from," normalize, dot product, normal/tangent decomposition, `move_toward` vs `lerp`.

## Godot / 2D Nodes

- [Sprite Facing & Rotation Convention](cheatsheets/sprite-facing-and-rotation/sprite-facing-and-rotation.md) — why art should face right at rotation 0, `flip_h` vs. rotating, `look_at()`, offsetting art that faces the wrong way.
- [RayCast2D](cheatsheets/raycast2d/raycast2d.md) — local vs. global space gotchas, `force_raycast_update()`, reading hits, filtering with exceptions/masks.
- [Line2D](cheatsheets/line2d/line2d.md) — width/color/gradient, the two-layer core+glow trick for a laser beam, wiring points to a RayCast2D each frame.

## Godot / UI

- [Control Foundations](cheatsheets/control-foundations/control-foundations.md) — why your UI won't stay put: anchors, offsets, size flags, and how containers take over positioning.
- [Control Nodes Reference](cheatsheets/control-nodes-reference/control-nodes-reference.md) — quick "what's this node for" pass over Label, ProgressBar, containers, and friends.
- [HUD Recipes](cheatsheets/hud-recipes/hud-recipes.md) — assembled examples: health bar, corner status readout, updating a HUD from game state via signals.

<!-- Add new entries here as sheets are added -->
