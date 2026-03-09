# RD-Agent 源码架构分析

## 1. 整体架构

```
RD-Agent/
├── rdagent/
│   ├── core/              # 核心框架
│   │   ├── evolving_agent.py      # 演化智能体基类
│   │   ├── evolving_framework.py  # 演化框架
│   │   ├── proposal.py            # 假设提案系统
│   │   ├── knowledge_base.py      # 知识库
│   │   ├── experiment.py          # 实验管理
│   │   └── scenario.py            # 场景抽象
│   │
│   ├── components/        # 功能组件
│   │   ├── proposal/              # 假设生成
│   │   ├── coder/                 # 代码生成
│   │   │   └── factor_coder/      # 因子代码生成
│   │   ├── document_reader/       # 文档阅读
│   │   ├── knowledge_management/  # 知识管理
│   │   ├── loader/                # 数据加载
│   │   ├── runner/                # 执行器
│   │   └── workflow/              # 工作流
│   │
│   ├── scenarios/         # 应用场景
│   │   ├── qlib/                  # 量化金融 (核心)
│   │   │   ├── proposal/          # 因子/模型假设
│   │   │   ├── developer/         # 代码实现
│   │   │   ├── experiment/        # 实验定义
│   │   │   └── factor_experiment_loader/  # 因子加载
│   │   ├── data_science/          # 数据科学
│   │   ├── kaggle/                # Kaggle 竞赛
│   │   └── general_model/         # 通用模型
│   │
│   └── app/               # 应用入口
│       └── qlib_rd_loop/          # Qlib R&D 循环
│           ├── factor.py          # 因子循环
│           ├── factor_from_report.py  # 从报告提取因子
│           └── model.py           # 模型循环
│
└── docs/
```

## 2. 核心模块详解

### 2.1 Research 阶段

#### 假设生成 (proposal/)
```python
# scenarios/qlib/proposal/factor_proposal.py
class QlibFactorHypothesisGen(FactorHypothesisGen):
    - prepare_context()    # 准备上下文（历史假设+反馈）
    - convert_response()   # 将 LLM 响应转为具体任务
```

**关键功能**：
- 从历史实验中学习
- 生成新的因子假设
- 将假设映射为可执行任务

#### 文档阅读 (document_reader/)
```python
# scenarios/qlib/factor_experiment_loader/pdf_loader.py
- load_and_process_pdfs_by_langchain()  # 加载 PDF
- classify_document()                    # 分类文档
- extract_factors_from_report()          # 提取因子表达式
```

**关键功能**：
- PDF 文档解析
- 因子表达式提取
- 文档分类（金融工程/非金融工程）

### 2.2 Development 阶段

#### 代码生成 (coder/factor_coder/)
```python
# components/coder/factor_coder/factor.py
class FactorExperiment:
    - factor_composite_tree  # 因子合成树

# components/coder/factor_coder/evolving_strategy.py
- Co-STEER 代码生成
- 从错误中学习
- 知识库演化
```

**关键功能**：
- 因子代码生成
- 因子合成（组合多个因子）
- 错误修正与知识积累

#### 实验执行 (runner/)
```python
# scenarios/qlib/developer/factor_runner.py
- 在 Qlib 中运行因子
- 回测验证
- 性能评估
```

### 2.3 Feedback 阶段

#### 知识管理 (knowledge_management/)
```python
# core/knowledge_base.py
- 存储执行轨迹
- 检索相似错误
- 迁移学习
```

#### 调度系统
```python
# core/evolving_framework.py
- Multi-Armed Bandit 调度
- 自适应方向选择
- 因子 vs 模型优化权衡
```

## 3. 能力评估

### ✅ 已支持的能力

#### a. 阅读研究报告，提取因子表达式
**文件**: `scenarios/qlib/factor_experiment_loader/pdf_loader.py`

**流程**:
1. 加载 PDF 文档
2. 使用 LLM 分类文档（金融工程 vs 其他）
3. 提取因子描述和公式
4. 转换为可执行代码

**示例命令**:
```bash
rdagent fin_factor_report --report-folder=<reports_path>
```

