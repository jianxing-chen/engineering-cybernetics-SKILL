# CLAUDE.md — 工程控制论 Skill 仓库

本仓库是一个 Claude Code Skill，将钱学森《工程控制论》的思想映射为 Agent 架构设计原则。

## 仓库结构

- `engineering-cybernetics/SKILL.md` — Skill 入口文件，包含触发条件、15 条设计原则、任务匹配表、检查清单
- `engineering-cybernetics/references/` — 参考资料目录
  - `principles-map.md` — 39 条完整映射表
  - `synthesis.json` — 结构化综合分析
  - `chapter-summaries.md` — 21 章速查
  - `methodology.md` — 构建方法论
  - `analysis/` — 各章 JSON 分析

## 编码规范

- Skill 内容使用简体中文
- SKILL.md frontmatter 遵循 YAML 格式
- 原则编号格式：`P{N} · {中文名} ({English Name})`
- 每条原则包含 ✅ do 和 ❌ don't 部分

## 修改指引

- 修改 Skill 行为：编辑 `SKILL.md`
- 修改映射/参考数据：编辑 `references/` 下对应文件
- 添加新章节分析：在 `references/analysis/` 下添加 `ch{NN}.json`
- 更新 synthesis：修改 `references/synthesis.json`，注意保持 JSON 结构一致
