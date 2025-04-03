# Protein-Simulation-Pipeline
This repository provides a comprehensive and structured workflow to perform molecular dynamics (MD) simulations of proteins using GROMACS. The pipeline begins with protein structure preparation and proceeds through solvation, energy minimization, equilibration phases (NVT and NPT), production MD run, and ends with essential post-simulation analyses.

This setup is intended for researchers working on protein stability, folding, or interaction studies, and is adaptable to a range of simulation conditions with appropriate .mdp configuration files.

## Requirements
- GROMACS (2020 or later recommended) installed and sourced properly
- Input protein structure file in PDB format: Protein.pdb
- Simulation parameter files: ions.mdp, minim.mdp, nvt.mdp, npt.mdp, md.mdp
- Optional: xmgrace or any other .xvg file plotting tool for visualization


## Workflow Overview
## 1. Environment Setup
```sh
source /usr/local/gromacs/bin/GMXRC
gmx
```

## 2. Structure Preparation
- Remove crystallographic water molecules
- Generate topology and initial coordinate files using a selected force field (e.g., OPLS-AA)
```sh
grep -v HOH Protein.pdb > clean.pdb
gmx pdb2gmx -f clean.pdb -o protein.gro -water spce -ignh
```

## 3. Define Simulation Box and Solvate
```sh
gmx editconf -f protein.gro -o box.gro -c -d 1.0 -bt cubic
gmx solvate -cp box.gro -cs spc216.gro -o solv.gro -p topol.top
```

## 4. Add Ions to Neutralize the System
```sh
gmx grompp -f ions.mdp -c solv.gro -p topol.top -o ions.tpr
gmx genion -s ions.tpr -o solv_ions.gro -p topol.top -pname NA -nname CL -neutral
```

## 5. Energy Minimization
```sh
gmx grompp -f minim.mdp -c solv_ions.gro -p topol.top -o em.tpr
gmx mdrun -v -deffnm em
```

## 6. Equilibration
- NVT (Constant Volume and Temperature)
```sh
gmx grompp -f nvt.mdp -c em.gro -r em.gro -p topol.top -o nvt.tpr
gmx mdrun -deffnm nvt
gmx energy -f nvt.edr -o temperature.xvg
```

- NPT (Constant Pressure and Temperature)
```sh
gmx grompp -f npt.mdp -c nvt.gro -r nvt.gro -t nvt.cpt -p topol.top -o npt.tpr
gmx mdrun -deffnm npt
gmx energy -f npt.edr -o pressure.xvg
gmx energy -f npt.edr -o density.xvg
```

## 7. Production MD Run
```sh
gmx grompp -f md.mdp -c npt.gro -t npt.cpt -p topol.top -o md.tpr
gmx mdrun -deffnm md
```

# Post-Simulation Analysis
## Trajectory Processing and Analysis
- Remove periodic boundary artifacts and center molecule:
```sh
gmx trjconv -s md.tpr -f md.xtc -o md_noPBC.xtc -pbc mol -center
```

## RMSD and RMSF Analysis
```sh
gmx rms -s md.tpr -f md_noPBC.xtc -o rmsd.xvg -tu ns
gmx rmsf -s md.tpr -f md_noPBC.xtc -o rmsf.xvg -res
```

## Radius of Gyration
```sh
gmx gyrate -s md.tpr -f md_noPBC.xtc -o gyrate.xvg
```

## Output Summary
| File  | Description
|----------|----------|
| *.gro, topol.top  | Coordinate and topology files   |
| *.tpr, *.cpt, *.edr  | Run input and output files   |
| *.xvg  | Energy and analysis output for plotting   |
All intermediate and final results are saved step-by-step to maintain reproducibility and traceability.


## Notes
Ensure the .mdp files used for each phase are appropriately tuned to your simulation system.
For high-throughput runs or automated setups, consider integrating the pipeline into a shell script or workflow manager.

## Maintainer
- Sourabh Kumar bioinfosourabh@gmail.com
- This repository is open for contributions, issues, and suggestions to enhance utility for the molecular simulation community.
