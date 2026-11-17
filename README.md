# WebGL 1000 Objects Demo

A tiny, dependency-free WebGL demo that animates a slider-controlled number of
colored particles (up to 20,000) bouncing around the screen, rendered with a
single `gl.POINTS` draw call per frame.

## Features

- Pure WebGL (no libraries/frameworks, no build step).
- Live slider to adjust the number of moving objects (1 – 20,000) without
  reloading the page.
- Per-object random color, size, and velocity; objects bounce off the edges
  of the viewport.
- On-screen FPS counter.

## Usage

Just open [webgl-1000-objects.html](./webgl-1000-objects.html) directly in a
browser (double-click it, or `open webgl-1000-objects.html`), or serve the
folder with any static file server, e.g.:

```bash
python3 -m http.server 8080
# then visit http://localhost:8080/webgl-1000-objects.html
```

## How it works

- All per-object simulation state (position, velocity, color, size) is
  pre-allocated for the maximum object count up front, so moving the slider
  only changes how many of those pre-allocated objects are simulated and
  drawn — no reallocation or GC pressure at runtime.
- Each frame, the CPU updates positions and bounces them off the `[-1, 1]`
  clip-space edges, then uploads just the active subset of positions to the
  GPU and issues one `gl.drawArrays(gl.POINTS, ...)` call.
- Points are rendered as soft, round sprites using `gl_PointCoord` in the
  fragment shader.
