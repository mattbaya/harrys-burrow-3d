# Harry's Burrow 3D

A browser-based digging game built with Three.js. You play as a wombat retrieving items from Harry Baya's BopList while Harry wanders around, falls in holes, and occasionally wanders off.

Play it live at: https://phred.boppers.net/harrys-burrow-3d/

## How to run locally

This is a single static HTML file. The easiest way to run it is with any local static server:

```bash
cd web_content/harrys-burrow-3d
python3 -m http.server 8000
# open http://localhost:8000
```

Or open `index.html` directly in a modern browser.

## Controls

| Key | Action |
|-----|--------|
| `W` / `A` / `S` / `D` or arrows | Move north / west / south / east |
| `I` / `K` or `PgUp` / `PgDn` | Move up / down a layer |
| `Space` | Pick up / drop an item or dirt cube |
| `P` | Stash item in pouch / take item out |
| `Shift` + direction | Push a chain of up to 6 rocks, dirt cubes, or found items |
| `E` | Drop a cube-shaped poop wall behind you |
| Camera panel | Toggle "Follow AI Wombat" to swap the camera target |
| `Q` | Sniff radar — points to the nearest item |
| `R` | Call Harry back when he wanders off |

## Core mechanics

- **Sky layer**: the wombat and Harry start above the grass and can walk around without digging.
- **Digging**: press `K` to drop to the grass, then dig down through dirt. Each full block is made of 1/4-size cubes; digging it spawns 4 small dirt cubes in the cardinal neighbor cells.
- **Items**: retrieve the objects on Harry's BopList and bring them back to him.
- **Pouch**: wombats can stash one item in their pouch, so you can carry two items at once.
- **Pushing**: hold `Shift` + direction to push a chain of up to 6 rocks, dirt cubes, or found items. Boulders and poop walls stop the chain.
- **Harry**: he gets bored and wanders, but now also picks his own projects — building towers, building ramps, chasing the AI wombat, or just wandering. He is not very aware and can fall into holes. Build dirt-cube walls around holes to keep him safe.
- **Filling holes**: drop or push a dirt cube into a dug surface hole to patch it.
- **Trees**: some items are hidden in trees; build a ramp of dirt cubes to reach them. Trees use a realistic GLTF model with a procedural fallback.
- **Boulders**: large 2×2 rocks that cannot be pushed; they fall if you dig out everything underneath them.
- **AI wombat**: a second autonomous wombat roams the map collecting rocks and occasionally dropping blue poop walls that only it can move.

## Deployment

The live version is deployed by copying `index.html` to:

```
~/phred.boppers.net/harrys-burrow-3d/index.html
```

No build step is required.

## Tech notes

- Single-file HTML/CSS/JS using Three.js loaded from a CDN.
- No external build tools or bundlers.
- Procedural 3D models for the wombat, Harry, rocks, trees, etc.
- All sounds are synthesized in the browser.
