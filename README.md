# prompt-industrializer-skill

把普通 prompt 转换为可复用、工业化规整的 YAML 规则提示词。

This skill converts ordinary prompts into reusable, industrialized YAML rule prompts.

## 中文说明

这个仓库包含一个可直接安装的 Codex skill。它会把粗糙、一次性的 prompt 转换成结构清晰的提示词契约，包含角色定义、任务描述、输入格式、处理规则和机器可解析的输出格式。

### 适用场景

- 把简单 prompt 改写成结构化 YAML 提示词
- 生成包含 `role`、`task_description`、`input`、`processing_rules`、`output_format` 的提示词契约
- 将分类、意图识别、抽取、改写、路由、评审、生成等任务规整成可多次复用的生产提示词
- 保留业务标签、领域术语、输出字段和下游解析约束

### 目录结构

```text
prompt-industrializer-skill/
├── README.md
├── LICENSE
└── SKILL.md
```

### 安装

把整个 `prompt-industrializer-skill` 文件夹复制到你的 Codex skills 目录。

Windows:

```powershell
Copy-Item -Path .\prompt-industrializer-skill -Destination "$env:USERPROFILE\.codex\skills\prompt-industrializer-skill" -Recurse -Force
```

macOS / Linux:

```bash
cp -R ./prompt-industrializer-skill ~/.codex/skills/prompt-industrializer-skill
```

### 使用示例

在 Codex 中调用：

```text
Use $prompt-industrializer-skill to convert this simple prompt into an industrialized YAML rule prompt:
判断用户是不是在问信用卡业务，并给出置信分数。
```

它会输出类似这样的带注释 YAML 提示词：

```yaml
# ================================
# 信用卡业务识别(Intent Recognition)提示词
# 语言: 中文
# 格式: YAML
# 场景: 用户意图识别
# ================================
role: "你是一个专业的信用卡业务意图识别引擎。"
task_description: "识别用户查询的主要意图，并给出置信度分数。"
input:
  description: "待识别的用户原始输入。"
processing_rules:
  - intent: "apply_for_card"
    description: "用户想要申请一张新的信用卡。"
output_format:
  type: "JSON"
  description: "输出必须是严格 JSON 对象，不能包含额外解释性文字。"
```

## English Version

This repository contains an installable Codex skill. It turns rough or one-off prompts into structured prompt contracts with clear roles, task descriptions, input formats, processing rules, and machine-readable output schemas.

### What It Does

Use this skill when you want to:

- Rewrite a simple prompt as a structured YAML prompt.
- Create a reusable prompt contract with `role`, `task_description`, `input`, `processing_rules`, and `output_format`.
- Standardize tasks such as classification, intent recognition, extraction, rewriting, routing, review, or generation.
- Preserve domain labels, business terminology, output fields, and downstream parsing requirements.

### Repository Structure

```text
prompt-industrializer-skill/
├── README.md
├── LICENSE
└── SKILL.md
```

### Installation

Copy the whole `prompt-industrializer-skill` folder into your Codex skills directory.

Windows:

```powershell
Copy-Item -Path .\prompt-industrializer-skill -Destination "$env:USERPROFILE\.codex\skills\prompt-industrializer-skill" -Recurse -Force
```

macOS / Linux:

```bash
cp -R ./prompt-industrializer-skill ~/.codex/skills/prompt-industrializer-skill
```

### Usage

Invoke the skill in Codex:

```text
Use $prompt-industrializer-skill to convert this simple prompt into an industrialized YAML rule prompt:
Determine whether the user is asking about credit card services and provide a confidence score.
```

The skill produces an annotated YAML prompt similar to this:

```yaml
# ================================
# Credit Card Intent Recognition Prompt
# Language: English
# Format: YAML
# Scenario: User intent recognition
# ================================
role: "You are a professional credit card intent recognition engine."
task_description: "Identify the user's primary intent and provide a confidence score."
input:
  description: "The original user query to classify."
processing_rules:
  - intent: "apply_for_card"
    description: "The user wants to apply for a new credit card."
output_format:
  type: "JSON"
  description: "Return a strict JSON object without any extra explanatory text."
```

## License

MIT License. Anyone may use, copy, modify, and distribute this skill.