**结论**: ✅ **完全支持**

---

#### b. 持续优化因子和因子合成方式
**文件**:
- `app/qlib_rd_loop/factor.py`
- `components/coder/factor_coder/evolving_strategy.py`

**流程**:
1. 生成初始因子假设
2. 实现因子代码
3. Qlib 回测验证
4. 根据反馈优化（准确率、IC 等）
5. 因子合成（组合多个因子）
6. 循环迭代

**关键特性**:
- **知识积累**: 存储成功/失败的代码模式
- **错误学习**: 相似错误自动检索修正
- **因子合成**: 支持树状组合结构

**示例命令**:
```bash
rdagent fin_factor  # 纯因子优化
rdagent fin_quant   # 因子+模型协同优化
```

**结论**: ✅ **完全支持**

---

#### c. 持续调整投资组合权重
**文件**: `scenarios/qlib/experiment/model_experiment.py`

**分析**:
- RD-Agent 主要关注 **因子挖掘** 和 **预测模型**
- **投资组合优化** 不是核心功能
- Qlib 本身支持组合优化，但 RD-Agent 未深度集成

**当前能力**:
- ✅ 生成收益预测信号
- ❌ 自动调整仓位权重
- ❌ 风险控制集成
- ❌ 组合优化策略

**结论**: ⚠️ **部分支持**（需二次开发）

---

## 4. 扩展建议

### 4.1 组合权重优化

**方案 A**: 集成现有组合优化库
```python
# 在 scenarios/qlib/ 下新增 portfolio/
- portfolio_optimizer.py  # 组合优化器
- risk_controller.py      # 风控模块
- rebalance_strategy.py   # 调仓策略
```

**方案 B**: 让 Agent 学习组合优化
- 添加组合优化假设生成
- 实现组合权重代码生成
- 回测验证组合表现

### 4.2 多市场适配

**当前**: 主要针对 A 股
**扩展**:
- 数据接口适配（美股、港股）
- 交易规则适配（T+0、做空）
- 市场特性因子

---

## 5. 关键技术点

### 5.1 Co-STEER 智能体
- **位置**: `components/coder/factor_coder/evolving_strategy.py`
- **作用**: 从实践中学习代码生成
- **特性**:
  - 错误知识库
  - 相似问题检索
  - 渐进式优化

### 5.2 因子合成树
- **位置**: `components/coder/factor_coder/factor.py`
- **结构**: 树状因子组合
- **操作**: 加权、条件、非线性组合

### 5.3 文档理解
- **位置**: `scenarios/qlib/factor_experiment_loader/`
- **技术**: LangChain + LLM
- **能力**: PDF 解析、公式提取、分类

---

## 6. 使用建议

### 快速上手
```bash
# 1. 安装
conda create -n rdagent python=3.10
conda activate rdagent
pip install rdagent

# 2. 配置 .env
CHAT_MODEL=gpt-4o
EMBEDDING_MODEL=text-embedding-3-small
OPENAI_API_KEY=your-key

# 3. 运行
rdagent fin_factor          # 因子优化
rdagent fin_factor_report   # 从报告提取因子
rdagent fin_quant           # 因子+模型协同
```

### 二次开发
- 继承 `QlibFactorHypothesisGen` 自定义假设生成
- 继承 `FactorExperiment` 自定义因子结构
- 扩展 `knowledge_base.py` 增强知识管理

---

## 7. 结论

**RD-Agent(Q) 能力总结**:

| 能力 | 支持度 | 说明 |
|------|--------|------|
| a. 阅读报告提取因子 | ✅ 完全支持 | PDF 解析 + 公式提取 |
| b. 持续优化因子和合成 | ✅ 完全支持 | 闭环迭代 + 知识积累 |
| c. 调整组合权重 | ⚠️ 部分支持 | 需二次开发 |

**建议**:
1. ✅ 可以直接使用 a、b 功能
2. 🔧 c 功能需要开发扩展模块
3. 📊 建议先跑通官方示例，再考虑定制

---

*分析日期: 2026-03-08*
