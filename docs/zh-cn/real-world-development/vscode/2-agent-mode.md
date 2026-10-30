---
title: "练习 2 - 使用 Copilot 代理模式添加新功能"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

即使是最简单的应用更新，通常也需要修改多个文件并执行运行测试等操作。开发者的工作流程通常包括：找到所有必需文件、进行更改、运行测试、调试、找出遗漏的文件、再次更新……后续工作还有很多。

Copilot 代理模式正适合处理这种情况。

Copilot 代理模式可以在 IDE 中更自主地工作。它的行为类似开发者：先探索现有项目结构，执行必要更新，运行测试等任务，并自动修复发现的问题。现在了解如何使用代理模式为网站添加新功能。

> [!NOTE]
> 代理模式和云智能体的名称相似，但适用于两种不同的体验。代理模式在 IDE 中执行任务，支持快速反馈和交互。云智能体则像团队成员一样异步工作，通过议题和拉取请求与你协作。

本练习将学习如何：

- 让 Copilot 代理模式探索项目、确定相关文件并协调更改。
- 使用 GitHub Copilot 代理模式跨 UI 和数据层实现新功能。
- 合并到代码库前审查 Copilot 代理模式生成的更改和测试。

## 场景

随着游戏列表不断增加，你希望用户能够按类别和发行商筛选。上一练习已添加发行商辅助函数，现在将使用 Copilot 代理模式完成其余的数据层、UI 和测试工作。

## 运行 Tailspin Toys 网站

在修改前，先浏览 Tailspin Toys 网站，了解其现有功能。

该网站是一个以开发者为主题的桌游众筹平台，可列出游戏并显示详细信息。它是一个单独的 Astro 应用，构建时将页面渲染为静态 HTML。页面通过 Drizzle ORM 在 frontmatter 中直接查询本地 SQLite 数据库，没有单独的后端 API 或客户端 UI 框架。可复用的数据访问辅助函数位于 `src/lib/` 中；交互功能由使用标准 DOM API 的小型、限定范围的 Astro `<script>` 实现。

### 启动网站

项目提供了 `npm run dev` 任务，用于启动单个 Astro 开发服务器，让网站更容易运行。按照以下步骤在 GitHub Codespace 中运行：

1. 返回 codespace。上一练习结束后，你应该在 `main` 分支上。
2. 按 <kbd>Control</kbd>+<kbd>\`</kbd>（Mac）或 <kbd>Ctrl</kbd>+<kbd>\`</kbd>（Windows/Linux），在 Codespace 中打开新终端。
3. 创建并切换到筛选功能分支：

   ```bash
   git checkout main
   git pull
   git checkout -b filtering-vscode
   ```

4. 运行以下命令启动网站：

   ```bash
   npm run dev
   ```

   Astro 开发服务器就绪后，会显示类似以下内容的横幅：

   ```bash
   🚀 Tailspin Toys is ready!
      Astro server: http://localhost:4321
      Press Ctrl-C to stop.
   ```

> [!NOTE]
> 如果弹出对话框，询问是否要为 `http://localhost:4321` 打开浏览器窗口，请选择 **x** 将其关闭。

5. 按住 <kbd>Command</kbd>（Mac）或 <kbd>Ctrl</kbd>（Windows/Linux），然后选择终端中的 Astro 服务器地址 `http://localhost:4321`，打开网站。

> [!NOTE]
> 在 codespace 中，从终端选择 localhost URL 会自动重定向到 `https://<your-codespace-name>-4321.app.github.dev/`。这是通往 codespace 的专用隧道，网站服务器正在该环境中运行。

### 浏览网站

网站运行后，可以浏览其功能。主要功能包括：

- **主页**：显示桌游列表，包括标题、图片和描述。
- **游戏详情页**：选择游戏后会打开详情页，显示标题、描述、发行商和类别等详细信息。

## 使用 Copilot 浏览积压工作

> [!TIP]
> **打开 Copilot Chat**
>
> 开始以下练习前，返回 codespace，打开 Copilot Chat 面板并选择 **New Chat**，开始一个干净的对话。不同练习的模式和模型设置有所不同；每个步骤会在适用时说明。

网站的初始实现已经可以使用，但我们希望添加更多功能。先查看积压工作。根据模板设置项目时，系统已自动创建议题积压工作；让 GitHub Copilot 显示这些项目。

1. 在 Chat 视图的智能体下拉菜单中选择 **Agent**。**Agent** 智能体可以自主规划并跨文件实现更改、运行终端命令和调用工具。

   ![Chat 视图中智能体选择器的截图。](../../../_images/shared-chat-mode-selector.png)

2. 从可用模型列表中选择 **Claude Sonnet 4.5**。

> [!CAUTION]
> 本工作坊作者并非推荐某个模型。构建本工作坊时使用的是 Claude Sonnet 4.5，因此将它写入了说明，希望让代码建议相对一致，以便获得良好体验。不过，由于 LLM 具有概率性，你看到的建议可能与工作坊中的示例不同。这完全正常，也在预期之内。

> [!NOTE]
> 由于 LLM 具有概率性，Copilot 可能会使用不同的 MCP 命令，但仍应能完成任务。

3. 向 Copilot 发送以下提示，询问议题积压工作：

   ```plaintext
   Please show me the backlog of items from my GitHub repository. Help me prioritize them based on those which will be most useful to the user.
   ```

