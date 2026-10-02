Here’s a practical system to keep the agent (worker) aligned with you (boss).

### 1. Make Goals Explicit and Measurable (Most Important)

Never give vague goals. Always force the agent to restate them.

**Prompt to use at the start of important work:**

```text
Before doing any work, restate my goal in your own words.

Then answer:
1. What does success look like?
2. What are the hard constraints (things you must never do)?
3. What are the soft preferences?
4. What would count as failure or misalignment?

Wait for my confirmation before proceeding.
```

This surfaces misunderstandings early.

---

### 2. Force a Plan → Approval Loop

Never let the agent jump straight into action on important tasks.

```text
First propose a short plan (max 5–7 steps).
For each step, state:
- What you will do
- Why it serves my goal
- What could go wrong

Do not execute anything until I say “Approved”.
```

This is one of the highest-leverage alignment techniques.

---

### 3. Regular Alignment Checkpoints

Insert these during long sessions:

**Mid-work check:**

```text
Pause. 
Current status vs original goal:
- What have you achieved so far?
- Are we still on track with my original intent?
- Have you drifted in any way? Be honest.
```

**Before finishing:**

```text
Before wrapping up, do an alignment review:
1. Does the final result fully satisfy my original goal?
2. Did you take any shortcuts or make assumptions I didn’t approve?
3. What would I most likely criticize?
```

---

### 4. Use a Verification / Critic Subagent (Strong Method)

Spawn an independent subagent whose only job is to check alignment:

```text
Spawn a Critic subagent with a fresh context.

Hand off only:
- My original goal and constraints
- What the main agent actually did / produced

Instruct the Critic:
“Your only job is to detect misalignment with the boss’s goals. 
Be strict. Point out any drift, unapproved assumptions, or missing requirements. 
Do not be polite or helpful.”
```

This is very effective because the critic doesn’t share the main agent’s context or rationalizations.

---

### 5. Maintain a Living “Goal Spec”

Keep a short file (e.g. `GOAL.md` or section in `AGENTS.md`) that contains:

- Current high-level goal
- Hard constraints
- Definition of Done
- Things to avoid

Then tell the agent:

```text
Before major decisions, re-read GOAL.md and confirm you are still aligned with it.
```

---

### 6. Simple Alignment Score (Optional but useful)

At the end of a session, ask:

```text
Rate your own alignment with my original goal from 1–10.
Explain any points you deducted.
What would raise the score to 10?
```

Over time you will see patterns in where agents tend to drift.

---

### Quick Daily Practice

| Stage         | What to do                           | Purpose                      |
| ------------- | ------------------------------------ | ---------------------------- |
| Start         | Force goal restatement + constraints | Catch misunderstanding early |
| Planning      | Require plan + approval              | Prevent unaligned action     |
| During work   | Mid-point alignment check            | Catch drift early            |
| Before finish | Self-critique + Critic subagent      | Final quality gate           |
| After finish  | Alignment score + lessons            | Continuous improvement       |

---

Would you like me to turn this into a ready-to-paste **Alignment Protocol** section you can put directly into your `AGENTS.md`?
