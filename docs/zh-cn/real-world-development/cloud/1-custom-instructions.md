---
title: "练习 1 - 自定义指令（云智能体）"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

[← 上一课：先决条件][previous-lesson] · [下一课：Copilot 云智能体 →][next-lesson]

使用生成式 AI 时，上下文至关重要。如果任务必须以特定方式完成，或 Copilot 应了解某些背景信息，就需要确保它能获取这些上下文。[指令文件][instruction-files]用于提供此类指导，让 Copilot 不仅了解你想让它做什么，也了解应如何完成。

本练习将学习如何：

- 通过存储库自定义指令和按路径限定的指令文件，将项目上下文、编码指南和文档标准传递给 Copilot。
- 向 `.github/copilot-instructions.md` 添加新的存储库级标准。

> [!NOTE]
> 与 VS Code 和 CLI 工具环境不同，这里不会运行“前后对比”提示。Copilot 云智能体会异步处理 GitHub 议题，因此较难实时并排展示指令的影响。后续练习中审查云智能体生成的拉取请求时，你会看到指令文件发挥的作用。

## 指令文件

### 场景

Tailspin Toys 和其他优秀的软件开发公司一样，有一套开发实践准则和要求，包括：

- 数据层始终需要单元测试。
- UI 应使用暗色模式并具有现代感。
- 应通过 TSDoc 文档注释为代码添加说明。
- 每个文件开头都应添加一段注释，说明文件的用途。

通过使用指令文件，可以确保 Copilot 获取正确的信息，并按照这些实践执行任务。

### 自定义指令

自定义指令为 Copilot 提供上下文和偏好设置，帮助它理解你的编码风格和要求。它可以让你引导 Copilot 提供更符合需求的建议和代码片段。你可以指定偏好的编码约定、库，甚至希望添加的注释类型。可以为整个存储库创建指令，也可以为特定文件类型创建指令，提供任务级上下文。

指令文件有两种：

- `.github/copilot-instructions.md`：单个指令文件，会随该存储库的**每个**请求发送给 Copilot。文件应包含适用于大多数 Copilot 聊天或 CLI 请求的项目级信息，例如技术栈、项目概述、最佳实践和全局指导。
- `.github/instructions/*.instructions.md`：针对特定任务或文件类型创建的文件。你可以为 TypeScript 或 Astro 等语言提供指导，也可以为创建 UI 组件或新的单元测试等任务提供指导。

> [!NOTE]
> 在 IDE 中，指令文件仅用于 Copilot Chat 代码生成，不用于代码补全或下一个编辑建议。
>
> Copilot Chat、Copilot CLI 和 Copilot 云智能体在生成代码时都会使用存储库级指令文件和包含 `applyTo` frontmatter 的 `*.instructions.md` 文件。
>
> Copilot 还[支持其他标准的指令文件][custom-instructions-support]，包括 AGENTS.md 和 CLAUDE.md 文件。

### 管理指令文件的最佳实践

在此完整讨论如何创建指令文件超出了本工作坊的范围。不过，示例项目展示了具有代表性的方法。总体而言：

- 让 `copilot-instructions.md` 专注于项目级指导，例如项目内容、项目结构和全局编码标准。
- 使用 `*.instructions.md` 文件针对文件类型（单元测试、Astro 组件、数据层）或特定任务提供指导。
- 使用自然语言。指导应清晰，并提供代码应如何编写（以及不应如何编写）的示例。

指令文件没有唯一的编写方式，正如 AI 也没有唯一的使用方式。你可以通过实践找出最适合项目的方法。

> [!TIP]
> 每个使用 GitHub Copilot 的项目都应具备完善的指令文件。浏览本项目中的文件时，可能会发现有针对多种任务的指令，包括 [UI 更新][ui-instructions]和 [Astro][astro-instructions]。
>
> Copilot 也可以帮助生成指令文件。不同界面提供的方式不同（例如 VS Code 中的 **Configure Chat → Generate Agent Instructions**，或 Copilot CLI 中的 `/init`）。相应工作坊会在适当位置介绍这些功能。
>
> 想查找模板或起点？可以浏览 [awesome-copilot][awesome-copilot]，其中包含大量指令文件、自定义智能体和其他资源。

[ui-instructions]: https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/ui.instructions.md
[astro-instructions]: https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/astro.instructions.md
[awesome-copilot]: https://github.com/github/awesome-copilot
[custom-instructions-support]: https://docs.github.com/copilot/reference/custom-instructions-support

## 浏览本项目中的自定义指令文件

花些时间阅读存储库自带的指令文件——其中有一个核心 `copilot-instructions.md` 文件，以及一组针对不同任务的 `*.instructions.md` 文件。可在编辑器或 GitHub 网页界面中打开这些文件。

