---
layout: post
title: "Homing a Telescope Mount With Nothing But Gravity"
date: 2025-05-20 10:00:00
description: How two accelerometers and an Extended Kalman Filter let an equatorial mount find its way home without encoders or limit switches — walked through gradually, with an interactive simulation.
tags: kalman-filter robotics astronomy
categories: celestial-sync
toc:
  sidebar: left
---

This is a deep dive into one piece of [CelestialSync]({{ '/projects/1_celestial_sync/' | relative_url }}), the open-source telescope mount I'm building. It started as a class paper — [the full write-up is here]({{ '/assets/img/projects/celestial_sync/CS_KalmanHoming.pdf' | relative_url }}) if you want the dense version with derivations and MATLAB code. This post is the version I wish I'd had going in: build the intuition first, then the math, with a couple of sliders to play with along the way.

**The problem, in one sentence:** the mount doesn't know which way it's pointing when it powers on, it has no encoders or limit switches to tell it, and yet it needs to figure that out to within a fraction of a degree using nothing but two accelerometers — sensors that only ever tell you which way is "down."

## Why the mount needs to know where it is

Equatorial mounts track the sky by slowly rotating one axis (Right Ascension, or RA) at exactly the sidereal rate, once every 23 hours 56 minutes. The gears that do that rotation are never perfect. CelestialSync uses a 5000:1 reduction (a planetary gearbox feeding a harmonic drive) to turn a stepper motor's steps into smooth, slow rotation — but the harmonic drive itself has a repeatable wobble, a few arcseconds to a few tens of arcseconds, that repeats once per revolution of the drive. Left alone, that wobble is enough to smear stars into short streaks in any exposure longer than a second or two.

The fix — Periodic Error Correction (PEC) — is simple in principle: measure the wobble once, then play back a compensating correction every time you track. The catch is that the correction table is indexed by *where the gear train currently is in its cycle*. If the mount doesn't know its gear phase at startup, it applies the correction at the wrong point in the cycle and makes tracking worse, not better.

So before anything else, the mount needs to find "home" — a precise, repeatable reference position — every time it's powered on. Off-the-shelf solutions use absolute encoders or homing switches for this. CelestialSync doesn't have either. Instead, it has two small accelerometers, one bolted to each axis, and a filter that turns their readings into a position estimate.

## What is a Kalman filter, really?

Set the telescope mount aside for a second. Imagine you're walking across a dark room, blindfolded, trying to find a specific spot. You have two sources of information, and neither one is good enough alone:

- **You can count your steps.** You know roughly how big your strides are, so you can keep a running estimate of where you are. But small errors creep in with every step — you drift a little left, a stride is a bit short — and after enough steps that estimate is worthless on its own.
- **You can occasionally reach out and touch something** — a wall, a piece of furniture you recognize. That gives you a fix on your true position, but it's noisy (your hand isn't perfectly precise) and it doesn't tell you anything about which *direction* you're facing between touches.

A Kalman filter is the formal version of doing both at once, and doing it well. At every step it:

1. **Predicts** where you should be now, based on where you were and what you just did (your stride count).
2. **Measures** something about the world (your hand touches a wall).
3. **Corrects** its prediction toward the measurement — but not all the way. It weighs the prediction and the measurement by how much it trusts each one, and blends them. If your steps have been very consistent, it leans on the prediction. If the wall-touch was clean, it leans on that instead.

The genuinely useful part is that the filter also tracks *how confident it is*, and that confidence shrinks over time as evidence accumulates — it's not just a single guess, it's a guess plus a shrinking error bar. The "**E**xtended" in Extended Kalman Filter just means the same predict/measure/correct loop, but adapted for cases where the relationship between your position and what you measure isn't a straight line — which, for a rotating accelerometer, it very much isn't. Instead of a simple weighted average, the EKF re-linearizes (takes a local straight-line approximation) around the current best guess at every step, using calculus to figure out the right blend.

