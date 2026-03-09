# RD-Agent 源码模块结构

## 模块总览表

### 1. 核心框架 (rdagent/core/)

| 文件 | 功能 | RD-Agent(Q) 相关 |
|------|------|-----------------|
| `evolving_framework.py` | 演化框架主逻辑 | ⭐ 核心框架 |
| `evolving_agent.py` | 演化智能体基类 | ⭐ 核心框架 |
| `proposal.py` | 假设提案系统 | ⭐ 核心框架 |
| `knowledge_base.py` | 知识库管理 | ⭐ 核心框架 |
| `experiment.py` | 实验抽象 | ⭐ 核心框架 |
| `scenario.py` | 场景抽象 | ⭐ 核心框架 |
| `developer.py` | 开发者接口 | ⭐ 核心框架 |
| `evaluation.py` | 评估系统 | ⭐ 核心框架 |
| `conf.py` | 配置管理 | - |
| `exception.py` | 异常处理 | - |
| `interactor.py` | 交互器 | - |
| `prompts.py` | Prompt 模板 | - |
| `utils.py` | 工具函数 | - |

### 2. 功能组件 (rdagent/components/)

| 模块 | 文件 | 功能 | RD-Agent(Q) 相关 |
|------|------|------|-----------------|
| **proposal/** | `__init__.py` | 假设生成 | ⭐ 假设生成 |
| **coder/CoSTEER/** | `evolving_strategy.py` | Co-STEER 演化策略 | ⭐ 代码生成核心 |
| | `knowledge_management.py` | 知识管理 | ⭐ 知识积累 |
| | `task.py` | 任务定义 | ⭐ 任务管理 |
| | `evaluators.py` | 评估器 | ⭐ 代码评估 |
| **coder/factor_coder/** | `factor.py` | 因子代码生成 | ⭐⭐⭐ 量化核心 |
| | `evolving_strategy.py` | 因子演化策略 | ⭐⭐⭐ 量化核心 |
| | `evaluators.py` | 因子评估 | ⭐⭐⭐ 量化核心 |
| | `config.py` | 因子配置 | ⭐⭐⭐ 量化核心 |
| **coder/model_coder/** | `model.py` | 模型代码生成 | ⭐⭐ 量化相关 |
| | `evolving_strategy.py` | 模型演化策略 | ⭐⭐ 量化相关 |
| **coder/data_science/** | `feature/`, `model/`, `pipeline/` | 数据科学代码 | - |
| **document_reader/** | `document_reader.py` | 文档阅读 | ⭐⭐ 报告解析 |
| **knowledge_management/** | `graph.py`, `vector_base.py` | 知识图谱/向量库 | ⭐ 知识管理 |
| **loader/** | `experiment_loader.py`, `task_loader.py` | 数据加载 | ⭐ 数据加载 |
| **runner/** | `__init__.py` | 执行器 | ⭐ 代码执行 |
| **workflow/** | `rd_loop.py` | R&D 循环 | ⭐ 工作流 |
| **agent/** | `base.py`, `rag/`, `context7/` | 智能体 | - |
| **benchmark/** | `eval_method.py` | 基准测试 | - |
| **interactor/** | `__init__.py` | 交互组件 | - |

### 3. 量化场景 (rdagent/scenarios/qlib/) - ⭐⭐⭐ RD-Agent(Q) 核心

| 子模块 | 文件 | 功能 |
|--------|------|------|
| **proposal/** | `factor_proposal.py` | 因子假设生成 |
| | `model_proposal.py` | 模型假设生成 |
| | `quant_proposal.py` | 量化综合假设 |
| | `bandit.py` | 多臂老虎机调度 |
| **developer/** | `factor_coder.py` | 因子代码实现 |
| | `factor_runner.py` | 因子执行运行 |
| | `model_coder.py` | 模型代码实现 |
| | `model_runner.py` | 模型执行运行 |
| | `feedback.py` | 反馈处理 |
| **experiment/** | `factor_experiment.py` | 因子实验定义 |
| | `model_experiment.py` | 模型实验定义 |
| | `quant_experiment.py` | 量化综合实验 |
| | `factor_from_report_experiment.py` | 从报告提取因子实验 |
| | `workspace.py` | 工作空间管理 |
| **factor_experiment_loader/** | `pdf_loader.py` | PDF 报告加载 |
| | `json_loader.py` | JSON 数据加载 |

### 4. 其他场景 (rdagent/scenarios/)

| 场景 | 路径 | 说明 | RD-Agent(Q) 相关 |
|------|------|------|-----------------|
| **data_science/** | `scenarios/data_science/` | 通用数据科学 | - |
| **kaggle/** | `scenarios/kaggle/` | Kaggle 竞赛 | - |
| **general_model/** | `scenarios/general_model/` | 通用模型 | - |
| **finetune/** | `scenarios/finetune/` | 模型微调 | - |
| **shared/** | `scenarios/shared/` | 共享工具 | - |

### 5. 应用入口 (rdagent/app/)

| 模块 | 文件 | 功能 | RD-Agent(Q) 相关 |
|------|------|------|-----------------|
| **qlib_rd_loop/** | `factor.py` | 因子循环主程序 | ⭐⭐⭐ 量化核心 |
| | `model.py` | 模型循环主程序 | ⭐⭐⭐ 量化核心 |
| | `quant.py` | 因子+模型协同 | ⭐⭐⭐ 量化核心 |
| | `factor_from_report.py` | 从报告提取因子 | ⭐⭐⭐ 量化核心 |
| **data_science/** | `loop.py` | 数据科学循环 | - |
| **kaggle/** | `loop.py` | Kaggle 循环 | - |
| **general_model/** | `general_model.py` | 通用模型入口 | - |
| **benchmark/** | `factor/`, `model/` | 基准测试 | ⭐ 测试评估 |
| **cli.py** | `cli.py` | 命令行接口 | ⭐ 入口 |
| **utils/** | `health_check.py`, `ws.py` | 工具 | - |

### 6. 日志系统 (rdagent/log/)

| 文件 | 功能 | RD-Agent(Q) 相关 |
|------|------|-----------------|
| `logger.py` | 日志记录 | ⭐ 日志 |
| `storage.py` | 日志存储 | ⭐ 日志 |
| `ui/app.py` | UI 界面 | ⭐ 可视化 |
| `server/app.py` | 服务器 | ⭐ 可视化 |

### 7. 工具模块 (rdagent/utils/)

| 模块 | 功能 | RD-Agent(Q) 相关 |
|------|------|-----------------|
| `agent/tpl.py` | 模板渲染 | ⭐ Prompt |
| `workflow/loop.py` | 循环控制 | ⭐ 工作流 |
| `env.py` | 环境变量 | ⭐ 配置 |

---

## RD-Agent(Q) 核心文件标记

⭐⭐⭐ **最核心** (量化金融专用):
- `scenarios/qlib/` 整个目录
- `components/coder/factor_coder/` 整个目录
- `app/qlib_rd_loop/` 整个目录

⭐⭐ **重要** (量化相关):
- `components/coder/model_coder/`
- `components/document_reader/`
- `core/proposal.py`, `core/knowledge_base.py`

⭐ **相关** (框架基础):
- `core/` 核心框架
- `components/CoSTEER/` 代码生成
- `log/` 日志系统

---

## 总计

- **总文件数**: ~300+ Python 文件
- **RD-Agent(Q) 核心文件**: ~40+ 文件
- **核心场景**: `scenarios/qlib/` (量化金融)
- **核心组件**: `components/coder/factor_coder/` (因子代码生成)
- **核心应用**: `app/qlib_rd_loop/` (量化 R&D 循环)

---

*整理日期: 2026-03-08*