1. 打开 `.github/copilot-instructions.md`。
2. 浏览文件，注意其中简要介绍了项目，并包含 **Agent notes**、**Code standards**、**Scripts** 和 **Repository Structure** 等部分。**Code standards** 下还有嵌套的 **GitHub Actions Workflows** 指导。这些内容适用于与 Copilot 的任何交互。
3. 打开 `.github/instructions` 文件夹并查看其中的文件。你会看到针对 Astro 文件、Drizzle 数据层、测试等内容的指令。
4. 打开 `.github/instructions/unit-tests.instructions.md`。注意顶部的 `applyTo` 字段，它用于设置决定指令适用文件的 glob。此处匹配任何 TypeScript 测试文件，例如符合 `**/*.test.ts` 的文件。
5. 查看此项目针对单元测试的具体指令。
6. 最后，打开 `.github/instructions/drizzle.instructions.md` 并滚动到文件底部。注意指向其他指令文件（如 `unit-tests.instructions.md`）和项目现有文件的链接。这样可以将较大的指令集拆分成较小、可复用的文件，并提供可供 Copilot 参考的示例。（这些路径相对于指令文件，而非存储库根目录。）

> [!NOTE]
> `copilot-instructions.md` 中的 **Code formatting requirements** 部分介绍了项目编码标准，但目前还没有要求代码内文档。接下来，你会添加 TSDoc 文档注释和文件注释头的规则。

## 添加新的存储库标准

接下来只需进行一项编辑：添加一条项目级规则，要求以 TSDoc 文档注释和文件级注释头的形式在代码中记录说明。之后，本工作流中云智能体处理议题时会遵循这条标准。

编辑前，先创建一个分支（云智能体会从议题目标分支读取指令，但仍建议干净地提交所做更改）：

1. 在 codespace 终端中创建并切换到新分支：

   ```bash
   git checkout -b custom-instructions-cloud
   ```

如前所述，`.github/copilot-instructions.md` 用于向 Copilot 提供项目级信息。现在将编码标准记录在此文件中，改善代码建议。

1. 重新打开 `.github/copilot-instructions.md`。
2. 找到 **Code formatting requirements** 部分，大约位于第 27 行。注意其中记录了项目编码标准，但没有代码内文档规则，这正是生成的辅助函数没有文档注释的原因。
3. 在现有标准下方添加以下 Markdown 内容，要求 Copilot 添加文件注释头和 TSDoc 文档注释：

   ```markdown
   - Every exported function should have a TSDoc comment describing its purpose, parameters, and return value.
   - Before imports or any code, add a comment block to the file that explains its purpose.
   ```

4. 保存 `copilot-instructions.md`。

> [!TIP]
> 如上一课所示，指令文件可以放在存储库级别（`.github/copilot-instructions.md`），以提供全局指导；也可以使用 `*.instructions.md` 文件针对特定语言、文件类型或任务提供指导。存储库级文件适合放置 TSDoc 注释规则等项目级标准。

## 提交、推送并合并指令更新

云智能体会从议题目标分支读取指令。下一练习中，将议题分配给 Copilot 时，它会从 `main` 分支创建新分支，因此必须先将指令更改合并到 `main`，云智能体才能使用。

1. 暂存并提交：

   ```bash
   git add .github/copilot-instructions.md
   git commit -m "Add doc comment and file-header standards to copilot instructions"
   ```

2. 推送分支：

   ```bash
   git push -u origin custom-instructions-cloud
   ```

3. 在 github.com 上创建从 `custom-instructions-cloud` 合并到 `main` 的拉取请求并将其合并。下一练习中，云智能体处理议题时就会读取这些指令。

> [!TIP]
> 如果希望在本工作坊中直接使用 `main`，可以跳过分支步骤并直接提交到 `main`。此处保留分支操作，是为了让工作坊更贴近真实项目流程。

## 总结和后续步骤

你了解了 Copilot 如何读取本项目的指令文件，并向 `.github/copilot-instructions.md` 添加了新的存储库级标准。后续练习中，你会看到云智能体生成的拉取请求遵循这条标准。

接下来，[将第一个议题分配给 Copilot 云智能体][next-lesson]。

## 资源

- [用于自定义 GitHub Copilot 的指令文件][instruction-files]
- [创建自定义指令的最佳实践][instructions-best-practices]
- [编写更好的 Copilot 自定义指令的五个技巧][copilot-instructions-five-tips]
- [Awesome Copilot：指令文件和其他资源集合][awesome-copilot]

[previous-lesson]: ../0-prerequisites/
[next-lesson]: ../2-cloud-agent/
[instruction-files]: https://docs.github.com/copilot/customizing-copilot/about-customizing-github-copilot-chat-responses
[instructions-best-practices]: https://docs.github.com/enterprise-cloud@latest/copilot/using-github-copilot/coding-agent/best-practices-for-using-copilot-to-work-on-tasks#adding-custom-instructions-to-your-repository
[copilot-instructions-five-tips]: https://github.blog/ai-and-ml/github-copilot/5-tips-for-writing-better-custom-instructions-for-copilot/