4. 选择 **Continue** 以运行列出所有议题的命令。
5. 查看生成的议题列表。

注意，Copilot 还会根据它认为对用户最有用的程度为项目排序。

## 查看指令文件

让智能体开始生成代码之前，先查看用于为 Copilot 工作提供上下文的指令文件。我们将使用[用户界面 (UI)][ui-instructions]指令文件，其中包含了如何为网站添加功能的上下文。

1. 在 codespace 中打开 `.github/instructions/ui.instructions.md`。
2. 注意其中提供的整体指导，包括：
   - 架构概述。
   - 组件设计、可测试性和无障碍原则。
   - 指向多种文件类型对应指令文件的链接，包括：
     - Astro
     - Tailwind CSS
     - Drizzle

> [!TIP]
> 指令文件可以引用其他指令文件以及项目中的文件。路径相对于指令文件所在位置，便于复用、拆分较大的指令集并提供示例和模板。

[ui-instructions]: https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/ui.instructions.md

## 实现筛选功能

要完成筛选功能，至少需要对应用进行以下三项独立更新：

- 在数据层（`src/lib/`）添加或完善其余筛选逻辑。
- 添加或更新筛选行为测试。
- 更新游戏列表页面并添加筛选 UI。

此外，合并前需要运行并通过测试。Copilot 代理模式可以完成这些任务！现在添加筛选功能。

1. 可以继续当前 Copilot 对话，也可以选择 **New Chat** 开始新对话。
2. 选择 **Add Context**、**Instructions** 和 **ui** 指令文件。

   ![选择 UI 指令文件的示例截图](../../../_images/ex3-select-instructions-file.png)

3. 确认 Chat 视图的智能体下拉菜单中仍选中 **Agent**。

   ![Chat 视图中智能体选择器的截图。](../../../_images/shared-chat-mode-selector.png)

4. 确认仍选择 **Claude Sonnet 4.5** 模型。
5. 使用以下提示，让 Copilot 根据积压工作中的相关议题实现功能：

   ```plaintext
   Please update the site to include filtering by publisher and category based on the requirements from the related GitHub issue in the backlog. A publishers helper already exists from the previous exercise, so preserve and refine that work as needed while completing the remaining data-layer, UI, and tests. Ensure all tests are passing before completion. The server is already running, so you do not need to start it up.
   ```

6. 观察 Copilot 如何先探索项目并查找相关文件。你会看到它查找数据层辅助函数、UI 和测试，随后修改文件并运行测试。

   ![Copilot 正在探索项目文件的截图](../../../_images/ex3-agent-mode-explores.png)

> [!NOTE]
> Copilot 会执行多个任务，例如探索项目、修改文件和运行测试。根据任务和代码库的复杂程度，这可能需要几分钟。过程中，代码编辑器中可能会出现 **Keep** 和 **Undo** 按钮。Copilot 完成后，所有更改都会显示 **Keep** 或 **Undo**，因此无需在工作进行期间选择它们。

7. 按 Copilot 的提示选择 **Continue** 以运行测试。

   ![Copilot Chat 窗格中询问用户是否允许运行测试的对话框截图](../../../_images/ex3-agent-mode-run-tests.png)

8. Copilot 可能会暂停一段时间，或在处理中途遇到测试失败。这没有关系！Copilot 会在生成代码和运行测试之间反复迭代，直到任务完成且未发现错误。

   ![Copilot 代理模式完整聊天会话的截图](../../../_images/ex3-agent-mode-proposed-changes.png)

9. 检查生成的代码，查找潜在问题。

> [!CAUTION]
> 请记住，始终需要审查 Copilot 或其他 AI 工具生成的代码。

10. 返回正在运行网站的浏览器，体验新功能。
11. 确认功能正常并审查代码后，在 Copilot Chat 窗口中选择 **Keep**。

## 总结和后续步骤

恭喜！本练习中，我们了解了如何使用 GitHub Copilot 代理模式为 Tailspin Toys 网站添加功能。你学会了如何：

- 使用 GitHub Copilot 代理模式跨 UI 和数据层实现新功能。
- 让 Copilot 代理模式探索项目、确定相关文件并协调更改。
- 合并到代码库前审查 Copilot 代理模式生成的更改和测试。

接下来，[使用 Playwright MCP 服务器测试功能][next-lesson]并为该功能创建拉取请求。

### 额外探索练习——实现分页

随着游戏列表不断增加，之后需要添加分页。运用本练习学到的技能，提示 Copilot 更新网站并实现分页。编写提示时可考虑：

- 遵循现有最佳实践，包括使用已有指令文件。
- 考虑分页方案，例如允许用户选择每页大小或将其设为固定值。
- 编写提示时，为 Copilot 提供实现目标所需的指导。
- 可能需要与 GitHub Copilot 反复迭代、提出更改请求并提供上下文。这正是使用 Copilot 的正常流程。

## 资源

- [Copilot 的 ask、edit 和 agent 模式：功能及适用场景][choose-mode]
- [VS Code 中的代理模式][vs-code-agent-mode]

[previous-lesson]: ../1-custom-instructions/
[next-lesson]: ../3-mcp/
[choose-mode]: https://github.blog/ai-and-ml/github-copilot/copilot-ask-edit-and-agent-modes-what-they-do-and-when-to-use-them/
[vs-code-agent-mode]: https://code.visualstudio.com/docs/copilot/chat/chat-agent-mode
