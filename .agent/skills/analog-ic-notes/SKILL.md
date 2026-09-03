---
name: analog-ic-notes
description: Turn an analog/RF IC discussion into a rigorous, concise Markdown engineering note for sharing and review. Use only when the user explicitly invokes $analog-ic-notes or asks to organize the discussion into a note; do not use for ordinary answers.
---

# Analog IC Notes

Produce a self-contained engineering note, not a transcript. Preserve the useful reasoning, remove conversational repetition, and correct technical mistakes rather than memorializing them.

## Invocation and storage

- Run only after an explicit request such as `$analog-ic-notes` or “整理成笔记”; never create a note merely because a technical discussion ended.
- In this repository, save the new note under the category directory: `docs/blog/posts/analog/`, `rf/`, `tools/`, or `life/`, followed by an English `kebab-case` slug. Derive a short, descriptive technical slug from the title.
- New notes always use `draft: true`. Do not commit, push, publish, or change the draft state; the user reviews and releases the note.
- Never overwrite an existing note. Check every `docs/blog/posts/**/<slug>.md`; if the slug already exists, ask the user to choose a new name or confirm a proposed alternative.

## Before writing

1. Identify the central engineering question and intended deliverable from the visible discussion and attachments.
2. Recover the minimum context needed to interpret the result: topology, operating point, analysis type, signal definition, bandwidth, reference impedance, temperature, process/model assumptions, and simulator when relevant.
3. When saving into an existing note collection, inspect a nearby note and retain its frontmatter schema and taxonomy. In this repository, use `date.created`, `draft`, `categories`, `tags`, and optional `authors`; default to `Analog Circuit`, use `RF` for RF/microwave content, and do not introduce `status`. Map categories to storage directories: `Analog Circuit` → `analog`, `RF` → `rf`, `CS`/`FPGA` → `tools`, and `Car`/`Lifestyle` → `life`.
4. Ask one focused question only when a missing fact would materially change the physics or conclusion. Otherwise state the missing quantity as an assumption or an unresolved item.
5. When equations, numerical results, noise, mismatch, stability, or simulator expressions are involved, read [references/analog-ic-review-checklist.md](references/analog-ic-review-checklist.md) before drafting.

## Choose the note shape

Read [references/note-structures.md](references/note-structures.md) and select the smallest structure that fits the discussion.

For calculation or simulation-analysis discussions, default to the user's preferred sequence:

1. 问题
2. 简化算法
3. 严格算法
4. ADS/仿真实现
5. 仿真现象解释
6. 结论

Add `前提与定义`, `设计权衡`, `验证方法`, or `待验证项` only when they improve correctness. Omit an inapplicable section instead of filling it with generic text. For derivation, topology comparison, debugging, naming/specification, and design-decision notes, use the corresponding adaptive structure in the reference.

## Drafting rules

- Match the user's language; default to concise Chinese when the discussion is in Chinese.
- Start with valid YAML frontmatter. Preserve user-provided metadata and the target collection's schema. Otherwise use the current date, a narrow category, and 2–6 searchable tags. Do not invent an author.
- Give the note a conclusion-oriented title that identifies the circuit/metric and question.
- Define every symbol at first use. State units and the direction/sign convention for error quantities.
- Label each important result as one of: exact relation, first-order approximation, engineering heuristic, simulation observation, or external/process-dependent value.
- Put assumptions next to the derivation they constrain. State the validity range of approximations such as small-signal, high-\(\beta\), weak mismatch, unilateral gain, or flat-band response.
- Keep one canonical derivation. Remove duplicated algebra and conversational backtracking unless a failed approach teaches an important pitfall.
- Label the basis of each nontrivial conclusion as `手算`, `仿真`, `后仿`, or `测量` when known. When missing information affects the conclusion, add compact `已知`、`假设`、`待验证` bullets rather than filling gaps.
- Use boxed equations only for results worth remembering. Use tables for input parameters, option comparisons, or simulator-variable mappings.
- For numerical examples, show inputs with units, the governing equation, the result with sensible precision, and at least one sanity or limiting-case check.
- If the note needs a new image, store it in `docs/blog/posts/image/<note-slug>/` with a descriptive `kebab-case` filename. Keep existing legacy images in place.
- Separate simulator syntax from physical equations. Include runnable ADS/Spectre/other expressions only when supported by the discussion; otherwise label them as pseudocode or note that syntax is version-dependent.
- Explain unexpected plots through a physical cause-and-effect chain. Do not infer more than an attached plot, schematic, or stated setup supports.
- Distinguish general conclusions from conclusions specific to the current bias, bandwidth, process, corner, temperature, or model.
- End with only the high-value conclusions: key equation or decision, physical interpretation, actionable design guidance, and main boundary condition as applicable.

## Handling corrections and uncertainty

- Do not silently repeat a technically incorrect statement from the conversation. Use the corrected result and add a short `修正说明` when the correction changes the conclusion or would help future review.
- Do not invent foundry statistics, device parameters, simulator behavior, or citations. Mark typical values as estimates and keep them separate from PDK/model data.
- Put unresolved ambiguity, missing verification, or conflicting results in `待验证项`, together with the smallest useful next simulation or hand calculation.
- If independent error sources are combined, state their statistical model and correlation assumption; do not replace an RSS result with worst-case addition without labeling it.

## Output contract

In this repository, write the complete Markdown note to its draft path, then report only the path and any `整理说明`. Elsewhere, return Markdown that can be saved directly. Do not precede it with a long explanation.

When useful, append a short section outside the note named `整理说明` containing only:

- material technical corrections made;
- assumptions introduced because information was missing;
- one recommended next verification step.

Omit `整理说明` when no such disclosure is needed.
