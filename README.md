# Project overview

This repository contains code and notebooks for numerical/simulation work on a two-multilevel-atom quantum Rabi–type model and QFI (quantum Fisher information) heatmap generation/processing, used in the paper 'Equilibrium thermometry in the multilevel quantum Rabi model' by Tabitha Doicin, Luis A. Correa, Jonas Glatthard, Andrew D. Armour, Gerardo Adesso.

The repo is organised as **three largely independent folders**, each with its **own** `requirements.txt` (kept folder-specific on purpose).

## Folder layout

- `Optimality_and_AA/`
  - Jupyter notebooks for the “Optimality and AA” calculations/figures.
  - `simulation_2MQRM.py`: QuTiP-based simulation utilities/classes used by the notebooks.
  - `requirements.txt`: dependencies for this folder’s notebooks/scripts.

- `QFIHeatmapGenerator/`
  - `HeatmapGenerator.py`: script that generates QFI vs temperature data (CSV + pickle outputs).
  - `requirements.txt`: dependencies for running the generator.

- `QFIHeatmapProcessing/`
  - `processing.ipynb`: notebook(s) for post-processing/plotting generated data.
  - `simulation_2MQRM.py`: QuTiP-based utilities used by the processing notebook(s).
  - `requirements.txt`: dependencies for this folder’s notebooks/scripts.

## Installation

Because requirements are **per-folder**, install from *inside* the folder you want to run.

Example (recommended: a fresh venv per folder):

```bash
cd QFIHeatmapGenerator
python -m venv .venv
source .venv/bin/activate  # Windows: .venv\Scripts\activate
pip install -r requirements.txt
```

Repeat the same pattern for `Optimality_and_AA/` or `QFIHeatmapProcessing/`.

### Note about `mpc` / `mpfr` lines

Two of the `requirements.txt` files include `mpc=...` and `mpfr=...`, which are commonly *conda* package names rather than pip package names. If `pip install -r requirements.txt` fails on those lines, the simplest fix is to use conda/mamba for that environment or remove those two lines for a pip-only environment.

## Usage

### Generate QFI heatmap data

From `QFIHeatmapGenerator/`:

```bash
python HeatmapGenerator.py
```

Outputs are written to the working directory (by default):
- `qfidataframe.csv`
- `avgqfi.pkl`
- `darkbrightqfi.pkl`
- `brightbrightqfi.pkl`
- `tlist.pkl`

Key run parameters (defined near the top of `HeatmapGenerator.py`) include:
- `Dg`, `De` (ground/excited degeneracies)
- `wc`, `wa` (frequencies)
- `ep1`, `ep2` (detuning noise scales)
- `minT`, `maxT`, `numT` (temperature grid)
- `totallines`, `totalsets`, `workers` (parallel workload)
- `theta` (cutoff)
- `Individuallynormalised` (normalisation choice)

### Process data

Open the notebook and generated files in `QFIHeatmapProcessing/processing.ipynb` (Jupyter, VS Code, etc.) after installing that folder’s requirements.

### Run the simulation utilities

Both `Optimality_and_AA/simulation_2MQRM.py` and `QFIHeatmapProcessing/simulation_2MQRM.py` are intended to be imported by notebooks/scripts, e.g.:

```python
from simulation_2MQRM import DoubleMultilevel
```

## Reproducibility notes

- Several workflows rely on randomness (`numpy.random`, Python `random`). If you need deterministic results, set seeds at the start of your script/notebook.
- Parallel generation uses `ProcessPoolExecutor`, so each worker has its own RNG state unless you explicitly manage seeding.

## Citation / license



(code and project files cleaned up with ChatGPT for enhanced readability)
