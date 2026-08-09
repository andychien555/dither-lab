# Dither Lab

A calibration bench for ordered dithering, running entirely in the browser.

**Live: https://andychien555.github.io/dither-lab/**

Load an image, find the grain you want, then read off the settings. Useful when
you need a repeatable dither recipe — for a house style, a print run, or a
generation prompt you want to match.

## What it does

- **Palette derived from the artwork.** Colours come from a median-cut pass over
  the image itself, so the result keeps the original's colour identity instead of
  snapping to a generic RGB cube.
- **8×8 Bayer ordered dither**, with adjustable cell size and spread.
- **Optional vertical streaks** — pixel-sorting artifacts, off by default.
- **Three presets**: Light, House, Poster. House is the reference target.
- **Fits the window.** Tall portraits scale down to the visible stage instead of
  running below the fold, and the readout says when the view is no longer 1:1 —
  so a scaled preview never quietly misleads the density call.
- **Split compare** against the original, draggable.
- **Pin versions** to compare candidates side by side.
- **The export matches the preview cell for cell.** Same grid, same palette, same
  Bayer decisions — scaled up by a whole number of pixels per cell. What you
  approve on screen is what lands on disk.

## Parameters

| Control | Range | Effect |
| --- | --- | --- |
| Palette size | 2–32 | Fewer colours flatten the image. Below 8 it reads as game art. |
| Cell size | 1–8 px | Width of one dither square, measured against the preview. Higher is chunkier. |
| Dither spread | 0–2 | How far each pixel is pushed before snapping to the palette. |
| Vertical streaks | 0–8% | Pixel-sorting artifacts. Skip on subjects that already read vertically. |

## Export

The preview works at up to 1100 px on the long edge, and the dither grid is
defined against it — so the grain you judge is the grain you get. The export
takes that same grid and scales each cell up by a whole number of pixels, which
puts the output within a few percent of the source size:

| Source | Grid at cell 2 | Export |
| --- | --- | --- |
| 900 × 600 | 450 × 300 | 900 × 600, 2 px cells |
| 3000 × 2000 | 550 × 366 | 2750 × 1830, 5 px cells |
| 6000 × 4000 | 550 × 366 | 6050 × 4026, 11 px cells |

Deriving the grid from the source instead would make the grain finer in
proportion to how far the original overshoots 1100 px — the same settings would
come back looking almost undithered on a large image. A whole-number scale keeps
every cell the same width; a fractional one leaves them at mixed widths that beat
against the regular Bayer pattern.

The stage bar reports the source size, the grid, and the export size with its
cell width, so none of this is a surprise at download time.

Filenames carry the source name and every parameter that changes the image:

```
seaside-painting-p12-c2-s1.00-k4.0.png
```

`p` palette, `c` cell, `s` spread, `k` streaks — in the same order and to the
same precision as the recipe line in the panel, so a file can be read straight
back onto the sliders. Two versions differing only in streaks get different
names rather than colliding in the download folder.

## Privacy

Images never leave the page. Everything — decoding, quantisation, dithering,
and the export — runs on canvas in your own browser. There is no server and no
upload.

## Running locally

It is a single static file with no dependencies or build step.

```sh
python3 -m http.server 8000
# then open http://localhost:8000
```

## Licence

MIT
