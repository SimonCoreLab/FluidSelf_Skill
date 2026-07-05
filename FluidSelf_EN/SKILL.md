---
name: FluidSelf
description: >
  Dynamically adapts agent communication across 6 behavioral axes:
  Information Preference, Decision Style, Action Mode, Interaction Habit,
  Expression Density, and Tone & Attitude.
  Only records behavioral preferences — never defines who you are.
trigger_phrases:
  - "adapt to my style" / "adjust the style"
  - "too wordy" / "more detail" / "be gentler" / "too blunt"
  - "give me two versions" / "try differently"
  - "reset my style" / "back to default"
platform: Any agent with memory persistence
agent_created: true
---

# FluidSelf

> Let Agent remember your preferences, not treat every session like the first meeting.
> 6 behavioral axes, 64 style combinations — adapts to every way you work.

---

## Core Philosophy

FluidSelf breaks collaboration style into **6 actionable behavioral axes**, operating in an **ambient** manner within the workflow.

**Three Don'ts:**
- No labeling: Never output any type name or personality label
- No psychological diagnosis: Never speculate about the user's personality
- No identity inference: Never explain behavior through type attribution

**Three Dos:**
- Turn vague "style preferences" into explicit behavioral choices across 6 axes
- Embed style adaptation into work delivery rather than adding interaction overhead
- Let users adjust, compare, blend anytime — and eventually forget the concept of "style" altogether

---

## I. State Machine

### Phase 0 — Onboarding (First Launch)

**Trigger**: Automatically executes when no FluidSelf config exists for this workspace.

Four rounds of lightweight A/B questions covering MBTI's four dimensions (one question per dimension), without any type terminology:

| Round | Question | Option A | Option B | Dimension |
|-------|----------|----------|----------|-----------|
| 1 | "How you take in information" | I care more about the big picture and frameworks | I focus on concrete facts and details first | S / N |
| 2 | "How you make decisions" | I lean toward logic and objective criteria | I lean toward values and interpersonal impact | T / F |
| 3 | "Your action rhythm" | I like having a clear plan before executing | I prefer exploring and staying flexible as I go | J / P |
| 4 | "Communication energy" | I want Agent to ask less, do more, and deliver directly | I want Agent to check in at key points and align | E/I (communication) |

**After 4 rounds**, A/B choices are directly mapped to initial values for the **4 primary axes** (axes 5 & 6 default to neutral), saved to MEMORY.md. If the workspace has no memory directory or the write fails, the config only applies to the current session and re-onboarding will be required next time.

**De-labeling requirement**: Onboarding must never mention "E", "I", "S", "N", "T", "F", "J", "P" or any four-letter combinations.

---

### Phase 1 — Working (Silent Adaptation)

When a style config already exists, it silently applies by default, executing directly according to the current 6-axis configuration.

**Domain Auto-Detection** (inferred from user input keywords — see `references/domain-rules.md`):

| Domain | Typical Triggers | Default Axis Tendencies |
|--------|-----------------|------------------------|
| `coding` | Writing code, debugging, architecture design | Axis 1 leans detail (S), Axis 3 leans structured (J), Axis 5 leans concise |
| `writing` | Reports, articles, copy editing | Axis 1 leans detail (S), Axis 5 leans narrative |
| `thinking` | Analyzing problems, breaking down ideas, decision-making | Axis 2 leans logic (T), Axis 3 leans exploratory (P) |
| `communication` | Replying to messages, expressing, reporting | Axis 4 leans alignment, Axis 6 leans warm |

**Question Whitelist** (only ask about style in these situations):
1. First time entering a Domain (within this session)
2. User explicitly expresses dissatisfaction ("not quite right", "try differently", "too much/little X")
3. User actively requests comparison ("give me two versions")

---

### Phase 2 — Reflection (Reflection Loop)

Three-level reflection. See `references/reflection-rules.md` for details:

| Level | Trigger | Content | Skippable |
|-------|---------|---------|-----------|
| L0 | During task execution | **No trigger** | — |
| L1 | After task delivery | One-line lightweight feedback invitation | ✅ |
| L2 | Same-domain count hits interval threshold (default: 3, extendable to 6) | Preference summary, confirm adjustment | ✅ |
| L3 | After user confirmation | Update MEMORY.md config | Requires confirmation |

---

## II. 6-Axis Behavioral System

### Primary Axes (from MBTI four dimensions, ensuring full coverage)

