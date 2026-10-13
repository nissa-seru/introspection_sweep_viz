# Introspection Manifold

A 3D WebGL visualization of a logit lens sweep across layer injection ranges ("Logit Lens × Layer Injection Sweep"). Each point is one injection range, given by its start and end layers, at one observation layer. Its color shows the logit lens value at that layer, with steering or without it, or the difference between the two.

The page credits the research and data to [Theia Vogel](https://vgel.me/) and the visualization to [Antra Tessera](https://animalabs.ai).

## What's here

- `index.html` is the whole viewer, with its WebGL 2 rendering, controls and styles in one page.
- `logit_lens_grid_sweep.json` is the sweep's data.
- `server.js` is a small Express server that serves the repository's directory as static files, with `index.html` at `/`.
- `package.json` declares `npm start` and the server's one dependency, Express, which `package-lock.json` pins with its own dependencies. `.gitignore` keeps `node_modules/` out of Git.

## The data

`logit_lens_grid_sweep.json` has two fields:

- `base` holds 65 values, one for each observation layer from 0 to 64, with no steering.
- `steered` holds 65 values in the same order for each injection range, keyed by its start layer and then its end layer, so `steered["5"]["9"]` is the range from layer 5 to layer 9. Ranges start at layers 1 to 63 and end at their start layer or any later layer up to 63, which makes 2,016 ranges.

## Using the viewer

The X axis is the injection start, Y the injection end, and Z the observation layer. A faint teal plane marks where the start equals the end.

- The color mode is Base, Steered, Δ Abs (steered minus base) or Δ Rel% (that difference as a percentage of the base value, or 0 where the base value is 0.001 or less). Δ Abs is the default.
- Sliders slice each axis, set the range of the color scale and set the point size. Two toggles turn on Auto-Scale and Clip to Color Scale, which hides points outside the color scale. The opacity settings are under "Advanced Opacity Controls".
- Clicking a point with the mouse opens its logit lens profile, a chart of the steered and base values across all observation layers for that injection range. A click picks only among points whose steered value is at least the Value Threshold, which is under "Advanced Opacity Controls".

With a mouse, drag to rotate, scroll to zoom and shift-drag to pan. On a touch screen, drag one finger to rotate, pinch to zoom and drag two fingers to pan.

The page needs a browser with WebGL 2.

## Running it

The server needs Node 18 or later.

```sh
npm install
npm start
```

Then open http://localhost:3000. Set `PORT` to use another port, as in `PORT=8080 npm start` for http://localhost:8080. The server listens on `0.0.0.0`.

The page fetches `logit_lens_grid_sweep.json` by a relative path, so a static file server run from the repository's root also works, such as `python3 -m http.server` (then open http://localhost:8000).
