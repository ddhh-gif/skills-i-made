**When calling subagents helps** (from official docs + research + practitioner consensus)

### Official guidance (Anthropic, OpenAI, LangChain)

**Subagents help in these clear cases:**

1. **Context protection / isolation**  
   The subtask generates a lot of intermediate noise (reading dozens of files, long search results, verbose logs).  
   Subagent works in its own clean context and only returns a short summary → main agent stays focused and doesn’t suffer context pollution.

2. **True parallelization**  
   Independent subtasks that don’t depend on each other (research different sources, review multiple files/documents, explore separate parts of a codebase).  
   Running them in parallel reduces wall-clock time.

3. **Specialization**  
   Different domains or toolsets (one agent for research, one for coding, one for verification/testing, one for compliance checking).  
   Each subagent gets a focused prompt + limited tools → better tool selection and less confusion.

4. **Fresh perspective / independent review**  
   Code review, verification, bug finding, or second opinion.  
   A new context window doesn’t inherit the main agent’s assumptions or blind spots (very commonly praised).

**Anthropic’s own research system**: A multi-agent setup (Opus lead + Sonnet subagents) beat single-agent Opus by **90.2%** on their internal research eval. They explicitly say multi-agent shines on breadth-first, parallelizable research tasks that exceed one context window.

**OpenAI docs**: Use multi-agent when work can be split into independent, bounded tasks (e.g. exploring separate parts of a large codebase, comparing multiple proposals, researching several sources in parallel). Prefer a single agent when steps are sequential or the task is small.

### When subagents usually do *not* help

- Tightly sequential / interdependent work (most coding changes)
- Simple tasks a single well-prompted agent can handle
- High coordination cost without clear parallel or context benefit
- When token cost / latency is more important than the quality gain (multi-agent systems often use 10–15× more tokens)

### Practical rule of thumb (from Anthropic + practitioners)

> Start with a single agent. Only introduce subagents when you can clearly point to one of these:
> 
> - Context is getting polluted
> - Real parallel independent work exists
> - You need a clean-slate review / verification
> - Specialization clearly improves tool use or focus

### Quick decision table

| Situation                                 | Subagents useful? | Why                          |
| ----------------------------------------- | ----------------- | ---------------------------- |
| Research many independent sources         | Yes               | Parallel + context isolation |
| Review / verify code or findings          | Yes               | Fresh perspective            |
| Large codebase exploration                | Yes               | Context protection           |
| Sequential coding / tightly coupled steps | No                | Coordination overhead hurts  |
| Simple single-domain task                 | No                | Unnecessary cost             |
| Many tools causing poor tool selection    | Yes               | Specialization               |

Would you like concrete prompt examples for the most common useful cases (research, code review, parallel file work)?



Here are ready-to-use prompt examples for the three most common cases where subagents actually help:

### 1. Research / Information Gathering (Context Isolation + Parallel)

```text
Use subagents to research the following topics in parallel. 
Each subagent should work in its own clean context and only return a concise summary.

Topics:
1. Current best practices for managing AI agent conversation history
2. Comparison of SAM 2 vs SAM 3 for object outline segmentation
3. Official recommendations from Anthropic and OpenAI on when to use multi-agent systems

For each subagent:
- Focus only on its assigned topic
- Read relevant sources thoroughly
- Return: Key findings (bullet points) + Important sources + Any strong disagreements in the literature

After all subagents finish, synthesize the results into a clear comparison and recommendation.
```

---

### 2. Code Review / Fresh Perspective

```text
I just finished implementing [feature / changes]. 

Spawn a subagent to perform an independent code review with a completely fresh context.

Instructions for the review subagent:
- Do not assume my previous reasoning is correct
- Check for bugs, edge cases, performance issues, and unclear logic
- Look for places where the code might overfit to the current tests
- Suggest concrete improvements
- Return only: 
  1. Critical issues (must fix)
  2. Important suggestions
  3. Minor nits

Do not let the review subagent see my previous planning or explanations — I want a clean second opinion.
```

---

### 3. Parallel File / Codebase Work

