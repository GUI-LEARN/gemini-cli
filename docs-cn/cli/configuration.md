# Gemini CLI 配置

Gemini CLI 提供了多种配置其行为的方法，包括环境变量、命令行参数和设置文件。本文档概述了不同的配置方法和可用设置。

## 配置层次

配置按以下优先级顺序应用（较低的数字被较高的数字覆盖）：

1. **默认值：** 应用程序内的硬编码默认值。
2. **用户设置文件：** 当前用户的全局设置。
3. **项目设置文件：** 项目特定的设置。
4. **系统设置文件：** 系统范围的设置。
5. **环境变量：** 系统范围或会话特定的变量，可能从 `.env` 文件加载。
6. **命令行参数：** 启动 CLI 时传递的值。

## 设置文件

Gemini CLI 使用 `settings.json` 文件进行持久配置。这些文件有三个位置：

- **用户设置文件：**
  - **位置：** `~/.gemini/settings.json`（其中 `~` 是您的主目录）。
  - **作用域：** 适用于当前用户的所有 Gemini CLI 会话。
- **项目设置文件：**
  - **位置：** 项目根目录中的 `.gemini/settings.json`。
  - **作用域：** 仅在从该特定项目运行 Gemini CLI 时适用。项目设置覆盖用户设置。
- **系统设置文件：**
  - **位置：** `/etc/gemini-cli/settings.json`（Linux）、`C:\ProgramData\gemini-cli\settings.json`（Windows）或 `/Library/Application Support/GeminiCli/settings.json`（macOS）。
  - **作用域：** 适用于系统上所有用户的所有 Gemini CLI 会话。系统设置覆盖用户和项目设置。对于企业的系统管理员控制用户的 Gemini CLI 设置可能很有用。

**设置中环境变量的说明：** 您的 `settings.json` 文件中的字符串值可以使用 `$VAR_NAME` 或 `${VAR_NAME}` 语法引用环境变量。这些变量将在加载设置时自动解析。例如，如果您有环境变量 `MY_API_TOKEN`，您可以在 `settings.json` 中这样使用：`"apiKey": "$MY_API_TOKEN"`。

### 项目中的 `.gemini` 目录

除了项目设置文件，项目的 `.gemini` 目录还可以包含与 Gemini CLI 操作相关的其他项目特定文件，例如：

- [自定义沙箱配置文件](#沙箱)（例如，`.gemini/sandbox-macos-custom.sb`、`.gemini/sandbox.Dockerfile`）。

此文档的其余部分包含了所有可用的配置选项、环境变量和命令行参数的详细信息。