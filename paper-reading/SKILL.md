---
name: paper-reading
description: Read a research paper in the current folder and produce a compact Obsidian Markdown note focused on interpreting the mathematical or physical problem behind the paper. Use this skill whenever the user asks to read, summarize, annotate, or turn a paper/PDF into notes, especially when they mention Obsidian, compact notes, mathematical explanation, physical intuition, modeling assumptions, derivations, proofs, formulas, variables, parameter tables, appendices, references, or method reconstruction. This skill includes the core of long-form-math and physics-style reasoning: intuition before formalism, typed variables, dimensional meaning, modeling choices, approximation logic, words between equations, proof strategy, and post-proof or post-derivation meaning.
metadata:
  mcpmarket-version: 1.0.0
---
# Paper Reading

Produce a dense, useful Obsidian note for the paper in the working folder. The note should preserve the paper's technical structure, but its center of gravity is the thought process behind the mathematical or physical problem: what is being modeled, what quantities matter, what assumptions make the problem tractable, why the equations have the form they have, and what the result means. Prefer substance over preface: do not add ceremonial openings, motivational filler, or generic conclusions.

## Core objective

Turn a paper into a compact but self-contained research note. The output should let the user recover six things quickly: what mathematical or physical problem the paper is really posing, what idealization or model converts the real problem into a solvable form, what method it uses, what every important symbol means, what parameters or experimental settings matter, and what references the paper relies on. When the paper contains equations, proofs, objectives, probabilistic models, algorithms, physical laws, or formal notation, use the built-in reasoning rules below rather than writing terse symbolic summaries.

## Chronological workflow

### 1. Identify the paper and reading target

1. Identify the target paper from the user prompt, the current directory, open tabs, or nearby PDF/Markdown files. If several plausible papers exist and the choice is genuinely ambiguous, ask one concise question; otherwise choose the most likely paper and state the filename in the note.
2. Determine the paper type before summarizing it: mathematical, physical, engineering, robotics, control, simulation, mechanics, optimization, scientific computing, experimental, theoretical, or system-oriented. This classification controls which later sections receive the most attention.
3. Preserve the paper's own terminology and notation, but normalize the final note into clean Obsidian Markdown with compact headings and retrieval-friendly tables.

### 2. Extract the raw material

1. Extract bibliographic metadata, abstract-level purpose, method, variables, experimental or theoretical setup, results, limitations, appendices, and references.
2. Collect all important definitions, equations, objectives, constraints, physical laws, conservation equations, scaling relations, algorithms, theorems, proofs, experimental settings, hyperparameters, and appendix tables.
3. If text extraction is imperfect, mark uncertain fields as `not reported`, `unclear in source`, or `inferred from <location>` rather than inventing details.
4. Expand common abbreviations on first use. In tables, prefer English full names with the abbreviation in parentheses, for example `Unmanned Aerial Vehicle (UAV)`, not only `UAV`.

### 3. Reconstruct the mathematical or physical problem

1. Identify the object or system being studied.
2. Identify the state variables, observable quantities, parameters, units, dimensions, and measurement setting.
3. Identify the governing principle, such as conservation, symmetry, variational minimization, equilibrium, stability, causality, uncertainty, invariance, force balance, energy minimization, probability modeling, or optimization.
4. Identify the constraints, boundary conditions, initial conditions, assumptions, and approximations that make the problem tractable.
5. Explain how the paper turns this structure into equations, algorithms, proofs, or experiments. The key question is not only “what method did the paper propose”, but “what structure of the problem did the authors exploit”.

For papers about mathematics, physics, engineering, robotics, control, simulation, mechanics, optimization, or scientific computing, make `数学/物理思想` the most important explanatory section. Do not reduce mathematical or physical interpretation to “the paper proposes method X”. Examples of the deeper structure include converting geometry into constraints, turning dynamics into an energy minimization problem, replacing a continuous process with a discretized state update, using symmetry to reduce variables, using a prior distribution to regularize an ill-posed inverse problem, or using a conserved quantity to control error.

### 4. Interpret formulas, derivations, and proofs

1. For each important definition, theorem, objective, constraint, physical law, conservation equation, scaling relation, or algorithmic formula, first state the motivating question or modeling need.
2. Give the intuition in plain language before presenting the formal expression.
3. Present the formal expression only after the reader knows why it exists.
4. Explain each symbol, assumption, approximation, and consequence.
5. Before an equation, say what it computes, enforces, optimizes, estimates, or bounds. After the equation, explain why the next step follows.