```text
I need to update the following independent parts of the codebase. 
Use parallel subagents so they don’t interfere with each other and keep the main context clean.

Tasks:
1. Update all API route handlers in /api/v2 to use the new error format
2. Refactor the authentication middleware to support the new token structure
3. Update the corresponding unit tests for both of the above

Rules for subagents:
- Each subagent works only on its assigned area
- Do not modify files outside its scope
- After finishing, return a short summary of what changed + any risks
- Wait until all subagents complete, then give me a final overview and any cross-cutting issues you notice
```

---

### Bonus: Verification Subagent (very high value)

```text
Before we finalize this change, spawn a verification subagent with a clean context.

Ask it to:
1. Re-read the original requirements
2. Check whether the current implementation fully satisfies them
3. Look for missing edge cases or silent failures
4. Report only problems and gaps (no praise)

I want an adversarial second opinion.
```

---





Here’s a much tighter, company-style version.  
The main agent is given **clear instructions on exactly what to hand off** and **what the subagent must return**.

---

### 1. Research (Clean Handoff + Parallel)

```text
You are the Lead Researcher. 

Break the following research question into independent workstreams and delegate them to subagents.

Research Question:
[Insert your research goal here]

For each subagent you create, you must hand off ONLY the following information:
- A clear, self-contained task description
- The exact scope (what to investigate and what to ignore)
- Preferred sources or constraints (if any)
- Required output format

Do NOT give subagents the full conversation history or unrelated context.

Each subagent must return:
1. Key findings (max 8 bullet points)
2. Important sources
3. Confidence level (High / Medium / Low)
4. Any major uncertainties or conflicting information

After all subagents finish, synthesize their results into a final recommendation. 
Keep the main context clean — only keep the summaries, not the raw research.
```

---

### 2. Code Review (Fresh Perspective + Strict Handoff)

```text
You are the Tech Lead.

I have just completed the following changes:
[Briefly describe what was done]

Spawn a Reviewer subagent with a completely fresh context.

When calling the subagent, hand off ONLY:
- The list of changed files
- The original requirements / acceptance criteria
- Any specific risks you want checked

Do NOT include your previous reasoning, planning, or justifications.

Instruct the Reviewer subagent to return strictly in this format:

**Critical Issues** (must fix)
- ...

**Important Suggestions**
- ...

**Minor Nits**
- ...

**Overall Risk Assessment**: Low / Medium / High

I want an independent second opinion, not confirmation of my thinking.
```

---

### 3. Parallel Implementation Work (Efficient Delegation)

```text
You are the Engineering Manager.

We need to complete these independent tasks in parallel:

1. [Task A]
2. [Task B]
3. [Task C]

For each task, create a dedicated subagent.

When handing off work to each subagent, provide ONLY:
- Clear task objective
- Exact files / modules they are allowed to touch
- Definition of Done
- Any constraints or style rules

Do not give them the full project history or other tasks’ context.

Each subagent must return:
- Summary of changes made
- Files modified
- Any risks or follow-up needed
- Confirmation that they stayed within scope

After all subagents complete, review their outputs for conflicts or integration issues, then give me a final status report.
```

---

### 4. High-Quality Verification (Adversarial)

```text
You are the Project Lead.

Before we ship, spawn a Verification subagent.

Hand off to the subagent ONLY:
- Original requirements
- Final implementation summary
- List of changed files

Instruct the subagent:
“Your job is to find gaps, missing edge cases, and incorrect assumptions. 
Do not be helpful or encouraging. Be strict and critical.
Return only problems and risks, ranked by severity.”

Keep the main conversation free of the verification process details — only retain the final findings.
```

---

### Key Principles Used (Company Style)

| Principle                | How it’s enforced                                      |
| ------------------------ | ------------------------------------------------------ |
| Minimal handoff          | Main agent is told exactly what information to pass    |
| Clean context            | Subagents get no conversation history                  |
| Clear contract           | Required output format is specified                    |
| Scope control            | Subagents are limited to specific files/tasks          |
| Synthesis responsibility | Main agent must combine results, not just forward them |

Would you like me to turn these into a reusable template you can keep in your `AGENTS.md`?
