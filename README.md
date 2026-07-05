# FluidSelf

> Personality is fluid, not fixed. / 人格是流动的，不是固定的。
>
> [English →](FluidSelf_EN/README.md) &nbsp;|&nbsp; [中文 →](FluidSelf_CN/README.md)

[![License: MIT](https://img.shields.io/badge/License-MIT-green.svg)](LICENSE)
[![Agent Rule](https://img.shields.io/badge/Agent-Rule-blue)](https://github.com/SimonCoreLab/FluidSelf_Skill)

---

**FluidSelf** is a universal agent behavior rule set. It uses **6 behavioral axes** to dynamically adapt agent communication style — only behavioral preferences, never fixed labels.

**FluidSelf** 是一套通用 Agent 行为规则。用 **6 条行为轴** 动态调整 Agent 的沟通方式——只记录行为偏好，不定义身份。

---

## File Structure / 文件结构

```
fluidself/
├── FluidSelf_EN/          # English — SKILL.md + references
├── FluidSelf_CN/          # 中文 — SKILL.md + references
├── README.md              # You are here / 你在这里
├── LICENSE
└── .gitignore
```

Each language version is fully self-contained — pick the one you need and copy it into your agent's skills directory.

两个语言版本完全独立——选择你需要的，复制到 Agent 的 skills 目录即可。

---

## Quick Install / 快速安装

```bash
git clone https://github.com/SimonCoreLab/FluidSelf_Skill.git

# English users
cp -r FluidSelf_Skill/FluidSelf_EN/ ~/.workbuddy/skills/fluidself/

# 中文用户
cp -r FluidSelf_Skill/FluidSelf_CN/ ~/.workbuddy/skills/fluidself/
```

---

## License / 许可证

MIT — see [LICENSE](LICENSE)
