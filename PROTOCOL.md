# Measuring Before Engraving — A Protocol for Curved Metal Surfaces

**Version 0.1 — unverified. No measurements have been taken.**
Every results table below is blank on purpose. See [CONTRIBUTING.md](CONTRIBUTING.md) if you can fill one in.


Three claims about engraving shallowly curved metal, none of them yet measured: surface tilt is negligible where defocus is not, the correction a tilting fixture must apply is bounded by the part's own sag, and two measurements are enough to choose a correction method. The rest of this document is the procedure that would confirm or kill each one.

## What we think is new

In descending order of how confident we are that it is not already written down somewhere.

**1. On shallowly curved parts, tilt is negligible and defocus dominates — by more than two orders of magnitude.** The instinct when a part curves is to tilt it back to perpendicular. For a belt buckle 80 mm wide with 1.5 mm of sag, the edge tilt is 4.3°, which elongates the focused spot by a factor of 1.003 — three parts in a thousand. The same 1.5 mm of sag, left uncorrected, grows the spot to 2.5× its focused diameter on a 30 µm source — a 150 % error. Tilting therefore chases 0.3 % and leaves 150 % in place, a ratio of about 500 to 1. We have not found this stated for hobby-scale parts, and it is the claim that saves someone the cost of a two-axis fixture.

**2. The Z correction a rotary fixture must apply equals the part's sag, exactly.** Chuck the part so the rotation axis passes through the part's own centre rather than the centre of its curvature. Rotate by φ to bring a new patch perpendicular to the beam, and that patch now sits ΔZ = R(1 − cos φ) above where it started. At the rotation that brings the edge to perpendicular, ΔZ is the sagitta — the same 1.5 mm you measured to characterise the part. This is the sagitta re-read rather than a new result, but the consequence does not seem to be drawn anywhere: the single measurement that describes the part is also the full magnitude of its compensation. No second measurement, no lookup table, and a hard bound on the Z travel the fixture needs.

**3. Two measurements choose the method.** Sag in X and sag in Y, against a depth of field you measured rather than read off a spec sheet, select between five treatments: do nothing, Z-banding, single-axis rotary, two-axis positioning, or a probed height map. The test is in the section below. Its value is less the logic than the comparability — two owners of the same machine who run it produce numbers that mean the same thing.

**Nothing here has been measured.** Every results table in this document is blank on purpose. The three claims are derived from manufacturer specifications and geometry, and each is stated so that a single afternoon of test coupons could falsify it.

## Prior art — what this does not claim

Everything in this table is established. It is listed so a reader can see exactly where our line sits, and so nobody mistakes the background for the contribution.

| Technique                                                   | Claimed here? | Established by                                                                                                                           |
|-------------------------------------------------------------|---------------|------------------------------------------------------------------------------------------------------------------------------------------|
| Depth of field falls with the square of spot size           | No            | Standard Gaussian-beam optics                                                                                                            |
| Published DoF per F-theta focal length                      | No            | Vendors publish spot diameter per focal length, and some publish a depth of focus — sources 2 and 3                                      |
| Spot elongation at tilt θ equals 1/cos θ                    | No            | Elementary projection geometry                                                                                                           |
| Sagitta: R = w²/(8s)                                        | No            | Classical geometry                                                                                                                       |
| Z-banding — slicing a part into focus bands                 | No            | Routine in industrial deep marking                                                                                                       |
| Rotary attachments for cylindrical parts                    | No            | Ubiquitous; tumblers, rings, pens                                                                                                        |
| Three-axis dynamic focus galvo heads                        | No            | Commercial for years — a focusing unit ahead of the scan head that moves the focus along the beam, source 4; entry price around \$10,000 |
| CNC height mapping by probe grid and bilinear interpolation | No            | Standard in PCB isolation milling; built into Candle, bCNC and others via G38.2                                                          |
| Greyscale depth maps driving MOPA relief marking            | No            | Standard feature in EZCAD and LightBurn, 256 levels at a single focal plane                                                              |

One determination from this work is a verified fact rather than a new one: the WeCreat Lumos Ultra has **no dynamic 3D focus**. It is a two-axis galvo with swappable F-theta lenses, a single-point camera rangefinder, manual two-dot focusing, and greyscale relief confined to one plane. The reliable way to tell a genuine three-axis head from a 2.5D one is a published **focus range in millimetres**; an interchangeable F-theta lens does not settle it either way.

