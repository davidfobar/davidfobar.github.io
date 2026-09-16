---
layout: post
title: "The Foucault Test, With Actual Numbers"
date: 2026-09-16 12:00:00
description: Everyone explains the Foucault test with a hand-wave and a photo of a donut. Nobody tells you how far the knife edge actually moves. Here's the real geometry, the real distances, and an interactive ray-traced simulation you can drag.
tags: optics telescopes atm
categories: automated-foucault
toc:
  sidebar: left
---

The entire job of parabolizing a 6-inch f/5 mirror — the difference between a sphere that shows you a fuzzy blob and a paraboloid that splits a double star — lives inside **1.9 millimetres of knife-edge travel**.

That number is the reason this post exists. Every explanation of the Foucault test I could find falls into one of two camps: a qualitative hand-wave with a photo of a donut-shaped shadow, or a dense zonal-measurement table that assumes you already know what you're doing. The part in the middle — *how far does the thing actually move, and why* — is almost always left out, buried in a dial indicator reading that nobody bothers to explain.

This is my attempt at the missing middle. It's a companion to [Automated Foucault]({{ '/projects/3_automated_foucault/' | relative_url }}), where I'm replacing the eyeball and the dial indicator with a camera and a motorised stage. The inspiration is a battered copy of *Understanding Foucault*, which is the book that finally made this click for me — this post is the interactive version of what that book does with diagrams.

Everything below is traced with the exact law of reflection off a real paraboloid. No paraxial approximation, no fudge factors. The pictures are not to scale; **the numbers are.**

## The apparatus

A Foucault tester is almost insultingly simple. You need three things, and they all ride on one stage that slides along the optical axis:

- **A pinhole light source** — a bright LED behind a small hole or a narrow slit.
- **A knife edge** — literally a razor blade, or the edge of anything flat and opaque.
- **Something to look with** — your eye, historically. A camera sensor, in my case.

You put the mirror at one end of a long room, and the stage at the other end, near the mirror's **centre of curvature** — twice the focal length away. For a 6" f/5, focal length 762 mm, that's a radius of curvature of **R = 1524 mm**, so the stage sits about five feet from the mirror.

The pinhole sits a couple of millimetres *below* the optical axis. Light from it travels down to the mirror, bounces, and comes back to a point a couple of millimetres *above* the axis. That flip is not a coincidence: at the centre of curvature, object and image are conjugate with magnification −1, so the returning image lands exactly as far on the other side of the axis as the source is on this side. The knife edge waits there, at the image, and slices into it.

That's the whole instrument. The subtlety is entirely in what happens when you slide the stage a fraction of a millimetre.

## Start with a sphere, because a sphere is easy

Grind two pieces of glass against each other with abrasive between them, and the surface you naturally get is a **sphere**. That's not a coincidence either — a sphere is the only shape where every part of one surface fits every part of the other in every orientation, so random strokes converge on it.

A sphere has a magic property for our purposes. Put a point source at its centre of curvature, and *every single ray* hits the glass dead perpendicular to the surface, reflects straight back along the path it came, and returns to exactly the point it started from. Not approximately. Exactly. The edge of the mirror and the centre of the mirror send their light back to the same place.

So when you slide the knife edge into that returning cone, it cuts **every zone of the mirror at the same instant**. The whole disk goes from lit to dark together, evenly, like a dimmer switch.

Set the simulation below to *Sphere* and it drops you straight onto that position: a flat, featureless grey disk. Now drag the stage off it in either direction. You'll see a shadow sweep across — that's just defocus, the blade cutting a cone that isn't converged yet — but notice what you *never* see: a ring, a bump, a dark centre, any hint that one part of the mirror behaves differently from another. The disk stays featureless right across. Every zone agrees.

That uniform grey-out is the most sensitive null test in optics. It's how you know you have a sphere.

## A paraboloid cannot do that

A telescope doesn't want a sphere. It wants a **paraboloid**, because a paraboloid is the shape that brings light from infinitely far away — a star — to a single point.

Here's the problem: those two requirements fight each other. The shape that focuses parallel light perfectly is not the shape that focuses light from its own centre of curvature perfectly. So the moment you start parabolizing, you break the clean null, and the mirror starts showing you structure.

How much do you actually have to change the glass? Less than you'd ever guess. The depth difference between the paraboloid you want and the sphere you started with, measured at the edge of the mirror, is

$$
\Delta z = \frac{r^4}{8R^3}
$$

For the 6" f/5 — edge radius $$r = 76.2$$ mm, $$R = 1524$$ mm — that comes out to **1.19 micrometres**. About two wavelengths of green light. A fifth the thickness of a red blood cell. That, and nothing more, is the total amount of glass standing between a sphere and a finished telescope mirror.

You obviously cannot measure 1.2 µm of glass with a ruler. What makes the Foucault test extraordinary is that it doesn't ask you to.

## The lever

