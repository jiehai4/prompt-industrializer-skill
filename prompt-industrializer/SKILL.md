---
name: prompt-industrializer
description: Convert ordinary prompts into reusable industrialized YAML rule prompts. Use when the user asks to standardize, structure, industrialize, normalize, or refactor a simple prompt into an annotated YAML prompt contract with sections such as metadata comments, role, task_description, input, processing_rules, and output_format; also use for Chinese requests like "简单prompt转工业化规整prompt", "YAML规则提示词", "提示词规整", or "把这个prompt工业化".
---

# Prompt Industrializer

## Overview

Transform a simple prompt into a reusable, annotated YAML prompt contract. The result should define the future agent's role, task, input shape, processing rules, and machine-readable output format clearly enough for repeated production use.

## Workflow

1. Identify the prompt's task type: classification, extraction, rewriting, routing, scoring, generation, review, planning, or another task.
2. Extract the user's explicit labels, definitions, business terms, constraints, examples, required fields, and forbidden behavior.
3. Choose the right rule shape:
   - Use a closed list for classification, routing, tagging, and intent recognition.
   - Use numbered procedural rules for generation, review, transformation, planning, and multi-step workflows.
   - Use field-level extraction rules when the prompt asks for structured information extraction.
4. Build the output contract before writing final wording. Make downstream parsing requirements explicit.
5. Render the final YAML prompt with annotated section comments and valid YAML syntax.
6. Return only one fenced `yaml` block unless the user asks for explanation, alternatives, or a diff.

Ask one concise question before rendering only when the scenario, input fields, category set, or output format is materially unclear.

## Default YAML Contract

Use this structure unless the user supplies a stricter schema:

```yaml
# ================================
# <提示词名字>
# 语言: <中文/English/other>
# 格式: YAML
# 场景: <使用场景>
# ================================

# [Role] 角色定义：为 AI 设定一个专家角色，以引导其行为模式
role: "<具体角色定义>"

# [Task Description] 任务描述：清晰、无歧义地说明 AI 需要完成的具体任务
task_description: "<任务描述>"

# [Input Format] 输入格式：严格规定输入的形式，以防产生偏差结果
input:
  description: "<输入说明>"
  fields:
    - name: "<字段名>"
      type: "<类型>"
      required: true
      description: "<字段含义>"

# [Processing Rules] 处理规则：列出唯一依据、判断顺序、边界条件或执行步骤
processing_rules:
  - id: "R1"
    rule: "<可执行、可检查的处理规则>"

# [Output Format] 输出格式：严格规定输出结构，便于下游程序解析
output_format:
  type: "JSON"
  description: "<输出约束>"
  schema:
    result:
      type: "string"
      description: "<字段含义>"
```

Keep YAML valid. Use ASCII `:` for keys, spaces for indentation, no tabs, one space after `#` in comments, and quoted strings for values that contain punctuation, backticks, braces, or colons.

## Section Rules

Write the opening as a comment banner with the prompt name, language, format, and scenario. The first comment line should name the concrete prompt, not just say "提示词名字".

Write `role` as a concrete operating identity. Include domain, responsibility, and decision posture when relevant.

Write `task_description` as the complete job in one compact paragraph. Mention confidence scoring only when the task involves uncertain classification, extraction, routing, multi-agent coordination, or the user explicitly asks for it.

Write `input` as an explicit contract. Include all text, files, fields, context, or optional parameters the future agent should receive. Mark required fields clearly.

Write `processing_rules` according to the task:

- For closed-set classification or intent recognition, use list items with category keys such as `intent`, `label`, or `category`, plus `description`. Add `Other` when out-of-scope or unclear inputs must be handled.
- For open procedural work, use `id` plus `rule`.
- For extraction, use target fields, extraction criteria, missing-value behavior, and conflict rules.

Write `output_format` as a machine-checkable schema. Specify type, required fields, allowed values, number ranges, explanation length, and whether extra text is forbidden.

## Quality Bar

Preserve user-provided labels and Chinese terminology exactly when they define business categories, product names, tones, or output fields.

Convert vague instructions into inspectable rules. For example, change "判断是不是信用卡业务" into a closed intent list, criteria for `Other`, and a confidence field.

Separate internal reasoning from final output. Do not ask the future agent to reveal chain-of-thought. If rationale is needed, request brief evidence, reason codes, or a concise explanation.

Avoid one-off prompt wording that only works for the current example. Generalize the structure so the prompt can be reused with new inputs.

Do not add unrelated prompt-engineering jargon, framework names, or implementation notes to the generated prompt.

## Final Check

Before finalizing, verify:

- The prompt has the banner comments plus `role`, `task_description`, `input`, `processing_rules`, and `output_format`.
- YAML indentation is consistent and contains no tabs.
- Keys use ASCII colons, not Chinese full-width colons.
- Each processing rule is actionable and testable.
- The output schema is strict enough for downstream parsing.
- Optional confidence scoring is included only when it helps the task.

## Minimal Example

Simple prompt:

```text
判断用户是不是在问信用卡业务，并给出置信分数。
```

Industrialized YAML prompt:

```yaml
# ================================
# 信用卡业务识别(Intent Recognition)提示词
# 语言: 中文
# 格式: YAML
# 场景: 用户意图识别
# ================================

# [Role] 角色定义：为 AI 设定一个专家角色，以引导其行为模式
role: "你是一个专业的信用卡业务意图识别引擎。你的核心任务是分析用户的输入文本，并将其精准地映射到预定义的意图类别中。"

# [Task Description] 任务描述：清晰、无歧义地说明 AI 需要完成的具体任务
task_description: "严格根据下面提供的 processing_rules 列表，识别用户查询的主要意图。你的分析必须准确客观，并为你的判断提供一个置信度分数。"

# [Input Format] 输入格式：严格规定输入的形式，以防产生偏差结果
input:
  description: "待识别的用户原始输入。"
  fields:
    - name: "user_query"
      type: "string"
      required: true
      description: "用户输入的完整文本。"

# [Processing Rules] 处理规则：以下列表是封闭的意图列表，是分类的唯一依据
processing_rules:
  - intent: "apply_for_card"
    description: "用户想要申请一张新的信用卡。"
  - intent: "query_bill"
    description: "用户想要查询信用卡账单，包括本期账单、历史账单、账单金额、明细等。"
  - intent: "cancel_card"
    description: "用户想要注销或关闭其名下的某张信用卡。"
  - intent: "Other"
    description: "用户的意图不属于上述任意一种，或意图不明。"

# [Output Format] 输出格式：严格规定输出结构，便于下游程序解析
output_format:
  type: "JSON"
  description: "你的输出必须是一个严格的 JSON 对象，不能包括任何额外的解释性文字。"
  schema:
    intent:
      type: "string"
      allowed_values:
        - "apply_for_card"
        - "query_bill"
        - "cancel_card"
        - "Other"
      description: "从 processing_rules 列表中选择的意图名称。"
    intent_score:
      type: "float"
      minimum: 0.0
      maximum: 1.0
      description: "一个介于 0.0 到 1.0 之间的浮点数，表示对意图判断的置信度，越高表示越确定。"
    explanation:
      type: "string"
      description: "一句简短的中文解释，说明为什么做出这个意图判断。"
```
