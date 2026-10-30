---
slug: zh-cn/real-world-development/cloud
title: "GitHub Copilot 云智能体"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

**[GitHub Copilot 云智能体](https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent)**让 GitHub Copilot 能够在云端异步工作。你可以在 GitHub 上分配任务，云智能体便会在后台开始处理——探索存储库、进行更改并创建拉取请求——而你可以继续处理其他工作。

通过这些练习，你将添加云智能体会遵循的自定义指令，然后分配 GitHub 议题并让它实现功能。你将审查并使用自定义智能体引导其工作方式，在智能体仪表板中监控并引导会话，最后审查它创建的拉取请求并迭代优化结果。

## 课程

| 课程 | 主题 | 说明 |
|----------|-------|-------------|
| [0. 先决条件][ex0] | 设置 | 创建存储库和 codespace |
| [1. 自定义指令][ex1] | 上下文 | 添加云智能体将遵循的自定义指令 |
| [2. 云智能体][ex2] | 异步智能体 | 将议题分配给 Copilot 云智能体 |
| [3. 自定义智能体][ex3] | 专用智能体 | 审查并使用自定义智能体 |
| [4. 管理智能体][ex4] | 监控 | 监控并引导智能体会话 |
| [5. 迭代优化][ex5] | 审查 | 审查 PR、迭代 Copilot 的工作并选择后续步骤 |

## 先决条件

参加本工作坊前，需要具备：

- 拥有有效 Copilot Student、Pro、Pro+、Business 或 Enterprise 计划的 GitHub 账户。
- 为账户或组织启用了 Copilot 云智能体。

> [!TIP]
> 没有付费计划？通过验证的学生可以通过 [GitHub Education][student-plan] 免费使用 GitHub Copilot。**Copilot Student** 计划包含本工作坊使用的智能体、MCP、代码审查和 Copilot CLI 功能，因此可以用它完成所有学习路径。

[student-plan]: https://github.com/education/students

> [!NOTE]
> Copilot 云智能体适用于 **Copilot Student、Pro、Pro+、Business 和 Enterprise**，所有计划均可使用，Copilot Free 除外。使用 Copilot Business 或 Enterprise 时，管理员必须为组织启用该功能。

> [!NOTE]
> 本工作流不介绍 MCP。要了解如何在 Copilot 中使用 MCP 服务器，请参阅 [CLI 工作流](../cli/)或 [VS Code 工作流](../vscode/)。

## 开始

**[从先决条件开始 →][ex0]**

[ex0]: 0-prerequisites/
[ex1]: 1-custom-instructions/
[ex2]: 2-cloud-agent/
[ex3]: 3-custom-agents/
[ex4]: 4-managing-agents/
[ex5]: 5-iterating/
