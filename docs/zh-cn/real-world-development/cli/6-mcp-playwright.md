---
title: "第 6 课 - 使用 Playwright MCP 验证功能"
description: "检查或配置 Copilot CLI 中的 Playwright MCP，并在浏览器中体验筛选功能。"
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

前面提到过，编写代码不只是写代码。我们还要处理数据、外部服务，并允许 Copilot 使用其他自动化工具。这正是 MCP 服务器的用途。MCP 服务器可以扩展 Copilot CLI 的内置功能，为它提供更多工具和服务。

本课将：

- 了解 Model Context Protocol (MCP) 及 Copilot CLI 对它的使用方式。
- 如果尚未配置 Playwright MCP 服务器，则添加它。
- 请求智能体操作浏览器，体验筛选功能。

## 什么是 Model Context Protocol (MCP)？

[Model Context Protocol (MCP)][mcp-blog-post] 为 AI 智能体提供了与外部工具和服务通信的方式。借助 MCP，AI 智能体可以实时访问最新信息，并代表你执行操作。

智能体通过 MCP 服务器访问这些工具和资源。MCP 服务器充当 AI 智能体与外部工具和服务之间的桥梁，每个服务器都提供一组可供智能体访问的工具和资源。

常见的 MCP 服务器包括：

- **[GitHub MCP Server][github-mcp]**：提供用于管理 GitHub 存储库、议题和拉取请求的 API。
- **[Playwright MCP Server][playwright-mcp-server]**：使用 Playwright 提供浏览器自动化功能。

还有许多其他 MCP 服务器。GitHub 提供了 [MCP 注册表][mcp-registry]，帮助发现生态系统中的服务器并参与贡献。

> [!CAUTION]
> 应像对待项目中的其他依赖项一样对待 MCP 服务器。使用前，请检查其源代码、验证发布者并考虑安全影响。

## 添加 Playwright MCP 服务器

如果 Copilot CLI 中尚未配置 Playwright MCP 服务器，请添加它，让 Copilot 能像用户一样操作网站。

> [!NOTE]
> 如果 Copilot CLI 中已经配置了 Playwright MCP，请跳过这些设置步骤，继续下一节。

1. 返回 Codespace。
2. 在 Copilot CLI 中输入以下命令，打开添加 MCP 服务器对话框：

   ```plaintext
   /mcp add
   ```

3. 输入名称 `playwright`，然后按 <kbd>Tab</kbd>。
4. 按 <kbd>Enter</kbd> 确认服务器类型为 **STDIO**，然后按 <kbd>Tab</kbd>。
5. 在 **Command** 对话框中粘贴以下命令：

   ```plaintext
   npx -y @playwright/mcp@latest --headless --no-sandbox
   ```

6. 按 <kbd>Control</kbd>+<kbd>S</kbd>（Mac）或 <kbd>Ctrl</kbd>+<kbd>S</kbd>（Windows/Linux）保存新的 MCP 服务器。
7. 按 <kbd>Esc</kbd> 退出 MCP 对话框。

## 请求 Copilot 使用 Playwright 探索功能

之前你已手动确认功能符合预期。现在使用 Copilot 中的 Playwright MCP 服务器执行同样的检查。

1. 使用以下提示，请求 Copilot 通过 Playwright MCP 服务器验证功能：

   ```plaintext
   Start the app and use Playwright MCP to check filtering against the issue and our plan. Tell me what works and what doesn't, without making changes. Stop the server you started when you're done.
   ```

> [!NOTE]
> 通常无需明确指定 MCP 服务器，Copilot 会自行判断。不过，既然你知道要使用哪个服务器，明确提示有助于保持结果一致，还能节省一些 token。

2. 观察 Copilot 在浏览器中执行各项操作以确认功能是否正常。
3. 阅读报告，确认所有行为符合预期。

Copilot 会启动服务器，通过 Playwright 操作网站，停止服务器，并报告结果。

## 总结与后续步骤

恭喜，你已在 Copilot CLI 中使用 Playwright MCP 服务器，通过真实浏览器体验功能！回顾一下，你：

- 了解了 Model Context Protocol (MCP) 及 Copilot CLI 对它的使用方式。
- 如果之前未配置，则添加了 Playwright MCP 服务器。
- 请求智能体操作浏览器，体验筛选功能。

接下来，创建一个[质量保证 (QA) 自定义智能体][next-lesson]，将技能和浏览器工具整合到专门角色中。

## 资源

- [MCP 是什么，为什么大家都在讨论它？][mcp-blog-post]
- [Microsoft Playwright MCP Server][playwright-mcp-server]
- [向 Copilot CLI 添加 MCP 服务器][add-mcp]

[previous-lesson]: ../5-agent-skills/
[next-lesson]: ../7-qa-agent/
[mcp-blog-post]: https://github.blog/ai-and-ml/llms/what-the-heck-is-mcp-and-why-is-everyone-talking-about-it/
[playwright-mcp-server]: https://github.com/microsoft/playwright-mcp
[github-mcp]: https://github.com/github/github-mcp-server
[mcp-registry]: https://github.com/mcp
[add-mcp]: https://docs.github.com/copilot/how-tos/copilot-cli/customize-copilot/add-mcp-servers
