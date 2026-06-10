# 工程控制论 Agent 指导框架

> 用钱学森《工程控制论》21 章的思想框架，指导 Agent 架构设计和复杂任务执行。

[![Skill](https://img.shields.io/badge/Claude%20Code-Skill-blue)](https://claude.com/claude-code)

## 这是什么？

这是一个 **Claude Code Skill**，将钱学森《工程控制论》的控制论概念系统性地映射为 Agent 架构设计和任务执行的可操作原则。

- 🎯 **15 条核心设计原则** — 从稳定性优先到复杂度适配
- 🗺️ **39 条控制论→Agent 映射** — 结构/行为/策略/评估/架构五类
- ✅ **14 项执行检查清单** — 复杂任务执行前的自检
- 📋 **10 种任务类型匹配** — 不同场景的适用原则和指导
- 📖 **21 章完整分析** — 257 个概念、166 条原则、167 条类比

## 快速开始

### 安装

```bash
# 克隆到 Claude Code 的 skills 目录
git clone https://github.com/<your-username>/engineering-cybernetics-SKILL.git ~/.claude/skills/engineering-cybernetics-SKILL
```

或者直接复制 `engineering-cybernetics/` 目录到任意 Claude Code 能读取的位置。

### 触发

当任务涉及以下任一特征时，Skill 自动加载：

- 多步骤、多依赖的复杂推理
- 多 Agent / 多工具并行协作
- 系统架构设计
- 错误恢复和容错需求
- 需要自适应调整策略
- 大规模信息聚合
- 不可逆的高风险操作

也可以手动触发：`/engineering-cybernetics`

## 核心原则速览

| # | 原则 | 核心思想 |
|---|------|---------|
| P1 | 稳定性优先 | 确保 Agent 在输入扰动下输出不发散 |
| P2 | 反馈闭环设计 | 生成→验证→修正→再生成的强制循环 |
| P3 | 状态可观测 | 推理过程必须通过日志和中间输出完整可重构 |
| P4 | 能控性边界 | 明确可控维度（指令调优）和不可控维度（模型局限） |
| P5 | 有限时间收敛 | 设定明确的步数/token/时间终止条件 |
| P6 | 多指标帕累托权衡 | 按场景确定准确率/延迟/成本/安全性的优先级 |
| P7 | 模块化串联 | 检索→推理→工具→验证 独立可开发 |
| P8 | 协调优先于独立最优 | 多 Agent 全局约束优先于局部最优 |
| P9 | 冗余容错 | 关键决策至少 2/3 表决，工具配备 fallback |
| P10 | Kalman 式递推估计 | 预测→观测→计算残差→更新信念状态 |
| P11 | 自适应与模型参考 | 以理想行为模板为参考，偏差驱动策略调整 |
| P12 | 信息效率最大化 | 上下文窗口有硬上限，每步推理追求最大信息增益 |
| P13 | 有限状态机完备性 | 穷举所有 (状态 × 输入) 组合的预定义行为 |
| P14 | Min-Max 鲁棒 | 早设检查点，针对最坏情况预设降级方案 |
| P15 | 复杂度适配 | 简单任务简化模型，复杂任务完整推理链 |

## 控制论隐喻速查

| 控制论概念 | Agent 等价物 |
|-----------|-------------|
| 传递函数 F(s) | LLM 推理链 + 工具调用组合 |
| 反馈闭环 | 生成→验证→修正循环 |
| 状态空间 | 上下文窗口 + 对话历史 |
| 阻尼比 ζ | Temperature 参数 |
| Kalman 滤波 | 推理-验证循环（预测-更新） |
| 有限自动机 | Agent 核心架构数学原型 |
| 信道容量 C | 上下文窗口信息上限 |
| Bang-Bang 控制 | 能力边界极限工具调用 |
| 极限环 | Agent 重复无效循环 |
| 冯·诺伊曼冗余 | 多路径推理投票 |

## 目录结构

```
engineering-cybernetics/
├── SKILL.md                     # 核心 Skill 定义（15 条原则 + 检查清单 + 任务匹配）
└── references/
    ├── principles-map.md        # 39 条控制论→Agent 完整映射表
    ├── synthesis.json           # v2.0 跨章节综合分析（结构化数据）
    ├── chapter-summaries.md     # 21 章核心概念速查
    ├── methodology.md           # 构建方法论（如何复用到其他书籍）
    └── analysis/                # 21 章完整结构化分析 (ch01~ch21.json)
```

## 构建方法论

这个 Skill 本身是通过一套三阶段流水线从《工程控制论》21 章原文中系统提取出来的：

1. **阶段一**：7 轮 × 3 subagent 并行章节分析 → 每章结构化 JSON
2. **阶段二**：1 subagent 跨章节综合 → synthesis.json（主题聚类 + 映射表）
3. **阶段三**：synthesis.json → SKILL.md + references/

详见 [`references/methodology.md`](engineering-cybernetics/references/methodology.md)。这套方法可复用于其他书籍/文档的知识提取。

## 致谢

基于钱学森《工程控制论》原著的思想框架。

## License

MIT
