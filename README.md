# AI 知识周报 Skill

一个面向 AI Agent 的、证据受限的知识周报工作流。它将资料盘点、周报初稿、人工 Review、分享版、版本归档和确认后的交付拆分为明确阶段，避免把推测写成事实，避免由 AI 越过人工确认 Gate。

## 适用场景

- 基于本周需求和原始工作副本生成知识周报。
- 需要明确资料来源、合并重复信息并标记“待确认”。
- 需要将初稿、内容确认、分享确认与最终交付分阶段控制。

Skill 的核心是通用 `SKILL.md`，不依赖某一个模型或厂商。仓库同时提供一份带飞书交付规则的 [AGENTS.md](AGENTS.md)，作为 ChatGPT 桌面版 / Codex 的已验证参考项目配置。可用于支持 Agent Skills 的 ChatGPT 桌面版、Claude Code、Trae、workbuddy、Cursor 及其他兼容 Agent；平台是否具备 Word、飞书或其他云文档能力取决于该平台的已授权工具。

## 安装

将整个 `ai-knowledge-weekly-report` 文件夹复制到项目的技能目录，保留目录名与 `SKILL.md` 中的 `name` 一致。

| 平台 | 常用项目级目录 | 说明 |
| --- | --- | --- |
| ChatGPT 桌面版 / Codex | `<项目根目录>\.agents\skills\` | 复制后路径为 `<项目根目录>\.agents\skills\ai-knowledge-weekly-report\`。 |
| Claude Code | `<项目根目录>\.claude\skills\` | 复制后按 Claude Code 的 Skills 机制加载。 |
| Trae | `<项目根目录>\.trae\skills\` | 复制后按 Trae Skills 机制加载。 |
| workbuddy | 使用其 Claude Code 集成对应的 Skills 目录 | workbuddy 建立在 Claude Code 集成之上。 |
| Cursor | `<项目根目录>\.cursor\skills\` | 复制后按 Cursor 当前的 Skills 设置加载。 |

不同版本可能支持自定义位置或改变目录约定，请以平台当前官方文档和本地设置为准。GitHub Copilot/VS Code 也支持 `.agents/skills/`、`.claude/skills/` 等项目级目录；Trae 支持 `.trae/skills/`。

参考：[VS Code Agent Skills](https://code.visualstudio.com/docs/agent-customization/agent-skills)、[Trae Skills 说明](https://forum.trae.cn/t/topic/67755)、[workbuddy 文档](https://docs.work-buddy.ai/)。

### 搭配 `AGENTS.md`

将仓库根目录的 `AGENTS.md` 复制到项目根目录，即可与本 Skill 搭配使用。Agent 应先读取项目规则，再调用 `$ai-knowledge-weekly-report`；如两者冲突，以项目规则为准。

本仓库的 `AGENTS.md` 保留了飞书私人文档交付规则，适合作为 ChatGPT 桌面版 / Codex 项目参考。使用者可以按自己的项目调整语言、目录或交付平台，但不应弱化事实证据边界、人工 Gate 和禁止覆盖既有成果等约束。

### 飞书 CLI 前置依赖

如需执行 `AGENTS.md` 中的 `$lark-doc` 飞书交付，须先按照[飞书 CLI 官方安装指南](https://open.feishu.cn/document/no_class/mcp-archive/feishu-cli-installation-guide)完成 CLI、必需 Skill、应用配置、登录和状态验证。未完成安装或授权时，仍可使用资料分析和本地周报流程，但不能完成飞书交付。

可将以下提示词交给支持安装 Skill 的 Agent：

```text
帮我安装飞书 CLI 且是全局级 Skill：https://open.feishu.cn/document/no_class/mcp-archive/feishu-cli-installation-guide.md
```

官方指南中的全局安装明确适用于 `@larksuite/cli`；CLI Skill 的安装范围由所用 Agent 和 `skills add` 环境决定。安装后应确认 `lark-cli`、`$lark-doc` 及其依赖 `$lark-shared` 均可用。飞书应用凭据、授权链接和令牌不得提交到项目或本仓库。

### 自动化与验证状态

- **ChatGPT 桌面版 / Codex**：`SKILL.md`、本仓库 `AGENTS.md`、飞书 CLI 交付，以及自动化/「已安排」任务的自动初稿流程均已测试成功。
- **Claude Code、Trae、workbuddy、Cursor 及其他 Agent**：尚未完成验证，不保证 `$lark-doc`、项目规则加载方式或自动化任务能力可以直接使用；请以对应平台的实际能力和授权工具为准。
- 自动化/「已安排」任务只能调用自动初稿模式，不得跨越人工 Review Gate；下一周目录准备独立于本周初稿是否成功。

## 使用方式

1. 在项目根目录提供项目规则文件（例如 `AGENTS.md` 或 `CLAUDE.md`），说明资料边界、目录约定和授权要求。
2. 确保周报需求与原始资料已经进入本周工作副本。
3. 向 Agent 说明要执行的阶段，例如“只盘点资料”“生成初稿”“已完成内容 Review，可以生成分享版”。
4. 仅在明确通过 Gate 1 与 Gate 2 后，继续处理对应确认版和最终交付。

可直接使用 [提示词模板](references/prompt-templates.md)。

## 核心约束

- 只使用当周可读取资料；不能确认的内容必须标记“无法确认”或“待确认”。
- 不修改原始工作副本，不以外部搜索或其他周次资料补足事实。
- Gate 1 前不生成分享版；Gate 2 前不生成最终 Word 或云端文档。
- 仅在平台存在已授权能力时创建云端文档；不公开分享、不伪造链接。

## 文件说明

- `SKILL.md`：平台无关的核心 SOP。
- `AGENTS.md`：带飞书交付、自动初稿和人工 Gate 规则的参考项目配置。
- `agents/openai.yaml`：OpenAI/Codex 的可选展示元数据；不是核心执行依赖。
- `references/prompt-templates.md`：按阶段复用的提示词模板。

## 隐私说明

此仓库不包含任何真实周报、原始资料、工作副本、生成文档、云端链接、账户凭据或本机路径。请勿将实际业务资料提交到公开仓库。

## 许可证

[MIT License](LICENSE)