Because the paraboloid is shallower at the edge than the matching sphere, the edge zone of the mirror is very slightly *less* curved than the middle. Less curvature means a longer radius of curvature, which means the light from the edge crosses the axis **farther away from the mirror** than the light from the centre does.

The mirror no longer has *a* centre of curvature. It has a smeared-out range of them — a different one for every zone — spread along the axis. And the spread is enormous compared to the surface error that caused it.

For a tester where the source and knife edge move together on one stage, the zone at radius $$r$$ nulls when the stage sits at

$$
x_r = R + \frac{r^2}{2R}
$$

Run the numbers for the edge of the 6" f/5: $$76.2^2 / (2 \times 1524) = 1.905$$ mm.

| | value |
|---|---|
| Glass you have to remove at the edge | 1.19 µm |
| Knife-edge travel it produces | 1.905 mm |
| **Amplification** | **≈ 1600×** |

That is the Foucault test. It's a lever that converts a micron of glass you cannot see into two millimetres of stage travel you can measure with a dial indicator. Every bit of the rest is bookkeeping.

## Try it

Drag the stage and watch three things at once: where the rays actually cross, where the blade is sitting, and what your eye would see looking into the mirror.

The middle panel is drawn to real millimetres along the axis — those tick marks are honest. The mirror face on the right is computed per-pixel from the same ray trace, not drawn by hand, which is why the shadows flip sides on their own when you pass through a crossing point.

**Things worth doing:** flip to *Sphere* and sweep the stage — nothing but flat grey, everywhere. Flip back to *Parabola* and sweep again. Then use the zone buttons to null the centre, the 70.7% zone, and the edge in turn, and watch the dark ring march outward while the readout counts off the millimetres.

<div class="fc-sim" id="fc-sim">
  <div class="fc-controls">
    <div class="fc-row">
      <label class="fc-inline">Mirror
        <select id="fc-mirror">
          <option value="6f5" selected>6&quot; f/5 &mdash; R = 1524 mm</option>
          <option value="6f8">6&quot; f/8 &mdash; R = 2438 mm</option>
          <option value="8f6">8&quot; f/6 &mdash; R = 2438 mm</option>
        </select>
      </label>
      <span class="fc-seg" id="fc-figure">
        <button type="button" data-fig="sphere">Sphere</button>
        <button type="button" data-fig="para" class="fc-on">Parabola</button>
      </span>
      <label class="fc-inline fc-check"><input type="checkbox" id="fc-zones"> Zone rings</label>
    </div>

    <div class="fc-slider-row">
      <label for="fc-stage">Stage position</label>
      <input type="range" id="fc-stage" min="0" max="1000" step="1" value="171">
      <span class="fc-val" id="fc-stage-val">+0.000 mm</span>
    </div>

    <div class="fc-slider-row">
      <label for="fc-knife">Knife insertion</label>
      <input type="range" id="fc-knife" min="0" max="1000" step="1" value="500">
      <span class="fc-val" id="fc-knife-val">0.000 mm</span>
    </div>

    <div class="fc-presets">
      <span>Null the zone at:</span>
      <button type="button" data-zone="0">centre</button>
      <button type="button" data-zone="0.5">50%</button>
      <button type="button" data-zone="0.707">70.7%</button>
      <button type="button" data-zone="0.85">85%</button>
      <button type="button" data-zone="1">edge</button>
    </div>
  </div>

  <div class="fc-panel fc-layout-panel">
    <h4>1 &mdash; The layout <em>(schematic: curvature and stage travel hugely exaggerated)</em></h4>
    <svg viewBox="0 0 720 180" class="fc-svg" id="fc-layout">
      <line x1="60" y1="90" x2="700" y2="90" class="fc-axis"></line>
      <path d="M 662 18 Q 700 90 662 162" class="fc-mirror"></path>
      <text x="676" y="176" class="fc-lbl fc-lbl-mid">mirror</text>
      <g id="fc-rays-layout"></g>
      <g id="fc-stage-g">
        <rect x="86" y="46" width="7" height="88" class="fc-sensor"></rect>
        <text x="89" y="40" class="fc-lbl fc-lbl-mid">sensor</text>
        <rect x="104" y="22" width="5" height="66" class="fc-knife"></rect>
        <text x="122" y="30" class="fc-lbl">knife edge</text>
        <circle cx="106" cy="98" r="4" class="fc-pinhole"></circle>
        <text x="122" y="112" class="fc-lbl">pinhole source</text>
        <rect x="80" y="140" width="38" height="6" rx="2" class="fc-carriage"></rect>
      </g>
      <line x1="112" y1="166" x2="660" y2="166" class="fc-dim"></line>
      <text x="386" y="160" class="fc-lbl fc-lbl-mid" id="fc-R-label">R = 1524 mm</text>
    </svg>
  </div>

  <div class="fc-panels">
    <div class="fc-panel">
      <h4>2 &mdash; Where the rays actually cross</h4>
      <svg viewBox="0 0 520 210" class="fc-svg" id="fc-zoom"></svg>
      <p class="fc-cap" id="fc-zoom-cap"></p>
    </div>

    <div class="fc-panel">
      <h4>3 &mdash; What you see in the mirror</h4>
      <canvas id="fc-face" width="320" height="320" class="fc-face"></canvas>
      <p class="fc-cap">Computed per-pixel from the ray trace &mdash; the shadow flips sides on its own.</p>
    </div>
  </div>

  <div class="fc-readout">
    <div><span>Stage offset from paraxial centre of curvature</span><b id="fc-ro-stage">+0.000 mm</b></div>
    <div><span>Same, in dial-indicator units</span><b id="fc-ro-thou">0.0 thou</b></div>
    <div><span>Zone currently nulled</span><b id="fc-ro-zone">&mdash;</b></div>
    <div><span>Total centre-to-edge travel, this mirror</span><b id="fc-ro-total">1.905 mm</b></div>
  </div>