For CelestialSync: the "stride count" is the known number of motor steps commanded during a slew (we know this exactly — we're the one telling the stepper motor what to do). The "wall touch" is the accelerometer reading. Neither one alone can find home. Together, they can.

## What the accelerometers actually measure

An accelerometer sitting still on a table isn't measuring "nothing" — it's measuring the force needed to hold it up against gravity, which is indistinguishable from measuring a constant ~9.81 m/s² pointing "up," in whatever direction "up" is *relative to the sensor's own body*.

That last part is the whole trick. Gravity itself never changes. But as you rotate the sensor, the same fixed gravity vector gets projected differently onto the sensor's own x, y, and z axes. Tip the sensor 90°, and a component that used to read zero now reads the full 9.81 m/s², and vice versa. The relationship between *orientation* and *what the sensor reports* is completely deterministic — pure trigonometry — provided you know how the sensor is mounted.

That means the reverse is also (in principle) true: read the three numbers off the accelerometer, and you can work backward to the sensor's orientation.

{% include figure.liquid path="assets/img/posts/sensorless-telescope-homing/CS_axes_annotated.jpg" class="img-fluid rounded z-depth-1" zoomable=true caption="The two accelerometers and their body-frame axes. The RA sensor rotates with the RA stage; the DEC sensor rides on top of it and rotates with the DEC stage." %}

CelestialSync has one accelerometer rigidly fixed to the RA stage, and a second fixed to the DEC stage — which itself sits on top of, and rotates with, the RA stage. That nesting matters:

- The **RA accelerometer**'s reading depends only on the RA angle, $$\theta_{RA}$$.
- The **DEC accelerometer**'s reading depends on *both* $$\theta_{RA}$$ and $$\theta_{DEC}$$, because whatever the RA stage is doing gets carried along before the DEC stage's own rotation is even applied.

Working out what a sensor *should* read, given a candidate orientation, means chaining together every rotation and fixed mounting offset between "gravity in the real world" and "gravity as this specific chip's x/y/z axes see it." Get one of those transforms wrong — or apply them in the wrong order — and the predicted reading is wrong. That chaining is the "coordinate frame transformation" problem, and it's worth building intuition for before touching the filter itself.

## Try it: watch gravity move through the coordinate chain

The two sliders below control the RA and DEC angles directly — exactly the two unknowns the real filter is trying to estimate. Everything else (the mount's latitude tilt, the sensors' fixed mounting offsets) is held constant, using the same 40°-altitude homing configuration the paper ultimately recommends. Each dial shows the accelerometer's own body-frame axes as a small "stage mark" (where the sensor is physically pointing) and the live gravity vector it measures, projected onto that sensor's x/y plane. The numbers underneath are the actual $$(g_x, g_y, g_z)$$ components that sensor would report right now, in m/s².

<div class="cs-sim" id="cs-homing-sim">
  <div class="cs-sim-controls">
    <div class="cs-slider-row">
      <label for="cs-ra-slider">RA angle <span class="cs-axis-tag cs-tag-ra">θ<sub>RA</sub></span></label>
      <input type="range" id="cs-ra-slider" min="0" max="360" step="1" value="30">
      <span class="cs-slider-value" id="cs-ra-value">30°</span>
    </div>
    <div class="cs-slider-row">
      <label for="cs-dec-slider">DEC angle <span class="cs-axis-tag cs-tag-dec">θ<sub>DEC</sub></span></label>
      <input type="range" id="cs-dec-slider" min="-90" max="90" step="1" value="20">
      <span class="cs-slider-value" id="cs-dec-value">20°</span>
    </div>
    <button type="button" id="cs-home-btn">Snap to home position (0°, 0°)</button>
  </div>

  <div class="cs-dials">
    <div class="cs-dial-panel">
      <h4>RA accelerometer</h4>
      <svg viewBox="0 0 200 200" class="cs-dial-svg">
        <defs>
          <marker id="cs-arrow-ra" viewBox="0 0 10 10" refX="8" refY="5" markerWidth="6" markerHeight="6" orient="auto-start-reverse">
            <path d="M0,0 L10,5 L0,10 z" class="cs-arrowhead"></path>
          </marker>
        </defs>
        <circle cx="100" cy="100" r="90" class="cs-dial-circle"></circle>
        <line x1="100" y1="10" x2="100" y2="190" class="cs-dial-gridline"></line>
        <line x1="10" y1="100" x2="190" y2="100" class="cs-dial-gridline"></line>
        <g id="cs-ra-stage-mark">
          <line x1="100" y1="100" x2="100" y2="25" class="cs-stage-mark"></line>
        </g>
        <line id="cs-ra-arrow" x1="100" y1="100" x2="100" y2="30" class="cs-gravity-arrow" marker-end="url(#cs-arrow-ra)"></line>
        <circle cx="100" cy="100" r="4" class="cs-dial-center"></circle>
      </svg>
      <div class="cs-readout">
        <div><span>g<sub>x</sub></span><b id="cs-ra-gx">0.00</b></div>
        <div><span>g<sub>y</sub></span><b id="cs-ra-gy">0.00</b></div>
        <div><span>g<sub>z</sub></span><b id="cs-ra-gz">0.00</b></div>
      </div>
    </div>

    <div class="cs-dial-panel">
      <h4>DEC accelerometer</h4>
      <svg viewBox="0 0 200 200" class="cs-dial-svg">
        <defs>
          <marker id="cs-arrow-dec" viewBox="0 0 10 10" refX="8" refY="5" markerWidth="6" markerHeight="6" orient="auto-start-reverse">
            <path d="M0,0 L10,5 L0,10 z" class="cs-arrowhead"></path>
          </marker>
        </defs>
        <circle cx="100" cy="100" r="90" class="cs-dial-circle"></circle>
        <line x1="100" y1="10" x2="100" y2="190" class="cs-dial-gridline"></line>
        <line x1="10" y1="100" x2="190" y2="100" class="cs-dial-gridline"></line>
        <g id="cs-dec-stage-mark">
          <line x1="100" y1="100" x2="100" y2="25" class="cs-stage-mark cs-stage-mark-dec"></line>
        </g>
        <line id="cs-dec-arrow" x1="100" y1="100" x2="100" y2="30" class="cs-gravity-arrow cs-gravity-arrow-dec" marker-end="url(#cs-arrow-dec)"></line>
        <circle cx="100" cy="100" r="4" class="cs-dial-center"></circle>
      </svg>
      <div class="cs-readout">
        <div><span>g<sub>x</sub></span><b id="cs-dec-gx">0.00</b></div>
        <div><span>g<sub>y</sub></span><b id="cs-dec-gy">0.00</b></div>
        <div><span>g<sub>z</sub></span><b id="cs-dec-gz">0.00</b></div>
      </div>
    </div>
  </div>

  <p class="cs-sim-note">The thin colored tick on each dial is the sensor's own physical orientation — where the stage has actually rotated to. The arrow is what gravity looks like <em>from that sensor's point of view</em> right now. Try moving only the RA slider: the DEC dial's arrow and numbers move too, even though the DEC slider never changed. That's the coupling the paper calls "the coupled rotation chain" — and it's why the filter has to estimate both angles jointly. Home is defined as the specific condition $$h_{RA,y}=0$$ and $$h_{DEC,z}=0$$ simultaneously — hit the button above to see exactly what that configuration looks like on both dials at once.</p>
</div>

<style>
  #cs-homing-sim.cs-sim {
    border: 1px solid var(--global-divider-color);
    border-radius: 12px;
    padding: 1.25rem 1.5rem 1.5rem;
    margin: 2rem 0;
    background: var(--global-card-bg-color);
  }
  #cs-homing-sim .cs-sim-controls {
    display: flex;
    flex-direction: column;
    gap: 0.6rem;
    margin-bottom: 1.25rem;
  }
  #cs-homing-sim .cs-slider-row {
    display: grid;
    grid-template-columns: 9rem 1fr 3.5rem;
    align-items: center;
    gap: 0.75rem;
  }
  #cs-homing-sim .cs-slider-row label {
    font-size: 0.9rem;
    color: var(--global-text-color);
  }
  #cs-homing-sim input[type="range"] {
    width: 100%;
  }
  #cs-homing-sim .cs-slider-value {
    font-variant-numeric: tabular-nums;
    text-align: right;
    font-size: 0.9rem;
    color: var(--global-text-color);
  }
  #cs-homing-sim .cs-axis-tag {
    font-size: 0.75rem;
    padding: 0.05rem 0.4rem;
    border-radius: 6px;
    margin-left: 0.25rem;
  }
  #cs-homing-sim .cs-tag-ra {
    background: rgba(124, 58, 237, 0.15);
    color: #7c3aed;
  }
  #cs-homing-sim .cs-tag-dec {
    background: rgba(234, 88, 12, 0.15);
    color: #ea580c;
  }
  #cs-homing-sim #cs-home-btn {
    align-self: flex-start;
    margin-top: 0.25rem;
    padding: 0.4rem 0.9rem;
    font-size: 0.85rem;
    border-radius: 8px;
    border: 1px solid var(--global-theme-color);
    background: transparent;
    color: var(--global-theme-color);
    cursor: pointer;
  }
  #cs-homing-sim #cs-home-btn:hover {
    background: var(--global-theme-color);
    color: var(--global-card-bg-color);
  }
  #cs-homing-sim .cs-dials {
    display: flex;
    gap: 2rem;
    flex-wrap: wrap;
    justify-content: center;
  }
  #cs-homing-sim .cs-dial-panel {
    flex: 1 1 220px;
    max-width: 260px;
    text-align: center;
  }
  #cs-homing-sim .cs-dial-panel h4 {
    margin: 0 0 0.5rem;
    font-size: 0.95rem;
    color: var(--global-text-color);
  }
  #cs-homing-sim .cs-dial-svg {
    width: 100%;
    height: auto;
  }
  #cs-homing-sim .cs-dial-circle {
    fill: none;
    stroke: var(--global-divider-color);
    stroke-width: 2;
  }
  #cs-homing-sim .cs-dial-gridline {
    stroke: var(--global-divider-color);
    stroke-width: 1;
    opacity: 0.5;
  }
  #cs-homing-sim .cs-dial-center {
    fill: var(--global-text-color);
  }
  #cs-homing-sim .cs-stage-mark {
    stroke: #7c3aed;
    stroke-width: 3;
    opacity: 0.55;
  }
  #cs-homing-sim .cs-stage-mark-dec {
    stroke: #ea580c;
  }
  #cs-homing-sim .cs-gravity-arrow {
    stroke: var(--global-text-color);
    stroke-width: 3;
  }
  #cs-homing-sim .cs-arrowhead {
    fill: var(--global-text-color);
  }
  #cs-homing-sim .cs-readout {
    display: flex;
    justify-content: center;
    gap: 0.9rem;
    margin-top: 0.6rem;
    font-size: 0.85rem;
  }
  #cs-homing-sim .cs-readout div {
    display: flex;
    flex-direction: column;
    align-items: center;
    color: var(--global-text-color);
  }
  #cs-homing-sim .cs-readout span {
    opacity: 0.6;
    font-size: 0.75rem;
  }
  #cs-homing-sim .cs-readout b {
    font-variant-numeric: tabular-nums;
  }
  #cs-homing-sim .cs-sim-note {
    font-size: 0.85rem;
    opacity: 0.85;
    margin: 1.1rem 0 0;
    line-height: 1.5;
  }
  @media (max-width: 600px) {
    #cs-homing-sim .cs-slider-row {
      grid-template-columns: 5.5rem 1fr 3.2rem;
    }
    #cs-homing-sim .cs-dials {
      flex-direction: column;
      align-items: center;
    }
  }
