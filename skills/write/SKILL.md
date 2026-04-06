---
name: write
description: Use only when explicitly asked to write or edit prose. Not for code comments, commit messages, or inline docs.
version: 2.4.0
disable-model-invocation: true
---

# 写作风格 / Writing Style

检测**被编辑文本**（不是用户的指令）的语言：
- 含中文字符 → 加载 `references/write-zh.md`
- 否则（英文、混合、不确定）→ load `references/write-en.md`

执行顺序: 读取对应规则文件，严格按规则处理，输出修改后的内容。

输出修改后的内容后，停止。除非用户主动询问，否则不解释改动。
