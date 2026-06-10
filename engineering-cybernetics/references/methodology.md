# 构建方法论：从书籍到 Skill 的流水线

本文档记录将《工程控制论》21 章转化为 Agent skill 的完整方法，可复用于其他书籍/文档的知识提取。

## 流水线概览

```
书籍（N章）
  │
  ├── 阶段一：并行章节分析（N/3 轮 × 3 subagent）
  │    每章 → 结构化 JSON {core_concepts, key_principles, agent_mapping, ...}
  │
  ├── 阶段二：跨章节综合映射（1 subagent）
  │    全部 JSON → synthesis.json {theme_clusters, design_principles, mapping_table, ...}
  │
  └── 阶段三：Skill 构建
        synthesis.json → SKILL.md + references/
```

## 阶段一关键设计

### Subagent 统一输出格式

每章 subagent 输出相同的 JSON 结构，包含两个关键映射字段：

```json
{
  "agent_mapping": {
    "direct_analogies": [
      {"cybernetics_concept": "控制论概念", "agent_equivalent": "Agent等价物", "rationale": "对应理由"}
    ],
    "design_principles_for_agent": ["从本章导出的Agent设计原则"],
    "task_execution_guidance": ["任务执行中的具体应用指导"]
  }
}
```

`agent_mapping` 是核心——它要求 subagent 在分析阶段就建立控制论→Agent 的映射，而非事后补充。

### 分批策略

- 每轮 3 个 subagent 并行（`delegate_task` 的 `tasks` 数组上限）
- 每轮完成后立即验证产物（JSON 格式校验、保存到统一目录）
- 发现不完整立即标记，最后补跑

### 常见问题

| 问题 | 表现 | 对策 |
|------|------|------|
| subagent 返回 JSON inline | summary 中有 JSON 但未写入文件 | 手动提取并 `write_file` 保存 |
| JSON 含中文引号 `""` | U+201C/U+201D 与 ASCII `"` 冲突，`json.loads()` 失败 | subagent 指令中用 `「」` 替代；或写入后 `sed` 替换 |
| 大章节读取不全 | subagent 只读了前半部分 | 明确要求用 `read_file` 分段读取，指定 offset/limit |
| 重建版数据贫乏 | subagent summary 截断导致 JSON 仅为简化版 | 补跑该章 subagent，显式要求 write_file |
| 分析深度不一致 | 有的 subagent 30+ 工具调用，有的仅 3 个 | 在 context 中设最低要求：≥5 concepts/principles/analogies |
| 输出路径不统一 | 文件散落 ~/、~/Downloads/、/tmp/ | 在 context 中显式指定统一输出路径 |

## 阶段二关键设计

### 综合 subagent 的输入

- 全部 N 个章的 JSON 文件（分批读取）
- 重点提示哪些章有特别丰富的 Agent 映射
- 输出 `synthesis.json` 覆盖旧版本

### synthesis.json 结构

```json
{
  "theme_clusters": [6个主题群，每群含章节、概述、洞察],
  "core_design_principles": [10-15条原则，每条含 do/don't],
  "full_mapping_table": [30+条映射，标注 mapping_type],
  "task_type_matching": {10种任务类型 → 适用原则 + 执行指导},
  "architectural_insights": [全局架构洞察]
}
```

## 阶段三关键设计

### SKILL.md 要素

- **触发条件**：明确 `triggers` 列表（中文关键词）
- **核心原则**：从 synthesis 的 15 条原则精简为可速查格式（✅/❌ 对每条）
- **任务匹配表**：表格形式快速路由
- **检查清单**：Agent 执行前的 13 项自检
- **核心隐喻速查**：20 个控制论→Agent 一对一对等表
- **反模式**：7 条常见错误

### references/ 文件

- `principles-map.md`：完整 39 条映射表（按类型分组）
- `chapter-summaries.md`：21 章快速摘要（每章 3 行）
- `methodology.md`（本文件）：构建方法论

## 工具使用陷阱

### read_file 在 execute_code 中

```python
# ❌ 错误：read_file 返回内容带行号前缀
content = read_file("/tmp/data.json")["content"]
data = json.loads(content)  # JSONDecodeError: "1|{" ...

# ✅ 正确：用 terminal 直接读
terminal("python3 -c 'import json; d=json.load(open(\"/tmp/data.json\"))'")
```

### 复杂 Python 在 terminal 中

```python
# ❌ 错误：execute_code 中嵌套 terminal("python3 -c \"...\"") 引号转义复杂
# SyntaxError: unterminated string literal

# ✅ 正确：写独立脚本文件再执行
write_file("/tmp/script.py", content)
terminal("python3 /tmp/script.py")
```

## 统计

21 章，约 35,000 行原文，最终产出：

| 指标 | 数值 |
|------|------|
| 提取概念 | 257 |
| 提取原则 | 166 |
| Agent 类比 | 167 |
| 核心映射 | 39 |
| subagent 轮次 | 7 轮 × 3 并行 |
| 总耗时 | ~50 分钟 |
