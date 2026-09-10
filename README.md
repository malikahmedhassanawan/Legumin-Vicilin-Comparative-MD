# Comparative Molecular Dynamics of Pea Legumin and Vicilin Homodimers

**Structural and dynamic characterization of two major pea seed storage proteins as a foundation for computational protein engineering toward plant-based meat alternatives.**

---

## Motivation

Global protein demand is outpacing sustainable supply, and animal agriculture remains a major driver of land use, water consumption, and greenhouse gas emissions. Plant-based meat alternatives offer a scalable path forward, but replicating the texture and functional behavior of animal muscle protein networks requires a detailed understanding of the plant proteins used as substitutes.

This project focuses on **pea legumin** (UniProt P02857) and **pea vicilin** (UniProt Q702P1) — two of the dominant storage proteins in *Pisum sativum*, a legume grown widely across South Asia, including Pakistan's Punjab region, where food security and agricultural sustainability are pressing regional concerns. Understanding how these proteins behave structurally under different thermal conditions is a first step toward rational engineering of plant-protein-based food matrices.

## Overview

This repository presents an end-to-end computational pipeline:

**AlphaFold3 structure prediction → GROMACS molecular dynamics simulation → trajectory analysis → comparative structural dynamics**

Both proteins were modeled as homodimers and simulated at two temperatures (300K, 400K) across two timescales (200ps, 500ps), yielding four independent production conditions per protein. Structural stability (RMSD), compactness (radius of gyration), solvent exposure (SASA), intramolecular hydrogen bonding, and per-residue flexibility (RMSF) were tracked throughout, then directly compared between the two proteins under matched conditions.

## Key Findings

### 1. Legumin exhibits a thermally-induced compaction signature that vicilin does not

At 400K over the 500ps trajectory, legumin shows a coordinated structural response absent
from every other condition tested — and absent from vicilin entirely:

| Metric | Legumin 400K/500ps | All other Legumin conditions | All Vicilin conditions |
|---|---|---|---|
| ΔRg (%) | **−1.09%** (compaction) | +0.96% to +1.96% (expansion) | +1.58% to +3.44% (expansion) |
| ΔSASA (%) | **−2.27%** (buried surface) | +0.02% to +1.32% (exposure) | +1.50% to +3.57% (exposure) |
| ΔH-bonds | **+3** (net gain) | −10 to −48 (net loss) | −22 to −56 (net loss) |

This is the only condition, across both proteins and all four temperature/duration
combinations, where radius of gyration and solvent-accessible surface area both *decrease*
while intramolecular hydrogen bonding *increases* — the classic signature of a folding/
compaction event rather than progressive unfolding. Every other trajectory (legumin at
300K, and vicilin at all four conditions) shows the opposite: Rg and SASA rising and
H-bond count falling, consistent with normal thermal loosening of the structure.

The compaction is not a whole-molecule effect: it is concentrated in the 260–320 residue
loop region, with visible asymmetry between chain A and chain B — one monomer tightens
more than the other, rather than both chains responding identically. This localized,
asymmetric behavior suggests a specific structural motif rather than a uniform global
response to heat.

### 2. Legumin is structurally less stable than vicilin at baseline

Across every matched condition, legumin shows meaningfully higher RMSD than vicilin
(roughly 1.5–2× higher at 300K, widening further at 400K). Legumin's RMSD nearly triples
from 300K to 400K (0.28 nm → 0.60 nm mean, 500ps), while vicilin's only doubles (0.18 nm →
0.31 nm). This indicates legumin's dimer interface is intrinsically more conformationally
flexible than vicilin's, independent of the compaction event described above — vicilin
is the more thermally robust of the two proteins under these simulation conditions.

### 3. Peak flexibility maps to different regions between the two proteins

RMSF analysis identifies the most flexible residue in each condition. Legumin's peak
flexibility clusters in the 276–310 range across all four conditions (within or adjacent
to the loop region implicated in the compaction event), while vicilin's peak flexibility
sits consistently around residue 176–185 — a distinct, temperature-invariant hotspot.
This difference in *where* flexibility concentrates, rather than just *how much* flexibility
exists, points to different structural vulnerabilities between the two storage proteins.

### Implications for plant-protein food applications

These results suggest legumin and vicilin would behave differently under the thermal
conditions relevant to high-moisture extrusion and other plant-based meat processing:
vicilin's greater conformational stability may make it more resistant to thermally-induced
structural rearrangement, while legumin's localized compaction behavior could translate to
different textural or network-forming properties during thermal processing. Validating this
functional link would require complementary experimental work beyond the scope of this
computational study.

### Limitations

These findings are based on single production-run trajectories per condition (not
replicated), relatively short timescales (200–500ps) relative to the biological processes
being modeled, and homodimer models rather than the full native oligomeric assemblies.
The compaction signature and stability differences reported here should be treated as
hypothesis-generating rather than conclusive, warranting validation via replicate
simulations and, ideally, experimental thermal analysis (e.g., DSC, circular dichroism).

## Repository Structure

- **structures/** — AlphaFold3 output structures (.cif) and processed dimer PDBs
- **alphafold_predictions/** — AlphaFold confidence (pLDDT) and chain-colored visualizations
- **simulation_setup/** - GROMACS .mdp parameter files for both proteins (complete: em, ions, nvt, npt, md × 4 conditions each) 
- **simulations/** — Raw analysis output (.xvg) per protein, per condition
- **analysis/** — Individual and comparison plots
- **movies/** — Rendered MD trajectory visualizations (PyMOL)
- **notebooks/** — Full analysis pipeline (Colab-executed)
- **docs/methods.md** — Detailed methodology, software versions, parameters

   
 ## Methods Summary

| Stage | Tool | Details |
|---|---|---|
| Structure prediction | AlphaFold3 | Homodimer models for both proteins |
| MD engine | GROMACS 2026.3 (conda-forge, CUDA) | GPU-accelerated (Colab T4) |
| Force field | AMBER99SB-ILDN | |
| Water model | SPC/E | |
| Conditions | 300K, 400K × 200ps, 500ps | 4 conditions per protein |
| Analysis | RMSD, Rg, SASA, H-bonds, RMSF (chain-split) | via `gmx rms`, `gmx gyrate`, `gmx sasa`, `gmx hbond`, `gmx rmsf` |
| Visualization | PyMOL, Matplotlib | Structure rendering and trajectory plots |

Full parameter details, exact command-line invocations, and version numbers are documented in [`docs/methods.md`](docs/methods.md).

## Reproducing This Work

All simulations were run in Google Colab using free-tier GPU resources, making this pipeline accessible without dedicated HPC infrastructure. The full notebook pipeline is provided in [`notebooks/`](notebooks/), covering:

1. Structure prediction and preprocessing
2. GROMACS simulation setup and production runs
3. Trajectory analysis and comparative visualization

## Author

**Malik Ahmed Hassan Awan**
BS (Hons) Food Science and Technology, Bahauddin Zakariya University, Multan, Pakistan
Contact: varietyhavenllc@gmail.com

---

*This work is part of an ongoing research effort connecting computational protein science to food security challenges in South Punjab, Pakistan, using locally available legume crops as a model system for sustainable protein engineering.*
