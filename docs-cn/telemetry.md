# Gemini CLI 可观测性指南

遥测提供有关 Gemini CLI 性能、健康状况和使用情况的数据。通过启用它，您可以通过跟踪、指标和结构化日志来监控操作、调试问题和优化工具使用。

Gemini CLI 的遥测系统基于 **[OpenTelemetry]（OTEL）** 标准构建，允许您将数据发送到任何兼容的后端。

[OpenTelemetry]: https://opentelemetry.io/

## 启用遥测

您可以通过多种方式启用遥测。配置主要通过 [`.gemini/settings.json` 文件](./cli/configuration.md) 和环境变量管理，但 CLI 标志可以为特定会话覆盖这些设置。

### 优先级顺序

以下列出了应用遥测设置的优先级，列表中较高的项目具有更高的优先级：

1. **CLI 标志（针对 `gemini` 命令）：**
   - `--telemetry` / `--no-telemetry`：覆盖 `telemetry.enabled`。
   - `--telemetry-target <local|gcp>`：覆盖 `telemetry.target`。
   - `--telemetry-otlp-endpoint <URL>`：覆盖 `telemetry.otlpEndpoint`。
   - `--telemetry-log-prompts` / `--no-telemetry-log-prompts`：覆盖 `telemetry.logPrompts`。

2. **环境变量：**
   - `OTEL_EXPORTER_OTLP_ENDPOINT`：覆盖 `telemetry.otlpEndpoint`。

3. **工作空间设置文件（`.gemini/settings.json`）：** 此项目特定文件中 `telemetry` 对象的值。

4. **用户设置文件（`~/.gemini/settings.json`）：** 此全局用户文件中 `telemetry` 对象的值。

5. **默认值：** 如果上述任何设置都未设置，则应用默认值。
   - `telemetry.enabled`：`false`
   - `telemetry.target`：`local`
   - `telemetry.otlpEndpoint`：`http://localhost:4317`
   - `telemetry.logPrompts`：`true`

**对于 `npm run telemetry -- --target=<gcp|local>` 脚本：**
此脚本的 `--target` 参数 _仅_ 为该脚本的持续时间和目的覆盖 `telemetry.target`（即选择启动哪个收集器）。它不会永久更改您的 `settings.json`。脚本将首先查看 `settings.json` 中的 `telemetry.target` 作为其默认值。

### 示例设置

以下代码可以添加到您的工作空间（`.gemini/settings.json`）或用户（`~/.gemini/settings.json`）设置中，以启用遥测并将输出发送到 Google Cloud：

```json
{
  "telemetry": {
    "enabled": true,
    "target": "gcp"
  },
  "sandbox": false
}
```

## 运行 OTEL 收集器

OTEL 收集器是一个接收、处理和导出遥测数据的服务。
CLI 使用 OTLP/gRPC 协议发送数据。

在 [文档][otel-config-docs] 中了解有关 OTEL 导出器标准配置的更多信息。

[otel-config-docs]: https://opentelemetry.io/docs/languages/sdk-configuration/otlp-exporter/

### 本地

使用 `npm run telemetry -- --target=local` 命令来自动化设置本地遥测管道的过程，包括在您的 `.gemini/settings.json` 文件中配置必要的设置。底层脚本安装 `otelcol-contrib`（OpenTelemetry 收集器）和 `jaeger`（用于查看跟踪的 Jaeger UI）。要使用它：

1. **运行命令**：
   从存储库根目录执行命令：

   ```bash
   npm run telemetry -- --target=local
   ```

   脚本将：
   - 如果需要，下载 Jaeger 和 OTEL。
   - 启动本地 Jaeger 实例。
   - 启动配置为从 Gemini CLI 接收数据的 OTEL 收集器。
   - 在您的工作空间设置中自动启用遥测。
   - 退出时禁用遥测。

2. **查看跟踪**：
   打开您的 web 浏览器并导航到 **http://localhost:16686** 以访问 Jaeger UI。在这里您可以检查 Gemini CLI 操作的详细跟踪。

3. **检查日志和指标**：
   脚本将 OTEL 收集器输出（包括日志和指标）重定向到 `~/.gemini/tmp/<projectHash>/otel/collector.log`。脚本将提供查看链接和用于本地跟踪遥测数据（跟踪、指标、日志）的命令。

4. **停止服务**：
   在运行脚本的终端中按 `Ctrl+C` 以停止 OTEL 收集器和 Jaeger 服务。

### Google Cloud

使用 `npm run telemetry -- --target=gcp` 命令来自动化设置本地 OpenTelemetry 收集器的过程，该收集器将数据转发到您的 Google Cloud 项目，包括在您的 `.gemini/settings.json` 文件中配置必要的设置。底层脚本安装 `otelcol-contrib`。要使用它：

1. **先决条件**：
   - 拥有 Google Cloud 项目 ID。
   - 导出 `GOOGLE_CLOUD_PROJECT` 环境变量以使其对 OTEL 收集器可用。
     ```bash
     export OTLP_GOOGLE_CLOUD_PROJECT="your-project-id"
     ```
   - 使用 Google Cloud 进行身份验证（例如，运行 `gcloud auth application-default login` 或确保设置了 `GOOGLE_APPLICATION_CREDENTIALS`）。
   - 确保您的 Google Cloud 账户/服务账户具有必要的 IAM 角色："Cloud Trace Agent"、"Monitoring Metric Writer" 和 "Logs Writer"。

2. **运行命令**：
   从存储库根目录执行命令：

   ```bash
   npm run telemetry -- --target=gcp
   ```

   脚本将：
   - 如果需要，下载 `otelcol-contrib` 二进制文件。
   - 启动配置为从 Gemini CLI 接收数据并将其导出到指定 Google Cloud 项目的 OTEL 收集器。
   - 在您的工作空间设置（`.gemini/settings.json`）中自动启用遥测并禁用沙箱模式。
   - 提供直接链接以在 Google Cloud 控制台中查看跟踪、指标和日志。
   - 退出时（Ctrl+C），它将尝试恢复您的原始遥测和沙箱设置。

3. **运行 Gemini CLI：**
   在单独的终端中运行您的 Gemini CLI 命令。这会生成收集器捕获的遥测数据。

4. **在 Google Cloud 中查看遥测**：
   使用脚本提供的链接导航到 Google Cloud 控制台并查看您的跟踪、指标和日志。 