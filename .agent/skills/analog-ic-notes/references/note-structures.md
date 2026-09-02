# Note structures

Select one primary structure. Merge adjacent sections when the result is short, and never keep an empty heading.

## Shared frontmatter

```yaml
---
title: <conclusion-oriented title>
date:
  created: YYYY-MM-DD
draft: true
categories:
  - <Analog Circuit | RF>
tags:
  - <circuit-or-metric>
  - <analysis-topic>
authors:
  - <include only when known>
---
```

Use the user's existing frontmatter keys when editing an established note collection. In this repository, retain `draft: true` until the user reviews the note.

## A. Calculation or simulation analysis

Default structure for noise, SNR, gain, impedance, bandwidth, linearity, mismatch, and similar metric calculations:

1. `问题`: metric, circuit context, known inputs, and what is uncertain.
2. `简化算法`: fast estimate, assumptions, usefulness, and failure mode.
3. `严格算法`: physical definition and derivation in the correct domain.
4. `ADS/仿真实现`: controller/setup, expressions, sweep/integration range, ports and reference conditions.
5. `仿真现象解释`: important result, surprising trend, cause-and-effect explanation, and cross-check.
6. `结论`: recommended definition/method, result, applicability, and next action.

Add `前提与定义` when signal conventions such as RMS/Vpp, differential/single-ended, PSD/integrated noise, or sign conventions are easy to confuse.

## B. Device or circuit derivation

Use for current mirrors, degeneration, feedback, small-signal models, pole-zero analysis, or transfer-function derivations:

1. `问题与电路定义`
2. `分析假设`
3. `精确关系或基本方程`
4. `一阶近似与推导`
5. `物理意义`
6. `数值/仿真验证`
7. `设计权衡与适用边界`
8. `结论`

For mismatch, keep device mismatch, passive mismatch, bias/common-mode effects, and systematic layout effects distinct. Show how independent random terms are combined.

## C. Simulation debugging

Use when the main value is diagnosing why a result looks wrong:

1. `现象与期望`
2. `仿真定义与复现条件`
3. `候选原因`
4. `证据与排除过程`
5. `根因`
6. `修正方法`
7. `回归检查`
8. `结论`

Separate model/setup errors from real circuit behavior. Record the exact metric definition, dataset, ports, termination, sweep, analysis mode, and relevant simulator expression.

## D. Topology or parameter tradeoff

Use for choosing a topology, device size, degeneration, bias, compensation, or cascode strategy:

1. `设计目标与约束`
2. `候选方案`
3. `一阶模型`
4. `关键权衡` as a comparison table
5. `推荐方案与理由`
6. `验证计划`
7. `结论`

Do not present a universal optimum when the recommendation depends on headroom, current, bandwidth, noise, linearity, stability, area, PVT, or process statistics.

## E. Naming, interface, or specification convention

Use for pins, nets, hierarchy, libraries, cell versions, and interface documents:

1. `目标与范围`
2. `命名/定义规则`
3. `推荐映射表`
4. `容易混淆的边界`
5. `最终约定`

Prefer a table with current name, recommended name, signal type/domain, direction, active polarity, voltage/current range, and description when those fields apply.

## Compression targets

- Short discussion: approximately 500–1000 Chinese characters plus equations/tables.
- Substantial derivation or debugging session: approximately 1000–2500 Chinese characters plus equations/tables.
- Exceed these ranges only when omitting detail would make the note non-reproducible or technically misleading.
