# Gemini CLI Core：工具 API

Gemini CLI 核心（`packages/core`）具有一个强大的系统，用于定义、注册和执行工具。这些工具扩展了 Gemini 模型的功能，使其能够与本地环境交互、获取 web 内容，并执行超出简单文本生成的各种操作。

## 核心概念

- **工具（`tools.ts`）：** 定义所有工具契约的接口和基类（`BaseTool`）。每个工具必须具有：
  - `name`：唯一的内部名称（在对 Gemini 的 API 调用中使用）。
  - `displayName`：用户友好的名称。
  - `description`：工具功能的清晰说明，提供给 Gemini 模型。
  - `parameterSchema`：定义工具接受的参数的 JSON 模式。这对于 Gemini 模型理解如何正确调用工具至关重要。
  - `validateToolParams()`：验证传入参数的方法。
  - `getDescription()`：提供工具在执行前将对特定参数执行的操作的人类可读描述的方法。
  - `shouldConfirmExecute()`：确定在执行前是否需要用户确认的方法（例如，对于潜在的破坏性操作）。
  - `execute()`：执行工具操作并返回 `ToolResult` 的核心方法。

- **`ToolResult`（`tools.ts`）：** 定义工具执行结果结构的接口：
  - `llmContent`：要包含在发送回 LLM 的历史记录中的事实字符串内容，以提供上下文。
  - `returnDisplay`：用于在 CLI 中显示的用户友好字符串（通常是 Markdown）或特殊对象（如 `FileDiff`）。

- **工具注册表（`tool-registry.ts`）：** 负责以下功能的类（`ToolRegistry`）：
  - **注册工具：** 持有所有可用内置工具的集合（例如，`ReadFileTool`、`ShellTool`）。
  - **发现工具：** 它还可以动态发现工具：
    - **基于命令的发现：** 如果在设置中配置了 `toolDiscoveryCommand`，则执行此命令。它预期输出描述自定义工具的 JSON，然后将其注册为 `DiscoveredTool` 实例。
    - **基于 MCP 的发现：** 如果配置了 `mcpServerCommand`，注册表可以连接到模型上下文协议（MCP）服务器以列出和注册工具（`DiscoveredMCPTool`）。
  - **提供模式：** 向 Gemini 模型公开所有已注册工具的 `FunctionDeclaration` 模式，以便它知道哪些工具可用以及如何使用它们。
  - **检索工具：** 允许核心按名称获取特定工具以供执行。

## 内置工具

核心附带一套预定义的工具，通常在 `packages/core/src/tools/` 中找到。这些包括：

- **文件系统工具：**
  - `LSTool`（`ls.ts`）：列出目录内容。
  - `ReadFileTool`（`read-file.ts`）：读取单个文件的内容。它接受 `absolute_path` 参数，必须是绝对路径。
  - `WriteFileTool`（`write-file.ts`）：向文件写入内容。
  - `GrepTool`（`grep.ts`）：在文件中搜索模式。
  - `GlobTool`（`glob.ts`）：查找与 glob 模式匹配的文件。
  - `EditTool`（`edit.ts`）：对文件执行就地修改（通常需要确认）。
  - `ReadManyFilesTool`（`read-many-files.ts`）：从多个文件或 glob 模式读取和连接内容（由 CLI 中的 `@` 命令使用）。
- **执行工具：**
  - `ShellTool`（`shell.ts`）：执行任意 shell 命令（需要仔细的沙箱化和用户确认）。
- **Web 工具：**
  - `WebFetchTool`（`web-fetch.ts`）：从 URL 获取内容。
  - `WebSearchTool`（`web-search.ts`）：执行 web 搜索。
- **内存工具：**
  - `MemoryTool`（`memoryTool.ts`）：与 AI 的内存交互。

这些工具中的每一个都扩展了 `BaseTool` 并实现了其特定功能所需的方法。

## 工具执行流程

1. **模型请求：** Gemini 模型基于用户提示和提供的工具模式决定使用工具，并在其响应中返回 `FunctionCall` 部分，指定工具名称和参数。
2. **核心接收请求：** 核心解析此 `FunctionCall`。
3. **工具检索：** 它在 `ToolRegistry` 中查找请求的工具。
4. **参数验证：** 调用工具的 `validateToolParams()` 方法。
5. **确认（如果需要）：**
   - 调用工具的 `shouldConfirmExecute()` 方法。
   - 如果它返回确认详情，核心将其传达回 CLI，CLI 提示用户。
   - 用户的决定（例如，继续、取消）被发送回核心。
6. **执行：** 如果验证并确认（或如果不需要确认），核心使用提供的参数和 `AbortSignal`（用于潜在的取消）调用工具的 `execute()` 方法。
7. **结果处理：** 核心接收来自 `execute()` 的 `ToolResult`。
8. **对模型的响应：** 来自 `ToolResult` 的 `llmContent` 被打包为 `FunctionResponse` 并发送回 Gemini 模型，以便它可以继续生成面向用户的响应。
9. **向用户显示：** 来自 `ToolResult` 的 `returnDisplay` 被发送到 CLI 以向用户显示工具所做的事情。

## 使用自定义工具进行扩展

虽然在提供的文件中没有明确详细说明用户直接程序化注册新工具作为典型最终用户的主要工作流程，但该架构通过以下方式支持扩展：

- **基于命令的发现：** 高级用户或项目管理员可以在 `settings.json` 中定义 `toolDiscoveryCommand`。当 Gemini CLI 核心运行此命令时，它应该输出 `FunctionDeclaration` 对象的 JSON 数组。然后核心将这些作为 `DiscoveredTool` 实例提供。相应的 `toolCallCommand` 将负责实际执行这些自定义工具。
- **MCP 服务器：** 对于更复杂的场景，可以设置一个或多个 MCP 服务器，并通过 `settings.json` 中的 `mcpServers` 设置进行配置。然后，Gemini CLI 核心可以发现并使用这些服务器公开的工具。如前所述，如果您有多个 MCP 服务器，工具名称将以您配置中的服务器名称为前缀（例如，`serverAlias__actualToolName`）。

这个工具系统提供了一种灵活而强大的方式来增强 Gemini 模型的功能，使 Gemini CLI 成为各种任务的多功能助手。 