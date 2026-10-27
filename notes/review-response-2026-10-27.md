# Response to the Codex manuscript review (27 Oct 2026)

Source: `claude_fsi_manuscript_review.md`. Verdict: accepted almost in full. The
code-level claims were checked against `Chanikya-Valeti/solids4foam` at commit
`882cf07` before accepting them.

## Verified against the pinned commit

| Claim | Checked | Result |
|---|---|---|
| `deltaT 0.001`, `writeInterval 0.01`, `writePrecision 8` | `benchmark1/system/controlDict` | confirmed |
| SNES/KSP/field-split/BoomerAMG options | `system/fvSolution.pressure` | confirmed, all values as listed |
| `snes_max_linear_solve_fail 100000`, no SNES max iterations set | same | confirmed |
| Viscous `Edot` is a one-step difference of `E`, not BDF2 | `viscoActiveHolzapfelOgdenMechanicalConstitutiveLaw.C` l.390–399 | confirmed |
| Scalar tangent `a + (af+as+afs)/6` = 3587.17 Pa, excludes `eta/dt` | same, l.143–148, 416–426 | confirmed |
| Draft's "current tutorial Δt = 5e-5 s" | the 5e-5 s is from the *FSI* `leftVentricle` tutorial, not the solid case | both now stated with their provenance |

## Accepted and applied to the template

1. **Cap/rim kinematics** (review 1.1): now an explicit `[AUTHOR DECISION]` with three
   options in §2.1; the solid problem is called AR25's only if the base is untouched.
2. **Meaning of the solid-only comparison** (1.2): my earlier wording ("isolates the effect of
   fluid inertia and viscosity") was too strong. Replaced by the three-way distinction
   (cross-code comparison / FSI verification / physical-model comparison) in §2.6 and §6.
3. **Robin parameters** (1.3): `robinPressureReference` / `robinFluxReferenceVelocity` are
   residual-normalisation floors, not Robin coefficients. Corrected in §3.4 and Table 5;
   the implemented Robin equation is a `[CODE CHECK]`.
4. **Residual and failure policy** (1.4): placeholder residual labelled as such; success in the
   robustness map = completed + every step converged + conservation + mesh quality.
5. **Waveforms and ICs** (1.5): activation and pressure ODEs, parameters, initial values and
   the tabulation procedure written out in §2.3; bidirectional open ports stated; net vs gross
   volume as QoIs; "AR25 Table 4" disambiguated from local tables.
6. **Interface tractions** (§2): outward normals, Nanson, sign of pressure stated.
7. **Solid law** (§2): invariants, energy, logistic switch, `U'(J)`, near-incompressibility,
   `J−1` as QoI.
8. **Volume and mass** (§2): closed-surface volume, `R_V(t)` ALE mass residual for all problems.
9. **Energy / GCL** (§4): full boundary identity; GCL as a separate compatible test.
10. **GCI** (§4): fine-to-coarse indexing, Celik unequal-ratio procedure, convergence
    classification, effective ratio 1.88 for M1→M2, solid/fluid refined separately.
11. **QoIs** (§2.7): region-averaged pressures, peak + time, fixed-time values, λ2 vs Q
    notation, RED reproduced verbatim with `[CODE CHECK]` on the zero denominator.
12. **Literature** (§1): prevalence claims removed until the survey exists; Monteleone 2026 and
    Hadar 2026 added with `[REFERENCE CHECK]`; "no equivalent exists" bounded.
13. **"Dilated"** removed; 168 mL described as an unloaded numerical geometry.
14. **Tense**: abstract, contributions and conclusions in "we define / to be released".
15. **Editorial**: helix angle renamed φ to free α_epi; "section Appendix A" fixed; port
    centres given as (y, z) mm; executable vs toolbox naming; both rim planes in Table 2.
16. **New subsection 3.1** "From the solid benchmark to the coupled problem" (Table 4),
    coupling-algorithm box (Table 5 in §3.4), settings manifest (§3.6).
17. Staged run plan and cost estimate (§4.9).

## Not adopted, or qualified

- **Title with "finite-volume"**: not adopted. The review itself notes the problem is not
  FV-specific; the current title is kept.
- **Review's suggested opening paragraph**: adopted in substance, final sentence kept in
  planning tense until results exist.
- **Sensitivity ranges** (c_u ∈ {1,10,100,1000}, etc.): still proposals, marked `[TODO]`.
- The review is cautious about F-Q; I keep it as a labelled *extension*, not optional, because
  it is the one problem that tests the kinematically closed cavity.

## Open items for the author (one batch)

1. FSI case repository/branch/commit and production environment (OpenFOAM, PETSc, hardware).
2. Cap/rim/port decision (§2.1 options i–iii) and whether AR25 basal mechanics must be preserved.
3. Reverse-flow treatment at open pressure ports.
4. Failure policy for unconverged coupling steps.
5. Completed runs, logs and QoI data; which settings are implemented vs proposed.
6. Core scope confirmation: F-p / C-p core, F-Q extension, C-R optional.
7. Compute budget, target journal, literature-search cutoff.

## Code checks to do before production runs

- Implemented Robin equation and `hsModel secant` from source.
- Exact FSI displacement residual: norm, support, weighting, floor; whether formed from the
  unrelaxed update.
- Coupling sequence of Table 5 vs `fluidSolidInterface`; predictor; old-time field handling.
- Resolved SNES max iterations; converged reasons in logs; predictor runtime behaviour.
- Stabilisation operators written out; whether `c_p^J` affects the converged solution.
- Independent ODE reproduction of τ_max and p_max; interpolation at solver times.
- RED convention at t = 0 in cardiac-benchmark-toolkit.
- Availability of Aitken / IQN-ILS with `elasticWallPressure` at the pinned version.