</style>

<script>
(function () {
  var G = 9.81;
  var LAT_DEG = 40; // the paper's recommended fixed-altitude homing configuration

  function deg2rad(d) { return (d * Math.PI) / 180; }

  function matVec(m, v) {
    return [
      m[0][0] * v[0] + m[0][1] * v[1] + m[0][2] * v[2],
      m[1][0] * v[0] + m[1][1] * v[1] + m[1][2] * v[2],
      m[2][0] * v[0] + m[2][1] * v[1] + m[2][2] * v[2],
    ];
  }

  function rRA(thetaDeg) {
    var t = deg2rad(thetaDeg), c = Math.cos(t), s = Math.sin(t);
    return [[c, -s, 0], [s, c, 0], [0, 0, 1]];
  }

  function rDEC(thetaDeg) {
    var t = deg2rad(thetaDeg), c = Math.cos(t), s = Math.sin(t);
    return [[1, 0, 0], [0, c, -s], [0, s, c]];
  }

  var T_DEC = [[-1, 0, 0], [0, 0, -1], [0, 1, 0]];

  function gPolar() {
    var lat = deg2rad(LAT_DEG);
    return [G * Math.cos(lat), 0, -G * Math.sin(lat)];
  }

  function computeReadings(thetaRA, thetaDEC) {
    var gp = gPolar();
    var gRA = matVec(rRA(thetaRA), gp);
    var gDEC = matVec(T_DEC, matVec(rDEC(thetaDEC), gRA));
    return { gRA: gRA, gDEC: gDEC };
  }

  var root = document.getElementById('cs-homing-sim');
  if (!root) return;

  var raSlider = root.querySelector('#cs-ra-slider');
  var decSlider = root.querySelector('#cs-dec-slider');
  var raValue = root.querySelector('#cs-ra-value');
  var decValue = root.querySelector('#cs-dec-value');
  var raStageMark = root.querySelector('#cs-ra-stage-mark');
  var decStageMark = root.querySelector('#cs-dec-stage-mark');
  var raArrow = root.querySelector('#cs-ra-arrow');
  var decArrow = root.querySelector('#cs-dec-arrow');
  var homeBtn = root.querySelector('#cs-home-btn');

  var readoutEls = {
    raGx: root.querySelector('#cs-ra-gx'),
    raGy: root.querySelector('#cs-ra-gy'),
    raGz: root.querySelector('#cs-ra-gz'),
    decGx: root.querySelector('#cs-dec-gx'),
    decGy: root.querySelector('#cs-dec-gy'),
    decGz: root.querySelector('#cs-dec-gz'),
  };

  var SCALE = 70 / G;

  function setArrow(el, gx, gy) {
    el.setAttribute('x2', (100 + gx * SCALE).toFixed(2));
    el.setAttribute('y2', (100 - gy * SCALE).toFixed(2));
  }

  function update() {
    var thetaRA = parseFloat(raSlider.value);
    var thetaDEC = parseFloat(decSlider.value);

    raValue.textContent = thetaRA.toFixed(0) + '°';
    decValue.textContent = thetaDEC.toFixed(0) + '°';

    raStageMark.setAttribute('transform', 'rotate(' + (-thetaRA) + ' 100 100)');
    decStageMark.setAttribute('transform', 'rotate(' + (-thetaDEC) + ' 100 100)');

    var r = computeReadings(thetaRA, thetaDEC);

    setArrow(raArrow, r.gRA[0], r.gRA[1]);
    setArrow(decArrow, r.gDEC[0], r.gDEC[1]);

    readoutEls.raGx.textContent = r.gRA[0].toFixed(2);
    readoutEls.raGy.textContent = r.gRA[1].toFixed(2);
    readoutEls.raGz.textContent = r.gRA[2].toFixed(2);
    readoutEls.decGx.textContent = r.gDEC[0].toFixed(2);
    readoutEls.decGy.textContent = r.gDEC[1].toFixed(2);
    readoutEls.decGz.textContent = r.gDEC[2].toFixed(2);
  }

  raSlider.addEventListener('input', update);
  decSlider.addEventListener('input', update);
  homeBtn.addEventListener('click', function () {
    raSlider.value = 0;
    decSlider.value = 0;
    update();
  });

  update();
})();
</script>

