# prompt-industrializer-skill

把普通 prompt 转换为可复用、工业化规整的 YAML 规则提示词。

这个仓库是一个可直接安装的 Codex skill。它适用于：

- 把简单 prompt 改写成结构化 YAML 提示词
- 生成包含 `role`、`task_description`、`input`、`processing_rules`、`output_format` 的提示词契约
- 将分类、意图识别、抽取、改写、路由等任务规整成可多次复用的生产提示词
- 保留业务标签、中文术语、输出字段和下游解析约束

## 目录结构

```text
prompt-industrializer-skill/
├── README.md
├── LICENSE
├── SKILL.md
└── agents/
    └── openai.yaml
```

## 安装

把整个 `prompt-industrializer-skill` 文件夹复制到你的 Codex skills 目录。

Windows:

```powershell
Copy-Item -Path .\prompt-industrializer-skill -Destination "$env:USERPROFILE\.codex\skills\prompt-industrializer-skill" -Recurse -Force
```

macOS / Linux:

```bash
cp -R ./prompt-industrializer-skill ~/.codex/skills/prompt-industrializer-skill
```

## 使用示例

在 Codex 中调用：

```text
Use $prompt-industrializer-skill to convert this simple prompt into an industrialized YAML rule prompt:
判断用户是不是在问信用卡业务，并给出置信分数。
```

它会输出一个带注释分区的 YAML 提示词，例如：

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

## 本地创作约定

在 `E:\aipower` 下，后续新创作的 skills 统一放到：

```text
E:\aipower\created_skills\<skill-folder-name>\
```

本 skill 的本地路径是：

```text
E:\aipower\created_skills\prompt-industrializer-skill\
```

## 许可

本仓库使用 MIT License。任何人都可以使用、复制、修改和分发这个 skill。
