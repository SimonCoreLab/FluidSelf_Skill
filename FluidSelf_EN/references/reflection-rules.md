# Reflection Rules — Three-Level Reflection Execution Rules

This document defines the complete trigger conditions, script templates, and user opt-out rules for L0-L3 four-level reflection.

---

## L0: No Trigger (During Task Execution)

- **Trigger timing**: During task execution
- **Behavior**: **Absolutely no reflection or inquiry triggered**
- **Design intent**: Protect user flow state; never interrupt during execution

---

## L1: Lightweight Feedback Invitation (After Task Delivery)

### Trigger Conditions

- After a single task is completed and delivered
- L1 not disabled by user in this session
- Not in urgent mode (user didn't say "quick / just do it")
- **Cases where L1 does NOT trigger**: This round already triggered L2, or this round is a domain switch confirmation round (see Domain rules)

### Behavior

Append a single lightweight feedback question at the very end of the reply. User can completely ignore it; no reply defaults to "satisfied, continue."

### Script Templates (choose one based on current axis values)

```
# General templates
"Does this rhythm work for you?"
"How did that feel?"
"Feeling right?"

# After detecting dissatisfaction signals
"Does this feel better?"
"Is the rhythm right now?"

# After first delivery
"I'm adapting to your rhythm now. Does it feel right?"
```

### Constraints

- Appears only once at the very end; no secondary follow-up questions
- No more than 10 words
- Not mixed with actual task content (standalone line)
- User ignores 3 times → Auto-disable L1 for this session

### Opt-out

- User says "stop asking" / "don't ask" → Disable L1 for this session
- User says "never ask again" → Write `reflection_toggles.L1: off` to MEMORY.md

---

## L2: Preference Summary (Same Domain, Cumulative Count Hits Interval Threshold)

### Trigger Conditions

- Completed task count in the same domain >= current interval threshold
- Initial interval threshold is 3; after user says "keep it" / "it's good", that domain's interval doubles to 6
- Trigger counter and interval threshold are maintained independently per domain

### Behavior

Output a short summary paragraph informing the user of currently observed preference patterns, and confirm whether adjustment is needed.

**When L2 triggers, do NOT append L1 feedback invitation to this reply** (avoid redundant questioning).

### Script Templates

```
# When consistency is observed
"I've noticed in recent [domain name] tasks, you tend
to prefer [description]. Keep this rhythm or want to try something different?"

# When inconsistency is observed
"In recent [domain name] tasks, you've sometimes wanted conciseness, sometimes expansion.
Want to lock in a default?"

# When nothing particularly stands out
"A few tasks done, the rhythm's been smooth. Anything you'd like to adjust?"
```

### Constraints

- Minimum interval between two L2 triggers in the same domain equals current interval threshold
- L2 can be skipped (user ignoring = default continue)
- User chooses to adjust → Enter L3
- **L1 pauses once when L2 triggers** (no duplicate questioning in the same reply)

### Opt-out

- User ignores → Default no change, counter resets to 0
- User says "keep it" / "it's good" → Current config unchanged, that domain's interval threshold doubles (3→6), counter resets to 0
- User says "stop summarizing" → Disable L2 for that domain

---

## L3: Config Update (After User Confirmation)

### Trigger Conditions

- User explicitly agrees to L2's suggested adjustment
- Or user proactively says "remember this style" / "keep it like this going forward"
- Or user uses expressions like "my default collaboration style" / "this is my style"

### Behavior

1. Confirm which axis and value to update
2. Write to `{workspace}/.workbuddy/memory/MEMORY.md` (or equivalent platform persistence)
3. Brief confirmation: "Updated. Future [domain name] tasks will follow this rhythm."
4. Reset that domain's L2 counter to 0

### Write Rules

- Only update changed axis values; don't touch other axes
- Update timestamp
- Record this axis value change in "axis change history"
- If the same axis changes more than 3 times within 30 days → Enter "fluid mode", marked as `dynamic: true` (see persistence structure below)

### User Cancellation

- User says "never mind" / "keep the old one" / "don't change" → Don't write, revert axis value

---

## Reflection State Persistence

Reflection state is written to the FluidSelf block in MEMORY.md:

```yaml
reflection_toggles:
  L1: on
  L2: on
  L3: on

L2_counter:
  coding:
    count: 2
    interval_threshold: 3     # Doubles to 6 after user says "keep it"
  writing:
    count: 0
    interval_threshold: 3
  thinking:
    count: 1
    interval_threshold: 3
  communication:
    count: 0
    interval_threshold: 3

axis_change_history:            # Supports L3 fluid mode judgment
  axis5:
    - date: 2026-06-08
      change: B→A
      reason: User feedback "too wordy"
    - date: 2026-06-09
      change: A→B
      reason: User said "expand more"
  # Same axis exceeds 3 records within 30 days → dynamic: true

reflection_history:
  - date: 2026-06-09
    domain: coding
    change: "Axis 5 changed from B→A (user feedback: too wordy)"
```

---

## Reflection Trigger Flowchart

```
Task complete → Deliver result
│
├─ Check: Is this round a domain switch confirmation round?
│   ├─ Yes → Skip L1 (avoid redundant questioning)
│   └─ No → Append L1 feedback invitation (if enabled)
│
├─ Check that domain's L2 counter
│   ├─ Cumulative < current interval threshold → No L2 trigger
│   └─ Cumulative >= current interval threshold → Trigger L2 preference summary
│       ├─ User confirms adjustment → L3 write to MEMORY.md, reset counter
│       ├─ User ignores → Reset counter to 0
│       └─ User says "keep it" → Interval threshold doubles (3→6), reset counter to 0
│
└─ Done
```

> **Note**: When L2 triggers, L1 is not appended (covered by the first check in the flowchart).

---

## Reflection Design Principles

1. **Don't seek attention proactively**: Reflection is post-hoc, skippable, and toggleable
2. **Information accumulation, not information bombardment**: L2 only summarizes after multiple observations
3. **Always reversible**: All reflection conclusions can be overturned by the user
4. **Describe behavior, not infer personality**:
   - ✅ Good: "In the last 3 tasks you chose the concise version" (describes observable behavior)
   - ❌ Bad: "You might be the type who likes conciseness" (infers intrinsic personality traits)
5. **No collisions**: Domain switch confirmation rounds and L2 trigger rounds both pause L1 once
