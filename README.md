# Machine Learning Research Project Structure Guide & Templates

**Organize machine learning research code so another person can reproduce the paper.**

Annotated folder layouts, English and Chinese README templates, a research gitignore, and a paper code release checklist.

[简体中文](README.zh-CN.md) · [Choose a layout](#choose-a-layout) · [What to publish](#what-belongs-in-a-paper-code-repository)

This is a guide with copyable documentation templates. The example trees describe your own future research project; this repository does not contain training implementations or generate them.

## Use the templates

| Take this | Preview | Raw source | Save as |
|---|---|---|---|
| Research README, English | [Preview](templates/README.research.en.md) | [Raw Markdown](https://raw.githubusercontent.com/charrrrls/machine-learning-research-project-template/main/templates/README.research.en.md) | `README.md` |
| 科研 README，中文 | [预览](templates/README.research.md) | [Markdown 原文](https://raw.githubusercontent.com/charrrrls/machine-learning-research-project-template/main/templates/README.research.md) | `README.md` or `README.zh-CN.md` |
| Research gitignore | [Preview](templates/gitignore.research) | [Raw text](https://raw.githubusercontent.com/charrrrls/machine-learning-research-project-template/main/templates/gitignore.research) | `.gitignore` |

Open **Raw source** to copy or save the original text, then replace the prompts with your actual project information. For an existing project, merge the sections and ignore rules you need instead of replacing working files.

The README templates include a **paper-result traceability table**: connect each paper table or figure to its configuration, command, run records, aggregation code, and published result.

## Choose a layout

| Your project | Start with | Add structure when… |
|---|---|---|
| One dataset and a few scripts | A flat layout: `data.py`, `method.py`, `train.py`, `evaluate.py`, a config, and a small check | You start copying functions between experiments |
| A paper with shared code, baselines, and ablations | The standard layout below | Configurations and run records become hard to distinguish |
| Many experiments or collaborators | Keep the standard code layout; group experiment configs and outputs by run | You have a concrete need for configuration composition or tracking tools |

Keep only files your method needs. A selection algorithm may have no model or loss of its own. A small project can combine modules rather than create empty directories.

### Standard paper repository

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
| `configs/` | Public experiment settings, seeds, hyperparameters, and dataset choices |
| `train.py`, `evaluate.py` | Training and evaluation entry points |
| `scripts/` | Data preparation and repeatable main/ablation launch commands |
| `analysis/` | Tables and plots from individual run records |
| `tests/` | Small checks for the algorithm, metrics, and shortest execution path |
| `data/` | Data instructions and local inputs; preserve necessary small splits |
| `results/` | Selected reference results with units and aggregation rules |

For the `src/my_method/` layout, configure the package in `pyproject.toml` and document editable installation. `requirements.txt` installs dependencies, not your local package. Dependencies may instead be declared in `pyproject.toml`; keep one authoritative source for them. A [minimal packaging example](docs/project-structure.md#src布局如何正确导入) is available in Chinese.

## Connect paper results to code

A reader should be able to follow this path without guessing:

```text
paper table or figure
  → configuration and command
  → data split and selected runs
  → individual metric records
  → aggregation or plotting code
  → published result file
```

For every result, record the dataset/split, seeds, metric units, checkpoint selection rule, and aggregation order. Distinguish a selected best run from an average over preset runs. The [English README template](templates/README.research.en.md#paper-result-traceability) provides a table to fill in, including a clearly labeled illustrative row.

## What belongs in a paper code repository?

| Artifact | Commit to Git? | Provide |
|---|---|---|
| Method, models, data preparation, evaluation, aggregation | Usually yes | The implementation that actually supports the reported results |
| Public configs and experiment scripts | Usually yes | Main, baseline, and ablation settings, including seeds |
| Credentials and machine-specific settings | No | A non-sensitive example and documented path parameters |
| Small split or selection indices | When needed and suitable for release | Their meaning, dataset version, and ID mapping |
| Large datasets, feature caches, checkpoints | Usually store separately | Access or reconstruction instructions and loading code |
| Full logs and intermediate outputs | Usually store separately | The records needed to check published results |
| Selected result tables and figures | Usually yes | Units, aggregation rules, and the code that produced them |

A gitignore filters paths, not file contents. Its exceptions for split indices and CSV/JSON results still require you to choose which files are necessary, appropriate to release, and small enough to keep in Git.

### Before releasing your paper repository

- [ ] Follow the README in a clean environment and complete one lightweight path.
- [ ] Make dataset access, input formats, splits, dependencies, and path settings explicit.
- [ ] Connect each reported result to its config, command, run records, and aggregation.
- [ ] Record seeds, metric units, checkpoint selection, and expected numerical variation.
- [ ] Check staged files and preserve required notices for third-party material.

These are preparation steps for your own research code. This guide does not supply or claim to have run the illustrated training pipeline.

## Reference projects

This is an original synthesis of project-organization practices, not a universal standard.

| Project | Learn from it |
|---|---|
| [Cookiecutter Data Science](https://github.com/drivendataorg/cookiecutter-data-science) | Data stages, source code, model artifacts, and reports |
| [PyTorch Template](https://github.com/victoresque/pytorch-template) | Data loading, models, training loops, and configuration |
| [Lightning Hydra Template](https://github.com/ashleve/lightning-hydra-template) | Experiment configurations and a broader training workflow |
| [Scientific Python Cookie](https://github.com/scientific-python/cookie) | Installable research packages, tests, docs, and maintenance tools |

You can use this guide to decide what you need before choosing a PyTorch project template or another training framework.

Detailed guides in Chinese: [directory boundaries and workflow](docs/project-structure.md), [reference comparisons](docs/references.md), and [reproducibility and release](docs/reproducibility.md). The core layout, copying instructions, release decisions, and README template are available in English above.

## Contributing and license

If this saves you time, star the repository to keep it handy. Corrections and concrete examples are welcome through issues or pull requests; include the source of any external practice.

Original content is available under the [MIT License](LICENSE). Referenced projects retain their own licenses. This guide does not redistribute their training code or claim to have installed and tested their frameworks.

Reference information checked: 2026-10-17.
