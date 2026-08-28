# Control Nodes Reference

*Which node for which job — not how positioning works. For that, see
[Control Foundations](../control-foundations/control-foundations.md).*

---

## Display

| Node | Use it for | Watch out for |
|---|---|---|
| `Label` | static or occasionally-updated text | doesn't expand to fill space by default — set size flags or `autowrap_mode` if text can grow |
| `RichTextLabel` | multi-style text: colored/bold spans, dialogue, scrolling logs | needs `bbcode_enabled = true` to use `[b]`/`[color]` tags; plain `.text` still works without it |
| `TextureRect` | a static icon or image (portraits, item icons) | set `stretch_mode` (`KEEP`, `SCALE`, `KEEP_ASPECT`) or it'll distort to fill its rect |
| `NinePatchRect` | a panel-like background that scales without stretching its corners | set the patch margins to match your art's border width |

## Bars & Meters

| Node | Use it for | Watch out for |
|---|---|---|
| `ProgressBar` | quick health/mana/loading bar with theme styling | looks like default Godot theme unless you reskin it — fine for prototyping, not final art |
| `TextureProgressBar` | a bar skinned from your own texture (the usual choice for a real game HUD) | needs `texture_under`/`texture_progress` set, and a `fill_mode` matching your art (left-to-right, radial, etc.) |

## Input

| Node | Use it for | Watch out for |
|---|---|---|
| `Button` | clickable text/icon button with built-in hover/press states | uses theme styling like ProgressBar — reskin via theme or `TextureButton` |
| `TextureButton` | a button skinned entirely from textures (normal/hover/pressed) | no built-in label — layer a `Label` over it if you need text |

## Containers (auto-layout — see Control Foundations for how they claim space)

| Node | Use it for |
|---|---|
| `VBoxContainer` | stack children vertically |
| `HBoxContainer` | stack children horizontally |
| `GridContainer` | wrap children into a fixed number of columns (inventory grids, skill icons) |
| `MarginContainer` | add padding around a single child (via `theme_override_constants/margin_*`) |
| `CenterContainer` | center a single child, ignoring its anchors |
| `PanelContainer` | draw a background/border behind a single child, sized to fit it |

**`Panel` vs `PanelContainer`:** `Panel` just draws a background — it does not lay
out its children at all, they behave like normal free Controls inside it. Use
`Panel` when you want a backdrop with manually-positioned or anchored content;
use `PanelContainer` when you want the backdrop to size itself to a container-laid-out child.

---

*[← back to index](../../README.md)*
