# flibustier open pack

High-resolution, photographic replacement art for
[flibustier](https://github.com/ocochard/flibustier), an open source engine
re-implementation of MicroProse's *Pirates! Gold*.

This pack holds no original game data. Every picture here was generated
from text prompts, with no original picture used as an input image.

## Status

**Draft.** The pictures are draft renders (8 sampling steps), made to
settle the compositions, the characters and the colour grade before the
final full-quality renders. Some screens are still missing, and the
game's small original sprites still draw over a few of these pictures.

The licence is **not decided yet**. Until a licence file is added, no
rights are granted to reuse this art outside of playing flibustier.

## Using it

The pack sits on top of the original data, which you still need: the
engine shows these pictures where the pack has them and the original
ones everywhere else.

```sh
flibustier --data ~/games/pirates_gold --pack ~/flibustier-openpack
```

The pack is drawn at 1080 lines (`art_height` in `pack.ini`), in a 4:3
picture pillarboxed on a 16:9 screen.

## Contents

- `pack.ini`: the pack's name, licence and art height, and the asset ID
  each file replaces (for example `pic/tavern = art/tavern.png`).
- `art/`: the pictures (PNG, 1440x1080 for full screens), and sprite
  sheets: a PNG with a `.sheet` file listing the rectangle of each frame
  (`art/tavern-sprites.sheet`, the tavern's visitors).
- `provenance/`: one JSON file per asset, recording how it was made, so
  any picture can be traced and remade: the prompts, seed, models,
  sketches and masks used (with their SHA-256), the colour grade, and
  the generator's git commit.

## How the art is made

The pictures are generated with [ComfyUI](https://www.comfy.org/) by
[`tools/artgen/artgen.py`](https://github.com/ocochard/flibustier/tree/main/tools/artgen)
of the flibustier repository, from the prompts and settings in
`tools/artgen/assets.toml`. The line sketches and masks it uses, drawn
from the game's click areas by `tools/artgen/layouts/sketch.py`, are in
the same place; provenance paths are relative to `tools/artgen/`.

The models, all under open licences:

| Use | Model | Licence |
|---|---|---|
| Pictures | Qwen-Image-2512 (fp8) with its Lightning 8-step LoRA | Apache-2.0 |
| Following the layout sketches | DiffSynth Qwen-Image canny model patch | Apache-2.0 |
| People from their portraits | Qwen-Image-Edit-2511 with its Lightning LoRA | Apache-2.0 |
| Cutting people out | BiRefNet | MIT |

- Prompts for the full-screen pictures describe only what each screen is
  for and where its parts are (for example where the clickable buildings
  of a town stand), written for this pack.
- Each person is described once and reused, so the bartender of the
  tavern is the same man as on his own portrait.
- One colour grade, matched to the governor's picture, keeps the series
  consistent.

To remake an asset (with ComfyUI and the models above on the server
named by `--server` or `FLIBUSTIER_COMFYUI`):

```sh
python3 tools/artgen/artgen.py --draft --pack ~/flibustier-openpack pic/tavern
```

Leave out `--draft` for the full-quality render.

## Files

Pictures, fonts and audio are stored with [Git LFS](https://git-lfs.com/):
run `git lfs install` once before cloning.
