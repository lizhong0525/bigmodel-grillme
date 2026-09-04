# 大模型标准节点清单与解析要点

逐项核对目标模型。模型没有的组件明确写"无该组件"，不要套用。

## 目录
- 整体架构
- 输入侧
- 核心块（Transformer block）
- 输出侧
- 训练与对齐
- 推理优化
- 多模态扩展（如适用）

## 整体架构
- 骨干类型：decoder-only / encoder-decoder / encoder-only；稠密 / MoE；混合架构（如 Mamba-Transformer 混合、RetNet）。
- 规模数字：总参数量、激活参数量（MoE）、层数、hidden size、上下文长度、词表大小。

## 输入侧
- **分词器**：算法（BPE / SentencePiece / tiktoken 系）、词表大小、特殊 token、多语言/代码覆盖策略。
- **嵌入层**：token embedding 维度、是否与输出头共享权重（weight tying）。
- **位置编码**：绝对 / 正弦 /  learned / RoPE（含 base、scaling 如 YaRN/NTK）/ ALiBi / NoPE；长上下文外推方案。

## 核心块
每层 Transformer block 内部：
- **注意力机制**：MHA / MQA / GQA（头数、KV 头数）/ MLA（DeepSeek 系）/ 滑动窗口 / 线性注意力；注意力归一化（QK-Norm 等）。
- **前馈网络**：标准 FFN（扩展比）/ SwiGLU 类门控 FFN / MoE（专家数、top-k、路由策略、共享专家、负载均衡 loss）。
- **归一化**：LayerNorm / RMSNorm；Pre-Norm / Post-Norm / Sandwich。
- **残差连接**：标准 / 缩放变体（如 DeepNorm）。
- **激活函数**：GELU / SiLU / SwiGLU 等。
- **偏置**：是否使用 bias（多数现代模型无偏置）。

## 输出侧
- **输出头**：线性投影到词表；是否与嵌入共享；logit 处理（soft-capping 等）。
- **解码策略**：温度、top-k/top-p、beam search——注意这是推理参数而非架构本身，明确区分。

## 训练与对齐（用户要求时）
- 预训练目标：CLM / MLM / UL2 / 多 token 预测（MTP）。
- 数据：规模、构成、清洗与配比策略。
- 对齐：SFT / RLHF（PPO、GRPO 等）/ DPO / RLVR。
- 训练基础设施：并行策略（DP/TP/PP/EP）、精度（BF16/FP8）、框架。

## 推理优化（用户要求时）
- KV cache 策略、量化方案、投机解码、分页注意力等。

## 多模态扩展（如适用）
- 视觉编码器（ViT 变体、分辨率策略）、音频编码器、模态投影器/连接器、统一 token 化方案。
