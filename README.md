<div align="center">
  <img src="https://gw.alipayobjects.com/zos/k/2h/waza.svg" width="120" />
  <h1>Waza</h1>
  <p><b>Engineering habits you already know, turned into skills Claude can run.</b></p>
  <a href="https://github.com/tw93/Waza/stargazers"><img src="https://img.shields.io/github/stars/tw93/Waza?style=flat-square" alt="Stars"></a>
  <a href="https://github.com/tw93/Waza/releases"><img src="https://img.shields.io/github/v/tag/tw93/Waza?label=version&style=flat-square" alt="Version"></a>
  <a href="LICENSE"><img src="https://img.shields.io/badge/license-MIT-blue.svg?style=flat-square" alt="License"></a>
  <a href="https://twitter.com/AiTw93"><img src="https://img.shields.io/badge/follow-Tw93-red?style=flat-square&logo=Twitter" alt="Twitter"></a>
</div>

<br/>

## Why

Waza (技) is a Japanese martial arts term for technique: a move practiced until it becomes instinct.

AI makes you faster, but not better:

- Your UI looks like every other AI-generated interface. Same spacing, same components, same defaults.
- Debugging becomes guess-and-retry. You patch symptoms instead of tracing root causes.
- Your docs hedge every claim and bloat every paragraph. Engineers can tell it wasn't thought through.
- You skim summaries instead of reading the source. Understanding stays shallow.

Waza encodes the fix for each of these into a skill Claude can execute.

<img src="https://gw.alipayobjects.com/zos/k/qa/waza_repaired_v4.svg" width="800" />

## Skills

Each engineering habit gets a [Claude Code skill](https://docs.anthropic.com/en/docs/claude-code/skills). Type the slash command, Claude follows the playbook.

| Skill | When | What it does |
| :--- | :--- | :--- |
| [`/think`](skills/think/SKILL.md) | Before building anything new | Depth-classified planning (Lightweight/Standard/Deep). No code until design is approved. Gotchas from 7 real failure cases. |
| [`/design`](skills/design/SKILL.md) | Building frontend interfaces | Locks an aesthetic direction before touching code. Enforces typography rules and a distinctiveness check against generic defaults. |
| [`/hunt`](skills/hunt/SKILL.md) | Any bug or unexpected behavior | Known failure shapes table, rationalization watch, hard stop after 3 failed hypotheses. Root cause stated at file:line before any fix. |
| [`/check`](skills/check/SKILL.md) | After a task, before merging | Security and architecture reviewer agents, destructive command hook, 3-tier review depth, autofix for safe issues. |
| [`/write`](skills/write/SKILL.md) | Writing or editing prose | Strips AI writing patterns. Bilingual references for Chinese and English. Language auto-detected from the text, not the prompt. |
| [`/learn`](skills/learn/SKILL.md) | Diving into an unfamiliar domain | 3 modes (deep/quick/write-to-learn), 6 phases from collection to published output. Source verification at each layer. |
| [`/read`](skills/read/SKILL.md) | Any URL or PDF | 3-stage proxy cascade script. Dedicated handlers for WeChat articles and Feishu docs. Output is clean Markdown. |
| [`/health`](skills/health/SKILL.md) | Auditing Claude Code setup | 6-layer audit framework, 2 specialist agents, MCP live check, severity classification for every finding. |

Each skill is a folder, not just a markdown file. Skills include reference docs, helper scripts, scoped hooks, and gotchas sections built from real project failures.

## Extras

**Statusline** shows context window and quota usage with color-coded thresholds. No progress bars, no noise.

<img src="https://gw.alipayobjects.com/zos/k/y9/RUgevg.png" width="800" />

```bash
curl -sL https://raw.githubusercontent.com/tw93/Waza/main/scripts/setup-statusline.sh | bash
```

**English Coaching**: English should be every engineer's first language when working with AI. The model thinks in English, the best resources are in English, and writing clearly in English is a skill that compounds over time. Passive grammar correction on every reply, with pattern names so you learn why.

> 😇 I very like this feature → I really like this feature (Unnatural phrasing)

```bash
curl -sL https://raw.githubusercontent.com/tw93/Waza/main/templates/english-coaching.md >> ~/.claude/CLAUDE.md
```

## Install

```bash
npx skills add tw93/Waza -g -y
```

Install a single skill:

```bash
npx skills add tw93/Waza -a claude-code -s health -y
```

Replace `health` with any skill name. Requires Node 18+ and Claude Code.

## Background

Tools like Superpowers and gstack are impressive, but they are heavy. Too many skills, too much configuration, too steep a learning curve for engineers who just want to get things done.

Every gotcha in a skill traces to a specific failure: a wrong code path that cost four rounds of debugging, a release announced before artifacts were uploaded, a server restarted eight times without reading the error. 30 days of real usage (300+ sessions, 7 projects, 500 hours) turned these into the rules each skill enforces. The goal is not completeness. It is the right amount, done well.

The `/health` skill is based on the six-layer framework described in [this post](https://tw93.fun/en/2026-03-12/claude.html).

## Support

- If Waza helped you, star the repo or [share it](https://twitter.com/intent/tweet?url=https://github.com/tw93/Waza&text=Waza%20-%20Claude%20Code%20skills%20for%20the%20complete%20engineer.) with friends.
- Have ideas or found bugs? Open an issue or PR.
- Like Waza? <a href="https://miaoyan.app/cats.html?name=Waza" target="_blank">Buy Tw93 a Coke</a> to support the project.

<details>
<summary>🥤 Supporters</summary>

<a href="https://miaoyan.app/cats.html?name=Waza"><img src="https://rawcdn.githack.com/tw93/MiaoYan/vercel/assets/sponsors.svg" width="1000" loading="lazy" /></a>

</details>

## License

MIT License. Feel free to use Waza and contribute.