## From intuition to an estimator: building the EKF

The sliders above compute a reading from a *known* orientation — the easy direction. The filter has to run that process in reverse, recover an *unknown* orientation from noisy readings, and it can't just invert the trig equations sample-by-sample. Three things get in the way:

- **The sensors have their own constant bias** — a small, unknown offset baked into each axis of each chip, on the order of ±0.1 m/s². Ignore it and you get a confidently wrong answer.
- **Every reading is noisy.** A single sample is a bad basis for a decision that needs to land within a fraction of a degree.
- **The relationship between angle and reading isn't unique in every instant** — but it *becomes* unique once you fuse many samples gathered while the mount is actually turning through a known trajectory.

That last point is why the mount doesn't just take one reading at startup — it performs a slow, controlled slew (driven at a known, commanded rate) while continuously sampling both accelerometers, and lets the filter fuse the entire sequence.

### The state

The filter tracks eight numbers at once:

$$
\boldsymbol{x} = \begin{bmatrix} \theta_{RA} \\ \theta_{DEC} \\ \boldsymbol{b}_{RA} \\ \boldsymbol{b}_{DEC} \end{bmatrix} \in \mathbb{R}^8
$$

— the two angles we actually care about, plus the three-axis bias vector for each accelerometer (six more unknowns). All eight have to be estimated together, because a biased sensor and a mis-estimated angle can look similar in a single reading; only by watching the readings evolve correctly over a whole slew can the filter tell them apart.

