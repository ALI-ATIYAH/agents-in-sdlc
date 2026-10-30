---
title: "练习 5：迭代优化 GitHub Copilot 的工作"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

| [← 上一课：管理智能体][previous-lesson] |
|:--|

## 审查工作成果

在整个实验中，你与 GitHub Copilot 协作完成了多项任务，以改进用户体验并添加功能。你让 Copilot 为代码添加文档、构建供设计团队迭代的相关游戏功能，以及实现包含高对比度和浅色模式切换开关的无障碍功能。现在来检查代码更改；必要时向 Copilot 提供反馈，改进其工作成果。

### 场景

前面多次强调，软件设计和 DevOps 的基本原则不会因引入生成式 AI 而改变。始终需要审查生成的代码，并遵循常规 DevOps 流程。基于这一点，在启用团队其他成员的审查之前，先审查 GitHub Copilot 创建的文档、相关游戏功能和无障碍功能。

## 安全和 GitHub Copilot 云智能体

由于 Copilot 云智能体会异步且无人监督地执行任务，因此设置了一些安全限制：

- Copilot 对存储库只有读取权限，对它用于提交代码的分支才有写入权限。
- 云智能体在 GitHub Actions 中运行，并创建独立的临时工作环境。
- 任何 GitHub Actions 工作流都必须经人工批准后才能运行。
- [默认情况下，外部资源访问受到限制](https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-firewall)，包括 MCP 服务器。

## 审查生成的文档

先查看 GitHub Copilot 云智能体创建的第一个拉取请求（PR），它为代码添加了文档。通过 GitHub.com 上的标准 PR 界面完成此任务。

> [!NOTE]
> 查看 PR 时，可能会看到 GitHub Copilot 被防火墙阻止的警告。这是**预期行为**，因为默认情况下 Copilot 对外部资源的访问受限，包括对外部 MCP 服务器的调用。你也可以[自定义或禁用 Copilot 云智能体的防火墙][agent-firewall]。

1. 返回 github.com 上的存储库。
2. 选择 **Pull Requests**，打开拉取请求列表。
3. 打开标题类似 **Add missing documentation** 的拉取请求。

> [!NOTE]
> 如果 Copilot 仍在处理任务，拉取请求会带有 **[WIP]** 标记。此时请等待 Copilot 完成工作。这可能需要几分钟，可以趁此休息一下或回顾目前学到的内容。

4. 拉取请求准备就绪后，选择 **Files changed** 选项卡并审查更改。

    ![Files changed 选项卡](../../../_images/shared-pr-files-changed.png)

5. 查看新更新的代码，其中包括新生成的 TSDoc 文档注释和其他文档。具体更改会有所不同。

    浏览更改时，留意 TSDoc 文档注释、TypeScript 约定和文件注释头。这些内容来自你在练习 2 开始时查看的自定义指令文件。

6. 审查更新并确认无误后，返回 **Conversation** 选项卡并向下滚动。
7. 查看是否有工作流等待批准的状态提示。
8. 如果有工作流等待批准，选择 **Approve and run workflows**。

   ![Approve and run workflows](../../../_images/shared-approve-workflows.png)

9. 你应能看到工作流已加入拉取请求的检查列表。若一切正常，单体 Astro 应用的项目检查将通过。此过程可能需要几分钟。

## 请求 GitHub Copilot 修改

在拉取请求中与 Copilot 协作并非单向流程。你可以像在团队 PR 中提及其他成员一样，在普通评论或代码行内评论中提及 Copilot。Copilot 会读取评论并启动新会话来处理请求。由于结果具有非确定性，无法提供唯一的标准提示文本。可以要求 Copilot：

- 为每个代码文件开头添加简短说明文件用途的注释头。
- 为 TypeScript 和 Astro 文件添加 TSDoc 文档注释。
- 创建 README，介绍 Astro 应用结构。

1. 添加评论，请求修改生成的文档，并像提及用户一样提及 **@copilot**。可以使用上面的任一建议，也可以提出其他文档改进建议。
2. 选择 **View Session**，观察 Copilot 执行工作。注意 Copilot 会启动新会话进行更新。
3. 选择 **Back to pull request** 返回拉取请求。

    ![Back to pull request](../../../_images/ex7-back-to-pr.png)

4. Copilot 完成更改后，拉取请求中应会出现新的提交。
5. 选择 **Files changed** 选项卡，审查更改。

可以继续迭代，直到满意为止。满意后，将 PR 从草稿转换为可审查状态，并合并到 main 分支。

![将 PR 转为可审查状态](../../../_images/ex7-ready-for-review.png)

## 审查相关游戏功能

接下来返回 Copilot 为“在游戏详情页显示相关游戏”议题创建的 PR。

1. 返回 GitHub.com 上的存储库。
2. 选择 **Pull Requests** 选项卡。
3. 选择标题类似 **Show related games on the game details page** 的 PR。
4. 选择 **Files changed** 选项卡，审查生成的代码。
5. 审查更新并确认无误后，返回 **Conversation** 选项卡并向下滚动。
6. 查看是否有工作流等待批准的状态提示。
7. 如果有工作流等待批准，选择 **Approve and run workflows**。

   ![Approve and run workflows](../../../_images/shared-approve-workflows.png)

