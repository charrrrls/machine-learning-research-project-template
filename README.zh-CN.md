# 机器学习科研项目架构与代码发布指南

**一份能直接参考的机器学习科研项目目录说明：代码放哪里、实验怎么组织、GitHub 需要上传什么。**

[English](README.md) · [目录与流程详解](docs/project-structure.md) · [参考模板](docs/references.md) · [复现与发布](docs/reproducibility.md)

适合刚开始搭建机器学习／深度学习科研项目，或准备把论文代码整理到 GitHub 的同学。这里提供推荐目录、文件职责、规模选择和可复制的文档模板。

**文中的树是你可以采用的科研项目结构；本仓库交付的是指南与文档模板。** 本仓库不包含训练实现，无需安装依赖，也不会自动生成示意树中的 Python 文件。

## 先拿走可用模板

| 需要什么 | 预览 | 原始文件 | 保存位置 |
|---|---|---|---|
| 中文科研 README | [预览](templates/README.research.md) | [Markdown 原文](https://raw.githubusercontent.com/charrrrls/machine-learning-research-project-template/main/templates/README.research.md) | `README.md` 或 `README.zh-CN.md` |
| 英文科研 README | [预览](templates/README.research.en.md) | [Markdown 原文](https://raw.githubusercontent.com/charrrrls/machine-learning-research-project-template/main/templates/README.research.en.md) | `README.md` |
| 科研 gitignore | [预览](templates/gitignore.research) | [纯文本原文](https://raw.githubusercontent.com/charrrrls/machine-learning-research-project-template/main/templates/gitignore.research) | `.gitignore` |

打开“原始文件”后复制或保存源文本，替换其中提示与示例。已有项目先合并需要的章节和忽略规则，保留原来有效的内容。

两份 README 模板都包含论文结果追溯表：把每张论文表／图对应到配置、命令、单次记录、汇总代码和公开结果。

## 先解决你的问题

| 你正在考虑什么 | 看哪里 |
|---|---|
| 只有几个脚本，需要拆很多文件夹吗？ | [最小、标准、多实验三种布局](docs/project-structure.md) |
| `src/`、`scripts/`、`analysis/` 到底怎么分工？ | [文件职责与完整实验流程](docs/project-structure.md) |
| README 应该写到什么程度？ | [可复制的科研 README 模板](templates/README.research.md) |
| 数据、权重、日志和结果表哪些上传？ | [复现与发布说明](docs/reproducibility.md)和[gitignore 模板](templates/gitignore.research) |
| 想直接参考别人的完整项目模板？ | [四个原始项目与选型建议](docs/references.md) |

## 先按规模选布局

| 项目情况 | 从什么结构开始 | 什么时候再拆分 |
|---|---|---|
| 一个数据集、几份脚本 | 平铺数据、方法、训练、评估与一个配置文件 | 开始重复复制函数时 |
| 单篇论文，包含基线和消融 | 下方标准结构，共享一套方法与评价实现 | 配置与运行记录难以区分时 |
| 多人、多组实验的长期项目 | 保留标准源码，按实验与运行分组配置和产物 | 确有配置组合或追踪需求时再引入工具 |

完整示例见[三种规模的目录详解](docs/project-structure.md#先按规模选择)。方法不需要的模型、损失和脚本可以合并或省略。

## 适合单篇论文的推荐结构

适合已有主实验、基线和消融，并且需要共享一套算法、训练和评价实现的项目：

```text
paper-project/
├── README.md
├── LICENSE
├── requirements.txt          # 运行依赖
├── pyproject.toml            # src 包安装所需的项目元数据
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

### 每部分为什么存在

| 位置 | 放什么 | 何时可以省略或合并 |
|---|---|---|
| `README.md` | 方法简介、安装、数据、复现命令、参考结果 | 建议始终保留 |
| `src/my_method/` | 复用的数据处理、方法、模型、损失和指标 | 几个脚本即可表达时采用平铺结构 |
| `configs/` | 主实验与消融的超参数、种子、数据集设置 | 实验少时合成一份 JSON 或 YAML |
| `train.py`、`evaluate.py` | 解析参数并启动训练、评价 | 没有训练步骤时可改成 `select.py` 等实际入口 |
| `scripts/` | 数据准备、批量启动主实验和消融 | 只执行单条命令时可以先写进 README |
| `analysis/` | 汇总多次运行、生成论文表格与图 | 内容少时合成一个脚本 |
| `tests/` | 算法、指标和最短流程的小型检查 | 小项目可以使用单个检查文件 |
| `data/` | 数据说明、本地原始数据与必要划分 | 数据可放外部磁盘，但说明与路径参数要保留 |
| `results/` | 精选参考结果及统计口径 | 可以改名，原始运行产物另行归档 |
| `assets/` | README 方法图等说明素材 | 不需要图时省略 |

`models.py` 放模型定义，checkpoint 是运行产物；`test.py` 有时表示模型评估，`tests/` 通常表示软件检查，应在说明中区分。不要为了照搬目录而建立没有用途的空文件。

采用示例的 `src/my_method/` 包布局时，需要配套的 `pyproject.toml` 和安装步骤；单独安装 `requirements.txt` 不会自动使自己的包可导入。[具体示例](docs/project-structure.md#src布局如何正确导入)

## 论文结果要能找到对应代码

每张表、每幅图都应能追溯到实际运行的配置与命令、数据划分、种子集合、单次指标记录和处理脚本。这样读者可以定位论文数字，作者也能在修订图表时找到来源。

[中文 README 模板](templates/README.research.md#论文结果追溯)和[英文模板](templates/README.research.en.md#paper-result-traceability)已经准备好可填写的追溯表，并将示意行明确标为未执行的例子。

## 数据到论文结果怎样串起来

```text
数据说明与固定划分
  → 数据准备脚本
  → 实验配置
  → 训练或方法执行
  → checkpoint／选样索引／预测
  → 评价记录
  → 多种子汇总
  → 论文表格与图
```

读者应能知道每一步的输入、命令、输出位置和下一步使用的文件。[流程详解](docs/project-structure.md#一次实验如何经过这些目录)用一组具体路径说明这种关系。

公开仓库优先准备：方法实现、环境依赖、数据准备与划分、主实验／基线／消融配置、评价指标、结果汇总及小型检查。完整文档网站、Docker、实验追踪平台和包发布可以在确有需要时增加。

## 怎么使用这份指南

1. 选择适合当前规模的布局，保留实际需要的文件。
2. 将[README 模板](templates/README.research.md)复制到自己的项目，替换其中提示和示例。
3. 将[gitignore 模板](templates/gitignore.research)复制为项目根目录的 `.gitignore`，按实际数据路径调整。
4. 实现方法与实验入口，先让一条轻量流程跑通，再执行完整主实验。
5. 按[发布说明](docs/reproducibility.md)核对配置、统计口径和待提交文件。

克隆这个仓库只会得到指南和文档模板。若需要带训练实现的起点，选择下面适合自己的原始模板。

## 推荐参考项目

| 项目 | 适合学习什么 |
|---|---|
| [Cookiecutter Data Science](https://github.com/drivendataorg/cookiecutter-data-science) | 数据、源码、模型产物和报告的整体分区 |
| [PyTorch Template](https://github.com/victoresque/pytorch-template) | 数据加载、模型、训练循环和配置的职责划分 |
| [Lightning Hydra Template](https://github.com/ashleve/lightning-hydra-template) | 多配置、多实验组合与训练流程组织 |
| [Scientific Python Cookie](https://github.com/scientific-python/cookie) | 可安装科研工具库的包结构、测试和文档 |

推荐目录是参考这些项目后整理的原创综合建议。具体出处与各自限制见[参考文档](docs/references.md)，并非某个仓库的原样目录或官方统一规范。

## 贡献与许可

如果这份指南帮你节省了整理项目的时间，可以 Star 保存以便以后查用。

欢迎通过 Issue 或 Pull Request 提出错误修正、使用中的疑问和更清楚的示例。引用外部实践时附原始来源。

本项目原创内容采用 [MIT License](LICENSE)，外部项目保留各自许可。参考信息核对日期：2026-10-17；本指南未安装或运行这些模板。
