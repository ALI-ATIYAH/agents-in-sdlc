---
title: "练习 2 - GitHub Copilot 云智能体"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

| [← 上一课：自定义指令][previous-lesson] |
|:--|

几乎所有组织都会遇到技术债问题，例如尚未解决的安全问题、需要更新的旧代码，或因缺少时间而长期搁置的功能请求。GitHub Copilot 云智能体可以自主执行更新代码和添加功能等任务。完成后，它会创建草稿 PR，供开发者审查。这样可以将繁琐任务交给智能体，加快开发流程，让开发者专注于更重要的工作。

你将使用 Copilot 云智能体了解以下内容：

- 自定义代码生成环境。
- 确保操作安全。
- 明确限定议题范围的重要性。
- 将议题分配给 Copilot。

## 场景

Tailspin Toys 有一些希望处理的技术债。最初为网站开发的承包商没有留下合适的文档——事实上，你会发现项目完全缺少文档。团队希望首先为应用中所有导出的函数添加 TSDoc 文档注释。

此外，设计团队希望改进游戏发现体验。他们想在每款游戏的详情页显示相关游戏（同类别的其他游戏），让用户可以继续浏览。他们目前不需要完善的实现，只需要一个可用于验收测试用户体验（UX）的版本。这项任务目前被阻塞，但其他议题的优先级更高。

这两项任务很容易被降级为低优先级，适合交给 Copilot 云智能体。它可以异步处理任务，让开发者专注于其他工作，之后再回来审查 Copilot 的成果并确认一切符合预期。

## 认识 GitHub Copilot 云智能体