</div>

<style>
  #fc-sim {
    border: 1px solid var(--global-divider-color);
    border-radius: 12px;
    padding: 1.25rem 1.4rem 1.4rem;
    margin: 2rem 0;
    background: var(--global-card-bg-color);
  }
  #fc-sim .fc-controls { display: flex; flex-direction: column; gap: 0.65rem; margin-bottom: 1.1rem; }
  #fc-sim .fc-row { display: flex; flex-wrap: wrap; align-items: center; gap: 1rem; }
  #fc-sim .fc-inline { font-size: 0.88rem; color: var(--global-text-color); }
  #fc-sim select { margin-left: 0.4rem; padding: 0.2rem 0.35rem; font-size: 0.85rem;
    background: var(--global-bg-color); color: var(--global-text-color);
    border: 1px solid var(--global-divider-color); border-radius: 6px; }
  #fc-sim .fc-check input { margin-right: 0.3rem; vertical-align: middle; }
  #fc-sim .fc-seg { display: inline-flex; border: 1px solid var(--global-divider-color); border-radius: 8px; overflow: hidden; }
  #fc-sim .fc-seg button { border: 0; background: transparent; color: var(--global-text-color);
    padding: 0.28rem 0.85rem; font-size: 0.85rem; cursor: pointer; }
  #fc-sim .fc-seg button.fc-on { background: var(--global-theme-color); color: #fff; }
  #fc-sim .fc-slider-row { display: grid; grid-template-columns: 8.5rem 1fr 6rem; align-items: center; gap: 0.75rem; }
  #fc-sim .fc-slider-row label { font-size: 0.88rem; color: var(--global-text-color); }
  #fc-sim input[type="range"] { width: 100%; }
  #fc-sim .fc-val { font-variant-numeric: tabular-nums; text-align: right; font-size: 0.85rem; color: var(--global-text-color); }
  #fc-sim .fc-presets { display: flex; flex-wrap: wrap; align-items: center; gap: 0.4rem; font-size: 0.85rem; color: var(--global-text-color); }
  #fc-sim .fc-presets span { opacity: 0.75; margin-right: 0.15rem; }
  #fc-sim .fc-presets button, #fc-sim .fc-seg button { font-family: inherit; }
  #fc-sim .fc-presets button { padding: 0.22rem 0.6rem; font-size: 0.8rem; border-radius: 7px;
    border: 1px solid var(--global-theme-color); background: transparent; color: var(--global-theme-color); cursor: pointer; }
  #fc-sim .fc-presets button:hover { background: var(--global-theme-color); color: #fff; }
  #fc-sim .fc-panel { margin-top: 0.9rem; }
  #fc-sim .fc-panel h4 { margin: 0 0 0.4rem; font-size: 0.86rem; font-weight: 600; color: var(--global-text-color); }
  #fc-sim .fc-panel h4 em { font-weight: 400; opacity: 0.6; font-size: 0.8rem; }
  #fc-sim .fc-panels { display: flex; gap: 1.2rem; flex-wrap: wrap; align-items: flex-start; }
  #fc-sim .fc-panels > .fc-panel:first-child { flex: 2 1 330px; min-width: 300px; }
  #fc-sim .fc-panels > .fc-panel:last-child { flex: 1 1 210px; min-width: 200px; max-width: 300px; }
  #fc-sim .fc-svg { width: 100%; height: auto; display: block; }
  #fc-sim .fc-face { width: 100%; height: auto; display: block; border-radius: 50%; background: #0b0b0b; }
  #fc-sim .fc-cap { font-size: 0.75rem; opacity: 0.7; margin: 0.4rem 0 0; line-height: 1.45; }
  #fc-sim .fc-axis { stroke: var(--global-divider-color); stroke-width: 1; stroke-dasharray: 5 4; }
  #fc-sim .fc-mirror { fill: none; stroke: var(--global-theme-color); stroke-width: 4; stroke-linecap: round; }
  #fc-sim .fc-ray { fill: none; stroke: #e0a92b; stroke-width: 1; opacity: 0.75; }
  #fc-sim .fc-ray-zoom { stroke: #e0a92b; stroke-width: 1.2; opacity: 0.85; }
  #fc-sim .fc-knife, #fc-sim .fc-knife-zoom { fill: var(--global-text-color); opacity: 0.85; }
  #fc-sim .fc-sensor { fill: var(--global-divider-color); }
  #fc-sim .fc-carriage { fill: var(--global-theme-color); opacity: 0.55; }
  #fc-sim .fc-pinhole { fill: #e0a92b; stroke: #e0a92b; stroke-width: 5; stroke-opacity: 0.25; }
  #fc-sim .fc-lbl { font-size: 9px; fill: var(--global-text-color); opacity: 0.7; }
  #fc-sim .fc-lbl-mid { text-anchor: middle; }
  #fc-sim .fc-dim { stroke: var(--global-text-color); stroke-width: 0.75; opacity: 0.35; }
  #fc-sim .fc-tick { stroke: var(--global-text-color); stroke-width: 0.75; opacity: 0.45; }
  #fc-sim .fc-cross { fill: var(--global-theme-color); }
  #fc-sim .fc-readout { display: flex; flex-wrap: wrap; gap: 1.4rem; margin-top: 1.1rem;
    padding-top: 0.9rem; border-top: 1px solid var(--global-divider-color); }
  #fc-sim .fc-readout div { display: flex; flex-direction: column; gap: 0.15rem; }
  #fc-sim .fc-readout span { font-size: 0.72rem; opacity: 0.65; color: var(--global-text-color); }
  #fc-sim .fc-readout b { font-variant-numeric: tabular-nums; font-size: 1rem; color: var(--global-text-color); }
  @media (max-width: 600px) {
    #fc-sim { padding: 1rem; }
    #fc-sim .fc-slider-row { grid-template-columns: 5.5rem 1fr 5rem; gap: 0.5rem; }
    #fc-sim .fc-panels > .fc-panel:last-child { max-width: none; }
  }
