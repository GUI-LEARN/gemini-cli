# Gemini CLI 架构概述

本文档提供了 Gemini CLI 架构的高级概述。

## 核心组件

Gemini CLI 主要由两个主要包组成，以及一套可在处理命令行输入过程中被系统使用的工具：

1. **CLI 包（`packages/cli`）：**
   - **目的：** 包含 Gemini CLI 的用户界面部分，如处理初始用户输入、呈现最终输出和管理整体用户体验。
   - **包中包含的关键功能：**
     - [输入处理](./cli/commands.md)
     - 历史记录管理
     - 显示渲染
     - [主题和 UI 自定义](./cli/themes.md)
     - [CLI 配置设置](./cli/configuration.md)

2. **核心包（`packages/core`）：**
   - **目的：** 作为 Gemini CLI 的后端。它接收来自 `packages/cli` 的请求，协调与 Gemini API 的交互，并管理可用工具的执行。
   - **包中包含的关键功能：**
     - 与 Google Gemini API 通信的 API 客户端
     - 提示构建和管理
     - 工具注册和执行逻辑
     - 对话或会话的状态管理
     - 服务器端配置

3. **工具（`packages/core/src/tools/`）：**
   - **目的：** 这些是扩展 Gemini 模型功能的单独模块，允许它与本地环境交互（例如，文件系统、shell 命令、网络抓取）。
   - **交互：** `packages/core` 基于 Gemini 模型的请求调用这些工具。

## 交互流程

与 Gemini CLI 的典型交互遵循以下流程：

1. **用户输入：** 用户在终端中输入提示或命令，这由 `packages/cli` 管理。
2. **请求到核心：** `packages/cli` 将用户输入发送给 `packages/core`。
3. **处理请求：** 核心包：
   - 为 Gemini API 构建适当的提示，可能包括对话历史记录和可用工具定义。
   - 将提示发送给 Gemini API。
4. **Gemini API 响应：** Gemini API 处理提示并返回响应。此响应可能是直接答案或使用可用工具之一的请求。
5. **工具执行（如果适用）：**
   - 当 Gemini API 请求工具时，核心包准备执行它。
   - 如果请求的工具可以修改文件系统或执行 shell 命令，用户首先会获得工具及其参数的详细信息，用户必须批准执行。
   - 只读操作（如读取文件）可能不需要明确的用户确认即可继续。
   - 一旦确认，或者如果不需要确认，核心包在相关工具中执行相关操作，结果由核心包发送回 Gemini API。
   - Gemini API 处理工具结果并生成最终响应。
6. **响应到 CLI：** 核心包将最终响应发送回 CLI 包。
7. **显示给用户：** CLI 包格式化并在终端中向用户显示响应。

## 关键设计原则

- **模块化：** 将 CLI（前端）与核心（后端）分离允许独立开发和潜在的未来扩展（例如，同一后端的不同前端）。
- **可扩展性：** 工具系统设计为可扩展，允许添加新功能。
- **用户体验：** CLI 专注于提供丰富的交互式终端体验。 