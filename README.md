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
──
├── 𝘴𝘵𝘳𝘶𝘤𝘵𝘶𝘳𝘦𝘴/              # 𝘈𝘭𝘱𝘩𝘢𝘍𝘰𝘭𝘥3 𝘰𝘶𝘵𝘱𝘶𝘵 𝘴𝘵𝘳𝘶𝘤𝘵𝘶𝘳𝘦𝘴 (.𝘤𝘪𝘧) 𝘢𝘯𝘥 𝘱𝘳𝘰𝘤𝘦𝘴𝘴𝘦𝘥 𝘥𝘪𝘮𝘦𝘳 𝘗𝘋𝘉𝘴
├── 𝘢𝘭𝘱𝘩𝘢𝘧𝘰𝘭𝘥_𝘱𝘳𝘦𝘥𝘪𝘤𝘵𝘪𝘰𝘯𝘴/    # 𝘈𝘭𝘱𝘩𝘢𝘍𝘰𝘭𝘥 𝘤𝘰𝘯𝘧𝘪𝘥𝘦𝘯𝘤𝘦 (𝘱𝘓𝘋𝘋𝘛) 𝘢𝘯𝘥 𝘤𝘩𝘢𝘪𝘯-𝘤𝘰𝘭𝘰𝘳𝘦𝘥 𝘷𝘪𝘴𝘶𝘢𝘭𝘪𝘻𝘢𝘵𝘪𝘰𝘯𝘴
├── 𝘴𝘪𝘮𝘶𝘭𝘢𝘵𝘪𝘰𝘯𝘴/              # 𝘙𝘢𝘸 𝘢𝘯𝘢𝘭𝘺𝘴𝘪𝘴 𝘰𝘶𝘵𝘱𝘶𝘵 (.𝘹𝘷𝘨) 𝘱𝘦𝘳 𝘱𝘳𝘰𝘵𝘦𝘪𝘯, 𝘱𝘦𝘳 𝘤𝘰𝘯𝘥𝘪𝘵𝘪𝘰𝘯
│   ├── 𝘭𝘦𝘨𝘶𝘮𝘪𝘯/
│   └── 𝘷𝘪𝘤𝘪𝘭𝘪𝘯/
├── 𝘢𝘯𝘢𝘭𝘺𝘴𝘪𝘴/
│   ├── 𝘪𝘯𝘥𝘪𝘷𝘪𝘥𝘶𝘢𝘭_𝘱𝘭𝘰𝘵𝘴/     # 𝘗𝘦𝘳-𝘱𝘳𝘰𝘵𝘦𝘪𝘯, 𝘱𝘦𝘳-𝘮𝘦𝘵𝘳𝘪𝘤 𝘱𝘭𝘰𝘵𝘴
│   └── 𝘤𝘰𝘮𝘱𝘢𝘳𝘪𝘴𝘰𝘯_𝘱𝘭𝘰𝘵𝘴/     # 𝘋𝘪𝘳𝘦𝘤𝘵 𝘓𝘦𝘨𝘶𝘮𝘪𝘯-𝘷𝘴-𝘝𝘪𝘤𝘪𝘭𝘪𝘯 𝘤𝘰𝘮𝘱𝘢𝘳𝘪𝘴𝘰𝘯 𝘱𝘭𝘰𝘵𝘴 𝘱𝘦𝘳 𝘤𝘰𝘯𝘥𝘪𝘵𝘪𝘰𝘯
├── 𝘮𝘰𝘷𝘪𝘦𝘴/                    # 𝘙𝘦𝘯𝘥𝘦𝘳𝘦𝘥 𝘔𝘋 𝘵𝘳𝘢𝘫𝘦𝘤𝘵𝘰𝘳𝘺 𝘷𝘪𝘴𝘶𝘢𝘭𝘪𝘻𝘢𝘵𝘪𝘰𝘯𝘴 (𝘗𝘺𝘔𝘖𝘓)
├── 𝘯𝘰𝘵𝘦𝘣𝘰𝘰𝘬𝘴/                 # 𝘍𝘶𝘭𝘭 𝘢𝘯𝘢𝘭𝘺𝘴𝘪𝘴 𝘱𝘪𝘱𝘦𝘭𝘪𝘯𝘦 (𝘊𝘰𝘭𝘢𝘣-𝘦𝘹𝘦𝘤𝘶𝘵𝘦𝘥)
└── 𝘥𝘰𝘤𝘴/
    └── 𝘮𝘦𝘵𝘩𝘰𝘥𝘴.𝘮𝘥             # 𝘋𝘦𝘵𝘢𝘪𝘭𝘦𝘥 𝘮𝘦𝘵𝘩𝘰𝘥𝘰𝘭𝘰𝘨𝘺, 𝘴𝘰𝘧𝘵𝘸𝘢𝘳𝘦 𝘷𝘦𝘳𝘴𝘪𝘰𝘯𝘴, 𝘱𝘢𝘳𝘢𝘮𝘦𝘵𝘦𝘳𝘴
   
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