### Predict, then correct

The loop is the same predict/measure/correct cycle from the walking analogy, applied at every accelerometer sample:

- **Predict:** advance each angle by the commanded step increment since the last sample (this part is linear and exact — we know precisely how many microsteps we told the motor to take).
- **Measure:** read the six accelerometer channels (three axes × two sensors).
- **Linearize:** compute the Jacobian — the local slope of "predicted reading" with respect to each of the eight states — at the current estimate. This is the "Extended" step: rather than solving the true nonlinear equations, the filter uses their best straight-line approximation right where it currently thinks it is.
- **Correct:** compare the actual reading to the predicted one, and nudge the state estimate toward the actual reading, weighted by the Kalman gain — how much the filter currently trusts the measurement versus its own prediction.

### Why two passes

Here's a wrinkle that only shows up once you look at how *fast* different parts of the state converge. The six bias terms and the RA angle become well-determined only after a good chunk of the slew has gone by, while the DEC angle sharpens quickly. Run the filter once, forward, and by the time it reaches the end of the data the bias estimates are solid — but the angle estimate at that final sample still carries leftover uncertainty from earlier in the run.

The fix is to run the filter twice:

{% include figure.liquid path="assets/img/posts/sensorless-telescope-homing/CS_fig_two_pass_ekf.png" class="img-fluid rounded z-depth-1" zoomable=true caption="The two-pass architecture: a forward pass nails down the accelerometer biases, then a backward pass — with biases frozen — sharpens the angle estimate right up to the home position." %}

