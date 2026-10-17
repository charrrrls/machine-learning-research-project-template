# 科研项目目录与流程详解

[中文首页](../README.zh-CN.md) · [English overview](../README.md)

本页解释如何选择布局，以及代码、配置、数据与结果怎样组成一条可复现流程。以下目录均为建议示意，并非本仓库已经提供的训练实现。

## 先按规模选择

| 布局 | 适合什么阶段 | 什么时候需要升级 |
|---|---|---|
| 最小结构 | 一个数据集、少量实验、几份脚本能说明整个方法 | 函数开始被反复复制，或不同实验的设置难以区分 |
| 标准论文结构 | 多个数据集、主实验、基线与消融共享实现 | 配置组合与运行记录增多，需要明确继承关系与索引 |
| 多实验结构 | 多人协作、长期研究、多组参数与多种运行后端 | 先确认实际管理问题，再决定是否引入 Hydra 等工具 |

### 最小结构

```text
my-paper/
├── README.md
├── LICENSE
├── requirements.txt
├── .gitignore
├── config.json
├── data.py
├── method.py
├── train.py
├── evaluate.py
└── test_method.py
```

方法只需要几十行辅助代码时，可以直接放在相关文件中。没有训练过程时，删去 `train.py` 并提供实际的方法入口。最小结构也应说明数据和评价方式。

### 标准论文结构

采用[中文首页的完整目录树](../README.zh-CN.md#适合单篇论文的推荐结构)。核心是复用一份算法实现，用不同配置运行主实验和消融，再从各次运行记录生成论文结果。

`src/my_method/` 是 Python 包，`train.py`、`evaluate.py` 和 `analysis/` 中的程序可以导入它。请按下文配置安装，避免在多个脚本里临时修改搜索路径。

### 多实验结构

在标准结构上扩展真正变化的配置和运行产物即可：

```text
configs/
├── data/
├── model/
└── experiments/
    ├── main.yaml
    └── ablation_no_component.yaml
outputs/
└── dataset-name/
    └── method-name/
        └── run-id/
            ├── config.json
            ├── metrics.json
            ├── best.ckpt
            └── train.log
```

`outputs/` 表示本地运行产物，通常不提交。配置的分层不要求特定框架：普通 JSON／YAML 也可以；确需配置组合与覆盖时，再参考 [Lightning Hydra Template](https://github.com/ashleve/lightning-hydra-template)。

## src布局如何正确导入

平铺文件和 `src/` 布局是两种可选方式。Python Packaging 官方文档说明了两者差别，`src/` 布局通常需要先安装项目。[官方说明](https://packaging.python.org/en/latest/discussions/src-layout-vs-flat-layout/)

如果采用 `src/my_method/`，创建 `src/my_method/__init__.py`，并在自己项目根目录配置包元数据。下面是针对示例名称的最小 `pyproject.toml` 片段：

```toml
[build-system]
requires = ["setuptools>=64"]
build-backend = "setuptools.build_meta"

[project]
name = "my-method"
version = "0.1.0"
requires-python = ">=3.10"

[tool.setuptools.packages.find]
where = ["src"]
include = ["my_method*"]
```

若运行依赖维护在 `requirements.txt`，在自己的科研项目根目录执行：

```bash
python -m pip install -r requirements.txt
python -m pip install --no-deps -e .
```

第二条命令将本地包安装成可编辑模式，之后可以使用 `from my_method import ...`。本例不在 `pyproject.toml` 重复声明运行依赖；如果决定在 `[project].dependencies` 中统一维护它们，应相应调整安装说明，避免两份声明长期不一致。

此片段只配置包安装，不实现训练。构建后端和 Python 版本按实际项目填写。[setuptools 配置依据](https://setuptools.pypa.io/en/latest/userguide/pyproject_config.html)

## 一次实验如何经过这些目录

以下以“一个数据集、一个方法、训练种子 42”为例。路径和字段用于说明文件关系，需要由自己的实现产生。

| 步骤 | 读取什么 | 谁负责 | 写出什么 |
|---|---|---|---|
| 数据准备 | 数据来源、原始图文、固定划分 | `scripts/prepare_data.py` 调用数据处理实现 | 处理后的数据、特征与样本 ID |
| 确定设置 | `configs/main.yaml` 与命令行覆盖 | 训练或方法入口 | 本次生效的完整配置 |
| 执行方法 | 训练数据、配置、种子 | `train.py` 与 `src/my_method/` | 模型 checkpoint、选样索引或预测 |
| 选择 checkpoint | 验证集指标 | 训练流程 | 按声明规则选出的模型 |
| 最终评价 | 选定模型、固定测试划分 | `evaluate.py` 调用 `metrics.py` | 单次运行的指标记录 |
| 汇总结果 | 各次运行记录 | `analysis/summarize.py` | 均值、标准差与参考结果表 |
| 生成论文图 | 汇总表或明确的原始记录 | `analysis/plot.py` | 论文图及数据来源 |

一条结果记录可以包含：数据集、方法、配置名、代码版本、数据划分标识、种子、保留预算、指标名称、指标单位和数值。字段按任务选择，不能用一个没有单位的 `score` 代替完整评价说明。

建议保存每次实际生效的配置，不能只保存会继续被修改的默认 YAML。多种子实验要说明先对哪些运行取均值、标准差在哪一层计算；单次最优模型与多种子平均也应分别命名。

## 目录边界常见问题

**src 与 scripts 怎么分？** 多处复用的处理与算法放源码；解析参数、启动组合实验等放入口。只调用一次且很短的逻辑可以留在入口，不必强行抽层。

**analysis 是不是另一个训练目录？** 它读取实验记录，生成统计与图表。需要重新训练才能形成的结果，应说明对应的训练配置。

**模型代码和模型文件放一起吗？** `models.py` 是模型定义；`best.ckpt` 是一次运行的产物，建议放对应运行目录。

**主实验与消融各复制一份算法吗？** 优先共享实现，用配置明确改变的组成部分，避免两份代码意外分叉。

**Notebook 可以保留吗？** 可以用于探索和展示。论文复现需要的步骤应能按说明稳定执行，并明确 Notebook 是否属于必经流程。

**是不是目录越多越规范？** 以读者能追踪完整流程为准。文件很少时选择最小结构，出现复用和配置管理需求后再拆分。

继续阅读[复现与发布](reproducibility.md)，或复制[科研 README 模板](../templates/README.research.md)。
