# Protein-Simulation-Pipeline
This repository provides a comprehensive and structured workflow to perform molecular dynamics (MD) simulations of proteins using GROMACS. The pipeline begins with protein structure preparation and proceeds through solvation, energy minimization, equilibration phases (NVT and NPT), production MD run, and ends with essential post-simulation analyses.

This setup is intended for researchers working on protein stability, folding, or interaction studies, and is adaptable to a range of simulation conditions with appropriate .mdp configuration files.

## Requirements
- GROMACS (2020 or later recommended) installed and sourced properly
- Input protein structure file in PDB format: Protein.pdb
- Simulation parameter files: ions.mdp, minim.mdp, nvt.mdp, npt.mdp, md.mdp
- Optional: xmgrace or any other .xvg file plotting tool for visualization


## Pipeline Steps
## 1. Source GROMACS
```sh

```

## 2. Run GROMACS Command-Line Tool
```sh

```

## 3. Clean the Input Structure (Remove Water)
```sh

```

## 4. Generate Topology and Coordinate File



5. Define Simulation Box

6. Solvate the Protein

7. Add Ions

8. Energy Minimization


9. Plot Energy

10. NVT Equilibration

11. NPT Equilibration

12. Production MD


# Post-Simulation Analysis

## 13. Remove PBC and Center


## 14. RMSD Analysis


## 15. RMSF Analysis

## 16. Radius of Gyration