Two cautions that follow, and that we would want anyone reproducing this to observe:

- **Do not compute depth of field from a published spot-size figure.** A quoted 0.0019 mm UV spot is a best-case diffraction number and will produce a DoF estimate that is wrong by a large factor. Measure it.

- **A probed height map needs a conductive surface** if the probe is electrical. Brass and steel qualify; anodised, coated or painted stock may not.

## Finding 1 — tilt is a red herring on shallow curves

For a gently curved part, surface tilt costs a fraction of a percent while defocus costs more than a hundred percent, so a fixture that tilts without also correcting Z solves the wrong problem.

Two independent errors arise when a curved surface is marked on a flat-field machine. Tilt turns the circular spot into an ellipse, lengthened along the direction of tilt:

$$
\text{elongation} = \frac{1}{\cos\theta}
$$

Defocus grows the spot in both directions at once, following the Gaussian beam, where z is the distance off the focal plane and z_R the Rayleigh range — which is what a depth-of-field figure is really quoting:

$$
\frac{w(z)}{w_0} = \sqrt{1 + \left(\frac{z}{z_R}\right)^{2}}
$$

For a circular-arc part of chord width w and sag s:

$$
R = \frac{w^{2}}{8s}, \qquad \theta_{\max} = \arcsin\!\left(\frac{w/2}{R}\right)
$$

On an 80 mm buckle with 1.5 mm of sag, that gives R = 533 mm and an edge tilt of 4.3°. Taking z_R = 0.66 mm, the value a 30 µm spot implies:

| Error source                                       | Magnitude on this part | Effect on the spot         |
|----------------------------------------------------|------------------------|----------------------------|
| Surface tilt at the edge                           | θ = 4.3°               | 1.003× — +0.3 % elongation |
| Defocus, focal plane set at the part centre        | 1.5 mm, or 2.3 × z_R   | 2.5× diameter              |
| Defocus, focal plane split between centre and edge | 0.75 mm, or 1.1 × z_R  | 1.5× diameter              |

The 0.66 mm Rayleigh range is derived from a specified spot size, not measured. It is the single number in this document most in need of a real measurement, which is why the procedure below starts there. One vendor's published working figure for a comparable spot is about five times tighter; that would make the defocus penalty larger still, so the direction of this finding does not change, only its size.

**When does tilt start to matter?** Substituting R into θ collapses the geometry to a single ratio:

$$
\sin\theta_{\max} = \frac{4s}{w}
$$

Absolute size drops out — only the sag-to-width ratio matters. Tilt reaches 10°, where elongation first passes 1.5 %, at s/w ≈ 0.043; it reaches 15°, and 3.5 % elongation, at s/w ≈ 0.065. So **tilt is worth correcting only once sag exceeds roughly 4 % of the part's width.** An 80 mm part would need 3.5 mm of sag to get there. Belt buckles, challenge coins and tin lids are not close. Spoons, bowls, bottle shoulders and ring interiors are.

The same threshold governs the CNC side: below about 10° of surface tilt, a V-bit's groove is near enough symmetric that carving to a probed height map needs no tilt compensation. Past 15° the groove walls become visibly unequal and a height map alone stops being sufficient.

## Finding 2 — the Z correction is bounded by the sag you already measured

If a rotary fixture holds the part about the part's own centre rather than the centre of its curvature, the Z offset needed at each rotation is R(1 − cos φ), and its maximum is exactly the part's sag. One measurement describes the part and sizes its own correction.

Rotate the part by φ to bring a patch at distance y from the centreline perpendicular to the beam. That patch arrives at a height above where it started:

$$
\Delta Z = R\,(1 - \cos\varphi), \qquad \varphi = \arcsin\!\left(\frac{y}{R}\right)
$$

At the rotation that brings the part's edge to perpendicular, y = w/2 and the offset is the sagitta itself:

$$
\Delta Z_{\max} = R\,(1 - \cos\varphi_{\max}) = s
$$

This is the sagitta recognised rather than discovered — it is true by the definition of a circular arc. What does not appear to be written down is the practical consequence. The number you measure once with a straight edge and a feeler gauge is simultaneously the characterisation of the part, the full travel the Z stage must provide, and the worst-case error if you skip the correction entirely. There is no second measurement and no calibration table.

