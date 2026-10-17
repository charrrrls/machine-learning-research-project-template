# 参考模板与选择建议

本指南的推荐目录综合了以下项目的组织思想。推荐树的具体文件名和取舍由本指南整理，没有逐字复制任何模板，也不是官方统一标准。下文的适用场景是本指南的选型建议。

## Cookiecutter Data Science

[GitHub 原仓库](https://github.com/drivendataorg/cookiecutter-data-science) · [目录说明](https://cookiecutter-data-science.drivendata.org/#directory-structure) · [设计原则](https://cookiecutter-data-science.drivendata.org/opinions/)

适合包含数据处理、特征构建、建模和结果分析的项目。模板区分 `data/raw/`、`data/interim/`、`data/processed/`，也为模型产物、Notebook 和报告预留位置；实现代码放在项目模块中。

建议先学习它对数据流转和源码／产物的划分，再按实际需要保留目录。它不绑定某个机器学习训练框架。数据目录存在，不意味着全部数据都要进入 Git；官方设计说明也讨论了数据的存放与版本管理。

## PyTorch Template

[GitHub 原仓库与目录说明](https://github.com/victoresque/pytorch-template#folder-structure) · [使用说明](https://github.com/victoresque/pytorch-template#usage)

适合希望理清原生 PyTorch 训练项目职责的研究者。主要分区是 `data_loader/`、`model/`、`trainer/`、`base/`、`logger/` 和 `utils/`，根目录有 `train.py`、`test.py` 与 `config.json`。

值得参考数据加载、模型与训练循环分离的方式。这里的 `test.py` 是模型评估入口，不等同于单元测试目录。直接套用需要理解其基类和配置机制；文档中的推荐依赖版本较早，使用时应另行验证兼容性。

## Lightning Hydra Template

[GitHub 原仓库与目录说明](https://github.com/ashleve/lightning-hydra-template#project-structure)

适合模型、数据集和实验组合较多，且愿意采用 Lightning 与 Hydra 的项目。它将配置分为数据、模型、训练器、实验等部分，并组织 `src/data/`、`src/models/`、`scripts/`、`tests/` 与 `logs/`。

值得参考的是用实验配置保存一次实验的参数组合。完整采用会带来更多框架约定。实验较少时，可以只借鉴配置组织，保留已经清晰可用的训练流程。

## Scientific Python Cookie

[GitHub 原仓库](https://github.com/scientific-python/cookie) · [Scientific Python 开发指南](https://learn.scientific-python.org/development/)

更适合准备长期维护、供其他项目安装与调用的科研工具库。它提供包构建、`pyproject.toml`、测试、文档和自动检查等配套工具，生成内容会随所选后端和选项变化。

值得参考的是如何把科研实现整理成别人可以安装、测试和维护的软件包。单篇论文的代码发布，可以先完成复现说明和核心检查，再决定是否需要完整的软件包发布流程。

## 怎么选

| 当前需要 | 建议先参考 |
|---|---|
| 想弄清一般项目如何分区 | Cookiecutter Data Science |
| 想拆分 PyTorch 训练代码 | PyTorch Template |
| 想管理大量实验组合 | Lightning Hydra Template |
| 想做长期维护的可安装工具库 | Scientific Python Cookie |

模板是起点。几份文件已经能清楚表达职责时，可以合并；只有实际出现复用或管理需求时，再增加层级。完整推荐树见[项目首页](../README.md)。

核对日期：2026-10-17。本指南核对了原项目的说明与结构，没有安装运行这些模板。
