---
title: "练习 4 - 监控和管理智能体"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

前两项练习中，你让 Copilot 云智能体处理了三项改进用户体验和添加功能的任务。云智能体虽然可以异步自主工作，但监控这些任务仍然很重要。

你可以使用多种工具管理分配给云智能体的任务，包括 GitHub.com 上的[智能体页面][agents-page]。在这个任务控制中心，你可以查看所有有未关闭拉取请求（PR）的智能体任务、了解它们执行的操作，甚至引导进行中的会话。

本课将学习如何：

- 浏览智能体页面，监控云智能体任务。
- 引导正在进行的会话，要求添加其他功能。

## 场景

将高对比度模式任务分配给智能体后，团队决定也添加浅色模式。既然正在更新网站样式并添加切换功能，加入这项功能很合理。现在引导智能体，同时添加浅色模式和高对比度模式。

## 查看 Copilot 云智能体任务

查看当前分配给 Copilot 云智能体的所有任务的状态。

1. 打开 [Copilot 智能体页面](https://github.com/copilot/agents)。
2. 查看主窗格和左侧窗格中的任务列表。你应能看到分配给 Copilot 的任务，包括：
    - 更新代码文档。
    - 生成用于修改游戏的 API。
    - 为网站添加高对比度模式。
3. 选择一项正在运行的任务，查看 Copilot 已执行的操作，包括：
    - 从存储库检出代码。
    - 为 Copilot 创建工作环境。
    - 设置 MCP 服务器。
    - 执行完成分配任务所需的各项步骤。

> [!NOTE]
> 列出的具体步骤会根据 Copilot 的工作状态和采用的方案而变化。

4. 还可以查看右侧显示的拉取请求（PR）窗格，了解 PR 和已更改文件，进一步监控任务。

## 引导云智能体

现在已经了解正在进行的任务，可以在智能体实现高对比度模式时要求它也添加浅色模式。

1. 选择与添加高对比度模式相关的会话。确切标题会根据 Copilot 使用的名称和当前工作状态而有所不同。

    ![任务控制中心中的无障碍会话](../../../_images/ex6-accessibility-session.png)

2. 观察会话几分钟，直到它完成环境设置并开始工作。此时会出现类似下方的消息。
3. 在 **Steer active session while Copilot is working** 对话框中，输入以下提示：

    ```plaintext
    While we are working on a high contrast mode, let's also add a light mode. There should be a switch for this mode as well where users can select their desired display mode.
    ```

    ![智能体页面上的云智能体任务截图，其中突出显示了在 Copilot 工作时引导活动会话的对话框](../../../_images/ex6-steer-cloud-agent-task.png)

4. 按 <kbd>Enter</kbd> 发送提示。
5. 注意 Copilot 会确认收到提示，并将其纳入工作流程。

## 让 Copilot 继续工作

和之前一样，Copilot 会继续处理更新后的任务！收到消息时，它会先完成当前步骤，然后将新请求加入工作流程。

这同样需要几分钟，可以利用这段时间回顾到目前为止学到和体验的内容。

## 总结和后续步骤

本课介绍了 Copilot 智能体页面，它是监控并引导 GitHub Copilot 云智能体任务的中心位置。在这个任务控制中心，你可以跟踪所有活动和已完成的任务、审查工作内容，甚至重定向正在进行的任务以调整范围或提供更多指导。

你了解了以下概念：

- 使用任务控制中心和智能体页面监控云智能体任务。
- 重定向正在进行的会话，要求添加其他功能。

Copilot 正在完成无障碍功能。接下来，[迭代优化 Copilot 创建的拉取请求][next-lesson]。

## 资源

- [Copilot 智能体页面][agents-page]
- [关于自定义智能体][custom-agents]

---

| [← 上一课：自定义智能体][previous-lesson] | [下一课：迭代 Copilot 的工作 →][next-lesson] |
|:--|--:|

[previous-lesson]: ../3-custom-agents/
[next-lesson]: ../5-iterating/
[agents-page]: https://github.blog/changelog/2025-07-03-agents-page-for-copilot-coding-agent-in-public-preview
[custom-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
