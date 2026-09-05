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

## Key Finding

At 400K over the 500ps trajectory, **legumin exhibits a compaction signature** — radius of gyration and SASA both decrease while hydrogen bond count increases after ~250–300ps — concentrated in the 260–320 residue loop region, with clear chain-to-chain asymmetry between the two monomers. This suggests a thermally-induced conformational tightening localized to a specific structural region rather than a uniform global response, a detail relevant to understanding thermal processing effects on this protein's functional behavior.

## Repository Structure
── structures/              # AlphaFold3 output structures (.cif) and processed dimer PDBs
├── alphafold_predictions/    # AlphaFold confidence (pLDDT) and chain-colored visualizations
├── simulations/              # Raw analysis output (.xvg) per protein, per condition
│   ├── legumin/
│   └── vicilin/
├── analysis/
│   ├── individual_plots/     # Per-protein, per-metric plots
│   └── comparison_plots/     # Direct Legumin-vs-Vicilin comparison plots per condition
├── movies/                    # Rendered MD trajectory visualizations (PyMOL)
├── notebooks/                 # Full analysis pipeline (Colab-executed)
└── docs/
    └── methods.md             # Detailed methodology, software versions, parameters
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
