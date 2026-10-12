# Task order in continual learning: evaluation data and analysis

Anonymous release accompanying the manuscript *What Is the Influence of Task Order in Continual Learning? An Empirical Study and Order-Aware Evaluation for Motion Forecasting*, submitted for double-anonymous review.

- **Project page:** [https://anonymous.4open.science/w/task-order-cl-4f7b/](https://anonymous.4open.science/w/task-order-cl-4f7b/)
- **Contents:** compact evaluation outcomes for all 24 orders of four motion-forecasting tasks, four continual learning algorithms, and three training replicates (288 runs; 1,152 stage logs; 4,608 stage-task records), the complete post-training analysis, and the code for the data figures.

## Quick start

```sh
python3 -m venv .venv
source .venv/bin/activate
python -m pip install -r requirements-tested.txt
python run.py --check-only   # verify frozen inputs and released results
python run.py                # regenerate all numerical results and LaTeX tables
python run.py --figures      # also regenerate the data figures (requires Arial)
```

`run.py` first verifies the SHA-256 hashes of the frozen inputs, then regenerates `analysis/results/` and independently reconstructs the main metrics. Tested with Python 3.12, NumPy 2.0.2, pandas 2.3.3, SciPy 1.13.1, and Matplotlib 3.9.4; `requirements.txt` gives compatible ranges.

## Layout

| Path | Contents |
| --- | --- |
| `analysis/inputs/` | Frozen compact inputs: per-run stage-task results, per-seed summaries, task orders, distance matrices and parameters, feature summaries, provenance, and the exploratory descriptor-scan table |
| `analysis/input_manifest.json` | SHA-256 hashes of the frozen inputs |
| `analysis/scripts/` | `recompute.py` (all statistics and tables), `validate.py` (independent checks), `figures.py` and `task_distributions.py` (data figures), `rebuild_distances.py` and `extract_inputs.py` (optional reconstruction from the raw archive) |
| `analysis/results/` | Per-order summaries, sum-of-squares decompositions (including the starting-task split and warm-start error), descriptor and position-of-A associations, replicate-bootstrap leader probabilities, weighted scores, diagnostics, and the generated manuscript tables |
| `figures/` | Data figures as PDF and SVG |
| `docs/` | The static project page shown at the link above |

## Data conventions

- `stage_task_results.csv` holds 4,608 evaluations; `stage` and `task` index presentation positions 1 to 4, and task identity follows from the sequence in `orders.csv`. Orders are numbered 0 to 23 in lexicographic sequence order.
- Tasks: A Urban Merge, B Highway Merge, C Intersection, D Roundabout (INTERACTION dataset).
- CT is the mean current-task error, BWT the stage-averaged change relative to the initial fit (positive values indicate degradation), and Final the mean error after the last stage. `CT_warm_start` omits the first stage. FDE is in metres, MR in percent, and MR differences in percentage points.
- Order-level values are means over three training replicates with sample standard deviations (`ddof=1`). The replicate bootstrap uses 5,000 draws with NumPy PCG64 seed 0.

The raw trajectory archive and model-training resources are not part of this release; `extract_inputs.py` and `rebuild_distances.py` document how the compact inputs were derived from it.