</style>

<script>
(function () {
  var root = document.getElementById('fc-sim');
  if (!root) return;

  /* ---------- mirrors ---------- */
  var MIRRORS = {
    '6f5': { D: 152.4, f: 762.0 },
    '6f8': { D: 152.4, f: 1219.2 },
    '8f6': { D: 203.2, f: 1219.2 }
  };

  var state = { key: '6f5', figure: 'para', stage: 0, knife: 0, zones: false };
  var geom = null;

  /* ---------- exact ray trace ---------- */
  // Surface sag and slope at height rr, for a paraboloid or a sphere of
  // vertex radius of curvature R. Mirror vertex at z = 0, opening toward +z.
  function surface(rr, R, figure) {
    if (figure === 'sphere') {
      var s = Math.sqrt(R * R - rr * rr);
      return [R - s, rr / s];
    }
    return [rr * rr / (2 * R), rr / R];
  }

  // On-axis point source at z = xStage. Reflect off the surface at height rr
  // using the exact law of reflection, then report where the returning ray
  // crosses the axis and how high it is back at the knife-edge plane.
  function trace(rr, R, figure, xStage) {
    var sp = surface(rr, R, figure);
    var Pz = sp[0];
    var nx = -sp[1], nz = 1.0;
    var nn = Math.sqrt(nx * nx + nz * nz); nx /= nn; nz /= nn;
    var dx = rr, dz = Pz - xStage;
    var dd = Math.sqrt(dx * dx + dz * dz); dx /= dd; dz /= dd;
    var dot = dx * nx + dz * nz;
    var ax = dx - 2 * dot * nx, az = dz - 2 * dot * nz;
    return {
      zCross: Pz - rr * az / ax,            // axial crossing of the return ray
      y: rr + ((xStage - Pz) / az) * ax,    // height at the knife-edge plane
      slope: ax / az
    };
  }

  /* ---------- per-frame model ---------- */
  var NZ = 240;
  function build() {
    var m = MIRRORS[state.key];
    var R = 2 * m.f, rEdge = m.D / 2;
    var xStage = R + state.stage;
    var ys = new Float64Array(NZ + 1), zc = new Float64Array(NZ + 1), sl = new Float64Array(NZ + 1);
    for (var i = 1; i <= NZ; i++) {
      var t = trace(rEdge * i / NZ, R, state.figure, xStage);
      ys[i] = t.y; zc[i] = t.zCross; sl[i] = t.slope;
    }
    // r = 0 is a limiting case the trace can't take directly: the paraxial ray
    // has no height to reflect at. Use the mirror equation for its crossing.
    ys[0] = 0; sl[0] = 0;
    zc[0] = 1 / (2 / R - 1 / xStage);
    return { R: R, rEdge: rEdge, xStage: xStage, ys: ys, zc: zc, sl: sl };
  }

  function setMirror(key) {
    state.key = key;
    var m = MIRRORS[key];
    var R = 2 * m.f, rEdge = m.D / 2;
    var edgeNull = rEdge * rEdge / (2 * R);
    geom = {
      R: R, rEdge: rEdge, edgeNull: edgeNull,
      stageMin: -0.30 * edgeNull,
      stageMax: 1.45 * edgeNull,
      yScale: (rEdge / R) * edgeNull * 2,          // typical |y| at the knife plane
      soft: (rEdge / R) * edgeNull * 2 * 0.06      // finite source size, in mm
    };
    // With a moving source the crossing point retreats at twice the stage rate,
    // so the window has to reach out to 2*edgeNull beyond the furthest blade.
    geom.zmin = R + geom.stageMin;
    geom.zmax = R + 2 * edgeNull - geom.stageMin;
    geom.zHalf = geom.yScale * 1.4;
    if (state.stage < geom.stageMin) state.stage = geom.stageMin;
    if (state.stage > geom.stageMax) state.stage = geom.stageMax;
    if (Math.abs(state.knife) > geom.yScale) state.knife = 0;
  }

  /* ---------- sliders map 0..1000 onto real millimetres ---------- */
  var stageEl = root.querySelector('#fc-stage');
  var knifeEl = root.querySelector('#fc-knife');

  function stageToSlider() {
    return Math.round(1000 * (state.stage - geom.stageMin) / (geom.stageMax - geom.stageMin));
  }
  function sliderToStage(v) {
    return geom.stageMin + (v / 1000) * (geom.stageMax - geom.stageMin);
  }
  function knifeToSlider() { return Math.round(500 + 500 * state.knife / geom.yScale); }
  function sliderToKnife(v) { return ((v - 500) / 500) * geom.yScale; }

  /* ---------- panel 1: layout ---------- */
  var layoutRays = root.querySelector('#fc-rays-layout');
  var stageG = root.querySelector('#fc-stage-g');
  var rLabel = root.querySelector('#fc-R-label');
  var LAYOUT_PX_PER_MM = 18;   // stage travel, hugely exaggerated so it is visible

  for (var k = 0; k < 5; k++) {
    var p = document.createElementNS('http://www.w3.org/2000/svg', 'polyline');
    p.setAttribute('class', 'fc-ray');
    layoutRays.appendChild(p);
  }
  var layoutHeights = [25, 57, 90, 123, 155];

  function drawLayout() {
    var dx = (state.stage - geom.stageMin) * LAYOUT_PX_PER_MM;
    stageG.setAttribute('transform', 'translate(' + (-dx).toFixed(2) + ' 0)');
    var srcX = 106 - dx, srcY = 98, imgY = 82;
    var nodes = layoutRays.childNodes;
    for (var i = 0; i < 5; i++) {
      var mx = 660, my = layoutHeights[i];
      nodes[i].setAttribute('points',
        srcX.toFixed(1) + ',' + srcY + ' ' + mx + ',' + my + ' ' + srcX.toFixed(1) + ',' + imgY);
    }
    rLabel.textContent = 'R = ' + geom.R.toFixed(0) + ' mm';
  }

  /* ---------- panel 2: the crossing region ---------- */
  var zoomSvg = root.querySelector('#fc-zoom');
  var zoomCap = root.querySelector('#fc-zoom-cap');
  var ZX0 = 58, ZX1 = 500, ZY = 100, ZYSPAN = 74;
  var ZONES = [1.0, 0.85, 0.707, 0.5, 0.3];
  var zoomParts = null;

  function svgEl(tag, attrs) {
    var e = document.createElementNS('http://www.w3.org/2000/svg', tag);
    for (var a in attrs) e.setAttribute(a, attrs[a]);
    return e;
  }

  function initZoom() {
    zoomSvg.innerHTML = '';
    var ticks = svgEl('g', {});
    var rays = svgEl('g', {});
    var dots = svgEl('g', {});
    zoomSvg.appendChild(svgEl('line', { x1: ZX0, y1: ZY, x2: ZX1, y2: ZY, 'class': 'fc-axis' }));
    zoomSvg.appendChild(ticks);
    zoomSvg.appendChild(rays);
    zoomSvg.appendChild(dots);
    var blade = svgEl('rect', { 'class': 'fc-knife-zoom', width: 4, x: 0, y: 0, height: 0 });
    zoomSvg.appendChild(blade);
    var rayEls = [], dotEls = [];
    for (var i = 0; i < ZONES.length; i++) {
      for (var s = 0; s < 2; s++) {
        var l = svgEl('line', { 'class': 'fc-ray-zoom' });
        rays.appendChild(l); rayEls.push(l);
      }
      var d = svgEl('circle', { r: 2.6, 'class': 'fc-cross' });
      dots.appendChild(d); dotEls.push(d);
    }
    zoomParts = { ticks: ticks, rays: rayEls, dots: dotEls, blade: blade };
  }

  function drawZoom(tab) {
    var zmin = geom.zmin, zmax = geom.zmax;
    var pxPerMm = (ZX1 - ZX0) / (zmax - zmin);
    var pxPerMmY = ZYSPAN / geom.zHalf;
    var Z = function (z) { return ZX0 + (z - zmin) * pxPerMm; };
    var Y = function (y) { return ZY - y * pxPerMmY; };

    // axis ticks every 0.5 mm from the paraxial centre of curvature
    zoomParts.ticks.innerHTML = '';
    for (var t = Math.ceil((zmin - geom.R) / 0.5) * 0.5; t <= zmax - geom.R + 1e-9; t += 0.5) {
      var px = Z(geom.R + t);
      zoomParts.ticks.appendChild(svgEl('line', { x1: px, y1: ZY - 4, x2: px, y2: ZY + 4, 'class': 'fc-tick' }));
      var lab = svgEl('text', { x: px, y: ZY + 17, 'class': 'fc-lbl fc-lbl-mid' });
      lab.textContent = (t >= 0 ? '+' : '') + t.toFixed(1);
      zoomParts.ticks.appendChild(lab);
    }
    var cap = svgEl('text', { x: (ZX0 + ZX1) / 2, y: ZY + 33, 'class': 'fc-lbl fc-lbl-mid' });
    cap.textContent = 'millimetres from the paraxial centre of curvature';
    zoomParts.ticks.appendChild(cap);

    // returning rays for a handful of zones, and their axial crossings
    var n = 0;
    for (var i = 0; i < ZONES.length; i++) {
      var idx = Math.round(ZONES[i] * NZ);
      var zc = tab.zc[idx], sl = tab.sl[idx];
      for (var s = 0; s < 2; s++) {
        var sgn = s ? -1 : 1;
        var y0 = sgn * sl * (zmin - zc), y1 = sgn * sl * (zmax - zc);
        var el = zoomParts.rays[n++];
        el.setAttribute('x1', Z(zmin)); el.setAttribute('y1', Y(y0));
        el.setAttribute('x2', Z(zmax)); el.setAttribute('y2', Y(y1));
      }
      var inWindow = zc >= zmin && zc <= zmax;
      zoomParts.dots[i].setAttribute('cx', Z(zc));
      zoomParts.dots[i].setAttribute('cy', ZY);
      zoomParts.dots[i].setAttribute('opacity', inWindow ? 1 : 0);
    }

    // the blade, occluding everything below the knife height
    var bx = Z(tab.xStage), by = Y(state.knife);
    zoomParts.blade.setAttribute('x', bx - 2);
    zoomParts.blade.setAttribute('y', by);
    zoomParts.blade.setAttribute('height', Math.max(0, ZY + ZYSPAN + 4 - by));

    var vExag = (1 / pxPerMm) / (1 / pxPerMmY);
    zoomCap.textContent = 'Horizontal scale is real millimetres. Vertical scale is exaggerated about '
      + vExag.toFixed(0) + '×. Dots mark where each zone’s light crosses the axis; '
      + 'the bar is the knife edge, riding at the stage position.';
  }

  /* ---------- panel 3: the mirror face ---------- */
  var faceCanvas = root.querySelector('#fc-face');
  var fctx = faceCanvas.getContext('2d');
  var SZ = faceCanvas.width;
  var faceImg = fctx.createImageData(SZ, SZ);

  function drawFace(tab) {
    var data = faceImg.data;
    var c = SZ / 2, pxPerMm = (SZ / 2 - 2) / geom.rEdge;
    var ys = tab.ys, rEdge = geom.rEdge;
    var lo = state.knife - geom.soft, hi = state.knife + geom.soft, span = hi - lo;
    for (var py = 0; py < SZ; py++) {
      var v = (py - c) / pxPerMm;
      for (var px = 0; px < SZ; px++) {
        var u = (px - c) / pxPerMm;
        var r = Math.sqrt(u * u + v * v);
        var o = (py * SZ + px) * 4;
        if (r > rEdge) { data[o + 3] = 0; continue; }
        var xk;
        if (r < 1e-5) {
          xk = 0;
        } else {
          var fi = r / rEdge * NZ, i0 = fi | 0;
          if (i0 >= NZ) i0 = NZ - 1;
          xk = (ys[i0] + (ys[i0 + 1] - ys[i0]) * (fi - i0)) * (u / r);
        }
        var b = (xk - lo) / span;
        if (b < 0) b = 0; else if (b > 1) b = 1;
        b = b * b * (3 - 2 * b);
        var g = (16 + b * 208) | 0;
        data[o] = g; data[o + 1] = g; data[o + 2] = g; data[o + 3] = 255;
      }
    }
    fctx.putImageData(faceImg, 0, 0);

    if (state.zones) {
      fctx.strokeStyle = 'rgba(224,169,43,0.55)';
      fctx.lineWidth = 1;
      for (var i = 0; i < ZONES.length; i++) {
        fctx.beginPath();
        fctx.arc(c, c, ZONES[i] * (SZ / 2 - 2), 0, Math.PI * 2);
        fctx.stroke();
      }
    }
  }

  /* ---------- readouts ---------- */
  var ro = {
    stage: root.querySelector('#fc-ro-stage'),
    thou: root.querySelector('#fc-ro-thou'),
    zone: root.querySelector('#fc-ro-zone'),
    total: root.querySelector('#fc-ro-total'),
    stageVal: root.querySelector('#fc-stage-val'),
    knifeVal: root.querySelector('#fc-knife-val')
  };

  function nulledZone(tab) {
    // A zone nulls exactly when its axial crossing lands on the knife plane.
    if (tab.zc[NZ] - tab.zc[0] < 0.004) {
      return Math.abs(tab.xStage - tab.zc[NZ]) < 0.004 ? 'every zone at once' : null;
    }
    for (var i = 1; i <= NZ; i++) {
      if ((tab.zc[i - 1] - tab.xStage) * (tab.zc[i] - tab.xStage) <= 0) {
        var f = (tab.xStage - tab.zc[i - 1]) / (tab.zc[i] - tab.zc[i - 1]);
        var frac = (i - 1 + f) / NZ;
        return (frac * 100).toFixed(1) + '% of radius (r = ' + (frac * geom.rEdge).toFixed(1) + ' mm)';
      }
    }
    return null;
  }

  function drawReadout(tab) {
    var s = state.stage;
    var txt = (s >= 0 ? '+' : '−') + Math.abs(s).toFixed(3) + ' mm';
    ro.stage.textContent = txt;
    ro.stageVal.textContent = txt;
    ro.thou.textContent = (s / 0.0254).toFixed(1) + ' thou';
    ro.knifeVal.textContent = state.knife.toFixed(3) + ' mm';
    var z = nulledZone(tab);
    ro.zone.textContent = z || 'none — stage is off the ends';
    ro.total.textContent = state.figure === 'sphere'
      ? '0 mm (a sphere has one)'
      : geom.edgeNull.toFixed(3) + ' mm (' + (geom.edgeNull / 0.0254).toFixed(1) + ' thou)';
  }

  /* ---------- render loop ---------- */
  var pending = false;
  function render() {
    pending = false;
    var tab = build();
    drawLayout();
    drawZoom(tab);
    drawFace(tab);
    drawReadout(tab);
  }
  function schedule() {
    if (pending) return;
    pending = true;
    window.requestAnimationFrame(render);
  }

  /* ---------- wiring ---------- */
  stageEl.addEventListener('input', function () { state.stage = sliderToStage(+stageEl.value); schedule(); });
  knifeEl.addEventListener('input', function () { state.knife = sliderToKnife(+knifeEl.value); schedule(); });

  root.querySelector('#fc-mirror').addEventListener('change', function (e) {
    setMirror(e.target.value);
    stageEl.value = stageToSlider();
    knifeEl.value = knifeToSlider();
    schedule();
  });

  root.querySelector('#fc-figure').addEventListener('click', function (e) {
    var b = e.target.closest('button');
    if (!b) return;
    state.figure = b.getAttribute('data-fig');
    // Park a sphere on its single null, so the point lands immediately.
    if (state.figure === 'sphere') { state.stage = 0; stageEl.value = stageToSlider(); }
    var btns = this.querySelectorAll('button');
    for (var i = 0; i < btns.length; i++) btns[i].classList.toggle('fc-on', btns[i] === b);
    schedule();
  });

  root.querySelector('#fc-zones').addEventListener('change', function (e) {
    state.zones = e.target.checked; schedule();
  });

  root.querySelector('.fc-presets').addEventListener('click', function (e) {
    var b = e.target.closest('button');
    if (!b) return;
    var f = parseFloat(b.getAttribute('data-zone'));
    var r = f * geom.rEdge;
    state.stage = state.figure === 'sphere' ? 0 : r * r / (2 * geom.R);
    stageEl.value = stageToSlider();
    schedule();
  });

  setMirror('6f5');
  state.stage = 0;
  stageEl.value = stageToSlider();
  knifeEl.value = knifeToSlider();
  initZoom();
  render();
})();
</script>