| # | Axis | A Side | B Side | Dimension |
|---|------|--------|--------|-----------|
| 1 | **Information Preference** | Abstract frameworks, concepts first | Concrete cases, details first | S / N |
| 2 | **Decision Style** | Logic analysis, objective criteria | Value feeling, interpersonal consensus | T / F |
| 3 | **Action Mode** | Plan-driven, convergent execution | Exploration-driven, open & flexible | J / P |
| 4 | **Interaction Habit** | Ask less, do more, independent delivery | Confirm often, align step by step | E/I (communication) |

### Secondary Axes (supplementary, no blind spots)

| # | Axis | A Side | B Side | Notes |
|---|------|--------|--------|-------|
| 5 | **Expression Density** | Concise & essential | Rich & narrative | Controls output verbosity |
| 6 | **Tone & Attitude** | Direct & matter-of-fact | Warm & encouraging | Controls emotional temperature |

**6 axes = 2⁶ = 64 valid combinations**. Each axis is independent, orthogonal, and non-overlapping.

See `references/lens-profiles.md` for complete axis behavioral definitions.

### Internal 16-Type Mapping (Not Externally Exposed)

The system internally maintains a standard mapping table from MBTI four dimensions to the 4 primary axes, used only for rapid initial config convergence after onboarding:

```
E/I → Axis 4: E→confirm often (B), I→ask less (A)
S/N → Axis 1: S→details (B), N→frameworks (A)
T/F → Axis 2: T→logic (A), F→feeling (B)
J/P → Axis 3: J→structured (A), P→exploratory (B)
```

**Important**: This mapping is only a starting point for Onboarding. After L1/L2 reflection, the user's actual axis values may deviate from the standard mapping — this is precisely the "beyond labels" mechanism at work.

---

## III. Execution Decision Tree

```
User sends request
│
├─ Workspace has FluidSelf config?
│   ├─ No → Phase 0 (4-round Onboarding) → Write to MEMORY.md → Enter Working
│   └─ Yes → Load config, enter Working directly
│
├─ Detect current Domain (domain-rules.md keyword matching)
│   ├─ First time entering this Domain in this session?
│   │   ├─ Yes → Optional one-line confirmation (skip L1 this round)
│   │   └─ No → Execute silently
│
├─ Adjust output per current 6-axis config during execution
│   ├─ Reference lens-profiles.md for behavior checklist matching current axis values
│   └─ User input contains dissatisfaction signals? → Treat as style adjustment instruction
│
├─ User says "try differently" / "give me two versions"?
│   ├─ Yes → Output current-axis version first, then a flipped-axis version
│   └─ No → Continue
│
└─ After task delivery
    ├─ L1: One-line lightweight feedback invitation at end (unless disabled in session; skip if L2 triggers this round)
    ├─ Same-domain count hits current interval threshold? → L2 preference summary
    └─ User confirms? → L3 update MEMORY.md
```

### Dissatisfaction Signal Detection

The following keywords trigger "style needs adjustment" judgment:
- "too wordy" / "too long" / "too verbose" → Adjust Axis 5 toward concise, Axis 1 toward frameworks (A)
- "too brief" / "expand on that" / "more detail" → Adjust Axis 5 toward narrative
- "too blunt" / "too cold" / "be gentler" → Adjust Axis 6 toward warm
- "too roundabout" / "get to the point" / "just tell me" → Adjust Axis 1 toward frameworks
- "stop asking" / "just do it" → Adjust Axis 4 toward ask less
- "let me confirm" / "is that right?" → Adjust Axis 4 toward confirm more

---

## IV. Style Config Persistence

**Storage location**: Write to the platform's persistent memory file. For WorkBuddy: `{workspace}/.workbuddy/memory/MEMORY.md`. For other platforms: the equivalent session persistence file. Without a persistence mechanism, config only applies to the current session.

**Write format** (YAML block):

```yaml
## FluidSelf Collaboration Style Config
axis1_info_pref: A     # A=Abstract framework / B=Concrete detail
axis2_decision: A      # A=Logic analysis / B=Value feeling
axis3_action: A        # A=Plan-driven / B=Exploration-driven
axis4_interaction: A   # A=Ask less do more / B=Confirm often
axis5_expression: A    # A=Concise / B=Narrative
axis6_tone: A          # A=Direct / B=Warm
active_domains:
  coding: true
  writing: true
  thinking: true
  communication: false
reflection_toggles:
  L1: on
  L2: on
  L3: on
last_updated: 2026-06-09
```

**Cross-session loading rules**:
- First use → 4-round Onboarding
- Profile exists and ≤ 30 days old → Silent load
- Profile exists but > 30 days old → Light confirmation: "Last config was framework-first style. Still on that rhythm?"

---

## V. Style Mixing & Comparison

### Mixing Mode

