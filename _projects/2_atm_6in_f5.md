---
layout: project
title: 'ATM - 6" F/5 (2026)'
description: Grinding, polishing, and figuring a 6-inch f/5 Newtonian primary from a raw blank — small on purpose, fast on purpose.
img: assets/img/projects/atm_6in_f5/ATM_mirror.jpg
importance: 2
category: astronomy
---

A 6" f/5 mirror, made the long way: ground, polished, and figured by hand from a raw blank. Focal length 762 mm, radius of curvature 1524 mm.

Picking those two numbers took longer than it should have, because the standard advice and what I actually wanted out of the project pull in opposite directions.

### Why 6 inches, and why f/5

Every first-mirror guide says the same thing: **start small.** It's good advice. Small glass is cheap, grinds fast, is light enough to handle wet without dropping, and forgives the mistakes you're guaranteed to make. I wanted to go bigger — everyone does — and I talked myself out of it.

The problem is what "start small" usually means in practice, which is a 6" f/8. And a 6" f/8 has a property that makes it a poor teacher: **you can leave it as a sphere and it's nearly good enough.**

That's not a figure of speech. Grinding two discs against each other naturally produces a **sphere**, and a sphere is not the shape a telescope wants — a paraboloid is. Getting from one to the other means deepening the centre: you hold the edge of the mirror where it is and take glass out of everything inside it, until the surface is a paraboloid instead.

How much glass depends brutally on focal ratio. The cut falls off as $$r^4$$ going outward, and scales as $$1/F^3$$, so it collapses as the mirror gets slower. Plotted for a 6" disc, that's the whole argument in one figure:

<style>
.sag-fig{--s4:#0d366b;--s5:#1c5cab;--s6:#2a78d6;--s8:#5598e7;--s10:#86b6ef;--gridc:rgba(0,0,0,.10);--axisc:rgba(0,0,0,.28);--mutedc:#6a6a6a;--inkc:var(--global-text-color,#000);--surfc:var(--global-bg-color,#fff);margin:1.6rem 0 1.2rem}
html[data-theme="dark"] .sag-fig{--s4:#b7d3f6;--s5:#6da7ec;--s6:#3987e5;--s8:#256abf;--s10:#184f95;--gridc:rgba(255,255,255,.13);--axisc:rgba(255,255,255,.32);--mutedc:#9b9b9b}
.sag-legend{display:flex;flex-wrap:wrap;gap:.35rem 1rem;margin-bottom:.6rem;font-size:.85rem;color:var(--inkc)}
.sag-key{display:inline-flex;align-items:center;gap:.4rem}
.sag-key .sw{width:14px;height:3px;border-radius:2px;display:inline-block;font-style:normal}
.sag-key .sw.s4{background:var(--s4)}.sag-key .sw.s5{background:var(--s5);height:4px}.sag-key .sw.s6{background:var(--s6)}.sag-key .sw.s8{background:var(--s8)}.sag-key .sw.s10{background:var(--s10)}
.sag-plot{position:relative}
.sag-fig svg{width:100%;height:auto;display:block;overflow:visible}
.sag-fig .grid{stroke:var(--gridc);stroke-width:1}
.sag-fig .axis{stroke:var(--axisc);stroke-width:1}
.sag-fig .tickmark{stroke:var(--axisc);stroke-width:1}
.sag-fig .tick{fill:var(--mutedc);font-size:12px}
.sag-fig .ta-e{text-anchor:end}.sag-fig .ta-m{text-anchor:middle}
.sag-fig .axlab{fill:var(--mutedc);font-size:12.5px}
.sag-fig .ln{fill:none;stroke-width:2;stroke-linecap:round;stroke-linejoin:round}
.sag-fig .ln.hero{stroke-width:3.4}
.sag-fig .ln.s4{stroke:var(--s4)}.sag-fig .ln.s5{stroke:var(--s5)}.sag-fig .ln.s6{stroke:var(--s6)}.sag-fig .ln.s8{stroke:var(--s8)}.sag-fig .ln.s10{stroke:var(--s10)}
.sag-fig .dot{stroke:var(--surfc);stroke-width:2}
.sag-fig .dot.s4{fill:var(--s4)}.sag-fig .dot.s5{fill:var(--s5)}.sag-fig .dot.s6{fill:var(--s6)}.sag-fig .dot.s8{fill:var(--s8)}.sag-fig .dot.s10{fill:var(--s10)}
.sag-fig .dlab{fill:var(--inkc);font-size:13px;font-weight:600;paint-order:stroke;stroke:var(--surfc);stroke-width:3px}
.sag-fig .dvalin{fill:var(--mutedc);font-size:11.5px;font-weight:400}
.sag-fig .dval{fill:var(--mutedc);font-size:11.5px;paint-order:stroke;stroke:var(--surfc);stroke-width:3px}
.sag-fig .limit{stroke:var(--mutedc);stroke-width:1.5;stroke-dasharray:5 4}
.sag-fig .limlab{fill:var(--mutedc);font-size:11.5px;paint-order:stroke;stroke:var(--surfc);stroke-width:3px}
.sag-fig .cross{stroke:var(--axisc);stroke-width:1;stroke-dasharray:3 3;pointer-events:none}
.sag-tip{position:absolute;pointer-events:none;background:var(--surfc);border:1px solid var(--gridc);border-radius:6px;padding:.45rem .6rem;font-size:.78rem;line-height:1.45;color:var(--inkc);box-shadow:0 2px 10px rgba(0,0,0,.18);white-space:nowrap;z-index:5}
.sag-tip b{font-weight:600}
.sag-tip .row{display:flex;justify-content:space-between;gap:.9rem}
.sag-tip .row i{width:9px;height:9px;border-radius:50%;display:inline-block;margin-right:.35rem}
@media (max-width:560px){.sag-fig .dvalin{fill:var(--mutedc);font-size:11.5px;font-weight:400}
.sag-fig .dval{display:none}}
</style>
<div class="sag-fig" id="sagFig">
<div class="sag-legend">
<span class="sag-key"><i class="sw s4"></i>f/4</span>
<span class="sag-key"><i class="sw s5"></i>f/5</span>
<span class="sag-key"><i class="sw s6"></i>f/6</span>
<span class="sag-key"><i class="sw s8"></i>f/8</span>
<span class="sag-key"><i class="sw s10"></i>f/10</span>
</div>
<div class="sag-plot"><svg viewBox="0 0 760 440" role="img" aria-labelledby="sagTitle sagDesc" preserveAspectRatio="xMidYMid meet">
<title id="sagTitle">Glass to remove from a sphere to reach a paraboloid, 6-inch mirror</title>
<desc id="sagDesc">Depth of glass that must be removed at each radius to turn a ground sphere into a paraboloid, holding the edge of the mirror fixed, for focal ratios f/4 through f/10 on a 6-inch disc. The cut is deepest at the centre and tapers to nothing at the edge; it is nearly flat across the inner half of the mirror and falls away steeply in the outer inch. A faster mirror needs far more glass removed: f/5 peaks at 1.19 microns at the centre against f/8 at 0.29, and the quarter-wave diffraction limit sits at 0.275 microns.</desc>
<line class="grid" x1="64" y1="388.0" x2="652" y2="388.0"/>
<text class="tick ta-e" x="54" y="392.0">0.0</text>
<line class="grid" x1="64" y1="316.0" x2="652" y2="316.0"/>
<text class="tick ta-e" x="54" y="320.0">0.5</text>
<line class="grid" x1="64" y1="244.0" x2="652" y2="244.0"/>
<text class="tick ta-e" x="54" y="248.0">1.0</text>
<line class="grid" x1="64" y1="172.0" x2="652" y2="172.0"/>
<text class="tick ta-e" x="54" y="176.0">1.5</text>
<line class="grid" x1="64" y1="100.0" x2="652" y2="100.0"/>
<text class="tick ta-e" x="54" y="104.0">2.0</text>
<line class="grid" x1="64" y1="28.0" x2="652" y2="28.0"/>
<text class="tick ta-e" x="54" y="32.0">2.5</text>
<line class="tickmark" x1="64.0" y1="388" x2="64.0" y2="394"/>
<text class="tick ta-m" x="64.0" y="410">0</text>
<line class="tickmark" x1="162.0" y1="388" x2="162.0" y2="394"/>
<text class="tick ta-m" x="162.0" y="410">0.5</text>
<line class="tickmark" x1="260.0" y1="388" x2="260.0" y2="394"/>
<text class="tick ta-m" x="260.0" y="410">1</text>
<line class="tickmark" x1="358.0" y1="388" x2="358.0" y2="394"/>
<text class="tick ta-m" x="358.0" y="410">1.5</text>
<line class="tickmark" x1="456.0" y1="388" x2="456.0" y2="394"/>
<text class="tick ta-m" x="456.0" y="410">2</text>
<line class="tickmark" x1="554.0" y1="388" x2="554.0" y2="394"/>
<text class="tick ta-m" x="554.0" y="410">2.5</text>
<line class="tickmark" x1="652.0" y1="388" x2="652.0" y2="394"/>
<text class="tick ta-m" x="652.0" y="410">3</text>
<line class="axis" x1="64" y1="388" x2="652" y2="388"/>
<line class="limit" x1="64" y1="348.4" x2="652" y2="348.4"/>
<text class="limlab" x="659" y="352.4">¼-wave limit</text>
<polyline class="ln s4" points="64.0,53.14 67.7,53.14 71.3,53.14 75.0,53.14 78.7,53.14 82.4,53.14 86.0,53.14 89.7,53.14 93.4,53.14 97.1,53.14 100.8,53.14 104.4,53.14 108.1,53.15 111.8,53.15 115.5,53.16 119.1,53.16 122.8,53.17 126.5,53.18 130.2,53.19 133.8,53.20 137.5,53.22 141.2,53.24 144.8,53.26 148.5,53.28 152.2,53.31 155.9,53.34 159.6,53.37 163.2,53.41 166.9,53.45 170.6,53.50 174.2,53.55 177.9,53.61 181.6,53.67 185.3,53.74 188.9,53.82 192.6,53.90 196.3,53.99 200.0,54.09 203.7,54.20 207.3,54.32 211.0,54.44 214.7,54.58 218.3,54.73 222.0,54.88 225.7,55.05 229.4,55.23 233.1,55.42 236.7,55.63 240.4,55.85 244.1,56.08 247.8,56.33 251.4,56.59 255.1,56.87 258.8,57.17 262.4,57.48 266.1,57.81 269.8,58.16 273.5,58.53 277.1,58.92 280.8,59.33 284.5,59.76 288.2,60.21 291.9,60.69 295.5,61.19 299.2,61.71 302.9,62.26 306.6,62.83 310.2,63.43 313.9,64.06 317.6,64.72 321.2,65.40 324.9,66.12 328.6,66.87 332.3,67.65 335.9,68.46 339.6,69.30 343.3,70.18 347.0,71.10 350.6,72.05 354.3,73.04 358.0,74.07 361.7,75.13 365.4,76.24 369.0,77.39 372.7,78.58 376.4,79.81 380.1,81.09 383.7,82.41 387.4,83.78 391.1,85.20 394.8,86.66 398.4,88.18 402.1,89.74 405.8,91.36 409.4,93.03 413.1,94.75 416.8,96.54 420.5,98.37 424.1,100.27 427.8,102.22 431.5,104.23 435.2,106.31 438.9,108.44 442.5,110.65 446.2,112.91 449.9,115.24 453.6,117.64 457.2,120.11 460.9,122.65 464.6,125.26 468.2,127.95 471.9,130.70 475.6,133.54 479.3,136.45 482.9,139.44 486.6,142.50 490.3,145.65 494.0,148.89 497.6,152.20 501.3,155.60 505.0,159.09 508.7,162.67 512.4,166.33 516.0,170.09 519.7,173.94 523.4,177.88 527.0,181.92 530.7,186.06 534.4,190.30 538.1,194.63 541.8,199.07 545.4,203.61 549.1,208.26 552.8,213.02 556.5,217.88 560.1,222.85 563.8,227.94 567.5,233.14 571.1,238.45 574.8,243.88 578.5,249.43 582.2,255.10 585.9,260.89 589.5,266.80 593.2,272.84 596.9,279.01 600.5,285.30 604.2,291.73 607.9,298.29 611.6,304.98 615.2,311.81 618.9,318.78 622.6,325.88 626.3,333.13 630.0,340.53 633.6,348.06 637.3,355.75 641.0,363.58 644.6,371.57 648.3,379.71 652.0,388.00"/>
<polyline class="ln s5 hero" points="64.0,216.55 67.7,216.55 71.3,216.55 75.0,216.55 78.7,216.55 82.4,216.55 86.0,216.55 89.7,216.55 93.4,216.55 97.1,216.55 100.8,216.55 104.4,216.55 108.1,216.56 111.8,216.56 115.5,216.56 119.1,216.56 122.8,216.57 126.5,216.57 130.2,216.58 133.8,216.58 137.5,216.59 141.2,216.60 144.8,216.61 148.5,216.62 152.2,216.64 155.9,216.65 159.6,216.67 163.2,216.69 166.9,216.71 170.6,216.74 174.2,216.76 177.9,216.79 181.6,216.82 185.3,216.86 188.9,216.90 192.6,216.94 196.3,216.99 200.0,217.04 203.7,217.10 207.3,217.16 211.0,217.22 214.7,217.29 218.3,217.36 222.0,217.44 225.7,217.53 229.4,217.62 233.1,217.72 236.7,217.83 240.4,217.94 244.1,218.06 247.8,218.19 251.4,218.32 255.1,218.46 258.8,218.61 262.4,218.77 266.1,218.94 269.8,219.12 273.5,219.31 277.1,219.51 280.8,219.72 284.5,219.94 288.2,220.17 291.9,220.42 295.5,220.67 299.2,220.94 302.9,221.22 306.6,221.51 310.2,221.82 313.9,222.14 317.6,222.48 321.2,222.83 324.9,223.20 328.6,223.58 332.3,223.98 335.9,224.39 339.6,224.83 343.3,225.28 347.0,225.75 350.6,226.23 354.3,226.74 358.0,227.27 361.7,227.81 365.4,228.38 369.0,228.97 372.7,229.57 376.4,230.21 380.1,230.86 383.7,231.54 387.4,232.24 391.1,232.96 394.8,233.71 398.4,234.49 402.1,235.29 405.8,236.12 409.4,236.98 413.1,237.86 416.8,238.77 420.5,239.71 424.1,240.68 427.8,241.68 431.5,242.71 435.2,243.77 438.9,244.87 442.5,245.99 446.2,247.15 449.9,248.35 453.6,249.58 457.2,250.84 460.9,252.14 464.6,253.48 468.2,254.85 471.9,256.26 475.6,257.72 479.3,259.21 482.9,260.74 486.6,262.31 490.3,263.92 494.0,265.57 497.6,267.27 501.3,269.01 505.0,270.80 508.7,272.63 512.4,274.51 516.0,276.43 519.7,278.40 523.4,280.42 527.0,282.49 530.7,284.61 534.4,286.78 538.1,289.00 541.8,291.27 545.4,293.59 549.1,295.97 552.8,298.41 556.5,300.90 560.1,303.44 563.8,306.05 567.5,308.71 571.1,311.43 574.8,314.21 578.5,317.05 582.2,319.95 585.9,322.92 589.5,325.95 593.2,329.04 596.9,332.20 600.5,335.42 604.2,338.71 607.9,342.07 611.6,345.49 615.2,348.99 618.9,352.56 622.6,356.20 626.3,359.91 630.0,363.69 633.6,367.55 637.3,371.49 641.0,375.50 644.6,379.59 648.3,383.75 652.0,388.00"/>
<polyline class="ln s6" points="64.0,288.78 67.7,288.78 71.3,288.78 75.0,288.78 78.7,288.78 82.4,288.78 86.0,288.78 89.7,288.78 93.4,288.78 97.1,288.78 100.8,288.78 104.4,288.78 108.1,288.78 111.8,288.79 115.5,288.79 119.1,288.79 122.8,288.79 126.5,288.79 130.2,288.80 133.8,288.80 137.5,288.81 141.2,288.81 144.8,288.82 148.5,288.82 152.2,288.83 155.9,288.84 159.6,288.85 163.2,288.86 166.9,288.87 170.6,288.89 174.2,288.90 177.9,288.92 181.6,288.94 185.3,288.96 188.9,288.98 192.6,289.01 196.3,289.04 200.0,289.06 203.7,289.10 207.3,289.13 211.0,289.17 214.7,289.21 218.3,289.25 222.0,289.30 225.7,289.35 229.4,289.40 233.1,289.46 236.7,289.52 240.4,289.58 244.1,289.65 247.8,289.73 251.4,289.81 255.1,289.89 258.8,289.98 262.4,290.07 266.1,290.17 269.8,290.27 273.5,290.38 277.1,290.49 280.8,290.62 284.5,290.74 288.2,290.88 291.9,291.02 295.5,291.17 299.2,291.32 302.9,291.48 306.6,291.65 310.2,291.83 313.9,292.02 317.6,292.21 321.2,292.42 324.9,292.63 328.6,292.85 332.3,293.08 335.9,293.32 339.6,293.57 343.3,293.83 347.0,294.10 350.6,294.39 354.3,294.68 358.0,294.98 361.7,295.30 365.4,295.63 369.0,295.97 372.7,296.32 376.4,296.68 380.1,297.06 383.7,297.45 387.4,297.86 391.1,298.28 394.8,298.71 398.4,299.16 402.1,299.63 405.8,300.11 409.4,300.60 413.1,301.11 416.8,301.64 420.5,302.18 424.1,302.75 427.8,303.32 431.5,303.92 435.2,304.54 438.9,305.17 442.5,305.82 446.2,306.49 449.9,307.18 453.6,307.89 457.2,308.63 460.9,309.38 464.6,310.15 468.2,310.95 471.9,311.76 475.6,312.60 479.3,313.47 482.9,314.35 486.6,315.26 490.3,316.19 494.0,317.15 497.6,318.13 501.3,319.14 505.0,320.17 508.7,321.23 512.4,322.32 516.0,323.43 519.7,324.57 523.4,325.74 527.0,326.94 530.7,328.17 534.4,329.42 538.1,330.71 541.8,332.02 545.4,333.37 549.1,334.74 552.8,336.15 556.5,337.59 560.1,339.07 563.8,340.57 567.5,342.11 571.1,343.69 574.8,345.30 578.5,346.94 582.2,348.62 585.9,350.34 589.5,352.09 593.2,353.88 596.9,355.71 600.5,357.57 604.2,359.48 607.9,361.42 611.6,363.40 615.2,365.43 618.9,367.49 622.6,369.60 626.3,371.74 630.0,373.93 633.6,376.17 637.3,378.44 641.0,380.77 644.6,383.13 648.3,385.54 652.0,388.00"/>
<polyline class="ln s8" points="64.0,346.14 67.7,346.14 71.3,346.14 75.0,346.14 78.7,346.14 82.4,346.14 86.0,346.14 89.7,346.14 93.4,346.14 97.1,346.14 100.8,346.14 104.4,346.14 108.1,346.14 111.8,346.14 115.5,346.14 119.1,346.15 122.8,346.15 126.5,346.15 130.2,346.15 133.8,346.15 137.5,346.15 141.2,346.15 144.8,346.16 148.5,346.16 152.2,346.16 155.9,346.17 159.6,346.17 163.2,346.18 166.9,346.18 170.6,346.19 174.2,346.19 177.9,346.20 181.6,346.21 185.3,346.22 188.9,346.23 192.6,346.24 196.3,346.25 200.0,346.26 203.7,346.28 207.3,346.29 211.0,346.31 214.7,346.32 218.3,346.34 222.0,346.36 225.7,346.38 229.4,346.40 233.1,346.43 236.7,346.45 240.4,346.48 244.1,346.51 247.8,346.54 251.4,346.57 255.1,346.61 258.8,346.65 262.4,346.69 266.1,346.73 269.8,346.77 273.5,346.82 277.1,346.86 280.8,346.92 284.5,346.97 288.2,347.03 291.9,347.09 295.5,347.15 299.2,347.21 302.9,347.28 306.6,347.35 310.2,347.43 313.9,347.51 317.6,347.59 321.2,347.68 324.9,347.77 328.6,347.86 332.3,347.96 335.9,348.06 339.6,348.16 343.3,348.27 347.0,348.39 350.6,348.51 354.3,348.63 358.0,348.76 361.7,348.89 365.4,349.03 369.0,349.17 372.7,349.32 376.4,349.48 380.1,349.64 383.7,349.80 387.4,349.97 391.1,350.15 394.8,350.33 398.4,350.52 402.1,350.72 405.8,350.92 409.4,351.13 413.1,351.34 416.8,351.57 420.5,351.80 424.1,352.03 427.8,352.28 431.5,352.53 435.2,352.79 438.9,353.06 442.5,353.33 446.2,353.61 449.9,353.91 453.6,354.21 457.2,354.51 460.9,354.83 464.6,355.16 468.2,355.49 471.9,355.84 475.6,356.19 479.3,356.56 482.9,356.93 486.6,357.31 490.3,357.71 494.0,358.11 497.6,358.53 501.3,358.95 505.0,359.39 508.7,359.83 512.4,360.29 516.0,360.76 519.7,361.24 523.4,361.74 527.0,362.24 530.7,362.76 534.4,363.29 538.1,363.83 541.8,364.38 545.4,364.95 549.1,365.53 552.8,366.13 556.5,366.73 560.1,367.36 563.8,367.99 567.5,368.64 571.1,369.31 574.8,369.98 578.5,370.68 582.2,371.39 585.9,372.11 589.5,372.85 593.2,373.61 596.9,374.38 600.5,375.16 604.2,375.97 607.9,376.79 611.6,377.62 615.2,378.48 618.9,379.35 622.6,380.24 626.3,381.14 630.0,382.07 633.6,383.01 637.3,383.97 641.0,384.95 644.6,385.95 648.3,386.96 652.0,388.00"/>
<polyline class="ln s10" points="64.0,366.57 67.7,366.57 71.3,366.57 75.0,366.57 78.7,366.57 82.4,366.57 86.0,366.57 89.7,366.57 93.4,366.57 97.1,366.57 100.8,366.57 104.4,366.57 108.1,366.57 111.8,366.57 115.5,366.57 119.1,366.57 122.8,366.57 126.5,366.57 130.2,366.57 133.8,366.57 137.5,366.57 141.2,366.58 144.8,366.58 148.5,366.58 152.2,366.58 155.9,366.58 159.6,366.58 163.2,366.59 166.9,366.59 170.6,366.59 174.2,366.60 177.9,366.60 181.6,366.60 185.3,366.61 188.9,366.61 192.6,366.62 196.3,366.62 200.0,366.63 203.7,366.64 207.3,366.64 211.0,366.65 214.7,366.66 218.3,366.67 222.0,366.68 225.7,366.69 229.4,366.70 233.1,366.72 236.7,366.73 240.4,366.74 244.1,366.76 247.8,366.77 251.4,366.79 255.1,366.81 258.8,366.83 262.4,366.85 266.1,366.87 269.8,366.89 273.5,366.91 277.1,366.94 280.8,366.97 284.5,366.99 288.2,367.02 291.9,367.05 295.5,367.08 299.2,367.12 302.9,367.15 306.6,367.19 310.2,367.23 313.9,367.27 317.6,367.31 321.2,367.35 324.9,367.40 328.6,367.45 332.3,367.50 335.9,367.55 339.6,367.60 343.3,367.66 347.0,367.72 350.6,367.78 354.3,367.84 358.0,367.91 361.7,367.98 365.4,368.05 369.0,368.12 372.7,368.20 376.4,368.28 380.1,368.36 383.7,368.44 387.4,368.53 391.1,368.62 394.8,368.71 398.4,368.81 402.1,368.91 405.8,369.01 409.4,369.12 413.1,369.23 416.8,369.35 420.5,369.46 424.1,369.59 427.8,369.71 431.5,369.84 435.2,369.97 438.9,370.11 442.5,370.25 446.2,370.39 449.9,370.54 453.6,370.70 457.2,370.86 460.9,371.02 464.6,371.18 468.2,371.36 471.9,371.53 475.6,371.71 479.3,371.90 482.9,372.09 486.6,372.29 490.3,372.49 494.0,372.70 497.6,372.91 501.3,373.13 505.0,373.35 508.7,373.58 512.4,373.81 516.0,374.05 519.7,374.30 523.4,374.55 527.0,374.81 530.7,375.08 534.4,375.35 538.1,375.62 541.8,375.91 545.4,376.20 549.1,376.50 552.8,376.80 556.5,377.11 560.1,377.43 563.8,377.76 567.5,378.09 571.1,378.43 574.8,378.78 578.5,379.13 582.2,379.49 585.9,379.86 589.5,380.24 593.2,380.63 596.9,381.02 600.5,381.43 604.2,381.84 607.9,382.26 611.6,382.69 615.2,383.12 618.9,383.57 622.6,384.02 626.3,384.49 630.0,384.96 633.6,385.44 637.3,385.94 641.0,386.44 644.6,386.95 648.3,387.47 652.0,388.00"/>
<circle class="dot s4" cx="64.0" cy="53.14" r="4"/>
<text class="dlab" x="75.0" y="45.14">f/4  <tspan class="dvalin">2.33 µm at centre</tspan></text>
<circle class="dot s5" cx="64.0" cy="216.55" r="5"/>
<text class="dlab" x="75.0" y="208.55">f/5  <tspan class="dvalin">1.19 µm at centre</tspan></text>
<circle class="dot s6" cx="64.0" cy="288.78" r="4"/>
<text class="dlab" x="75.0" y="280.78">f/6  <tspan class="dvalin">0.69 µm at centre</tspan></text>
<circle class="dot s8" cx="64.0" cy="346.14" r="4"/>
<text class="dlab" x="75.0" y="338.14">f/8  <tspan class="dvalin">0.29 µm at centre</tspan></text>
<circle class="dot s10" cx="64.0" cy="366.57" r="4"/>
<text class="dlab" x="75.0" y="380.57">f/10  <tspan class="dvalin">0.15 µm at centre</tspan></text>
<text class="axlab ta-m" x="358" y="434">distance from centre of the mirror (inches)</text>
<text class="axlab" transform="translate(16,208) rotate(-90)" text-anchor="middle">glass to remove (µm)</text>
<rect id="sagHit" x="64" y="28" width="588" height="360" fill="transparent"/>
<line id="sagCross" class="cross" x1="0" y1="28" x2="0" y2="388" style="display:none"/>
</svg><div id="sagTip" class="sag-tip" hidden></div></div>
</div>
<script>
(function(){
  var fig=document.getElementById('sagFig'); if(!fig) return;
  var svg=fig.querySelector('svg'), hit=document.getElementById('sagHit'),
      cross=document.getElementById('sagCross'), tip=document.getElementById('sagTip'),
      plot=fig.querySelector('.sag-plot');
  var X0=64,X1=652,Y0=28,Y1=388,RMAX=3,YMAX=2.5,D=6,UM=25400;
  var series=[[4,'--s4'],[5,'--s5'],[6,'--s6'],[8,'--s8'],[10,'--s10']];
  function rem(r,F){var R=2*F*D; return ((Math.pow(3,4)-Math.pow(r,4))/(8*Math.pow(R,3)))*UM;}
  function hide(){cross.style.display='none'; tip.hidden=true;}
  function move(ev){
    var rect=svg.getBoundingClientRect(), k=rect.width/760;
    var cx=(ev.clientX-rect.left)/k;
    if(cx<X0){cx=X0;} if(cx>X1){cx=X1;}
    var r=(cx-X0)/(X1-X0)*RMAX;
    cross.setAttribute('x1',cx); cross.setAttribute('x2',cx); cross.style.display='';
    var cs=getComputedStyle(fig);
    var html='<b>'+r.toFixed(2)+'&Prime; from centre</b>';
    series.forEach(function(s){
      html+='<div class="row"><span><i style="background:'+cs.getPropertyValue(s[1]).trim()+'"></i>f/'+s[0]+'</span><span>'+rem(r,s[0]).toFixed(3)+' &micro;m</span></div>';
    });
    tip.innerHTML=html; tip.hidden=false;
    var px=cx*k, ty=(ev.clientY-rect.top);
    var tw=tip.offsetWidth, th=tip.offsetHeight;
    var left=px+14; if(left+tw>rect.width) left=px-tw-14; if(left<0) left=0;
    var top=ty-th/2; if(top<0) top=0; if(top+th>rect.height) top=rect.height-th;
    tip.style.left=left+'px'; tip.style.top=top+'px';
  }
  hit.addEventListener('mousemove',move);
  hit.addEventListener('mouseleave',hide);
  hit.addEventListener('touchmove',function(e){if(e.touches[0]){move(e.touches[0]);}},{passive:true});
  hit.addEventListener('touchend',hide);
})();
</script>
Three things to read off it.

**The deepest cut is at the centre, and it's tiny.** Every curve peaks at $$r=0$$ and runs down to zero at the edge, because the edge is the datum you're working to. For f/5 the peak is 1.19 µm — you are removing rather less than a wavelength of glass from the middle of a six-inch disc, and that is the difference between a telescope and a paperweight.

**All the shaping is in the outer inch.** Because the fall-off goes as the fourth power of radius, the curves are nearly flat across the inner half of the mirror and then plunge in the last inch. The profile barely changes for the first two inches and does almost everything in the final one, so figuring is overwhelmingly a problem of controlling the outer zone — which is also the hardest part of the glass to control, and why "turned edge" is the classic failure.

**The dashed line is where this project got decided.** Convert the ¼-wave Rayleigh criterion — the point past which the optics stop being the thing holding the image back — into glass, and it lands at 0.275 µm. A **6" f/8 peaks at 0.29 µm: it sits on the line.** You could stop at "polished sphere," point it at Jupiter, and have a perfectly pleasant telescope. Which means you'd never have to learn to figure, and figuring is the part I actually wanted to learn.

The same disc at **f/5 peaks at 1.19 µm — more than four times over.** There is no version of this project where I get away with not parabolising. The skill stops being optional and becomes the gate.

So the decision resolved itself: hold the diameter small, where the advice is right and the glass is forgiving, and buy the difficulty back with focal ratio instead of aperture. Small disc, real figuring problem.

### The 1.2 microns

That 1.19 µm at the peak of the f/5 curve is the entire job — roughly fifty millionths of an inch, taken out of the middle of a six-inch face.

Remove it in precisely the right radial distribution and you have a telescope; remove it in slightly the wrong distribution and you have a turned edge or a zone. You cannot see 1.2 µm, you cannot feel it, and you cannot measure it with anything in a normal workshop. The whole craft of figuring is about making that quantity visible.

### Testing

Which is why the test gear matters as much as the grinding, and why it's a project of its own.

The [Automated Foucault]({{ '/projects/3_automated_foucault/' | relative_url }}) tester covers the first half. The Foucault test is a genuinely remarkable piece of instrumentation — a pinhole, a razor blade, and a way to move them turns a sub-micron surface error into a visible pattern of light and shadow. I wrote up [how it actually works, with real numbers]({{ '/blog/2026/foucault-test-explained/' | relative_url }}), because the geometry is more elegant than most explanations let on.

But Foucault is a zonal test — it reads the surface a ring at a time, through a knife edge, with your eye or a camera as the detector. Getting a fast mirror the last of the way home wants something quantitative across the whole aperture at once, and that means a **Bath interferometer**: a common-path shearing interferometer that's simple enough to build on a bench and precise enough to resolve fringes at a fraction of a wave. Building one and learning to read it is part of the point of choosing f/5. A slower mirror wouldn't have justified it.

Developing the testing routine — the setup, the repeatability, the discipline of measuring before and after every session — is as much the deliverable here as the mirror.

### Where it ends up

Undecided, and comfortably so. The obvious home is a Dobsonian: a 6" f/5 has a 762 mm focal length, so the tube is short enough to build a compact, genuinely grab-and-go Dob.

The other possibility is that it ends up on [CelestialSync]({{ '/projects/1_celestial_sync/' | relative_url }}) — the equatorial mount is being built anyway, and a fast 6" is a reasonable imaging aperture. Time will tell. The mirror gets made either way; the tube it lands in is a problem for later.

### Posts

- [Casting a Full-Diameter Lap Tool Out of Dental Stone]({{ '/blog/2026/dental-stone-tool/' | relative_url }}) — casting the lap substrate in Velmix die stone straight off the mirror's own face, the 1.9 mm of sagitta that rules out a flat one, and why a lap tool gets no tiles.