## The numbers I could never find

Here is the table I went looking for and never turned up. This is a 6" f/5, with the source and knife edge riding on the same stage — the ordinary way an amateur tester is built.

| Zone | 30% | 50% | 70.7% | 85% | edge |
|---|---|---|---|---|---|
| Radius $$r$$ (mm) | 22.9 | 38.1 | 53.9 | 64.8 | 76.2 |
| Stage offset (mm) | 0.172 | 0.476 | 0.952 | 1.376 | **1.905** |
| Stage offset (thou) | 6.8 | 18.8 | 37.5 | 54.2 | **75.0** |

Two things jump out. The first is how small the whole range is: the entire test happens inside seventy-five thousandths of an inch. A cheap dial indicator reads to one thou, which gets you about seventy-five discriminable steps across the mirror. That's *workable*, and it's exactly why dial indicators are the traditional answer — but it's not generous.

The second is that the spacing is not even. The offset goes as $$r^2$$, so the zones bunch up badly near the centre. The first 30% of the mirror's radius occupies less than 7 thou of travel. The last 15% occupies more than 20. This is why zonal masks put their openings at equal *area* intervals rather than equal radius intervals — the outer zones are both optically more important and easier to measure, so that's where you spend your resolution.

For other mirrors, the whole scale moves:

| Mirror | R (mm) | Glass to remove at edge | Centre-to-edge travel |
|---|---|---|---|
| 6" f/8 | 2438 | 0.29 µm | 1.191 mm (46.9 thou) |
| 6" f/5 | 1524 | 1.19 µm | 1.905 mm (75.0 thou) |
| 8" f/6 | 2438 | 0.92 µm | 2.117 mm (83.3 thou) |

Fast mirrors are harder to make but *easier to test* — a shorter radius of curvature spreads the zones further apart. The f/8 is a gentler figuring job, but you're reading it through a smaller window.

## The factor of two that wrecked my afternoon

Now the part that cost me the most time, and the reason I'm confident the tables above are worth publishing.

**Published Foucault tables disagree with each other by exactly 2×, and almost none of them say which convention they're using.**

There are two ways to build a tester:

- **The source moves with the knife edge.** Pinhole and blade on one carriage. This is how nearly every amateur tester is built, mine included.
- **The source stays put** at the centre of curvature, and only the knife edge slides.

These do not give the same readings, and the reason is a small piece of optics that's easy to miss. Near the centre of curvature a mirror images with magnification −1, and the longitudinal consequence of that is a mirror image too: **push the source one millimetre away from the mirror, and the returning image moves one millimetre *toward* it.**

