# HUD Recipes

*Assembled examples. For the "why" behind anchors/containers, see
[Control Foundations](../control-foundations/control-foundations.md). For "what's
this node," see [Control Nodes Reference](../control-nodes-reference/control-nodes-reference.md).*

More recipes get added here as they're actually needed — this isn't meant to be
exhaustive.

---

## The general pattern: signal, don't poll

Every recipe below follows the same wiring: the HUD does **not** reach into the
player/game state every `_process` frame to check "did health change?" Instead,
whatever owns the state emits a signal when it changes, and the HUD listens.

```gdscript
# Player.gd
signal health_changed(current: int, max: int)

var health: int = 100:
    set(value):
        health = value
        health_changed.emit(health, max_health)
```

```gdscript
# HUD.gd
func _ready() -> void:
    player.health_changed.connect(_on_health_changed)

func _on_health_changed(current: int, max: int) -> void:
    health_bar.value = current
    health_bar.max_value = max
```

This keeps the HUD decoupled — it doesn't need to know *when* health changes, just
*that* it did, and it only updates when there's actually something new to show.

---

## Recipe: Health Bar

```
CanvasLayer
└── MarginContainer          (anchor preset: top-left; theme margin ~16px)
    └── TextureProgressBar   (fill_mode = LEFT_TO_RIGHT)
```

- `MarginContainer` gives you screen-edge padding without hand-tuning offsets.
- Reach for `TextureProgressBar` over plain `ProgressBar` as soon as you have real
  art for it — see [Control Nodes Reference](../control-nodes-reference/control-nodes-reference.md#bars--meters).
- Wire `value`/`max_value` via the signal pattern above, not by polling.

---

## Recipe: Corner Status Readout

A stack of labels (score, ammo, wave number, whatever) pinned to a screen corner:

```
CanvasLayer
└── MarginContainer          (anchor preset: top-right)
    └── VBoxContainer
        ├── Label            ("Score: 0")
        ├── Label            ("Wave: 1")
        └── Label            ("Ammo: 30 / 90")
```

- `VBoxContainer` stacks the labels and keeps them tight regardless of individual
  text length — no manual y-offsets to maintain.
- Anchor the **`MarginContainer`**, not the individual labels — labels inside a
  container ignore their own anchors anyway (see
  [Control Foundations](../control-foundations/control-foundations.md#containers-take-over-positioning-entirely)).
- If a label's text length varies a lot (e.g. "Ammo: 999 / 999" vs "Ammo: 3 / 90")
  and it's shifting other UI around, give the `VBoxContainer` a fixed
  `custom_minimum_size.x` so the right edge doesn't jitter.

---

*[← back to index](../../README.md)*
