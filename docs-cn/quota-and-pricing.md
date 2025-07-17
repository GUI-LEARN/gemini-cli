# Gemini CLI：配额和定价

您的 Gemini CLI 配额和定价取决于您用于与 Google 进行身份验证的账户类型。此外，配额和定价可能根据模型版本、请求和使用的令牌数量以不同方式计算。模型使用情况摘要可通过 `/stats` 命令获取，并在会话结束时退出时显示。有关隐私政策和服务条款的详细信息，请参阅[隐私和条款](./tos-privacy.md)。注意：公布的价格是挂牌价格；可能适用额外的商业折扣。

本文概述了使用不同身份验证方法时适用于 Gemini CLI 的特定配额和定价。

## 1. 使用 Google 登录（Gemini Code Assist 免费层）

对于使用 Google 账户进行身份验证以访问面向个人的 Gemini Code Assist 的用户：

- **配额：**
  - 每分钟 60 个请求
  - 每天 1000 个请求
  - 令牌使用量不适用
- **成本：** 免费
- **详细信息：** [Gemini Code Assist 配额](https://developers.google.com/gemini-code-assist/resources/quotas#quotas-for-agent-mode-gemini-cli)
- **注意事项：** 未指定不同模型的特定配额；可能会发生模型回退以保持共享体验质量。

## 2. Gemini API 密钥（未付费）

如果您使用 Gemini API 密钥的免费层：

- **配额：**
  - 仅限 Flash 模型
  - 每分钟 10 个请求
  - 每天 250 个请求
- **成本：** 免费
- **详细信息：** [Gemini API 速率限制](https://ai.google.dev/gemini-api/docs/rate-limits)

## 3. Gemini API 密钥（付费）

如果您使用付费计划的 Gemini API 密钥：

- **配额：** 根据定价层而异。
- **成本：** 根据定价层和模型/令牌使用量而异。
- **详细信息：** [Gemini API 速率限制](https://ai.google.dev/gemini-api/docs/rate-limits), [Gemini API 定价](https://ai.google.dev/gemini-api/docs/pricing)

## 4. 使用 Google 登录（适用于 Workspace 或许可的 Code Assist 用户）

对于 Gemini Code Assist 标准版或企业版的用户，配额和定价基于分配许可证席位的固定价格订阅：

- **标准层：**
  - **配额：** 每分钟 120 个请求，每天 1500 个
- **企业层：**
  - **配额：** 每分钟 120 个请求，每天 2000 个
- **成本：** 包含在您的 Gemini for Google Workspace 或 Gemini Code Assist 订阅中的固定价格。
- **详细信息：** [Gemini Code Assist 配额](https://developers.google.com/gemini-code-assist/resources/quotas#quotas-for-agent-mode-gemini-cli), [Gemini Code Assist 定价](https://cloud.google.com/products/gemini/pricing)
- **注意事项：**
  - 未指定不同模型的特定配额；可能会发生模型回退以保持共享体验质量。
  - Google 开发者计划的成员可能通过其成员资格拥有 Gemini Code Assist 许可证。

## 5. Vertex AI（快速模式）

如果您使用快速模式的 Vertex AI：

- **配额：** 配额是可变的，特定于您的账户。有关更多详细信息，请参阅来源。
- **成本：** 在您的快速模式使用量被消耗并且您为项目启用计费后，成本基于标准的 [Vertex AI 定价](https://cloud.google.com/vertex-ai/pricing)。
- **详细信息：** [Vertex AI 快速模式配额](https://cloud.google.com/vertex-ai/generative-ai/docs/start/express-mode/overview#quotas)

## 6. Vertex AI（常规模式）

如果您使用标准的 Vertex AI 服务：

- **配额：** 受动态共享配额系统或预购买的预置吞吐量管制。
- **成本：** 基于模型和令牌使用量。请参阅 [Vertex AI 定价](https://cloud.google.com/vertex-ai/pricing)。
- **详细信息：** [Vertex AI 动态共享配额](https://cloud.google.com/vertex-ai/generative-ai/docs/resources/dynamic-shared-quota)

## 7. Google One 和 Ultra 计划，Gemini for Workspace 计划

这些计划目前仅适用于使用 Google 基于体验提供的基于 web 的 Gemini 产品（例如，Gemini web 应用程序或 Flow 视频编辑器）。这些计划不适用于驱动 Gemini CLI 的 API 使用。支持这些计划正在积极考虑中，以便未来支持。 