**Pass 1** runs forward through the whole slew as an 8-state filter, estimating both angles and all six biases together. By the end, the bias estimates have converged.

**Pass 2** freezes those bias values and restarts as a leaner 2-state filter — just the two angles — sweeping *backward* through the same measurement sequence, from the end of the slew back to the start. Running backward means uncertainty keeps shrinking as the filter approaches sample zero, which is exactly where the home position needs to be extracted. By construction, that's the point in the entire dataset where the filter has the most accumulated evidence behind it.

## Does it actually work? Results from the paper

Before trusting any of this, the measurement model itself needs validating against real hardware — not just simulation.

{% include figure.liquid path="assets/img/posts/sensorless-telescope-homing/CS_fig_sensor_validation.png" class="img-fluid rounded z-depth-1" zoomable=true caption="Experimental accelerometer data (black) against the simulated model (blue) and noiseless true trajectory (red dashed), for a real homing sweep on the physical mount. The DEC channels visibly track both axes' motion — direct evidence of the coupling from the simulator above." %}

The real sensors track the model closely, including the DEC channel's dependence on both angles — the same coupling the sliders above make interactive. That agreement is what justifies trusting the simulation-based results that follow.

With the model validated, Pass 1 converges the six bias states within roughly 140° of simulated rotation:

