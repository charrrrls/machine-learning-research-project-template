# Machine Learning Research Project Template

**Project structure, annotated directory trees, and README templates for reproducible ML research.**

[简体中文](README.zh-CN.md) · [Project structure / 目录详解](docs/project-structure.md) · [Reference projects](docs/references.md)

How should you organize a machine learning research project? What code belongs in a paper repository? Where should data, configs, experiments, and results go?

This guide helps you answer those questions before your experimental scripts become difficult to share. It includes suggested folder structures and copyable documentation templates. **The trees describe your future research repository; this repository itself contains documentation, not a runnable training framework.** No installation is needed to read or use the guide.

## Start here

| You want to… | Start with… |
|---|---|
| Choose a layout for a small project or a paper | [Small, standard, and multi-experiment layouts](docs/project-structure.md) — Chinese |
| Understand each directory and the full experiment flow | [Directory responsibilities and workflow](docs/project-structure.md#一次实验如何经过这些目录) — Chinese |
| Write a README others can follow | [Copyable research README](templates/README.research.md) — Chinese |
| Decide what to commit | [Research gitignore template](templates/gitignore.research) and [release guide](docs/reproducibility.md) |
| Compare existing templates | [Four reference projects](docs/references.md) |

Copy a template's contents into your own project and adapt it. Cloning this repository downloads the guide; it does not generate the suggested training files.

## Suggested paper repository structure

Use this layout when one paper has shared method code, main experiments, baselines, and ablations. File names are examples, and modules you do not need can be merged or removed.

```text
paper-project/
├── README.md
├── LICENSE
├── requirements.txt          # Runtime dependencies
├── pyproject.toml            # Package metadata for the src layout
├── .gitignore
├── configs/
│   ├── main.yaml
│   └── ablation.yaml
├── src/
│   └── my_method/
│       ├── __init__.py
│       ├── data.py
│       ├── method.py
│       ├── models.py
│       ├── losses.py
│       └── metrics.py
├── scripts/
│   ├── prepare_data.py
│   ├── run_main.sh
│   └── run_ablation.sh
├── train.py
├── evaluate.py
├── analysis/
│   ├── summarize.py
│   └── plot.py
├── tests/
├── data/
│   └── README.md
├── results/
│   └── main_results.csv
└── assets/
    └── framework.png
```

| Location | Responsibility |
|---|---|
| `src/my_method/` | Reusable data processing, method, models, losses, and metrics |
| `configs/` | Shareable experimental settings, including seeds and hyperparameters |
| `train.py`, `evaluate.py` | Training and evaluation entry points |
| `scripts/` | Data preparation and repeatable experiment launch commands |
| `analysis/` | Aggregation, tables, and plots from individual run records |
| `tests/` | Small checks for the algorithm, metrics, and shortest execution path |
| `data/` | Data instructions and local inputs; publish necessary small splits |
| `results/` | Selected reference results with units and aggregation rules |

For only a few scripts, a flat layout is enough. For a `src/my_method/` package, add suitable `pyproject.toml` metadata and document editable installation. A dependency list alone does not install your own package. See the [packaging example](docs/project-structure.md#src布局如何正确导入).

The two dependency-related files have different roles in this example: `requirements.txt` records the research environment, while `pyproject.toml` installs the local package. You can instead declare runtime dependencies in `pyproject.toml`; choose one authoritative source for them.

## What readers need to reproduce a paper

Provide the method implementation, supported environment, data source and preprocessing, fixed splits, experiment settings, evaluation rules, and aggregation code. Explain how one run travels through:

```text
data → preprocessing → configuration → training → checkpoint
     → evaluation → run records → aggregation → paper tables and figures
```

Document baseline sources and pinned versions, including any changes you made. Record seeds, checkpoint selection rules, metric units, and the order of aggregation. Give reference results and relevant hardware conditions without promising identical values on every machine.

Large datasets, feature caches, weights, and logs can live outside Git, with download or reconstruction instructions. Keep source code, configurations, important small indices, checks, and selected results in the repository.

## Reference projects

This guide is an original synthesis, with its own directory and documentation recommendations. The layout is not a universal standard.

| Project | What to learn from it |
|---|---|
| [Cookiecutter Data Science](https://github.com/drivendataorg/cookiecutter-data-science) | Data stages, source code, model artifacts, and reports |
| [PyTorch Template](https://github.com/victoresque/pytorch-template) | Data loading, models, training loops, and configuration |
| [Lightning Hydra Template](https://github.com/ashleve/lightning-hydra-template) | Multiple experiment configurations and a broader training workflow |
| [Scientific Python Cookie](https://github.com/scientific-python/cookie) | Installable research packages, tests, docs, and maintenance tools |

See [references and selection advice](docs/references.md) for original documentation and scope. Adopt only the structure and tools your project needs.

## Contributing and license

Corrections, clearer examples, and focused improvements are welcome through issues or pull requests. Keep suggestions practical and identify their sources.

Original content is available under the [MIT License](LICENSE). Referenced projects retain their own licenses. This guide does not redistribute their source code or claim to have installed and tested them.

Reference information checked: 2026-10-17.
