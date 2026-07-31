# Harry's Burrow 3D — Agent Notes

This directory is a standalone mini-repo for the Harry's Burrow 3D browser game. It is intentionally separate from the main `phred-agent` codebase.

## File layout

```
web_content/harrys-burrow-3d/
├── index.html   # entire game (HTML, CSS, Three.js code)
├── README.md    # human-facing quick start
└── CLAUDE.md    # this file
```

## Architecture

- Single-file game using Three.js (loaded from CDN).
- Grid-based world:
  - `l = 0` — sky layer (empty, walkable without digging)
  - `l = 1` — surface grass (diggable)
  - `l = 2..4` — dirt layers
  - `l = 5` — bedrock floor
- The player (wombat), Harry, thieves, and the AI wombat live on the grid.
- Objects (items, rocks, dirt cubes, boulders, trees) are stored in arrays and rendered as Three.js meshes.

## Key constants

See the top of the `<script>` block in `index.html`:

- `BLOCK` — size of one grid cell.
- `COLS`, `ROWS` — horizontal grid dimensions.
- `LAYERS` — total vertical layers (including sky and bedrock).
- `SKY = 0`, `SURFACE = 1`, `BEDROCK_LAYER = LAYERS - 1`.

## Testing

### Syntax check

Extract the inline JavaScript and run Node's syntax checker:

```bash
python3 - <<'PY'
import re
with open('index.html') as f:
    html = f.read()
scripts = re.findall(r'<script[^>]*>(.*?)</script>', html, re.DOTALL)
open('temp/burrow_check.js','w').write('\n'.join(scripts))
PY
node --check temp/burrow_check.js
```

### Local server

```bash
python3 -m http.server 8000
# open http://localhost:8000
```

## Deployment

Copy `index.html` to the live directory:

```bash
cp index.html ~/phred.boppers.net/harrys-burrow-3d/index.html
```

Live URL: https://phred.boppers.net/harrys-burrow-3d/

## Conventions

- Keep the game in one file unless it becomes unwieldy.
- Prefer Three.js primitives over external assets for now.
- Do not add build tools or bundlers without checking with Harry/Matt.
- When changing mechanics, update this file and `README.md` if the public interface changes.

## Things that are intentionally rough / easy to extend

- The AI wombat uses a simple state machine and can get stuck.
- Boulders do not damage characters when they land.

## Positioning convention

Characters and loose objects always sit on top of the voxel/cube below them via `getFloorY(c, r, l)`. Each moving mesh stores its world height in `userData.height` so carried items can be held at mouth level rather than a fixed offset.