**Read Finding 1 and Finding 2 together and the design changes.** Below about 4 % sag-to-width, the rotation buys nothing measurable — the tilt it corrects was never the problem. What the part needs is the Z schedule alone: a motorised Z stage stepping through bands, with the part held flat. The rotary is the expensive half of the fixture and, in this regime, the half that does nothing. Above 4 %, both halves earn their place, and the equation above supplies the Z schedule for the rotation for free.

**The schedule is machine-agnostic.** Nothing in it refers to a laser. A fourth-axis rotary on a CNC, carrying the same part, takes the same φ and the same ΔZ. One measured sag produces one table that drives marking on the laser and carving on the mill, which is what would let a single fixture design serve both machines.

**Double curvature, unverified.** A buckle curves in X and Y. Two stacked rotations should, to first order, add their offsets:

$$
\Delta Z_{\text{total}} \approx R_x(1-\cos\varphi_x) + R_y(1-\cos\varphi_y) \;\le\; s_x + s_y
$$

The bound — total Z travel never exceeds the sum of the two sags — follows from the same argument applied twice, but the cross-term has not been worked through and the approximation has not been tested. Treat the inequality as a design budget, not a result.

## Finding 3 — two measurements choose the method

Sag in X, sag in Y, and a depth of field you measured rather than read off a spec sheet are sufficient to select between doing nothing, Z-banding, a single-axis rotary, a two-axis positioner, and a probed height map.

![Decision test: three tests, four treatments](figures/decision-tree.png)

decision test · 3 tests, 4 treatments

The second test is the one that does the work. A part that fails the first test still usually passes the second, which routes it to Z-banding with the part held flat — the cheap treatment — rather than to a fixture that tilts. Only parts with sag above about 4 % of their width reach the lower half of the tree.

The contribution here is less the logic than the comparability. Each test resolves against a number anyone can produce with a straight edge, a feeler gauge and a strip of test coupons, so two owners of the same machine who run the protocol produce figures that mean the same thing. A single data set from one bench is an anecdote; a protocol that others can run is a result that accumulates.

## How to take the two measurements

Sag needs a straight edge and feeler gauges. Depth of field needs a row of test marks and one prescribed threshold. Everything else in the protocol is computed from those.

### Sag

Lay a straight edge across the part and measure the gap at the centre with feeler gauges or a dial indicator. Record the chord width w you spanned and the gap s. Do it twice, once along X and once along Y. A straight edge resolves to about 0.05 mm, which is an order of magnitude finer than the depth of field it will be compared against, so a structured-light scan is not needed and adds alignment error for no gain.

**Measure sag across the engraved area, not the part.** This is the step most likely to be done wrong, and getting it right is itself a correction method. With the radius fixed, sag falls with the square of the span:

$$
s(w') = s \left(\frac{w'}{w}\right)^{2}
$$

Shrinking a design from 80 mm to 56 mm — 70 % of the width, which most people would guess costs 30 % of the sag — drops it from 1.50 mm to 0.74 mm. That is very likely inside the depth of field, which means the artwork change alone moves the part from the bottom of the decision tree to the top. **The cheapest correction is a smaller design, and the quadratic makes it far more effective than it looks.**

### Depth of field

Sag is a property of the part. Depth of field is a property of the machine, and it has to be measured because published spot sizes are best-case diffraction figures.

1.  Fix every marking parameter at the settings you actually use — power, speed, frequency, pulse width, line spacing — and change nothing but Z.

2.  On a flat coupon of the real material, mark a row of identical short lines or small filled squares.

3.  Step Z by a fixed amount between marks: 0.25 mm is a good step, out to ±2.0 mm or until the mark plainly fails.

4.  Measure the width of each mark under a loupe or a microscope, or scan the coupon and measure in software.

5.  **Depth of field is the ± range over which mark width stays within 1.41× its minimum.** This threshold is the Rayleigh criterion. It is stated here as a rule because without a fixed criterion two people measuring the same machine will report different numbers, and comparability is the point.

Repeat for each combination you care about: source, lens, and material. Brass and stainless behave differently and a result from one does not transfer.

**A known confound, stated so nobody is surprised by it.** Mark formation on metal is a threshold process — below a certain fluence nothing marks at all. The Z at which a mark visibly fails is therefore not necessarily the Z at which the spot has grown by 1.41×, and the discrepancy will vary with power. Measuring mark width rather than judging pass or fail reduces this, but does not remove it. Anyone comparing results should record the power setting alongside the depth of field.

