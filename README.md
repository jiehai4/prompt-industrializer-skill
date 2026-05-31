# Created Skills

这个仓库用于公开分享可复用的 Codex skills。目前包含：

- `prompt-industrializer`: 将普通 prompt 转换为可复用、工业化规整的 YAML 规则提示词。

## prompt-industrializer

`prompt-industrializer` 适用于这些场景：

- 把简单 prompt 改写成结构化 YAML 提示词
- 生成包含 `role`、`task_description`、`input`、`processing_rules`、`output_format` 的提示词契约
- 将分类、意图识别、抽取、改写、路由等任务规整成可多次复用的生产提示词
- 保留业务标签、中文术语、输出字段和下游解析约束

## 目录结构

```text
created_skills/
├── README.md
├── LICENSE
└── prompt-industrializer/
    ├── SKILL.md
    └── agents/
        └── openai.yaml
```

## 安装

把 `prompt-industrializer` 文件夹复制到你的 Codex skills 目录。

Windows:

```powershell
Copy-Item -Path .\prompt-industrializer -Destination "$env:USERPROFILE\.codex\skills\prompt-industrializer" -Recurse -Force
```

macOS / Linux:

```bash
cp -R ./prompt-industrializer ~/.codex/skills/prompt-industrializer
```

## 使用示例

在 Codex 中调用：

```text
Use $prompt-industrializer to convert this simple prompt into an industrialized YAML rule prompt:
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

## 许可

本仓库使用 MIT License。任何人都可以使用、复制、修改和分发这些 skills。
