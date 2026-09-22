---
layout: post
title: "Casting a Full-Diameter Lap Tool Out of Dental Stone"
date: 2026-09-20 10:00:00
description: A full-diameter lap tool for the 6" mirror, cast in Velmix die stone straight off the mirror's own face. Why you cast instead of buy, the 1.9 mm that rules out a flat tool, and why this one has no tiles in it.
tags: optics telescopes atm
categories: atm-6in-f5
toc:
  sidebar: left
---

Working a mirror takes two discs, not one. The mirror is the piece you keep; the **tool** is the other half of the sandwich, the thing the abrasive or the polishing compound works between. For the [6" f/5]({{ '/projects/2_atm_6in_f5/' | relative_url }}) I cast a full-diameter tool out of dental stone — directly against the mirror it's going to work.

This one is specifically a **lap tool**. It gets a pitch lap poured onto it and goes to polishing; it is not what I'd use for rough or fine grinding. That distinction turns out to drive two of the decisions below.

<div class="row">
  <div class="col-sm mt-3 mt-md-0">
    {% include figure.liquid path="assets/img/posts/dental-stone-tool/DS_scored_face.JPEG" class="img-fluid rounded z-depth-1" zoomable=true caption="The finished lap tool, working face up, scored with a utility knife to key the pitch." %}
  </div>
</div>

## Why cast a tool at all

The obvious move is to buy a second glass blank and grind the two against each other, which is how mirrors were made for a century and works fine. It's also twice the glass and a lot of hogging out material you'll throw away.

Casting sidesteps the whole thing. Dental stone is cheap, it pours into any shape you can build a dam around, and — the part that matters here — it takes the curve you cast it against **exactly**, with no grinding required to get there. You skip straight to the tool you would have spent hours producing.

What goes *into* the casting depends on the job. A tool meant for rough or fine grinding gets ceramic tiles embedded in the face as it's poured: the tiles do the cutting, because they're hard enough to abrade glass for hours and the stone around them is not. A tool meant to carry a pitch lap doesn't need tiles, because it never touches the glass — the pitch does. This one is bare stone for exactly that reason, and the difference shows up again in how the face gets finished.

The specific product is **Velmix**, a Type IV dental die stone. Dentists use it for working models that have to hold fine detail and survive handling, which is nearly the same spec sheet a mirror tool wants: fine particle size, high compressive strength, and low setting expansion. That last one is the important property. A stone that swells appreciably as it sets gives you a tool that no longer matches the curve you cast it on, and the whole reason to cast is that the curve comes out right.

## The 1.9 millimetres

Here's the number that rules out the lazy option — casting the tool flat on a bench and calling it close enough.

A 6" f/5 mirror has a focal length of 762 mm and a radius of curvature of **R = 1524 mm**. The depth of the curve at the centre, the sagitta, is

$$ s \approx \frac{r^2}{2R} = \frac{(76.2\ \text{mm})^2}{2 \times 1524\ \text{mm}} = 1.9\ \text{mm} $$

So the mirror's centre sits 1.9 mm deeper than its edge. That sounds like nothing. It isn't. A pitch lap is a *contact* instrument — it polishes where it touches, and it removes material measured in fractions of a micron per session. Lay a flat lap on a 1.9 mm dish and it rides on the outer edge, polishing a narrow annulus and never reaching the centre at all. The pitch will eventually cold-flow to fit, but you'd be spending hours of press time getting the lap to the shape you could have simply cast it at.

Cast against the mirror, the tool starts at zero mismatch. That's the entire argument.

## The mould is the mirror

<div class="row">
  <div class="col-sm mt-3 mt-md-0">
    {% include figure.liquid path="assets/img/posts/dental-stone-tool/DS_mold_ready.JPEG" class="img-fluid rounded z-depth-1" zoomable=true caption="The dam taped up around the mirror. The mirror is face-up underneath, covered in plastic film held down with radial tape strips." %}
  </div>
  <div class="col-sm mt-3 mt-md-0">
    {% include figure.liquid path="assets/img/posts/dental-stone-tool/DS_fresh_pour.JPEG" class="img-fluid rounded z-depth-1" zoomable=true caption="Fresh pour. Velmix goes in about the consistency of heavy cream and self-levels." %}
  </div>
</div>

The setup is about as simple as it gets: the mirror face-up on the bench, a layer of plastic film over it as a release layer, and a dam wrapped around it and taped.

The dam is **cake collar** — the clear acetate strip used for lining cake tins. It's the right material almost by accident: it's stiff enough to hold a circle, thin enough to wrap a tight radius, cheap by the roll, and transparent, so you can watch the pour fill from the side and see whether air is trapped underneath instead of guessing.

I used 4" collar. I'd use 2" next time. The tool only needs to be an inch or so thick, so most of that height was just wall above the pour doing nothing except making the thing awkward to tape and easy to bow. Buy the narrow roll.

One other detail earns its keep: the film gets pulled down and taped radially so it lies against the glass rather than bridging across the dish. Any wrinkle that bridges is a void in the tool.

The film does cost you a little fidelity. You're casting against plastic-wrapped glass, not bare glass, so the tool inherits the film's thickness and whatever texture it has. Here that's irrelevant: a pitch lap is millimetres of pitch thick and conforms under pressure anyway, so a few thousandths of an inch of film in the substrate disappears entirely. It would matter if you were trying to cast an optical surface, which you are not.

Velmix mixes at a low water-to-powder ratio, which is what buys the strength, and it pours thick. Mix it too wet to make pouring easier and you trade away exactly the property you picked this material for.

## Out of the dam

<div class="row">
  <div class="col-sm mt-3 mt-md-0">
    {% include figure.liquid path="assets/img/posts/dental-stone-tool/DS_cured_in_dam.JPEG" class="img-fluid rounded z-depth-1" zoomable=true caption="Cured and still in the dam, sitting on the grinding stand's plate." %}
  </div>
  <div class="col-sm mt-3 mt-md-0">
    {% include figure.liquid path="assets/img/posts/dental-stone-tool/DS_edge_cleanup.JPEG" class="img-fluid rounded z-depth-1" zoomable=true caption="Knocking the edge back with a file. Die stone cuts easily and makes a lot of very fine dust." %}
  </div>
</div>

Peeling the dam off leaves a clean cylinder with a ragged rim — the meniscus where the stone climbed the plastic, plus a thin flash where it crept under. A file takes care of both in a few minutes, and the edge gets a bevel while the file is out.

That bevel is not cosmetic. An unbevelled stone edge chips, and a chip off the tool edge becomes a hard fragment loose in the abrasive, which is how you put a scratch into a mirror you've already spent hours on. Bevel everything, always, on both the tool and the mirror.

Die stone cuts fast and throws a huge amount of very fine dust, so this is a mask-and-outside job. Both of these photos are on the [concrete grinding stand]({{ '/projects/11_grinding_stand/' | relative_url }}) — the pink dust is all over the index marks on the grinding plate.

## Scoring the face

<div class="row">
  <div class="col-sm mt-3 mt-md-0">
    {% include figure.liquid path="assets/img/posts/dental-stone-tool/DS_scored_face.JPEG" class="img-fluid rounded z-depth-1" zoomable=true caption="Random crosshatch, cut freehand with a utility knife — a mechanical key for the pitch. No grid, on purpose." %}
  </div>
</div>

The last step is to cut a crosshatch of grooves across the working face with a utility knife. These are there for one reason: **to give the pitch something to grab.**

Pitch poured onto a smooth stone face is held by not much more than surface adhesion, and polishing is hours of exactly the shear load that finds that out. Cut grooves into the stone first and the pitch flows into them as it's poured, so once it sets the lap is keyed to the substrate mechanically rather than just stuck to it. The grooves are undercut by the knife's V-profile, which helps.

The pattern is deliberately random rather than a neat grid. A regular grid has a period, and anything with a period in mirror work is a chance to print that period into the glass. Random cuts have no preferred direction and no spacing to transfer.

Worth being clear about what this scoring is *not*: it isn't channelling for grinding slurry. A grinding tool solves that problem differently, with the gaps between its embedded tiles.

## Next: pitch

The tool is done and waiting on the thing it exists for: a pitch lap poured onto this face. That's the surface that will do the polishing and, eventually, the figuring that the [Foucault test]({% post_url 2026-09-16-foucault-test-explained %}) is there to measure.

That's the next post.
