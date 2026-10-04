# Contributing

The contribution this repository wants is **numbers**. A single bench's data is an anecdote; the same procedure run on six machines is a result.

You do not need to write anything up. Nine values and four lines of settings is a complete contribution.

## The fastest useful thing: run the defocus ladder

About an hour, including setup.

1. Fix every marking parameter at the settings you actually use — power, speed, frequency, pulse width, line spacing. Change **nothing but Z**.
2. On a flat coupon of real material, mark a row of identical short lines or small filled squares.
3. Step Z by 0.50 mm between marks, from −2.00 mm to +2.00 mm. Nine marks.
4. Measure the width of each mark under a loupe or microscope, or scan the coupon and measure in software. Record in µm.
5. Your depth of field is the ± range over which mark width stays within **1.41×** its minimum.

Then [open an issue using the "Defocus ladder result" template](../../issues/new?template=defocus-ladder.yml), or copy [`results/TEMPLATE.csv`](results/TEMPLATE.csv), fill it in, and open a pull request adding it to `results/`.

**Record the power setting.** Mark formation on metal is a threshold process, so the Z at which a mark visibly fails is not necessarily the Z at which the spot has grown 1.41×, and the gap between them varies with power. A ladder without its power setting cannot be compared with anyone else's.

## Also valuable

- **Sag measurements.** A straight edge and feeler gauges across a part, in X and in Y, with the span you measured across. Thirty seconds per part. Table 1 in the protocol.
- **A falsification.** Each of the three claims has a named test in [PROTOCOL.md](PROTOCOL.md#what-would-falsify-each-claim). If one fails on your bench, that is the single most valuable thing anyone can contribute, and it will be credited as such.
- **A correction.** If the optics or the geometry is wrong, say so in an issue. Show the working if you can; point at the error if you cannot.
- **An answer to an open question.** The protocol lists five. Two of them — whether a Vectric Gadget could merge a scanned surface with a greyscale depth map, and whether absorptivity-versus-incidence-angle swamps the geometry at steep tilts — are probably already known to somebody.

## What is not needed

Please do not open PRs that reword the prose, restructure the document, or add material that is not backed by a measurement or a citation. The document is deliberately narrow and says what it does not claim.

## House rules for data

- **Measure mark width; do not judge pass/fail.** A number someone else can plot beats a verdict.
- **One machine, one lens, one source, one material per ladder.** Brass and stainless do not transfer, and neither do two lenses.
- **State what you did not do.** "I eyeballed it under a 10× loupe" is a usable contribution and an honest one. A figure presented as more precise than it is costs more than it gives.
- **Raw numbers, not conclusions.** Put the widths in; let the analysis live in the document.

## Credit

Anyone contributing a complete ladder is listed in the results file and in the protocol's acknowledgements, with their machine named. If your data overturns a claim, that gets said plainly in the finding itself.
