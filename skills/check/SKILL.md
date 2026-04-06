---
name: check
description: Use after completing a task or before merging. Not for exploring ideas or debugging. 融合 Waza Check + code-review + verify-before-done
version: 1.6.0
allowed-tools:
  - Bash
  - Read
  - Edit
  - Write
  - Grep
  - Glob
  - Agent
  - AskUserQuestion
hooks:
  PreToolUse:
    - matcher: Bash
      hooks:
        - type: command
          command: "input=\"$CLAUDE_TOOL_INPUT\"; if echo \"$input\" | grep -qE 'git push --force|git push -f |rm -rf /|DROP TABLE|--no-verify'; then echo 'BLOCK: Destructive command during /check review. Confirm with user first.' >&2; exit 1; fi"
---

# Check: Review Before You Ship

Read the diff, find the problems, fix what can be fixed safely, ask about the rest. Do not claim done until verification has run in this session.

## Phase 1: Get the Diff

```bash
git fetch origin
git diff origin/main
```

If the base branch is not `main`, ask before running. Already on the base branch? Stop and ask which commits to review.

## Phase 2: Scope Check

Before reading the code, check for scope drift:
- Pull up recent commit messages and any task files
- Does the diff match the stated goal? Flag anything outside that scope: unrelated files, voluntary additions, missing requirements
- Label it: **on target** / **drift** / **incomplete**

## Phase 3: Six-Dimension Review

Based on `code-review` skill's 6-dimension analysis:

### Hard stops (P0-P1 — fix before merging)

| 维度 | 检查内容 | 严重性 |
|------|----------|--------|
| **安全漏洞** | XSS/注入/SSRF/竞态条件/密钥泄露 | P0-P1 |
| **正确性 Bug** | 逻辑错误、数据丢失 | P0-P1 |
| **Injection** | SQL/command/path injection | P0 |
| **External trust** | 未消毒的 LLM/API 输出 | P1 |
| **Missing cases** | enum/match 穷举性 | P1 |
| **Dependency changes** | 非预期的包版本变更 | P1 |

### Soft signals (P2-P3 — flag, do not block)

| 维度 | 检查内容 | 严重性 |
|------|----------|--------|
| **SOLID 原则** | SRP/OCP/LSP/ISP/DIP 违规 | P2 |
| **性能问题** | N+1查询/CPU热点/内存泄漏 | P1-P2 |
| **错误处理** | 异常吞没/异步错误/边界缺失 | P1-P2 |
| **边界条件** | 空值处理/空集合/数值限制 | P1-P2 |
| **死代码** | 未使用代码 | P2-P3 |
| **代码异味** | 命名/风格不一致 | P3 |

### Severity Levels

| 级别 | 名称 | 处理策略 |
|------|------|----------|
| **P0** | Critical | 必须阻止合并 |
| **P1** | High | 合并前应修复 |
| **P2** | Medium | 修复或创建后续任务 |
| **P3** | Low | 可选改进 |

## Phase 4: Fix Safe Items

Fix directly when the correct answer is unambiguous:
- Clear bugs, null checks on crash paths
- Style inconsistencies matching surrounding code
- Trivial test additions
- Dead code removal

Batch everything else into a single AskUserQuestion when the fix involves behavior changes, architectural choices, or anything where "right" depends on intent:

```
[N items need a decision]

1. [P0/P1] What: ... Suggested fix: ... Keep / Skip?
2. ...
```

## Phase 5: Judgment Quality

Ask three questions a senior reviewer would ask:

- **Right problem?** Does the diff solve what was actually needed, or a slightly different version of it?
- **Mature approach?** Is the implementation idiomatic for this codebase and language?
- **Honest edge cases?** Does the code handle nil, empty, zero, concurrent access, and upstream failure?

## Phase 6: Regression Coverage

For every new code path: trace it, check if a test covers it. If this change fixes a bug, a test that fails on the old code must exist before this is done.

## Phase 7: Verification

Auto-detect and run verification:

```bash
if [ -f Cargo.toml ]; then cargo check && cargo test
elif [ -f tsconfig.json ]; then npx tsc --noEmit && npm test
elif [ -f package.json ] && grep -q '"test"' package.json; then npm test
elif [ -f Makefile ] && grep -q '^test:' Makefile; then make test
elif [ -f pytest.ini ] || [ -f pyproject.toml ] || find . -maxdepth 2 -name "test_*.py" | grep -q .; then pytest
else echo "(no test command detected)"; fi
```

If nothing detected, ask the user for the verification command.

Paste the full output. Report exact numbers. Done means: command ran in this session and passed.

If verification fails: halt. Do not claim done.

## Sign-off

```
files changed:    N (+X -Y)
scope:            on target / drift: [what]
hard stops:       N found, N fixed, N deferred
signals:          N noted
new tests:        N
verification:     [command] → pass / fail
```

## Output Template (Structured Report)

```markdown
## Check 审查报告

**审查文件**: X个文件, Y行变更
**整体评估**: [APPROVE / REQUEST_CHANGES / COMMENT]

### 维度覆盖
- ✅ SOLID原则
- ✅ 安全漏洞
- ✅ 性能分析
- ✅ 错误处理
- ✅ 边界条件
- ✅ 死代码识别

---

## 发现问题

### P0 - Critical
(无 或 列表)

### P1 - High
- **[file:line]** 问题标题
  - **问题描述**: [详细说明]
  - **修复建议**: [具体方案]

### P2 - Medium
...

### P3 - Low
...

## 验证结果

### 已执行
- [PASS/FAIL] 命令A
  - 输出摘要：...

### 未执行
- 命令B
  - 原因：...

## 结论
- 是否满足完成标准：是/否
- 剩余风险：...
```
