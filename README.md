# prompt-industrializer-skill

[![English](https://img.shields.io/badge/Language-English-blue)](./README.md)
[![中文](https://img.shields.io/badge/%E8%AF%AD%E8%A8%80-%E4%B8%AD%E6%96%87-red)](./README.zh-CN.md)

Convert ordinary prompts into reusable, industrialized YAML rule prompts.

This repository contains an installable Codex skill. It turns rough or one-off prompts into structured prompt contracts with clear roles, task descriptions, input formats, processing rules, and machine-readable output schemas.

## What It Does

Use this skill when you want to:

- Rewrite a simple prompt as a structured YAML prompt.
- Create a reusable prompt contract with `role`, `task_description`, `input`, `processing_rules`, and `output_format`.
- Standardize tasks such as classification, intent recognition, extraction, rewriting, routing, review, or generation.
- Preserve domain labels, business terminology, output fields, and downstream parsing requirements.

## Repository Structure

```text
prompt-industrializer-skill/
├── README.md
├── README.zh-CN.md
├── LICENSE
└── SKILL.md
```

## Installation

Copy the whole `prompt-industrializer-skill` folder into your Codex skills directory.

Windows:

```powershell
Copy-Item -Path .\prompt-industrializer-skill -Destination "$env:USERPROFILE\.codex\skills\prompt-industrializer-skill" -Recurse -Force
```

macOS / Linux:

```bash
cp -R ./prompt-industrializer-skill ~/.codex/skills/prompt-industrializer-skill
```

## Usage

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