{% include figure.liquid path="assets/img/posts/sensorless-telescope-homing/CS_fig_bias_convergence.png" class="img-fluid rounded z-depth-1" zoomable=true caption="Pass 1: all six accelerometer bias states converge to within their ±2σ bounds well before the end of the slew." %}

Pass 2 then sharpens both angle estimates as it sweeps backward toward the home position at sample zero:

{% include figure.liquid path="assets/img/posts/sensorless-telescope-homing/CS_fig_angle_convergence.png" class="img-fluid rounded z-depth-1" zoomable=true caption="Pass 2: angle estimates (blue) track the true trajectory (red) closely, with 1σ uncertainty (right axis, log scale) shrinking steadily as the backward pass approaches the home extraction point." %}

At the extraction point, 1σ uncertainty lands around 0.01°–0.03° — well inside the ±180° (±25,600 microstep) homing budget the low-cost rotary encoder needs to lock onto a precise gear phase.

The real test is whether this holds up over many independent trials, not just one representative run. Over 100 Monte Carlo trials at two site latitudes:

| | λ = 52° RA | λ = 52° DEC | λ = 40° RA | λ = 40° DEC |
|---|---|---|---|---|
| Mean error (microsteps) | −1,807 | 382 | −1,505 | 317 |
| Std deviation (microsteps) | 14,555 | 7,177 | 11,370 | 7,792 |
| RMSE (microsteps) | 14,594 | 7,151 | 11,413 | 7,759 |
| Max abs error (microsteps) | 38,948 | 17,392 | 28,115 | 19,867 |
| **Pass rate** | **92%** | **100%** | **99%** | **100%** |

{% include figure.liquid path="assets/img/posts/sensorless-telescope-homing/CS_fig_homing_error_histograms.png" class="img-fluid rounded z-depth-1" zoomable=true caption="Homing error distributions over 100 Monte Carlo trials. Dashed red lines mark the ±25,600-microstep pass/fail boundary. DEC comfortably clears it at both latitudes; RA is the tighter margin, and improves at the lower latitude." %}

DEC passes essentially every time, at both latitudes. RA is the limiting axis — its observability (how strongly its angle actually shows up in the accelerometer readings) depends on site latitude, and gets weaker the more polar-aligned the mount is. That's exactly why the interactive sliders above default to a 40° altitude rather than a polar-aligned angle: tilting the mount to a fixed 40° altitude for the homing routine — decoupled from wherever true polar alignment happens to be for that location — keeps the RA accelerometer's gravity projection strong and consistent, independent of where on Earth the mount is set up. That single change took the RA pass rate from 92% to 99%.

A 99% pass rate is good enough for how this gets used in practice: homing only has to happen once per power-on, to seed the PEC table, and the absolute position can then be retained across a session (with periodic EEPROM writes so an ungraceful shutdown doesn't lose it entirely).

## What's next

The filter above is validated in simulation and against real accelerometer data, but it hasn't run closed-loop on the physical mount yet. That's next: swap the simulated step counter for real stepper-pulse timestamps, and confirm the 40°-altitude homing configuration actually delivers on the pass-rate improvement outside of Monte Carlo.

Follow the build on the [CelestialSync project page]({{ '/projects/1_celestial_sync/' | relative_url }}), or check out the code on [GitHub](https://github.com/davidfobar/CelestialSync).
