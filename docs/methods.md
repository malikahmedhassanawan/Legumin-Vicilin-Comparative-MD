# Methods

Technical reference for the Legumin-Vicilin-Comparative-MD pipeline. For motivation, results, and interpretation, see the main [README](../README.md).

## 1. Structure Prediction

| Protein | UniProt ID | pTM | ipTM |
|---|---|---|---|
| Pea legumin (homodimer) | P02857 | 0.69 | 0.62 |
| Pea vicilin (homodimer) | Q702P1 | 0.76 | 0.67 |

- Tool: AlphaFold3
- Legumin's pTM/ipTM is consistent with its known hexameric biological assembly; vicilin's higher confidence reflects a more straightforward homodimer interface.
- Output: `.cif` structures in `structures/`, confidence and chain-colored visualizations in `alphafold_predictions/`.

## 2. Structure Preparation

- Tool: PyMOL (desktop application, run locally — not part of the Colab pipeline)
- AlphaFold3 homodimer predictions were exported as `.cif` and converted to PDB format in PyMOL, producing `legumin_legumin.pdb` and `vicilin_dimer.pdb` (the files the Colab notebooks load directly into `gmx pdb2gmx`)
- Chains were labeled A/B during this step, consistent with the two-chain PDBs used throughout the GROMACS pipeline
- 3D structure renders were generated in PyMOL at this stage, colored by chain and separately by AlphaFold3 confidence (pLDDT); these are the images in `alphafold_predictions/`
- Chain coloring convention used for downstream comparative plots: **red = legumin, lightblue = vicilin**

## 3. System Setup (GROMACS)

- GROMACS version: 2026.3 (conda-forge/bioconda, CUDA-enabled build), installed via `mamba install -c conda-forge -c bioconda gromacs=2026.3`
- Executable: `gmx` (GPU runs on the legumin system occasionally required `gmx_mpi` and an explicit `LD_LIBRARY_PATH=/usr/lib64-nvidia` to expose the GPU correctly on Colab)
- Force field: AMBER99SB-ILDN (`-ff amber99sb-ildn`)
- Water model: SPC/E (`-water spce`), solvated with `spc216.gro`
- Box: cubic, minimum 1.0 nm from protein to box edge (`gmx editconf -c -d 1.0 -bt cubic`)
- Ions: NaCl (`-pname NA -nname CL`), system charge-neutralized (`-neutral`); the legumin system additionally set bulk ionic strength to 0.15 M (`-conc 0.15`), while vicilin used neutralization only — see Limitations
- Energy minimization: steepest descent (`integrator = steep`), `emtol = 1000.0` kJ/mol/nm, `emstep = 0.01`, max `nsteps = 50000`
- Equilibration:
  - **NVT** (100 ps: 50,000 steps × 0.002 ps): position-restrained (`define = -DPOSRES`), LINCS constraints on h-bonds, PME electrostatics, V-rescale thermostat (`tau_t = 0.1`) at the run's target temperature (300K or 400K), velocities generated from a Maxwell distribution at that temperature
  - **NPT** (100 ps: 50,000 steps × 0.002 ps): continued from NVT with restraints retained, target pressure 1.0 bar, `tau_p = 2.0`, compressibility `4.5e-5`. Barostat differed between the two proteins: vicilin used C-rescale, legumin used Parrinello-Rahman (see Limitations)

Compute environment: Google Colab, T4 GPU (free tier). Nonbonded, PME, and bonded interactions were offloaded to GPU (`-nb gpu -pme gpu -bonded gpu`) where supported by the build in use.

## 4. Production Runs

4 conditions per protein, 8 total. Integration timestep `dt = 0.002` ps throughout; PME electrostatics, LINCS h-bond constraints, V-rescale thermostat, `refcoord_scaling = com`, `gen_vel = no` (velocities carried over from NPT).

| Protein | Temperature | Duration | Steps |
|---|---|---|---|
| Legumin | 300K | 200ps | 100,000 |
| Legumin | 300K | 500ps | 200ps run extended +300ps (`gmx convert-tpr -extend 300`) |
| Legumin | 400K | 200ps | 100,000 |
| Legumin | 400K | 500ps | 200ps run extended +300ps |
| Vicilin | 300K | 200ps | 100,000 |
| Vicilin | 300K | 500ps | 200ps run extended +300ps |
| Vicilin | 400K | 200ps | 100,000 |
| Vicilin | 400K | 500ps | 200ps run extended +300ps |

Each 500ps trajectory is a continuation of its corresponding 200ps run, not an independent run — extended via `gmx convert-tpr` and restarted from the checkpoint (`-cpi`), rather than launched fresh at 500ps.

MDP parameter files for both proteins, all four conditions, are in `simulation_setup/`.

## 5. Trajectory Analysis

| Metric | GROMACS command |
|---|---|
| RMSD | `gmx rms` |
| Radius of gyration (Rg) | `gmx gyrate` |
| Solvent-accessible surface area (SASA) | `gmx sasa` |
| Hydrogen bonds | `gmx hbond` |
| Per-residue flexibility (RMSF) | `gmx rmsf` (chain-split) |

- Raw output (`.xvg`) per protein, per condition: `simulations/`
- Plots generated with Python/matplotlib: `analysis/legumin/`, `analysis/vicilin/`
- Head-to-head comparison plots (normalized Rg/SASA overlays) and `summary_table.csv`: `analysis/comparison/legumin_vs_vicilin/`

## 6. Trajectory Visualization

- Tool: PyMOL
- Rendered MD movies (H-bond visualization, chain coloring per convention above): `movies/`

## 7. Key Quantitative Result

Legumin at 400K/500ps, relative to all other conditions tested:

- ΔRg: −1.09% (compaction) vs. +0.96% to +1.96% (expansion) elsewhere
- ΔSASA: −2.27% (buried surface) vs. +0.02% to +1.32% (exposure) elsewhere
- ΔH-bonds: +3 (net gain) vs. −10 to −48 (net loss) elsewhere

Localized to the 260–320 loop region, with asymmetric behavior between chain A and chain B. Not observed in vicilin under any condition. Full numeric summary: `analysis/comparison/legumin_vs_vicilin/summary_table.csv`.

## 8. Limitations

- Single production run per condition — not replicated
- Short timescales (200–500ps) relative to the biological processes modeled; each 500ps trajectory is an extension of its 200ps run rather than an independently seeded replicate
- Homodimer models, not the full native oligomeric assemblies (legumin is hexameric; vicilin is trimeric in its native state)
- The two proteins' equilibration protocols were not fully identical: vicilin was ion-neutralized only, legumin was additionally set to 0.15 M ionic strength; vicilin's NPT step used the C-rescale barostat, legumin's used Parrinello-Rahman. Both reach the same target temperature/pressure conditions, but this cross-protein asymmetry should be kept in mind when comparing absolute (not just relative) values between the two proteins
- Findings should be treated as hypothesis-generating, pending validation by replicate runs and experimental methods (e.g. DSC, circular dichroism)

## 9. Reproducing This Work

Full pipeline, Colab-executed: `notebooks/`

1. Structure prediction and preprocessing
2. GROMACS simulation setup and production runs
3. Trajectory analysis and comparative visualization

All commands, exact invocations, and software versions are documented above; raw and cleaned notebooks preserve the full execution history.