8. 你应能看到工作流已加入拉取请求的检查列表。若一切正常，单体 Astro 应用的项目检查将通过。此过程可能需要几分钟。
9. **可选：**可以在 Codespace 中切换到该分支，手动测试相关游戏功能。打开 Codespace 终端并运行以下命令（将 `<branch-name>` 替换为 Copilot 创建的分支名称，例如 **copilot/fix-8**）：

    ```bash
    git fetch origin
    git checkout <branch-name>
    ```

Copilot 已构建相关游戏功能！和之前一样，可以通过迭代让 Copilot 云智能体继续修改。例如，可以要求 Copilot 调整显示的相关游戏数量，或添加文件注释头和 TSDoc 文档注释（请记住，分配此任务时，你还没有更新自定义指令）。与之前一样，可以在 **Conversation** 选项卡中添加评论来提出请求，Copilot 会读取评论并启动新会话。

## 审查无障碍功能

最后，审查由无障碍自定义智能体实现的无障碍功能。此 PR 应同时包含练习 3 中分配的高对比度模式，以及练习 4 中在任务控制中心提出的浅色模式。

1. 返回 GitHub.com 上的存储库。
2. 选择 **Pull Requests** 选项卡。
3. 选择标题类似 **Add high contrast mode to website** 的 PR。

> [!NOTE]
> 如果 Copilot 仍在处理任务，拉取请求会带有 **[WIP]** 标记。此时请等待 Copilot 完成工作，这可能需要几分钟。

4. 选择 **Files changed** 选项卡，审查生成的代码。
5. 审查实现，尤其注意：
   - 用于切换模式的 UI 组件。
   - 使用本地存储保存用户偏好。
   - 高对比度和浅色模式的 CSS 或样式更改。
   - 无障碍属性（ARIA 标签、键盘导航等）。
   - 管理模式切换的 JavaScript/TypeScript 代码。

6. 审查更新并确认无误后，返回 **Conversation** 选项卡并向下滚动。
7. 查看是否有工作流等待批准的状态提示。
8. 如果有工作流等待批准，选择 **Approve and run workflows**。

   ![Approve and run workflows](../../../_images/shared-approve-workflows.png)

9. 你应能看到工作流已加入拉取请求的检查列表。若一切正常，单体 Astro 应用的项目检查将通过。此过程可能需要几分钟。
10. **可选：**可以在 Codespace 中切换到该分支，手动测试无障碍功能。打开 Codespace 终端并运行以下命令（将 `<branch-name>` 替换为 Copilot 创建的分支名称）：

    ```bash
    git fetch origin
    git checkout <branch-name>
    ```

    然后启动应用并在浏览器中测试高对比度和浅色模式切换开关，确认功能正常，并且刷新页面后设置仍然保留。

注意，无障碍自定义智能体帮助 Copilot 根据无障碍最佳实践实现了这些功能。如果发现无障碍问题或改进点，可以像处理之前的 PR 一样，在评论中提及 **@copilot** 并请求更新。

## 可选练习——继续委派任务

云智能体适合处理实际积压工作，让你可以将任务交给它并转而处理其他工作。为了养成这种习惯，可以在存储库中创建更多议题并分配给 Copilot。以下是一些想法：

- 在游戏详情页创建支持者兴趣表单。
- 在游戏列表页实现分页。
- 为数据访问辅助函数添加输入验证和错误处理。

## 总结

你已完成云智能体工作流。整个课程中，你：

- **检查了存储库自带的自定义指令文件**，以便稍后观察它们对云智能体输出的影响。
- **将议题分配给 Copilot 云智能体**，观察它设置环境、制定计划并异步执行任务。
- **创建并使用了无障碍自定义智能体**，添加高对比度和浅色模式切换开关。
- **将 Copilot 智能体页面用作任务控制中心**，监控并引导正在运行的无障碍会话。
- **审查并迭代优化了云智能体的拉取请求**，通过提及 `@copilot` 请求更改并批准工作流。

## 回顾和后续步骤

你已完成云智能体工作流。其他工作流可以补充你在这里练习的内容：

- 🖥️ **[VS Code 工作流](../../vscode/)**——在 IDE 中体验 Copilot Chat 代理模式和 MCP 集成。
- 💻 **[CLI 工作流](../../cli/)**——通过终端交付两个经过审查的更改，体验 Plan 和 Autopilot 模式、智能体技能、自定义 QA 智能体、Playwright MCP、Agent Merge，以及可选的 `/delegate` 云智能体桥接。

还可以在自己的存储库中尝试以下后续任务：

- 将重构或测试覆盖率议题分配给云智能体。
- 为其他领域创建自定义智能体。
- 为其他技术栈设置 `copilot-setup-steps.yml`。

## 资源

- [GitHub Copilot][github-copilot]
- [关于 Copilot 智能体][copilot-agents]
- [将 GitHub 议题分配给 Copilot][assign-issue]
- [Copilot 云智能体设置工作流最佳实践][cloud-agent-best-practices]
- [配置 Copilot 云智能体防火墙][agent-firewall]

---

| [← 上一课：管理智能体][previous-lesson] |
|:--|

[github-copilot]: https://github.com/features/copilot
[cloud-agent-overview]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[assign-issue]: https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/start-copilot-sessions
[setup-workflow]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[copilot-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[cloud-agent-best-practices]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[agent-firewall]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-firewall

[previous-lesson]: ../4-managing-agents/
