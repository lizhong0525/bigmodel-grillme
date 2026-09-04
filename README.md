# LLM Architecture Analyzer

> 一个面向大语言模型 / 多模态模型的**架构解析技能**：把目标模型拆成可枚举的节点，逐节点按"是什么 / 内容是什么 / 做什么 / 目标 / 意义"五维解析，对含糊项执行反向 *grill-me* 自我拷问补全证据，并在条件允许时附架构流程图。

支持解析的模型示例：GPT、Llama、Qwen、DeepSeek、Kimi、Grok、Gemini、Claude、Mixtral，以及各类 MoE 模型。

## 使用方式

本项目是一个 [skill](https://github.com/) —— 把它克隆到任意兼容 skill 协议的 Agent（Kimi Code、Claude Code、Cursor、Codex 等）的 skills 目录下，然后对 Agent 说：

> "帮我拆解一下 DeepSeek-V3 的架构"
> "讲讲 Llama 3 每一层是干什么的"
> "Qwen2.5 用了什么注意力变体？"

Agent 会自动加载本技能，按 SKILL.md 中定义的工作流完成解析。

## 工作流程

1. **确认目标** — 锁定模型名称、版本、变体
2. **收集权威资料** — 官方论文 / 技术报告 / GitHub config / HuggingFace 模型卡
3. **拆解节点** — 按"整体 → 输入侧 → 核心块 → 输出侧 → 训练/推理"分层
4. **五维解析** — 每个节点回答 *是什么 / 内容是什么 / 做什么 / 目标 / 意义*
5. **反向 grill-me** — 对含糊项自我拷问，证据不足处明确标"未公开"
6. **架构流程图** — 条件触发，附 mermaid / graphviz 等可编辑图表

## 项目结构

```
bigmodel-grillme/
├── SKILL.md                    # 技能主文件（Agent 实际加载的就是这个）
├── README.md                   # 本文件，GitHub 访客入口
├── LICENSE                     # MIT 许可证
└── references/
    ├── node_taxonomy.md        # 标准节点清单，用于逐项核对
    ├── grill_me_checklist.md   # 反向拷问问题清单
    └── output_template.md      # 输出格式模板
```

## 核心原则

- **硬数字必带来源**：层数、hidden size、注意力头数等数字必须能追溯到论文 / 官方 config / HF 模型卡
- **查不到的写"未公开"**：绝不编造，不打"通常""大概"的擦边球
- **事实与推断分开**：推断处显式标注"（推断）"
- **五维一条不缺**：每个节点的"是什么 / 内容是什么 / 做什么 / 目标 / 意义"五项必须齐全

## 适用场景

- 想了解某模型的整体骨架与设计取舍
- 论文阅读前的"先看个架构概览"
- 对比同代模型（如 Llama 3 vs Qwen2.5 vs DeepSeek-V3）的架构差异
- 复现论文时确认关键超参

## 许可证

MIT License — 详见 [LICENSE](LICENSE)。
