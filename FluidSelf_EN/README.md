[← Back to Home](../README.md) | [中文入口 →](../FluidSelf_CN/README.md)

# FluidSelf

> Tired of re-explaining your preferences every new conversation?
> Let Agent remember you — recalibrate anytime, takes effect immediately.
> No personality diagnosis, no labels — just adapts to how you work.

[![License: MIT](https://img.shields.io/badge/License-MIT-green.svg)](../../LICENSE)
[![Agent Rule](https://img.shields.io/badge/Agent-Rule-blue)](https://github.com/SimonCoreLab/fluidself)

[中文入口 / Chinese →](../FluidSelf_CN/README.md)

---

## What Is This?

**FluidSelf** is a universal agent behavior rule set that lets Agent dynamically adapt its communication style based on your collaboration preferences.

It doesn't slap fixed labels on you or say "you're an XXX." Instead, it uses **6 behavioral axes** to precisely describe your preferences across different work scenarios, then silently adapts in every conversation.

### Core Philosophy

| Other approaches | The FluidSelf Way |
|-----------------|-------------------|
| Slapping fixed labels ("You are an XXX") | Only records behavioral preferences, never outputs labels |
| One assessment, permanent label | Adjustable per task — style is fluid |
| 16 fixed types | 6 axes × 2 poles = 64 combinations |

---

## See It in 30 Seconds

**Before (Default Agent):**
> Alright, let me help you analyze this. First, we need to understand your context and requirements, then step through several possible causes... (200 words later)
> So my recommendation is to try Option B.

**After (FluidSelf · conclusion-first + logic-priority):**
> Recommend Option B. Two reasons: 40% better performance, more scalable. Code below...

**After (FluidSelf · detailed-walkthrough + value-alignment):**
> Interesting direction. Let me map out the layers involved — which ones matter most to you?

---

## Quick Start

### Installation

FluidSelf is a universal agent rule set, not tied to any specific platform.

**Option 1: As Rule Files (Recommended)**

Copy your language version of choice into your agent's rules/skills directory:

| Platform | Action |
|----------|--------|
| WorkBuddy | Place in `~/.workbuddy/skills/` |
| Cursor | Place in `.cursor/rules/`, or add via Rules panel |
| Claude Code | Place in `~/.claude/` and reference `SKILL.md` |
| CodeBuddy Code | Place in `~/.codebuddy/skills/` |
| Generic Agent | Inject `SKILL.md` and `references/` as system prompts |

**Option 2: Via Git**

```bash
git clone https://github.com/SimonCoreLab/fluidself.git
# For English users:
cp -r fluidself/FluidSelf_EN/ ~/.workbuddy/skills/fluidself/
# For Chinese users (中文用户):
cp -r fluidself/FluidSelf_CN/ ~/.workbuddy/skills/fluidself/
```

### First Use

FluidSelf auto-detects whether you have a config. On first use, you'll go through 4 rounds of lightweight A/B choices (no questionnaire vibes):

1. "I care more about the big picture and frameworks" vs. "I focus on concrete facts first"
2. "Logic and objective criteria come first" vs. "Values and interpersonal impact come first"
3. "I like a clear plan before executing" vs. "I prefer exploring as I go"
4. "Ask less, do more, deliver complete results" vs. "Check in at key points for alignment"

After 4 rounds, the agent runs on your preferences. Say "try a different way" anytime to adjust.

Your current session config is stored in persistent memory. Say "show my style config" anytime to check.

### How to Adjust Style

No trigger words to memorize. When you want to adjust, just say how you feel:

> "Too wordy" / "Just give me the conclusion"
> "Expand on that" / "More detail please"
> "Too blunt, be gentler" / "Don't sugarcoat it"
> "Try a different way" / "Give me two versions to compare"

The agent auto-detects the direction from your expression and adjusts. The more natural, the better.

---

## Architecture: Three-Phase State Machine

FluidSelf doesn't label you — it tunes in like a radio: listens, remembers, gets more accurate over time.

```
Onboarding (Phase 0)     Working (Phase 1)          Reflection (Phase 2)
Discover preferences        Adapt style                  Continuously fine-tune
     │                          │                              │
     ▼                          ▼                              ▼
  Onboarding  ────────►      Working  ──────────►       Reflection
  4 rounds of natural        Adapt output per             L1 Lightweight feedback after task
  conversation to converge   6-axis config                L2 Summary when same-domain count
  6-axis initial values      Auto domain detection          hits interval threshold (default: 3)
                             Real-time fix on               L3 Update config upon confirmation
                             dissatisfaction signals
                                │                            │
                                └────────────────────────────┘
                                      Feedback loop (closed)
```

---

## The 6 Behavioral Axes

| # | Axis | A Side | B Side |
|---|------|--------|--------|
| 1 | Information Preference | Abstract frameworks, concepts first | Concrete cases, details first |
| 2 | Decision Style | Logic analysis, objective criteria | Value feeling, interpersonal consensus |
| 3 | Action Mode | Plan-driven, convergent execution | Exploration-driven, open & flexible |
| 4 | Interaction Habit | Ask less, do more, independent delivery | Confirm often, align step by step |
| 5 | Expression Density | Concise & essential | Rich & narrative |
| 6 | Tone & Attitude | Direct & matter-of-fact | Warm & encouraging |

6 independent axes, freely combined, forming **64 style configurations**.

---

## Domain Auto-Adaptation

| What you're doing | Scenario | How Agent adjusts |
|-------------------|----------|----------------|
| Coding | Writing code, debugging, architecture design | More concise, logic-first, less fluff |
| Writing | Reports, articles, product copy | More narrative, encouraging tone, confirm feelings |
| Thinking | Breaking down problems, making decisions | Give frameworks, allow multiple paths, show reasoning |
| Communicating | Replying to messages, writing reports, external expression | Consensus-oriented, confirm alignment, provide alternatives |

---

## File Structure

```
fluidself/
├── FluidSelf_CN/                     # Chinese version (中文版)
│   ├── README.md
│   ├── SKILL.md
│   └── references/
│       ├── lens-profiles.md
│       ├── domain-rules.md
│       └── reflection-rules.md
├── FluidSelf_EN/                     # English version
│   ├── README.md                     ← You are here
│   ├── SKILL.md
│   └── references/
│       ├── lens-profiles.md
│       ├── domain-rules.md
│       └── reflection-rules.md
├── LICENSE
└── .gitignore
```

---

## Core Features

1. **Complete Coverage**: Full internal mapping logic; externally only presents behavioral dimensions
2. **Reflection Toggle**: L1/L2/L3 three-level reflection can all be turned off by you with one word
3. **Domain Auto-Adaptation**: Different task types (code/writing/thinking/communication) auto-adjust output style

---

## License

MIT License — see [LICENSE](../../LICENSE)

---

## Contributing

Issues and PRs welcome to improve axis definitions, supplement domain rules, or refine script templates.