When a formula appears, ask what kind of statement it is. Is it a definition, a conservation law, a force balance, an energy functional, a probability model, an optimization objective, a constraint, a discretization, a scaling law, or an empirical fit? State this explicitly before explaining the formula. For physical quantities, include units or dimensions when available, and use dimensional reasoning to explain why terms can be added, compared, or normalized. If nondimensionalization, normalization, linearization, small-angle approximation, continuum approximation, quasi-static assumption, independence assumption, Gaussian assumption, or low-rank/sparsity assumption appears, explain what it buys and what it may lose.

For equations, never present bare equation chains. Use connective language such as “because”, “therefore”, “hence”, “by definition”, “under this assumption”, and “this term penalizes”. Every displayed equation should be part of a sentence and should have punctuation when appropriate. Type every variable when it first appears: say whether an object is a scalar, vector, matrix, tensor, set, index, graph, function, probability distribution, random variable, hyperparameter, physical quantity, or unit-bearing measurement. If the paper uses conventional notation but does not explicitly define it, mark the explanation as inferred. Never reuse the same explanatory name for two different symbols, and never introduce a symbol without a role.

For proofs or theorem-heavy papers, use a compact three-phase proof explanation. Start with `Proof strategy`, where you say what the proof tries to establish and why the chosen technique works. Then write `Proof skeleton`, where you restate assumptions, identify the key lemmas or transformations, and connect equations with words. End with `Meaning`, where you state what the theorem teaches in plain language and what assumptions limit it. If a proof uses contradiction, construction, induction, case split, both directions, or a reduction, name the technique and explain why it fits.

### 5. Explain the method

1. First, give the mathematical or physical intuition of why the method should work: the structure, conservation principle, constraint, approximation, or representation it exploits.
2. Second, describe the operational pipeline in the order a reproducer would implement it.
3. Third, reconstruct the formal objective, algorithm, theorem, derivation, or proof argument using the reasoning core above.
4. If the paper uses a named method, define the English full name and abbreviation on first mention, then use the abbreviation consistently.

For experimental papers, make the method reproducible: specify inputs, preprocessing, model or system components, training or optimization loop, evaluation metrics, baselines, ablations, and reported settings. For theory papers, make the argument checkable: specify assumptions, definitions, theorem statements, proof strategy, key inequalities or reductions, and the final implication. For system papers, make the architecture inspectable: specify modules, data flow, control flow, complexity, bottlenecks, and failure modes.

### 6. Build the variable and parameter tables

1. Build the variable table before finalizing the method explanation, because the table is the anchor of the note. Include every symbol necessary to understand the method, objective, constraints, algorithm, theorem, or experimental metrics.
2. For each variable, include the symbol, English full name, type or unit, meaning, and where it is used. Do not merely copy notation; explain what the variable means in plain language.
3. Build the parameter table when the paper contains hyperparameters, thresholds, physical constants, simulation settings, benchmark settings, architecture sizes, loss weights, optimization tolerances, or appendix configuration values.
4. If the user asks to complete missing appendix parameter tables, create two appendix sections even when the source paper names them differently; map the paper's appendix tables into `附录参数表 A` and `附录参数表 B`, and mark uncertain values as `not reported` or `inferred from <location>` rather than guessing.

### 7. Write results, limitations, and reproducibility notes

1. For experimental papers, write the setting, metrics, baselines, ablations, and main table or figure conclusions.
2. For theoretical papers, write the theorem conditions, proof strategy, proof skeleton, and meaning of the result.
3. For system papers, write the architecture, modules, data flow, control flow, complexity, bottlenecks, and failure modes.
4. Write limitations and reproducibility notes, including assumptions, untested regimes, missing data or code, incomplete hyperparameters, unclear appendix values, and risks in reproducing the result.

### 8. Append references and finalize the Obsidian note

1. Append a complete reference table at the end whenever the paper includes a bibliography.
2. Preserve numbering if the paper uses numbered citations. If the source uses author-year citations, create stable indices in reading order.
3. Each row should include enough information to identify the work: authors, title, venue or source, year, and identifier such as DOI, arXiv ID, patent number, or URL when available.
4. Add a short `Used for` note only when the citation's role is clear from the paper; otherwise write `cited by paper`.
5. End with the reference table, not a generic conclusion.

