---
name: write
description: Use only when explicitly asked to write or edit prose. Not for code comments, commit messages, or inline docs. 融合 Waza 写作风格指南 + humanizer AI 去痕
version: 2.6.0
disable-model-invocation: true
allowed-tools:
  - Read
  - Write
---

# Write: 写作风格 + AI 去痕

## Language Detection

检测**被编辑文本**（不是用户的指令）的语言：
- 含中文字符 → 使用 `references/write-zh.md` + `humanizer-zh` 的 24 种 AI 模式
- 英文或混合 → 使用 `references/write-en.md`

如果受众不明确（博客读者？RFC？邮件？），先问清楚再编辑。

## Two Modes

### Mode 1: General Writing (Waza Style)

按照 Waza 写作风格指南执行：
- 读取 `references/write-zh.md`（中文）或 `references/write-en.md`（英文）
- 严格按规则处理
- 输出修改后的内容

### Mode 2: AI Text Rewriting (Humanizer)

当需要去除 AI 写作痕迹时，同时参考 `humanizer-zh` 或 `humanizer-en`：

**24 种 AI 写作模式检测（中文）**：
- 内容模式：过度强调意义、-ing 肤浅分析、宣传式语言、模糊归因等
- 语言语法：AI 词汇（此外、至关重要）、否定式排比、三段式法则
- 风格模式：破折号过度、粗体过度、表情符号
- 交流模式：协作痕迹、知识截止声明、填充短语

**英文 AI 模式检测**：
- filler phrases、negative parallelism、tricolon abuse
- rhetorical self-questions、false agency、anaphora abuse
- Pompous copulas (serves as, represents)

## Core Principle

不仅要"干净"，更要"鲜活"：
- 有观点，不只报告事实
- 变化节奏，混合长短句
- 承认复杂性
- 适当使用"我"
- 允许一些混乱
- 对感受要具体

## Execution

1. 读取对应规则文件
2. 严格按规则处理
3. 输出修改后的内容后停止

除非用户主动询问，否则不解释改动。
