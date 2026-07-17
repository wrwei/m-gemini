# experiments

Scripts that reproduce every figure in the paper from the published dataset.

## Setup

1. Download the accelerated-ageing dataset from figshare (DOI to be added on
   acceptance) into `experiments/data/` — see `data/README.md`.
2. Install dependencies:
   ```bash
   pip install numpy matplotlib openpyxl scipy
   ```

## Figures

Each script in `figures/` regenerates one paper figure, e.g.:

```bash
cd experiments/figures
python _make_figure_pilot_ageing.py      # pilot ageing trajectories
python _fig_validation.py                # internal cross-validation (Q^2)
python _make_figure_mould.py             # mould-index trajectory
python _make_figure_salt.py              # salt crystallisation pressure
python _make_figure_scenarios.py         # 50-year scenario comparison
python _make_figure_composite.py         # composite-risk map
python _make_figure_pigment_map.py       # pigment segmentation + projection
python _make_figure_driver_maps.py       # baked driver fields (Supplementary)
```

Scripts read the dataset from `../data/` and write figure output alongside
themselves (git-ignored).

## Script prerequisites

Baked driver maps, the composite zone data, and the baseline mesh render are
bundled in `experiments/assets/`, so most scripts run directly from a clean
clone:

| Script | Runs from clean clone? | Needs |
|---|---|---|
| `_make_figure_mould.py` | yes | — |
| `_make_figure_salt.py` | yes | — |
| `_make_figure_scenarios.py` | yes | — |
| `_make_figure_driver_maps.py` | yes | bundled `assets/` maps |
| `_make_figure_composite.py` | yes | bundled `assets/` zone data + render |
| `_make_figure_pilot_ageing.py` | after dataset download | dataset in `../data/` |
| `_fig_validation.py` | after dataset download | dataset in `../data/` |
| `_bake_driver_maps.py` | no | photogrammetric mesh (`statue.obj`, CC-BY, Sketchfab) |
| `_make_figure_pigment_map.py` | no | photogrammetric mesh (`statue.obj`) |

The two mesh-dependent scripts regenerate the bundled `assets/` maps from the
full 3D mesh; the mesh (CC-BY 4.0) is not redistributed here — see the top-level
README. Scripts write figure output alongside themselves (git-ignored).
