---
title: "第 4 课 - 使用自定义指令引导 Copilot"
description: "了解存储库指令，添加文档标准，并将其应用到筛选代码。"
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

上下文对于生成式 AI 至关重要。如果任务需要以特定方式完成，就应确保 Copilot 能获取相应指导。[指令文件][instruction-files]不仅描述你希望编写什么代码，也说明代码应如何组织。现在你已构建筛选功能，接下来将了解 Copilot 使用的指令、添加文档标准并将其应用到代码中。

本课将：

- 了解存储库指令和按路径限定的指令文件如何提供给智能体。
- 更新指令文件，确保遵循编码标准。
- 观察指令文件对代码的影响。

## 场景

Tailspin Toys 为开发实践制定了一套规范和要求，包括：

- 注释应解释意图和不明显的决策，而不是复述代码。
- `db/` 和 `src/lib/` 中导出的函数应使用 TSDoc/JSDoc 说明其用途、参数和返回值；如果有可注入的 `db` 参数，也应予以说明。
- 可复用 Astro 组件应说明其 `Props` 契约；相关代码更改时，注释也应保持更新。
- 应保留现有格式规范和 lint 指南。

通过指令文件，可以确保 Copilot 遵循这些实践完成任务。

## 指令文件

自定义指令可为 Copilot 提供上下文和偏好，帮助它理解你的编码风格和要求。你可以通过指令引导 Copilot 生成更相关的建议和代码片段，例如指定编码约定、库以及注释风格。指令可应用于整个存储库，也可针对特定文件类型提供任务级上下文。

指令文件分为两类：

- `.github/copilot-instructions.md`：每次针对该存储库的请求都会发送给 Copilot 的单个指令文件。它应包含适用于大多数请求的项目级信息。
- `.github/instructions/*.instructions.md`：为特定语言、文件类型或任务提供指导的指令文件。

> [!NOTE]
> 不同环境对其他指令格式的支持有所不同。依赖特定格式前，请查阅[自定义指令支持参考][custom-instructions-support]。

## 浏览项目中的自定义指令文件

起始项目已包含一组指令文件。先了解现有内容，再进行更改并观察效果。

1. 返回 Codespace。
2. 在 Codespaces 编辑器（不是终端）中打开 `.github/copilot-instructions.md`。
3. 浏览文件，了解项目简介和编码指导。这些指令适用于 Copilot 在此存储库中的每次交互。
4. 打开 `.github/instructions` 文件夹并浏览内容，其中包括 Astro、Drizzle 数据层、测试等指令。
5. 打开 `.github/instructions/unit-tests.instructions.md`。注意顶部的 `applyTo` 字段，它指定指令适用文件的 glob 模式。
6. 打开 `.github/instructions/drizzle.instructions.md`，查看它指向的其他指令文件和项目文件。这种方式可以将大型指令集拆分成可复用的小文件，并提供生成代码时可参考的示例。

> [!NOTE]
> `.github/copilot-instructions.md` 中的 **Code formatting guidance** 部分介绍了项目编码标准，但尚未要求在代码中添加文档。接下来会添加 TSDoc 文档注释标准。

## 更新指令文件以符合团队指导

修改核心 `copilot-instructions.md` 文件，为新生成的 TypeScript 添加 TSDoc 注释。

1. 打开 `.github/copilot-instructions.md`，找到 **Code formatting guidance** 部分。
2. 在该部分末尾添加以下项目：

   ```markdown
   - All new TypeScript should contain TSDocs comments for documentation purposes.
   ```

文件会自动保存！

## 使用更新后的指导

Copilot CLI 会在对话开始时加载存储库指令。保存更新后，退出并恢复筛选对话，让 Copilot 在保留功能上下文的同时加载新指导。

1. 如果筛选对话仍处于打开状态，输入 `/exit` 关闭。Copilot CLI 会自动保存对话。
2. 在 `game-filters-cli` 分支的存储库根目录中恢复已保存的对话：

   ```bash
   copilot --resume
   ```

   如果系统提示，请选择筛选对话，并确认当前检出的是 `game-filters-cli` 分支。
3. 请求 Copilot 应用更新后的指导：

   ```plaintext
   We just updated our instructions and code guidance. Can you please update the code you generated to match that guidance?
   ```

4. 输入 `/diff` 并查看更改的 TypeScript 文件。注意新生成的 TSDoc 注释，并确认其准确说明了代码。

## 总结与后续步骤

你已了解 Copilot 如何从指令文件获取上下文，并将新的标准应用于功能代码。接下来，[自定义并运行可复用的 quality-checks 技能][next-lesson]，确保一致地运行 lint 和测试。

## 资源

- [为 Copilot CLI 添加自定义指令][instruction-files]
- [自定义指令支持][custom-instructions-support]
- [创建自定义指令的最佳实践][instructions-best-practices]
- [Awesome Copilot：指令文件合集][awesome-copilot]

[instruction-files]: https://docs.github.com/copilot/how-tos/copilot-cli/customize-copilot/add-custom-instructions
[instructions-best-practices]: https://docs.github.com/copilot/concepts/prompting/response-customization#writing-effective-custom-instructions
[awesome-copilot]: https://awesome-copilot.github.com/
[custom-instructions-support]: https://docs.github.com/copilot/reference/custom-instructions-support
[previous-lesson]: ../3-agent-modes/
[next-lesson]: ../5-agent-skills/
