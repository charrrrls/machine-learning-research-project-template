# [Project name]

> Copy this source into your research repository, remove this note, and replace every prompt. Paths and commands below illustrate documentation structure; they are not implementations supplied by this guide.

[Describe the problem, proposed method, and what a reader can run in one or two sentences.]

- Paper: [title, URL, venue if applicable]
- Method overview: [figure or detailed explanation when useful]
- Status: [available method/training/evaluation entry points and any unreleased parts]

## Environment and installation

| Item | Verified condition |
|---|---|
| Python | [version] |
| Framework and dependencies | [versions or dependency file] |
| GPU / CUDA | [requirements, or state CPU support] |
| Memory / VRAM | [measured or verified conditions] |

[Give installation commands you have actually checked in a clean environment. If using a src package, include its pyproject.toml configuration and local installation step. Remove commands that do not apply.]

## Data preparation

- Source and access: [original URL, version, application or access requirements]
- Preprocessing: [entry point and required feature-extractor or weight sources]
- Splits: [fixed train/validation/test definitions and split-file locations]
- Input format: [fields, dimensions, sample IDs, and paired-data alignment]
- Paths: [configuration key or command-line option]
- Cached inputs: [download or reconstruction instructions and useful checks]

## Minimal run

[Give one lightweight command you have run. State its required inputs, expected duration, output files, expected contents, and the part of the pipeline it checks.]

## Main experiments, baselines, and ablations

The following illustrates how to document an entry point, configuration, and seed. Replace it with commands your project actually supports.

```bash
python scripts/prepare_data.py --data-root /path/to/dataset
python train.py --config configs/main.yaml --seed 42
python evaluate.py --checkpoint /path/to/best.ckpt --split test
python analysis/summarize.py --runs /path/to/run-records --output results/main_results.csv
```

| Experiment | Configuration / full command | Input | Output | Verification |
|---|---|---|---|---|
| Main | [path and command] | [files] | [run directory] | [verified / not run] |
| Baseline | [path and command] | [files] | [run directory] | [verified / not run] |
| Ablation | [path and command] | [files] | [run directory] | [verified / not run] |

## Paper-result traceability

Create one row for each reported table or figure. Associate it with the data split, method, seed set, metric units, and aggregation rule. The first row only illustrates a relationship between paths; it does not represent a supplied or executed experiment.

| Paper item | Configuration and entry point | Individual records | Aggregation / plotting | Published result |
|---|---|---|---|---|
| Table 1, main (illustrative; not executed) | `configs/main.yaml`, `scripts/run_main.sh` | `outputs/<run-id>/metrics.json` | `analysis/summarize.py` | `results/main_results.csv` |
| [table / figure ID] | [config and full command] | [actual record location] | [script and arguments] | [file or download URL] |

## Evaluation and aggregation

- Metric definitions and units: [for example, macro-mAP on a 0–1 or percentage scale]
- Checkpoint selection: [validation metric and selection rule]
- Final evaluation: [test split, retrieval pool, relevance definition, or task details]
- Random seeds: [separate split, selection, and training seeds where applicable]
- Aggregation: [order of averaging, variability calculation, and included preset runs]

## Reference results and computational cost

| Dataset / setting | Metric | Mean | Variation | Number of runs |
|---|---|---|---|---|
| [fill in] | [fill in] | [fill in] | [fill in] | [fill in] |

- Hardware: [actual conditions]
- Timing scope: [which preparation, feature extraction, selection, or training steps are included]
- Known variation: [observed dependency, hardware, or random-seed effects]
- Paper plots: [source records and commands that produce each table or figure]

## Checks

[Give real commands for core algorithm, metric, and minimal-pipeline checks.]

## Third-party material and license

[List baseline implementations, pretrained weights, datasets, and reused code with their sources and versions. Describe your changes and preserve required notices.]

## Citation

[Add the actual paper BibTeX, or omit this section when no paper exists.]
