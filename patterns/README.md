# Patterns — 行为模式库

## 设计目标

解决 AI 自生成内容难以管控的问题。通过预定义的模式约束，使 AI 输出符合：
- 真实业务流程
- 用户使用习惯
- 特定领域要求
- 质量与格式标准

## 工作原理

```
任务到达
   ↓
Planner 分析任务
   ↓
识别适用的 Pattern ← patterns/ 目录
   ↓
在 playbook.md 中指定 Pattern
   ↓
Coordinator 调度专家
   ↓
专家读取 Pattern + 执行任务
   ↓
输出符合 Pattern 约束的交付物
```

## Pattern 的结构

每个 Pattern 是一个 Markdown 文件，包含以下部分：

```markdown
# Pattern 名称

## 适用场景
描述该 Pattern 适用的任务类型、领域或上下文

## 约束规则
明确的"必须"、"禁止"、"优先"规则

## 输出规范
- 格式要求
- 结构模板
- 质量标准

## 示例
好的示例 vs 不符合 Pattern 的示例

## 检查清单
执行前/执行后的验证点
```

## Pattern 分类

### 1. 领域特定模式 (Domain-Specific Patterns)
针对特定业务领域的约束，例如：
- `academic-writing.md` — 学术论文写作规范
- `legal-contract.md` — 法律合同起草规则
- `technical-spec.md` — 技术规格文档标准

### 2. 流程模式 (Process Patterns)
定义工作流程的执行方式，例如：
- `iterative-refinement.md` — 迭代优化流程
- `multi-round-review.md` — 多轮审查机制
- `parallel-synthesis.md` — 并行综合方法

### 3. 质量模式 (Quality Patterns)
确保输出质量的约束，例如：
- `evidence-based.md` — 基于证据的论证
- `consistency-check.md` — 一致性检查
- `clarity-simplicity.md` — 清晰简洁原则

### 4. 格式模式 (Format Patterns)
输出格式的标准化，例如：
- `structured-report.md` — 结构化报告模板
- `markdown-conventions.md` — Markdown 规范
- `api-documentation.md` — API 文档格式

## 使用规范

### Planner 的职责

1. **分析任务特征**，识别需要的约束类型
2. **选择适用的 Pattern**（可以多个）
3. **在 playbook.md 中声明**：

```markdown
## Pattern 约束

本任务应用以下 Pattern：

- `patterns/academic-writing.md` — 确保学术规范
- `patterns/evidence-based.md` — 所有论点需引用支撑
```

### 专家的职责

1. **执行任务前**，读取 playbook 中指定的所有 Pattern
2. **执行过程中**，按 Pattern 约束生成内容
3. **输出前**，对照 Pattern 检查清单验证

### Coordinator 的职责

1. **调度时**，确保专家能访问到指定的 Pattern 文件
2. **质量检查时**，验证输出是否符合 Pattern 约束
3. **反馈时**，如果违反 Pattern，要求专家重新执行

## Pattern 演进

Pattern 本身也可以迭代优化：

1. **任务执行后**，Coordinator 评估 Pattern 的有效性
2. **识别不足**，Planner 标记需要改进的 Pattern
3. **升级 Pattern**，Talent Architect 更新 Pattern 定义
4. **版本管理**，保留 Pattern 的演进历史

## 示例：学术论文 Pattern 应用

### 任务
"基于 source/ 中的文献，撰写关于多智能体系统的综述"

### Planner 生成的 playbook.md 片段

```markdown
## Pattern 约束

- `patterns/academic-writing.md` — 学术写作规范
- `patterns/evidence-based.md` — 每个论点需文献支撑
- `patterns/structured-report.md` — 使用标准学术结构

## 专家任务

**Research Analyst**
- 输入：source/ 中的文献摘要
- Pattern：academic-writing.md, evidence-based.md
- 输出：提取关键论点 + 引用映射

**Technical Architect**
- 输入：Research Analyst 的论点汇总
- Pattern：academic-writing.md, structured-report.md
- 输出：完整的综述文章草稿
```

### 专家执行

Research Analyst 读取 `evidence-based.md`，了解到：
- 每个论点必须有至少 1 个文献引用
- 引用格式：`[作者, 年份]`
- 禁止未经验证的推测性陈述

Technical Architect 读取 `academic-writing.md` 和 `structured-report.md`，了解到：
- 必须包含：摘要、引言、主体、结论、参考文献
- 段落长度：3-5 句
- 避免口语化表达

### 结果

输出的综述文章：
- ✅ 结构完整，符合学术规范
- ✅ 每个论点都有文献支撑
- ✅ 格式统一，引用正确
- ✅ 无需人工大幅修改

## 核心价值

| 问题 | Pattern 如何解决 |
|------|------------------|
| AI 输出不符合行业标准 | 预定义行业 Pattern，强制遵守 |
| 每次输出风格不一致 | Format Pattern 统一格式 |
| 缺乏质量保障 | Quality Pattern + 检查清单 |
| 业务流程匹配度低 | Process Pattern 定义标准流程 |
| 需要反复修改才能用 | Pattern 前置约束，一次到位 |

## 与其他系统组件的关系

```
specialists/        → 定义"谁"来执行（角色能力）
patterns/          → 定义"如何"执行（行为约束）
notes/{task}/      → 定义"做什么"（具体任务）
```

三者结合：
- **Specialist** 提供能力
- **Pattern** 提供约束
- **Playbook** 提供指令

最终输出 = 能力 × 约束 × 指令
