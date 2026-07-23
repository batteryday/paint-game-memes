# paint-game-memes

Paint-by-numbers design data for the Roblox game **Paint A Meme**, served to the live game
over [jsDelivr](https://www.jsdelivr.com/).

Roblox can't decode an image at runtime, so each meme is quantized on a PC (median-cut, a
deterministic per-image colour count capped at 48, ~175k tiles) into a small palette + a
base64 byte-per-tile grid. The game fetches these and paints them.

## Layout

- `index.json` — `{ version, memes: [{ id, name, width, height, aspect, colors, tiles }] }`.
  The game loads this once and the picker searches it.
- `designs/<id>.json` — one packed design, `{ width, height, palette, cells }`, fetched on
  demand and cached. `cells` is base64 of `width*height` bytes, each a 1-based palette index.

## Regenerating

Produced by `generator/src/memes-bake.js` in the main game repo from images in
`generator/memes/`. Drop an image in, re-bake, commit here, tag a release; the game picks it
up. IDs come from the filename, so they stay stable across re-numbering.

Served at `https://cdn.jsdelivr.net/gh/batteryday/paint-game-memes@<tag>/index.json`.