So when the source rides along with the blade, every millimetre of stage travel closes the gap between blade and image by *two* millimetres. You reach the null in half the distance.

$$
\text{moving source: } \delta = \frac{r^2}{2R}
\qquad\qquad
\text{fixed source: } \delta = \frac{r^2}{R}
$$

The classic textbook figure — "a 6-inch f/8 needs 0.094 inch of knife-edge travel" — is the **fixed-source** number. My ray trace puts it at 0.0938 inch, so that's clearly where it comes from. But if you build the usual moving-source tester, bolt on a dial indicator, and go looking for 94 thou, you will be hunting for a null that is only ever going to be 47 thou away, and you will conclude that your mirror, your tester, or your arithmetic is broken.

It's none of the three. It's the convention.

*(If you want to check me: the simulation above is a moving-source tester, because the pinhole is drawn on the carriage. That's not decoration — the stage position it reports is the source position too, and the null it finds is the moving-source null.)*

## Why zones, instead of just looking

If you've made it this far you might reasonably ask: if the shadow pattern tells you the shape, why measure anything? Why not just look at the mirror until it looks right?

Because your eye is superb at detecting *whether* a zone is uniformly grey, and hopeless at judging *how grey* it is compared to the zone next to it. The Foucault test is a fantastic null detector and a poor photometer. So you don't ask it "how much error is here" — you ask it "is this zone nulled *yet*," slide the stage until the answer is yes, and write down the number.

That's what a **Couder mask** is for: a piece of cardboard over the mirror with pairs of windows cut at known radii, one pair exposed at a time. You null each pair in turn, record the stage reading, and you now have a handful of numbers that describe the surface. Compare them against the $$r^2/2R$$ column, and the differences are your figure errors — which you convert to wavefront error and, eventually, to a decision about where to push on the glass next.

One detail that looks arbitrary until it isn't: the mask windows always sit to the **left and right** of the vertical knife edge, never above and below. That's because a knife edge cutting vertically is only sensitive to ray displacement in the horizontal direction, and the windows have to be where the blade can actually read them.

## Where the automation comes in

Everything above is done, traditionally, by a human being in a dark room, squinting at a grey disk and turning a micrometer by hand. It works — it has produced most of the good amateur telescope mirrors in existence — but it has three problems: your eye adapts and drifts, one thou of dial indicator is a coarse ruler on a 75-thou range, and a full set of zonal readings takes long enough that the mirror's temperature changes while you're taking them.

All three are solvable, and none of the solutions are exotic:

- **Replace the eye with a camera.** A sensor doesn't adapt, and it reports actual numbers per pixel rather than an impression. You can difference two frames and see a null far more precisely than you can judge one by eye.
- **Replace the dial indicator with a motorised stage.** A stepper on a fine-pitch screw resolves well under a micron of travel, and it can sweep the entire 1.9 mm range in seconds.
- **Replace the null-hunting with a fit.** Once you have images across a full sweep, you don't need to find each zone's null by hand at all — you fit the whole measured set against a forward model of the surface and let the optimiser find the figure.

That last point is why I wrote the simulation the way I did. The code in this page *is* a forward model: give it a surface and a stage position, and it predicts the image. Automated testing is that same function, run backwards — take the images, and solve for the surface that produced them.

That's the project. Follow along at [Automated Foucault]({{ '/projects/3_automated_foucault/' | relative_url }}), where the mirror under test is the [6" f/5]({{ '/projects/2_atm_6in_f5/' | relative_url }}) I'm grinding.
