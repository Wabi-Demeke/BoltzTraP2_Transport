# BoltzTraP2 Transport Properties for ZnPHC

## Overview

This directory contains **BoltzTraP2** calculations for thermoelectric transport properties of ZnPHC MOF.

## What is BoltzTraP2?

**BoltzTraP2** (Boltzmann Transport Properties) calculates thermoelectric properties from DFT band structures:
- **Seebeck coefficient (S)**: Voltage generated per temperature difference [μV/K]
- **Electrical conductivity (σ)**: Ability to conduct electricity [S/m]
- **Power factor (PF)**: S²σ [mW/(m·K²)] - figure of merit for thermoelectrics

## Files

- **BoltzTraP2_Final.ipynb**: Main calculation notebook
  - Copies required VASP files from `../02_step2_tetrahedron/`
  - Runs BoltzTraP2 interpolation (7×7×1 → 35×35×5)
  - Runs BoltzTraP2 integration at T=300K
  - Processes and visualizes transport properties

- **BoltzTraP2_Working/**: Working directory (created by notebook)
  - Contains VASP input files
  - Contains BoltzTraP2 output files
  - `interpolation.bt2`: Interpolated band structure
  - `interpolation.trace`: Transport properties vs chemical potential

## Usage

1. **Prerequisites**: Make sure VASP calculation in `../02_step2_tetrahedron/` is complete
   - Need: `vasprun.xml`, `POSCAR`, `OUTCAR`, `POTCAR`
   - K-mesh: 7×7×1 (converged)
   - Bandgap: 0.6758 eV

2. **Run the notebook**:
   ```bash
   jupyter notebook BoltzTraP2_Final.ipynb
   ```
   Or use VS Code Jupyter extension

3. **Execute all cells sequentially**:
   - Cell 1: Copy VASP files
   - Cell 2: Verify input data
   - Cell 3: BoltzTraP2 interpolation (~1-2 min)
   - Cell 4: BoltzTraP2 integration (~30 sec)
   - Cell 5: Load and process data
   - Cell 6: Analyze data ranges
   - Cell 7: Create publication plot

## Parameters

- **Material**: ZnPHC 2D MOF (semiconductor)
- **Temperature**: T = 300 K (room temperature)
- **Relaxation time**: τ = 10⁻¹⁴ s (typical for MOFs)
- **Chemical potential**: μ ∈ [-0.5, +0.5] eV (around VBM)
- **Interpolation**: 5× multiplier (7×7×1 → 35×35×5)

## Expected Output

- **Transport curves**: S(μ), σ(μ), PF(μ)
- **Publication plot**: `transport_properties_ZnPHC.png`
- **Analysis**: Optimal doping levels, peak power factor

## Scientific Context

### Why Calculate Transport Properties?

Thermoelectric materials can:
- Convert waste heat to electricity (energy harvesting)
- Solid-state cooling (Peltier effect)
- Applications in sensors, generators

### Key Concepts

**Chemical potential (μ)**: Represents doping level
- μ < 0: p-type doping (holes in valence band)
- μ > 0: n-type doping (electrons in conduction band)
- μ ≈ 0: Intrinsic (undoped) semiconductor

**Power factor (PF)**: Figure of merit
- PF = S² × σ
- Higher PF → better thermoelectric performance
- Peaks near band edges (optimal doping)

## Comparison with Zn3C6O6

This calculation follows the same workflow as:
- `/gscratch/wdiriba/Literature_Reproduce/Zn3C6O6/BoltzTraP2_Transport/BoltzTraP2_Final.ipynb`

Differences:
- **K-mesh**: 7×7×1 (ZnPHC) vs 9×9×1 (Zn3C6O6)
- **Bandgap**: 0.6758 eV (ZnPHC) vs 0.9644 eV (Zn3C6O6)
- **Interpolation**: 35×35×5 (ZnPHC) vs 45×45×5 (Zn3C6O6)

## Notes

- **BoltzTraP2 runs silently**: Even on success, produces no stdout
- **vtk/pyfftw warnings**: Normal, these are optional dependencies
- **Relaxation time approximation**: τ=10⁻¹⁴s is typical but material-specific
- **2D material**: In-plane transport only (no out-of-plane)

## Expected Runtime

- **Interpolation**: ~1-2 minutes
- **Integration**: ~30 seconds
- **Total**: ~3 minutes (much faster than VASP!)

## References

- BoltzTraP2: https://www.boltztrap.org/
- Madsen et al., Comp. Phys. Comm. 231, 140 (2018)
- Relaxation time values: Literature survey of MOF transport

## Troubleshooting

**Issue**: "interpolation.bt2 not found"
- **Solution**: Run interpolation cell (Cell 3) first

**Issue**: "vasprun.xml not found"
- **Solution**: Make sure `../02_step2_tetrahedron/` calculation is complete

**Issue**: No output from btp2 commands
- **Solution**: This is normal! Check if output files exist

**Issue**: Large memory usage
- **Solution**: Reduce interpolation multiplier (e.g., -m 3 instead of -m 5)