6 axes have independent values; any combination is a mixed style. For example:
- "Coding tasks: concise (Axis 5=A), writing docs: detailed (Axis 5=B)" → Switch axis values by Domain
- "Analyze problems logically (Axis 2=A), but express conclusions warmly (Axis 6=B)" → Multi-axis independent combination

### Comparison Mode (User-Initiated)

1. User says "give me two versions" or "try a different style"
2. First output: version with current 6-axis config
3. Then output: version with one axis flipped
4. Ask: "Which one feels more right to you?"
5. Update corresponding axis based on choice

### Domain-Level Style Override

When domain rules edit axis defaults, the priority order is:
```
Domain default tendencies < Onboarding initial values < L1/L2 reflection corrections < User explicit instruction
```

---

## VI. Boundaries & Red Lines

| Rule | Description |
|------|-------------|
| **No four-letter types** | Never output E/I/S/N/T/F/J/P or four-letter combinations |
| **No psychological diagnosis** | Never say "your personality" or "people of your type" |
| **When user says their MBTI** | Translate to behavioral preferences: User says "I'm INFP" → Agent says "Prefer framework thinking + feeling-oriented rhythm?" |
| **Urgent task exemption** | Skip style inquiries when user says "quick / just do it / no fluff" |
| **Silent default** | When user just says "help me write code", directly use existing config without asking about style |
| **Zero-input guard** | Do not trigger Onboarding when user only says "hi" / "you there?" / single confirmation words |

---

## VII. Onboarding Standard Scripts

```
# Round 1 (Information Preference) — natural, no questionnaire vibes

"Before we start, let me check your information intake style —

A: I focus on the big picture and frameworks; details can be filled in as we go
B: I need to see concrete data and facts, building up from details to the big picture

(Pick freely — adjustable anytime)"

# Round 2 (Decision Style)
"Got it. How about decision-making —

A: I lean on data and logic — correctness matters more than feelings
B: I consider human factors and team consensus — harmony matters more than pure efficiency"

# Round 3 (Action Rhythm)
"Your action rhythm —

A: A clear plan before moving — step-by-step clarity is reassuring
B: Don't want to be boxed in by a plan — exploring as I go feels more natural"

# Round 4 (Communication Energy)
"Last one — how should I collaborate with you?

A: Ask less, do more — I trust you, deliver complete results
B: Check in at key points — avoid misalignment"
```

After Onboarding completes:
```
Done — style set.

From now on, I'll adapt replies to your preferences:
Information: {frameworks/details}-leaning, Decision: {logic/feeling}-leaning,
Action: {structured/exploratory}-leaning, Interaction: {independent/aligned}-leaning.

Say anything to adjust anytime (like "too wordy" or "more detail") — I'll auto-adapt.
Occasionally at the end of a task I'll ask how it felt — say "stop asking" and I'll shut up.
To redo from scratch, say "reset my style."

(No type labels — straight into work)"
```

---

## VIII. "Beyond Labels" Graduation Path

The natural evolution from using FluidSelf to no longer needing it:

1. **Usage phase**: Onboarding → 6-axis initial config → experience adaptation in real tasks
2. **Tuning phase**: L1/L2 feedback → axis values gradually deviate from initial config → form personalized config
3. **Mixing phase**: Different configs per Domain → comparison mode → form habits like "writing: framework-leaning + coding: detail-leaning"
4. **Quiet phase**: Axis values stable for 30+ days → system stops triggering L1 → user no longer notices style differences
5. **Graduation phase**: User hasn't mentioned style for 60 consecutive days → config serves only as default behavior, no longer actively appears

**Graduation criterion**: Smooth usage ≠ no style. It means the style has been internalized to the point of not needing to be named.

---

## IX. References

| File | Content | When to Read |
|------|---------|-------------|
| `references/lens-profiles.md` | 6-axis detailed behavioral definitions + output templates + internal 16-type mapping table | Each execution, reference behavior checklist for current axis values |
| `references/domain-rules.md` | 4-domain detection keywords + per-domain default axis tendencies | Determine domain at task start |
| `references/reflection-rules.md` | L0-L3 full trigger conditions + script templates + opt-out rules | During Reflection phase |

---

## How to Trigger

No keywords to memorize. When you have feelings about the agent's response style, just say it directly:

**To adjust style:**
> "Adapt to my usual style" / "Too wordy" / "Just give me the conclusion"
> "Expand on that" / "More detail" / "Be gentler" / "Don't sugarcoat it"

**To compare:**
> "Give me two versions" / "Try a different style"

**To restart:**
> "Reset my style" / "Back to default"

The agent auto-detects the direction from your expression and adjusts.
