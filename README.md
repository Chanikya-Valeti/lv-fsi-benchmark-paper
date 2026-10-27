# LV FSI benchmark paper

Manuscript draft: *An open fluid–structure interaction benchmark for an idealised left
ventricle: passive filling and active contraction without valves* (Valeti & Cardiff, UCD).

An extension of Benchmark 1 of Aróstica et al. (2025), *CMAME* 435:117485, to FSI, using
solids4foam.

## Layout
```
main.tex            elsarticle driver (switch \journal / class options per target)
macros.tex          notation + \TODO{} (red) and \RES{} (orange) markers
sections/           one file per section, journal-agnostic
refs.bib            bibliography (entries need verification, see header)
figures/ tables/    put generated figures/tables here
notes/              planning: value assessment, reviewer risks, target journals
```

## Build
Overleaf: import from GitHub, set the main document to `main.tex`, use pdfLaTeX.
Locally: `latexmk -pdf main.tex`.

Search for `\TODO{`, `\RES{`, `\AUTHOR{`, `\CODECHECK{`, `\DATA{` and `\REFCHECK{` to see what is left to do; the last four are handoff markers (see `notes/review-response-2026-10-27.md`) and must not appear in a submitted version.
