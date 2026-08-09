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
- **Split compare** against the original, draggable.
- **Pin versions** to compare candidates side by side, then download at full
  source resolution.

## Parameters

| Control | Range | Effect |
| --- | --- | --- |
| Palette size | 2–32 | Fewer colours flatten the image. Below 8 it reads as game art. |
| Cell size | 1–8 px | Width of one dither square. Higher is chunkier. |
| Dither spread | 0–2 | How far each pixel is pushed before snapping to the palette. |
| Vertical streaks | 0–8% | Pixel-sorting artifacts. Skip on subjects that already read vertically. |

## Privacy

Images never leave the page. Everything — decoding, quantisation, dithering,
and the full-size export — runs on canvas in your own browser. There is no
server and no upload.

## Running locally

It is a single static file with no dependencies or build step.

```sh
python3 -m http.server 8000
# then open http://localhost:8000
```

## Licence

MIT