**Why the published figures cannot be used.** Vendors quote spot diameter freely and depth of field rarely, and when both appear they disagree by a large factor. The theoretical Rayleigh range follows from the spot diameter alone:

$$
z_R = \frac{\pi w_0^{2}}{\lambda}, \qquad w_0 = \tfrac{1}{2}\,(\text{spot diameter})
$$

Applied to one vendor's published spot sizes at 1064 nm, with the right-hand column computed here rather than quoted:

| Lens  | Working field | Spot diameter (quoted) | Rayleigh range at 1064 nm (computed) |
|-------|---------------|------------------------|--------------------------------------|
| F-100 | 70 × 70 mm    | ~27 µm                 | ±0.54 mm                             |
| F-160 | 120 × 120 mm  | ~45 µm                 | ±1.50 mm                             |
| F-254 | 190 × 190 mm  | ~68 µm                 | ±3.41 mm                             |
| F-330 | 240 × 240 mm  | ~88 µm                 | ±5.72 mm                             |
| F-420 | 310 × 310 mm  | ~112 µm                | ±9.26 mm                             |

A second vendor publishes a depth of focus directly: 0.2 mm for a 26 µm spot and 0.7 mm for a 47 µm spot. Those two numbers scale with the square of spot size to within 7 %, exactly as the model requires — so the physics is not in dispute. But they are around five times tighter than the Rayleigh range for the same spots. The vendor is quoting a practical working tolerance; the formula gives the optical one; neither is wrong and they are not the same quantity.

That factor of five is the argument for measuring. A depth-of-field number without its criterion attached is not usable, which is why step 5 fixes the criterion rather than leaving it to judgement.

### Tilt

Tilt is not measured. It is computed from the same two numbers using sin θ = 4s/w, and compared against the 4 % threshold.

## Results tables, blank

Copy these and fill them in. The first row of each is the worked example from this document — computed, never measured — so the format and units are unambiguous.

### Table 1 — part geometry

| Part                                | Axis | Span w (mm) | Sag s (mm) | s / w | R = w²/8s (mm) | θₘₐₓ = asin(4s/w) | Tilt matters? |
|-------------------------------------|------|-------------|------------|-------|----------------|-------------------|---------------|
| Belt buckle (example, not measured) | X    | 80.0        | 1.50       | 0.019 | 533            | 4.3°              | no            |
|                                     |      |             |            |       |                |                   |               |
|                                     |      |             |            |       |                |                   |               |
|                                     |      |             |            |       |                |                   |               |
|                                     |      |             |            |       |                |                   |               |

Span is the width you will actually engrave, not the width of the part.

### Table 2 — defocus ladder

Mark width in µm at each Z offset, all other parameters fixed. Record the power and speed used, since the threshold confound makes results power-dependent.

| Z offset (mm)                     | Brass · MOPA | Brass · UV | Stainless · MOPA | Stainless · UV |
|-----------------------------------|--------------|------------|------------------|----------------|
| −2.00                             |              |            |                  |                |
| −1.50                             |              |            |                  |                |
| −1.00                             |              |            |                  |                |
| −0.50                             |              |            |                  |                |
| 0.00                              |              |            |                  |                |
| +0.50                             |              |            |                  |                |
| +1.00                             |              |            |                  |                |
| +1.50                             |              |            |                  |                |
| +2.00                             |              |            |                  |                |
| **Settings used**                 |              |            |                  |                |
| Lens focal length (mm)            |              |            |                  |                |
| Power (%) and speed (mm/s)        |              |            |                  |                |
| Frequency (kHz), pulse width (ns) |              |            |                  |                |

Use 0.25 mm steps across the central millimetre if the 0.50 mm steps leave the minimum ambiguous.

### Table 3 — derived

| Source · lens · material   | DoF ± (mm), at 1.41× minimum width | Part        | Sag (mm) | bands = ceil( sag ÷ 2·DoF ) | Treatment from the tree |
|----------------------------|------------------------------------|-------------|----------|-----------------------------|-------------------------|
| MOPA · — · brass (example) | 0.66, derived not measured         | Belt buckle | 1.50     | 2                           | Z-banding only          |
|                            |                                    |             |          |                             |                         |
|                            |                                    |             |          |                             |                         |
|                            |                                    |             |          |                             |                         |

