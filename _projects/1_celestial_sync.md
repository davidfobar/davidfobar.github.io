---
layout: project
title: CelestialSync
description: An open-source equatorial telescope mount and control system, built from the ground up — mechanics, electronics, and firmware.
img: assets/img/projects/celestial_sync/CS_workbench.JPEG
importance: 1
category: astronomy
github: https://github.com/davidfobar/CelestialSync
---

CelestialSync is an open-source equatorial telescope mount and control system. Commercial GoTo mounts in this price class trade off tracking precision, repeatability, or hackability — I wanted a mount I could fully understand, tune, and extend, so I'm building one from scratch: mechanical design, motor control, and the algorithms that make an equatorial mount track the sky precisely.

<div class="row">
  <div class="col-sm mt-3 mt-md-0">
    {% include figure.liquid path="assets/img/projects/celestial_sync/CS_workbench.JPEG" class="img-fluid rounded z-depth-1" zoomable=true caption="The mount on the workbench, mid-build." %}
  </div>
</div>

<p class="mt-3">
  <a href="https://github.com/davidfobar/CelestialSync" target="_blank" rel="noopener" class="btn btn-outline-primary">
    <i class="fa-brands fa-github"></i>&nbsp; View the code on GitHub
  </a>
</p>

### Hardware

Each axis (Right Ascension and Declination) is driven by a NEMA 17 stepper motor through a 50:1 planetary gearbox feeding a 100:1 harmonic drive — a 5000:1 total reduction. At 200 full steps per motor revolution and 256x microstepping, that works out to roughly 5.06 milli-arcseconds of commanded resolution per microstep at the mount output.

- **Motors:** NEMA 17 stepper, one per axis
- **Reduction:** 50:1 planetary + 100:1 harmonic drive (5000:1 total)
- **Microstepping:** 256x per full step
- **Sensing:** body-fixed accelerometers on each axis (no absolute encoders or homing switches)

### Why no encoders?

Harmonic drives are precise, but not perfect — the compound gear train introduces *periodic error*, a repeatable positional wobble that completes one cycle per revolution of the drive stage. Left uncorrected, this shows up as 10–40 arcseconds of drift at the mount output, which is more than enough to trail stars in exposures longer than a second or two.

The standard fix is Periodic Error Correction (PEC): record the error once, then play a compensating correction back during every tracking session. PEC only works, though, if the mount can find the *same* starting phase in that error cycle every time it powers on — otherwise the correction table lines up with the wrong point in the cycle and makes tracking worse, not better. Absolute encoders make this trivial, but they're expensive and add another point of failure. CelestialSync skips them and instead estimates the mount's orientation directly from the gravity vector sensed by an accelerometer rigidly mounted to each axis — no additional sensors beyond what's already on the mount.

Homing this way — from accelerometer readings on a slew, with no direct angle measurement — turned out to be a genuinely interesting estimation problem. It's the subject of the first project write-up, linked below.

### Status

The mechanical build and drive electronics are in progress (see the photo above). The homing algorithm — a two-pass Extended Kalman Filter that estimates both axis angles from accelerometer data alone — has been validated in simulation with a 99% homing success rate at mid-latitudes, and is next up to run on the physical mount.

### Posts

- [Homing a Telescope Mount With Nothing But Gravity]({% post_url 2025-05-20-sensorless-telescope-homing %}) — a gradual walkthrough of the sensorless homing algorithm above, with an interactive gravity-vector simulation.