[GitHub Copilot 云智能体](https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent#overview-of-copilot-cloud-agent-formerly-copilot-coding-agent)可以像开发者一样在后台执行任务。就像与开发者协作一样，可以通过多种方式启动任务，例如[将 GitHub 议题分配给 Copilot](https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/start-copilot-sessions)。分配后，Copilot 会创建草稿拉取请求以跟踪进度、设置环境并开始处理任务。你可以在 Copilot 会话进行期间或完成后查看详情。解决方案准备好供你审查时，Copilot 会在拉取请求中提及你。

## 明确限定指令范围的重要性

GitHub Copilot 没有魔法——你仍然需要思考。即使看似简单的操作，仔细分析后也会发现其复杂性。因此，[为 Copilot 云智能体明确限定任务范围][cloud-agent-best-practices]非常重要。应将它视为 AI 结对程序员：分阶段工作、学习、试验并不断调整。引入生成式 AI 并不会改变软件开发的基本原则。

## 本存储库中的自定义指令

前面的练习介绍了自定义指令以及它们如何引导 Copilot。如果你已完成自定义指令的实践练习，就见过 `.github/copilot-instructions.md` 的作用；如果还没有，现在可以快速查看。在将工作分配给 Copilot 云智能体前，先只读查看存储库中的指令文件，稍后便能观察它们的影响。

在存储库的 GitHub 网页界面中打开以下文件；如果已有运行中的 codespace，也可以在其中打开：

- `.github/copilot-instructions.md`：查看 **Code standards** 部分，尤其是 TypeScript 约定和 TSDoc 文档注释要求。
- `.github/instructions/unit-tests.instructions.md`：注意 `applyTo` frontmatter，它将指令限定为 `**/*.test.ts` 文件。

下一节将 `Code lacks documentation` 议题分配给云智能体时，留意它生成的拉取请求中是否包含 TSDoc 注释、TypeScript 约定和文件注释头。这些内容由上述指令文件提供。后续练习审查 PR 时还会再次提及。

## 为 Copilot 云智能体设置开发环境

无论由谁编写代码，通常都需要特定环境并运行一些设置脚本，确保一切就绪。将任务分配给 Copilot 时也是如此，因为它会以类似软件工程师（SWE）的方式执行任务。

云智能体使用 [GitHub Actions][github-actions] 环境开展工作。你可以创建专用的设置工作流来定制环境，在 `.github/workflows/copilot-setup-steps.yml` 文件中配置该工作流 [setup workflow][setup-workflow]，让它在智能体开始工作前运行。这样可以为智能体提供所需的开发工具和依赖项。本实验已提前配置好该工作流，以确保流程顺畅并提供学习机会。它会为单体 Astro 应用安装 Node.js、项目依赖项、端到端测试所需的浏览器，以及已迁移并填充数据的 SQLite 数据库：

```yaml
name: "Copilot Setup Steps"

# 允许你从存储库的 "Actions" 选项卡测试设置步骤
on: workflow_dispatch

env:
  ASTRO_TELEMETRY_DISABLED: "1"

jobs:
  copilot-setup-steps:
    runs-on: ubuntu-latest
    # 将权限限制为 *你的步骤* 所需的最低级别。Copilot 会使用自己的令牌执行操作。
    permissions:
      # 如果要在设置步骤中克隆存储库（例如安装依赖项），需要 `contents: read` 权限。如果未在设置步骤中克隆存储库，Copilot 会在步骤完成后自动为你克隆。
      contents: read
    steps:
      - name: Checkout code
        uses: actions/checkout@v5

      # 前端/应用设置 - Node.js（整个应用现在基于 Astro + Drizzle/libSQL）
      - name: Set up Node.js
        uses: actions/setup-node@v6
        with:
          node-version: "lts/*"
          cache: "npm"
          cache-dependency-path: "./package-lock.json"

      - name: Install JavaScript dependencies
        run: npm ci

      - name: Install Playwright browsers
        run: npx playwright install --with-deps chromium

      # 迁移并填充本地 SQLite 数据库，为构建和测试准备数据。
      - name: Set up the database
        run: npm run db:setup
```

这看起来像其他 GitHub 工作流文件，但有几个关键点：

- 工作流只有一个名为 `copilot-setup-steps` 的作业。Copilot 开始处理拉取请求前，GitHub Actions 会先运行此作业。
- 注意 `workflow_dispatch` 触发器。它允许你从存储库的 **Actions** 选项卡手动运行工作流，以便在等待 Copilot 运行前测试工作流。

## 添加文档

每个人都明白文档的重要性，但大多数项目的信息要么已过时，要么完全缺失。这类技术债常常得不到处理，降低生产效率，也让维护代码库或帮助新开发者加入团队变得更困难。Copilot 擅长创建文档，因此很适合将这类议题分配给 Copilot 云智能体。它会在后台生成所需文档。后续练习中，你会回来审查成果。

1. 在新的浏览器标签页中打开 github.com 上的存储库。
2. 选择 **Issues** 选项卡。
3. 选择 **New issue** 打开新建议题对话框。
4. 选择 **Blank issue** 创建空白议题。
5. 将 **Title** 设置为 `Code lacks documentation`。
6. 将 **Description** 设置为：

    ```plaintext
    Our organization has a requirement that functions and methods include TSDoc doc comments where helpful. Unfortunately, recent updates haven't followed this standard. We need to update the existing code to ensure doc comments are included where they clarify behavior.
    ```

7. 选择 **Create** 创建议题。
8. 在右侧选择 **Assign to Copilot**，打开分配对话框。

    ![将议题分配给 Copilot](../../../_images/shared-assign-copilot.png)

9. 选择 **Assign**。
10. 选择 **Pull Requests** 选项卡。
11. 打开刚创建的拉取请求（PR），其标题类似 `[WIP]: Code lacks documentation`。如果列表中尚未出现新 PR，请稍等片刻并刷新浏览器。
12. 几分钟后，Copilot 应会创建待办事项列表。

> [!NOTE]
> Copilot 的待办事项列表可能需要几分钟才会出现在 PR 中。Copilot 正在创建环境（运行前面介绍的工作流）、分析项目并确定最佳解决方案。

13. 查看列表及其计划完成的任务。
14. 向下滚动拉取请求时间线，应能看到 Copilot 已开始处理议题的更新。
15. 选择 **View session** 按钮。

    ![Copilot 会话视图](../../../_images/ex4-view-session.png)

> [!CAUTION]
> 可能需要刷新页面才能看到更新后的状态指示器。

16. 浏览实时会话，了解 Copilot 如何解决问题，包括探索代码、了解当前状态、暂停思考并确定合适的计划，以及编写代码。

这可能需要几分钟。Copilot 云智能体的主要目标之一是异步执行任务，让我们能专注于其他工作。接下来利用这一特性，再给云智能体分配一项任务，然后我们转而编写代码，为应用添加功能。

## 在游戏详情页添加相关游戏部分

如前所述，GitHub Copilot 云智能体的一大优势是可以分工处理任务，让你专注于一组工作，而它负责另一组。为设计团队添加相关游戏部分或许不需要太久，但这段时间仍可用于其他任务。现在把它分配给 Copilot 云智能体。

1. 返回 github.com 上的存储库。
2. 选择 **Issues** 选项卡。
3. 选择 **New issue** 打开新建议题对话框。
4. 选择 **Blank issue** 使用空白模板。
5. 将 **Title** 设置为：`Show related games on the game details page`
6. 将 **Description** 设置为：

    ```markdown
    We want to help people discover more games by showing related games on each game's details page. The design team wants to explore the UX and do some acceptance testing. Our requirements are:

   - Add a data-access helper in `src/lib/` that returns other games in the same category as a given game, excluding that game
   - Show a "Related games" section on the game details page that uses the helper
   - Handle the case where a game has no related games
   - There should be unit tests created for the new helper
   - Before creating the PR, ensure all tests pass
    ```

7. 选择 **Create** 创建议题。
8. 在右侧选择 **Assign to Copilot**，打开分配对话框。

    ![将议题分配给 Copilot](../../../_images/shared-assign-copilot.png)

9. 选择 **Assign**。

稍后你会在议题的第一条评论中看到一组 👀，表示 Copilot 已开始处理。

![Copilot 使用眼睛表情符号表示正在处理议题](../../../_images/ex4-issue-eyes-emoji.png)

Copilot 正在处理第二项请求！Copilot 云智能体会像软件工程师一样工作，因此无需主动监控；等它完成后再审查即可。现在可以开始了解如何创建和使用自定义智能体。

## 总结和后续步骤

本课介绍了 [GitHub Copilot 云智能体][copilot-agents]。你可以将议题分配给云智能体，让它异步处理。可以让 Copilot 处理技术债、创建新功能，或协助将代码迁移到其他框架。

你了解了以下概念：

- 自定义代码生成环境。
- 确保操作安全。
- 明确限定议题范围的重要性。
- 将议题分配给 Copilot。

云智能体正在后台工作，我们现在可以转而创建和使用自定义智能体。[Copilot 云智能体也可以使用 MCP 服务器][cloud-agent-mcp]，并能读取自定义指令；前面的模块已经介绍过这部分内容。

## 资源

- [关于 Copilot 云智能体][copilot-agents]
- [将 GitHub 议题分配给 Copilot][assign-issue]
- [Copilot 云智能体设置工作流最佳实践][cloud-agent-best-practices]

| [下一课：自定义智能体 →][next-lesson] |
|--:|

[previous-lesson]: ../1-custom-instructions/
[next-lesson]: ../3-custom-agents/
[cloud-agent-mcp]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/extend-cloud-agent-with-mcp
[assign-issue]: https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/start-copilot-sessions
[setup-workflow]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[copilot-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[cloud-agent-best-practices]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[github-actions]: https://docs.github.com/actions