### Table 4 — rotate-and-drop schedule

Only needed for parts that reach the lower half of the tree. Values shown are the worked example at R = 533 mm.

| y from centre (mm) | φ = asin(y/R) | ΔZ = R(1 − cos φ) (mm) |
|--------------------|---------------|------------------------|
| 0                  | 0.00°         | 0.000                  |
| 10                 | 1.07°         | 0.094                  |
| 20                 | 2.15°         | 0.375                  |
| 30                 | 3.23°         | 0.844                  |
| 40                 | 4.30°         | 1.502                  |

The last row returns the sag, as Finding 2 requires. A schedule that does not is a sign the part was chucked about the wrong centre.

## What would falsify each claim

Each claim is stated so that a single coupon can kill it. That is deliberate: the value of the protocol is that it is cheap to disprove.

**Finding 1 fails** if a coupon held at 4.3° but kept in focus marks measurably worse than a flat coupon in focus. Two coupons, same settings, compare mark width and edge quality. If the tilted one is visibly worse, the 1/cos θ model is not capturing what matters.

> **A limitation we know about.** The 1/cos θ argument is about spot *geometry* only. It does not model the change in absorptivity with angle of incidence, which on polished metal rises as the angle grows. At 4.3° this is almost certainly negligible, but the claim should not be extended to steep angles on that basis — at 30° on polished brass, reflectivity effects may dominate the geometry entirely. Someone should check.

**Finding 2 fails** if the part is not a circular arc. The identity is true by definition for an arc, so the exposure is in the assumption, not the algebra. The test uses the same straight edge: measure sag at several spans and check that s scales with w². Measure at 40, 60 and 80 mm; if the 40 mm sag is not close to one quarter of the 80 mm sag, R is not constant across the part and a single-radius schedule will not hold.

**Finding 3 fails** if a part the tree routes to “engrave flat” still shows visibly degraded marks at its edges. That is one buckle and one test pattern of repeated identical marks across the full span.

### Open questions

- **The double-curvature cross-term has not been worked through.** The additive approximation, and the bound ΔZ ≤ sₓ + sᵧ, are stated as a design budget only.

- **Does a hobby-cost two-axis positioner exist?** Two stacked rotaries are heavy, lose rigidity, and may introduce more runout than the sag they correct. If Finding 1 holds, this question mostly goes away for flat-ish parts — but not for spoons, bowls or ring interiors.

- **Can affordable CAM merge a scanned surface with a greyscale depth map?** VCarve Pro cannot: it builds no relief from greyscale, combines no reliefs, and imports one STL per job. Aspire does all three at roughly three times the price. Whether a Vectric Gadget — they are Lua, and supported in VCarve Pro, and can create both geometry and toolpaths — could bridge that gap is open and, if it can, is worth more than any of the findings above.

- **Non-conductive surfaces on the CNC.** An electrical Z-probe covers bare metal. Anodised, coated and painted stock needs a mechanical touch probe, and what one costs at hobby scale is unestablished here.

- **Is 1.41× the right criterion?** It is the Rayleigh criterion for spot size. Whether mark quality degrades at the same point is an assumption, and the threshold confound above is the reason to doubt it. A different criterion would shift every depth-of-field number but not the structure of the decision tree.

### Scope

Everything here assumes a flat-field, two-axis galvo marking a part that does not move during the mark, or a three-axis router carving one. A machine with genuine dynamic three-axis focus solves this problem in hardware and needs none of it. The protocol is for people whose machines do not.

## Methods, collaboration and sources

### Status

This is a protocol. No measurement in it has been taken. Every figure is either computed from a formula shown in the text or quoted from a source listed below, and each is labelled where it appears. The worked example is an 80 mm belt buckle with an assumed 1.50 mm sag, chosen because it is the part that prompted the work, not because it was measured.

### Equipment it was developed against

A WeCreat Lumos Ultra — a two-axis galvo with MOPA and UV sources and interchangeable F-theta lenses, no dynamic focus — and a Genmitsu 3030-PROVer Ultra three-axis router running GRBL. Nothing in the protocol depends on those particular machines, but the numbers quoted as examples come from their specifications.

### Collaboration

Suggested wording, to adapt:

> This protocol was developed in conversation between Lee Gillie and Claude (Anthropic). The questions, the machines, the parts and the practical judgement are Lee's. The optical calculations, the geometric derivations and the decision framework were worked out jointly. Every figure taken from a manufacturer has been checked against the source cited, and the derived figures are marked as derived. Nothing has yet been measured on a bench. Errors are ours.

