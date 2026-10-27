# Planning notes: value, gaps to close, target journals

## Is the paper worth writing?
Yes, if it is framed as a **benchmark/verification paper**, not "another LV FSI simulation".

What is new:
- No fully specified, openly released LV FSI benchmark exists. Solid-only cardiac benchmarks
  (Land 2015; Aróstica 2025) and generic FSI benchmarks (Turek–Hron 2006; Hessenthaler 2017)
  exist, but none has an active, anisotropic, nearly incompressible solid enclosing a cavity.
- Direct continuity with Aróstica 2025 (CMAME): Step 0 reuses their geometry, material,
  fibres and nine-code reference data. Reviewers can check it at once.
- A hierarchy of steps (solid → prescribed-motion CFD → filling → full cycle) lets other
  codes find where they disagree.
- Systematic mesh / Δt / scheme / coupling-tolerance studies with GCI, plus a reporting checklist.

What reviewers will push on (decide early):
1. **Evidence for the gap claim.** Do a systematic survey (Table 1 in the intro) to back up the
   statement that dimensions, mesh and Δt studies and schemes go unreported. That table is the
   motivation of the paper.
2. **One code only.** A benchmark with a single solution is weaker than AR25's nine. Options:
   (a) recruit 1–3 more groups (AR25 authors: lifex/Polimi, 4C/TUM, svFSI/Columbia, Ambit);
   (b) add a second solids4foam route (e.g. a different coupling algorithm, or mixed vs.
   displacement) and call it "reference solutions + open call"; (c) aim for a follow-up
   multi-group paper. (a) gives the highest impact.
3. **Valve-free driving.** With Q prescribed on every open port, the cavity volume is imposed
   kinematically: this is the "incompressibility dilemma" (Küttler et al. 2006). Present it as a
   deliberate, hard test and explain how solids4foam handles it (Robin coupling). Consider a
   Windkessel-outlet variant so that volume/EF are outputs.
4. **Consistency with AR25.** The current `leftVentricle` FSI tutorial uses Guccione +
   electroMechanicalLaw with its own fibres. For the benchmark, use the AR25 geometry +
   viscoActiveHolzapfelOgden + AR25 fibres + Robin BCs (i.e. the `ArosticaEtAl2025` setup).
5. **AR25 LV is large** (unloaded cavity ≈ 168 mL). Fine for a benchmark, but say so.
6. Fix the activation timing relative to filling (AR25 t_sys = 0.16 s vs. filling to 0.65 s
   in the current tutorial).

## Target journals (in order of fit)
1. **International Journal for Numerical Methods in Biomedical Engineering (IJNMBE, Wiley).**
   Best fit: cardiac numerics, verification, benchmark papers (Hessenthaler 2017 FSI
   benchmark appeared here).
2. **Computer Methods in Applied Mechanics and Engineering (CMAME).** Where AR25 appeared, so
   the natural sequel. Higher bar: needs multi-code results or a strong methods contribution.
3. **Journal of Fluids and Structures / Computers & Fluids.** FSI-benchmark readership;
   good if the paper leans on coupling and verification.
4. **Biomechanics and Modeling in Mechanobiology / Journal of Biomechanical Engineering (ASME).**
   Biomech audience; JBME is a good fit if framed around V&V (ASME V&V40) and credibility.
5. **Computers in Biology and Medicine / Annals of Biomedical Engineering.** Broader reach, less
   numerical depth expected.

The template uses `elsarticle` (CMAME, Comput. Fluids, JFS, CIBM). For IJNMBE, move the
`sections/` files into the Wiley NJD template on Overleaf.

## Open inputs needed from the xenosim runs
- Port geometry (radii, centres, inflow/outflow tubes), fluid mesh counts.
- Q_in(t), Q_out(t) definitions (currently tabulated in `inletFlowRate.dat` / `outletFlowRate.dat`).
  For the benchmark these should be analytical.
- Coupling algorithm actually used (fsiProperties: fixedRelaxation, ω = 1, tol 5e-5, 30 iters).
- Δt (5e-5 s in the current controlDict), fluid time scheme (Euler at present: consider backward
  for second order).
