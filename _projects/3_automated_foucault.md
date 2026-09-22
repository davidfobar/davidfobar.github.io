---
layout: project
title: Automated Foucault
description: Replacing the eyeball and the dial indicator with a camera and a motorised stage, to measure a telescope mirror's figure automatically.
img: assets/img/projects/automated_foucault/foucault_shadowgram.png
importance: 3
category: astronomy
---

The Foucault test is how amateur telescope makers have measured mirror figure for more than a century. It's elegant, it costs almost nothing to build, and it's sensitive to surface errors far smaller than a wavelength of light. It's also, in its traditional form, a manual process performed by a person squinting at a grey disk in a dark room.

This project replaces the two weakest links in that chain — the human eye and the dial indicator — with a camera and a motorised stage, and then replaces the null-hunting entirely with a fit against a ray-traced forward model.

<div class="row">
  <div class="col-sm mt-3 mt-md-0">
    {% include figure.liquid path="assets/img/projects/automated_foucault/foucault_shadowgram.png" class="img-fluid rounded z-depth-1" zoomable=true caption="A Foucault shadowgram of a 6\" f/5 paraboloid with the knife edge 0.66 mm outside the paraxial centre of curvature — ray-traced, not photographed. The dark lens-shaped region is the zone whose light the blade is currently cutting; sliding the stage another 1.2 mm sweeps it out to the rim." %}
  </div>
</div>

### Why it's worth automating

For the [6" f/5]({{ '/projects/2_atm_6in_f5/' | relative_url }}) mirror I'm grinding, the entire test — centre of the mirror to the very edge — spans **1.9 mm of stage travel**. The surface error that travel is reporting on is about 1.2 µm of glass. The test amplifies it roughly 1600×, which is what makes the whole thing possible in a garage.

But it means the measurement is only as good as your ability to resolve that 1.9 mm. Three things limit the manual version:

- **The eye adapts.** Judging whether a zone is *uniformly* grey is something human vision does well; judging *how* grey it is relative to its neighbour is something it does badly, and your dark adaptation drifts while you work.
- **A dial indicator reads to a thousandth of an inch.** That's about 75 discriminable steps across the full range — workable, but not generous, and the zones bunch up badly near the centre where the spacing goes as $$r^2$$.
- **A full set of zonal readings takes a while.** Long enough that the mirror's temperature — and therefore its figure — changes while you're measuring it.

### Approach

- **Camera instead of an eye.** A sensor doesn't dark-adapt and reports actual per-pixel numbers, so nulls can be found by differencing frames rather than by judgement.
- **Motorised stage instead of a dial indicator.** A stepper on a fine-pitch screw resolves well under a micron and can sweep the whole 1.9 mm range in seconds, repeatably.
- **Model fitting instead of null-hunting.** With images captured across a full sweep, there's no need to find each zone's null by hand. Fit the whole measured stack against a forward model of the surface and let the optimiser solve for the figure directly.

That last step is the interesting one, and it's the reason the simulation in the post below exists: it *is* the forward model. Give it a surface and a stage position and it predicts the image. Automated testing is the same function run backwards.

### Status

Early. The optical model is worked out and validated against exact ray tracing, and it's what drives the interactive simulation in the write-up below. Hardware — stage, camera mount, and control — is next.

### Posts

- [The Foucault Test, With Actual Numbers]({% post_url 2026-09-16-foucault-test-explained %}) — what the Foucault test actually measures, how far the knife edge really travels, and an interactive ray-traced simulation of the whole thing.
