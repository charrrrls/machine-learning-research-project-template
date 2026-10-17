# 机器学习科研项目架构指南

一份面向机器学习论文实验与代码发布的中文项目组织指南：解释需要哪些文件、代码应该放在哪里，以及怎样让别人按说明复现实验。

**推荐目录是参考公开模板后整理的综合建议，不是某个模板的原样复制，也不是 GitHub、PyTorch 或会议规定的统一标准。** 目录可以随项目规模删减。

本仓库本身是文档项目，无需安装依赖；下面的 Python 文件与目录是科研项目的示意，不是本仓库已经实现的训练框架。

## 从哪里开始

- [推荐目录与文件用途](#推荐目录与文件用途)：先了解各部分的职责。
- [参考模板与选择建议](docs/references.md)：查看四个原始模板及其适用场景。
- [复现与发布说明](docs/reproducibility.md)：了解该提供哪些内容、哪些产物适合另行存放。

## 推荐目录与文件用途

适合有主实验、基线和消融的单篇机器学习论文项目：

```text
paper-project/
├── README.md                 # 环境、数据、实验命令、预期结果
├── LICENSE                   # 项目内容的使用许可
├── requirements.txt          # Python 依赖；也可采用 pyproject.toml
├── .gitignore                # 排除本地配置和大型运行产物
│
├── configs/                  # 可公开、可复用的实验参数
│   ├── main.yaml             # 主实验
│   └── ablation.yaml         # 消融实验
│
├── src/                      # 可复用实现，按职责组织
│   ├── data.py               # 数据读取、预处理、划分
│   ├── method.py             # 论文提出的核心方法
│   ├── models.py             # 模型结构
│   ├── losses.py             # 损失函数
│   └── metrics.py            # 指标计算
│
├── scripts/                  # 数据准备和批量实验入口
│   ├── prepare_data.py
│   ├── run_main.sh
│   └── run_ablation.sh
│
├── train.py                  # 训练入口
├── evaluate.py               # 评估入口
│
├── analysis/                 # 从原始记录生成论文图表
│   ├── summarize.py          # 多种子汇总、均值与标准差
│   └── plot.py               # 绘图
│
├── tests/                    # 核心算法、指标和流程的轻量检查
├── data/
│   └── README.md             # 数据获取、格式、划分说明
├── results/
│   └── main_results.csv      # 精选的参考结果
└── assets/
    └── framework.png         # README 使用的方法图，可选
```

这棵树展示职责，不要求每项都成为独立文件。例如只有一种损失时，可以和模型放在一起；没有训练步骤的选择算法，可以将入口改成 `select.py`；小项目的配置可以先放一个 JSON 文件。需要安装成 Python 包时，再将源码组织为 `src/<package>/` 并配置包安装。

## 优先准备什么

| 内容 | 读者应能知道什么 |
|---|---|
| 核心方法与评价实现 | 算法、模型、损失、指标如何计算 |
| 数据准备与划分 | 数据从哪里来，输入格式是什么，训练／验证／测试如何固定 |
| 环境依赖 | Python、关键依赖和必要的 CUDA／硬件条件 |
| 实验配置与入口 | 主实验、基线和消融分别执行什么，使用哪些参数和种子 |
| 结果与汇总代码 | 单次运行记录如何形成论文表格，参考数值使用什么单位与统计口径 |
| README 与许可说明 | 从新环境开始的步骤，以及项目与第三方内容如何使用 |

完整文档网站、Notebook、Docker、实验追踪平台、自动化检查和包发布可以按需增加，不必为了符合目录树而提前搭建。

## 如何参考现有模板

| 模板 | 更适合参考的内容 |
|---|---|
| [Cookiecutter Data Science](https://github.com/drivendataorg/cookiecutter-data-science) | 数据、源码、模型产物与分析报告的整体分区 |
| [PyTorch Template](https://github.com/victoresque/pytorch-template) | 数据加载、模型、训练循环与配置的职责划分 |
| [Lightning Hydra Template](https://github.com/ashleve/lightning-hydra-template) | 多配置、多实验组合和测试的组织 |
| [Scientific Python Cookie](https://github.com/scientific-python/cookie) | 可安装科研工具库的包结构、文档与维护工具 |

通常先看前两个就能决定基本布局。各模板有自己的框架与工具约定；具体来源和适用范围见[参考文档](docs/references.md)。

## 本说明项目实际包含什么

```text
ml-research-project-guide/
├── README.md
├── LICENSE
├── .gitignore
├── AGENTS.md
└── docs/
    ├── references.md
    └── reproducibility.md
```

这里没有创建空的训练实现，也没有包含数据集、模型权重或任何既有研究项目的实验材料。本地工作记录与机器配置不纳入 Git。

## 来源与许可

本项目原创说明采用 [MIT License](LICENSE)。引用的外部模板保留各自许可；本仓库提供原始链接，不重新分发其模板源码。

参考信息核对日期：2026-10-17。模板可能更新，实际使用时以原项目当前说明为准。