The disclosure is deliberately specific about which parts came from where, because a vague acknowledgement is worth less than none. Anyone reproducing this should be able to see exactly which claims rest on measurement, which on arithmetic, and which on a conversation.

**On listing Claude as an author rather than a contributor.** Every major style guide — and every journal and preprint server — holds that an AI system cannot be an author, on the reasoning that authorship carries accountability: an author approves the final version, answers for its errors, and can be held responsible for them. Claude can do none of those three. The practical consequence matters more than the principle here: a document that lists an AI in the byline gets dismissed on sight by precisely the readers this protocol is trying to reach, and that costs the work the thing it was written for.

The better argument for the position Lee holds — that AI involvement in technical work should be visible rather than hidden — is a contribution statement that is specific enough to be checked. A byline asserts; a statement that says which derivations came from where can be audited and survives scrutiny. It is listed in the bibliography as a tool, cited the way a style guide prescribes, and described in full above. That is a stronger claim for openness than a byline, not a weaker one.

### How to contribute numbers

Run the ladder, fill Tables 1 to 3, and publish them with the machine, the lens, the source, the material and the power setting named. A single bench's data is an anecdote. The protocol is only worth anything if several people run the same one.

### Bibliography

All URLs accessed 4 October 2026. Figures taken from these sources are manufacturer specifications, not measurements.

1.  Anthropic. *Claude* (Opus 5) [large language model]. 2026. <https://claude.ai> — the system with which this protocol was developed. See **Collaboration** above for what it contributed.

2.  Trotec Laser. "Galvo focus lenses." Product specifications. <https://www.troteclaser.com/en-gb/laser-machines/laser-accessories/galvo-focus-lenses> — spot diameter and working field for F-100 through F-420 at 1064 nm. Source of the quoted spot sizes in the computed Rayleigh-range table; the depths of field in that table are computed here, not taken from Trotec.

3.  Thunder Laser. "Laser Marking F-Theta Lenses: Everything You Need to Know." Laser Wiki. <https://thunderlaser.com/laser-wiki/machine-and-component/laser-marking-machine-f-theta-lens.html> — publishes a depth of focus alongside spot diameter (0.2 mm at 26 µm, 0.7 mm at 47 µm). The pair scales as spot size squared, confirming the model; their absolute values are roughly five times tighter than the Rayleigh range, which is the discrepancy that motivates measuring rather than looking up.

4.  SCANLAB GmbH. *varioSCAN II / varioSCANde II focusing systems.* Product datasheet. <https://www.scanlab.de/sites/default/files/2020-07/10_varioSCAN%2BvarioSCANde_focusing%20systems.pdf> — a focusing unit that positions the laser focus along the beam direction, extending a two-axis scanning system to three-axis processing and able to replace a flat-field objective. The commercial answer to the problem this protocol works around.

5.  Full Spectrum Laser. "2D vs. 3D Galvo Lasers." <https://fslaser.com/blog/2d-vs-3d-galvo-lasers> — plain-language statement of the difference between a flat-field two-axis head with an F-theta lens and one with dynamic Z-axis focus control for curved, angled or stepped surfaces.

6.  SainSmart. "How to utilize height mapping in Candle." Documentation. <https://docs.sainsmart.com/article/kj4xzak19j-how-to-utilize-height-mapping-in-candle> — the vendor procedure for probing a height map on a Genmitsu machine. The reference for the CNC branch of the decision tree.

7.  Denvi. *Candle* — GRBL controller application with G-code visualiser [software]. <https://github.com/Denvi/Candle> — the sender in which height-map probing and compensation are implemented.

8.  whitman.tech. "PCB height probe code." <https://whitman.tech/ferrouscnc/PcbHeightProbeCode.aspx> — a worked G38.2 probe-grid implementation for PCB milling, the closest existing analogue to the CNC branch.

### How to cite this document

> Gillie, L. (2026). *Measuring before engraving: a protocol for curved metal surfaces* [protocol]. Developed in collaboration with Claude (Anthropic). Version 0.1, unverified — no measurements taken.

The version number and the words "unverified — no measurements taken" belong in the citation until Tables 1 to 3 have real numbers in them. Once they do, the version increments and that clause comes out.