## Obsidian compact output template

Write the note as Markdown that works well in Obsidian. Use YAML frontmatter, compact headings, callouts, and tables. Avoid one-sentence paragraphs, long chat-style spacing, and filler transitions. Use this structure unless the paper clearly requires a different order:

```markdown
---
type: paper-note
title: "<paper title>"
authors: [<author names>]
year: <year>
source: <venue/arXiv/DOI/URL if available>
tags: [paper, <topic-1>, <topic-2>]
---

# <paper title>

> [!summary] 一句话结论
> <用一两句说明本文解决什么问题、核心方法是什么、主要结论是什么。>

## 元信息
| Field | Content |
|---|---|
| Paper | <filename or citation> |
| Problem | <research problem> |
| Core idea | <mathematical/physical thought that makes the problem solvable> |
| Method | <method family and concrete method> |
| Data/Setting | <dataset, benchmark, simulation, theorem setting, or experimental system> |
| Main result | <most important quantitative/theoretical result> |
| Relevance | <why this matters for the user's notes> |

## 研究问题
<说明背景、缺口、本文要回答的问题。直接进入技术内容，不写泛泛而谈的开场。>

## 数学/物理思想
<解释本文如何把原始问题转化成数学或物理问题：研究对象是什么，状态量是什么，守恒量、约束、对称性、尺度、边界条件或概率结构是什么，哪些因素被忽略，哪些近似让问题变得可解。重点写“为什么这样建模”，不是只复述公式。>

## 方法
<先给直觉，再给流程，再解释关键公式、算法或证明。数学内容必须说明变量类型、单位、取值范围和每个公式的作用。>

## 变量表
| Symbol | English full name | Type / Unit | Meaning | Where used |
|---|---|---|---|---|
| <symbol> | <full name> | <scalar/vector/matrix/set/probability/etc.> | <clear explanation> | <equation/section/table> |

## 参数表
| Parameter | English full name | Value / Range | Role | Source |
|---|---|---|---|---|
| <parameter> | <full name> | <value/range> | <what it controls> | <section/table/appendix> |

## 附录参数表 A
| Parameter | English full name | Value / Range | Role | Notes |
|---|---|---|---|---|
| <parameter> | <full name> | <value/range> | <role> | <appendix location or inference> |

## 附录参数表 B
| Parameter | English full name | Value / Range | Role | Notes |
|---|---|---|---|---|
| <parameter> | <full name> | <value/range> | <role> | <appendix location or inference> |

## 实验 / 证明 / 结果
<按论文类型选择：实验论文写设置、指标、基线、主要表图结论；理论论文写定理条件、证明思路、结论含义；系统论文写架构、模块、复杂度和消融。>

## 局限与可复现性
<列出假设、未验证情形、数据/代码/超参数缺口、复现风险。>

## 完整参考文献表
| Index | Reference | Used for |
|---|---|---|
| [1] | <full reference entry> | <background/method/baseline/etc.> |
```

## Built-in mathematical and physical reasoning principles

When explaining mathematical or physical content, preserve the core of long-form-math inside the paper note and add physics-style modeling interpretation. The goal is not to write a long textbook chapter; the goal is to make every definition, formula, approximation, proof step, and physical law earn its place. Prefer understanding over extreme compression, show the reasoning that connects symbols, and treat mathematical writing as communication rather than decoration.

Avoid long-form-math anti-patterns: do not use “clearly” or “obviously” to hide missing reasoning; do not use proof by example for universal claims; do not start a sentence with a bare symbol; do not rely on logical shorthand such as `forall`, `exists`, `s.t.`, `=>`, or `iff` in finished prose; do not assume the conclusion as a premise; and do not say “the proof is omitted” unless the paper itself omits it, in which case summarize the missing dependency honestly.

## Style constraints

Use Chinese for explanation unless the user requests another language, but keep technical terms, variable names, method names, dataset names, theorem names, and table headers in precise English when that improves retrieval. Keep the opening and ending concise: start with metadata and the summary callout; end with the reference table, not a generic conclusion. The final note should be compact in layout but not cryptic in reasoning: fewer empty lines, fewer pleasantries, more typed variables, more explained equations, and clearer method reconstruction.
