# CLAUDE.md

Guidance for Claude Code when working in this repo.

## What this is

A personal wiki of cheat sheets — Jason is the real audience, despite this being a
public repo. Each sheet distills a topic to the minimum needed to read and write
code again after forgetting the details. Not tutorials, not full courses. The
trigger for a new sheet is usually "I had to look this up again."

Built openly with Claude's help — that's fine to be visible in commit history and
the README, no need to hide or downplay it.

## Structure

- `cheatsheets/<topic>/<topic>.md` — one self-contained folder per topic, named
  the same as the file inside it (e.g. `cheatsheets/vector-math-2d-physics/
  vector-math-2d-physics.md`). No category subfolders — topics are siblings
  directly under `cheatsheets/`, so cross-links between sheets never break as the
  collection grows or gets recategorized.
- `cheatsheets/<topic>/images/*.svg` — diagrams for that topic, colocated in its
  own folder rather than a shared images directory. Keeps each topic deletable/
  movable as a unit and avoids filename collisions across unrelated topics
  (`arrow.svg` in one sheet doesn't clash with `arrow.svg` in another).
- `README.md` — the index. Categories are `##` sections listing links to sheets,
  e.g. `## Game Dev / Math`. A sheet can be listed under more than one category if
  it's cross-cutting.
  - If `README.md` gets unwieldy, split a category out into its own
    `CATEGORY-NAME.md` (list of links, same shape as the README section it
    replaced) and link to it from the README instead of inlining the list. Don't
    do this preemptively — only once a section is actually crowding the index.

A sheet's own back-link footer and its image references are both relative, so a
sheet at `cheatsheets/<topic>/<topic>.md` links to the repo root README as
`../../README.md` and to its own images as `images/<name>.svg`.

## Writing a sheet

No fixed template — structure follows the topic. `vector-math-2d-physics.md` is
one example (numbered concepts, one diagram each, closing quick-reference table),
not a mold to force every topic into. A sheet about signals or node lifecycle
might read better as tables with no diagrams at all. What should carry across
every sheet regardless of structure:

- Terse. Prose exists to connect ideas, not pad them.
- Assumes the reader already codes — explain the Godot/GDScript-specific gotcha,
  not general programming concepts.
- A diagram earns its place only when a picture genuinely clarifies something
  words fumble (spatial/geometric relationships, before/after transforms). Don't
  add one out of habit.
- End with `*[← back to index](../../README.md)*`.
- Add the new sheet to the relevant `README.md` section (or `CATEGORY-NAME.md` if
  that category has been split out) in the same change.
- Link to related sheets inline where it's genuinely useful, using relative paths
  up and back down into the other topic's folder (`[normalize](../vector-math-2d-physics/vector-math-2d-physics.md)`)
  — don't force a "see also" section if nothing fits.

### Diagram conventions (SVG)

Look at an existing sheet's images before adding a new one. Conventions so far:
- Solid white background (`fill="#ffffff"` on a full-size `rect`) — these render
  on GitHub, not in a themeable app, so no dark-mode variables.
- A small fixed palette reused across diagrams (`#e94560` red, `#0f3460` navy,
  `#16a34a` green) rather than inventing new colors per sheet.
- `font-family="sans-serif"`, labels sized ~14px.
- Keep each SVG scoped to one idea — this repo builds one diagram per concept,
  not one big composite figure per sheet.

## Proposing new sheets

Jason will usually bring the topic. But if you notice a pattern worth capturing —
you've explained the same Godot/GDScript thing more than once in a session, or hit
a gotcha mid-task that would've been faster with a reference — say so and suggest
adding a sheet. Don't just add it unprompted; propose it, then write it once he
agrees.
