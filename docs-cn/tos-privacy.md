# Gemini CLI：服务条款和隐私声明

Gemini CLI 是一个开源工具，让您可以直接从命令行界面与 Google 的强大语言模型交互。适用于您使用 Gemini CLI 的服务条款和隐私声明取决于您用于与 Google 进行身份验证的账户类型。

本文概述了适用于不同账户类型和身份验证方法的具体条款和隐私政策。注意：有关适用于您使用 Gemini CLI 的配额和定价详情，请参阅 [配额和定价](./quota-and-pricing.md)。

## 如何确定您的身份验证方法

您的身份验证方法指的是您用于登录和访问 Gemini CLI 的方法。有四种身份验证方式：

- 使用您的 Google 账户登录到面向个人的 Gemini Code Assist
- 使用您的 Google 账户登录到面向 Workspace、标准版或企业版用户的 Gemini Code Assist
- 使用 Gemini Developer 的 API 密钥
- 使用 Vertex AI GenAI API 的 API 密钥

对于这四种身份验证方法中的每一种，可能适用不同的服务条款和隐私声明。

| 身份验证                        | 账户                    | 服务条款                                                                                                      | 隐私声明                                                                                                                                                                                                     |
| :---------------------------- | :------------------ | :------------------------------------------------------------------------------------------------------ | :----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| 通过 Google 的 Gemini Code Assist | 个人          | [Google 服务条款](https://policies.google.com/terms?hl=zh-CN)                                   | [面向个人的 Gemini Code Assist 隐私声明](https://developers.google.com/gemini-code-assist/resources/privacy-notice-gemini-code-assist-individuals)                                    |
| 通过 Google 的 Gemini Code Assist | 标准版/企业版 | [Google Cloud Platform 服务条款](https://cloud.google.com/terms)                                | [面向标准版和企业版的 Gemini Code Assist 隐私声明](https://cloud.google.com/gemini/docs/codeassist/security-privacy-compliance#standard_and_enterprise_data_protection_and_privacy) |
| Gemini Developer API          | 未付费              | [Gemini API 服务条款 - 未付费服务](https://ai.google.dev/gemini-api/terms#unpaid-services) | [Google 隐私政策](https://policies.google.com/privacy)                                                                                                                                     |
| Gemini Developer API          | 付费                | [Gemini API 服务条款 - 付费服务](https://ai.google.dev/gemini-api/terms#paid-services)     | [Google 隐私政策](https://policies.google.com/privacy)                                                                                                                                     |
| Vertex AI Gen API             |                     | [Google Cloud Platform 服务条款](https://cloud.google.com/terms/service-terms/)                    | [Google Cloud 隐私声明](https://cloud.google.com/terms/cloud-privacy-notice)                                                                                                               |

## 1. 如果您使用 Google 账户登录到面向个人的 Gemini Code Assist

对于使用 Google 账户访问 [面向个人的 Gemini Code Assist](https://developers.google.com/gemini-code-assist/docs/overview#supported-features-gca) 的用户，适用以下服务条款和隐私声明文档：

- **服务条款：** 您对 Gemini CLI 的使用受 [Google 服务条款](https://policies.google.com/terms?hl=zh-CN) 约束。
- **隐私声明：** 您的数据收集和使用在 [面向个人的 Gemini Code Assist 隐私声明](https://developers.google.com/gemini-code-assist/resources/privacy-notice-gemini-code-assist-individuals) 中描述。

## 2. 如果您使用 Google 账户登录到面向 Workspace、标准版或企业版用户的 Gemini Code Assist

对于使用 Google 账户访问 Gemini Code Assist [标准版或企业版](https://cloud.google.com/gemini/docs/codeassist/overview#editions-overview) 的用户，适用以下服务条款和隐私声明文档：

- **服务条款：** 您对 Gemini CLI 的使用受 [Google Cloud Platform 服务条款](https://cloud.google.com/terms) 约束。
- **隐私声明：** 您的数据收集和使用在 [面向标准版和企业版用户的 Gemini Code Assist 隐私声明](https://cloud.google.com/gemini/docs/codeassist/security-privacy-compliance#standard_and_enterprise_data_protection_and_privacy) 中描述。

## 3. 如果您使用 Gemini API 密钥登录到 Gemini Developer API

如果您使用 Gemini API 密钥通过 [Gemini Developer API](https://ai.google.dev/gemini-api/docs) 进行身份验证，适用以下服务条款和隐私声明文档：

- **服务条款：** 您对 Gemini CLI 的使用受 [Gemini API 服务条款](https://ai.google.dev/gemini-api/terms) 约束。这些条款可能因您使用的是未付费或付费服务而有所不同：
  - 对于未付费服务，请参阅 [Gemini API 服务条款 - 未付费服务](https://ai.google.dev/gemini-api/terms#unpaid-services)。
  - 对于付费服务，请参阅 [Gemini API 服务条款 - 付费服务](https://ai.google.dev/gemini-api/terms#paid-services)。
- **隐私声明：** 您的数据收集和使用在 [Google 隐私政策](https://policies.google.com/privacy) 中描述。

## 4. 如果您使用 Gemini API 密钥登录到 Vertex AI GenAI API

如果您使用 Gemini API 密钥通过 [Vertex AI GenAI API](https://cloud.google.com/vertex-ai/generative-ai/docs/reference/rest) 后端进行身份验证，适用以下服务条款和隐私声明文档：

- **服务条款：** 您对 Gemini CLI 的使用受 [Google Cloud Platform 服务条款](https://cloud.google.com/terms/service-terms/) 约束。
- **隐私声明：** 您的数据收集和使用在 [Google Cloud 隐私声明](https://cloud.google.com/terms/cloud-privacy-notice) 中描述。

### 使用统计选择退出

您可以按照此处提供的说明选择退出向 Google 发送使用统计信息：[使用统计配置](./cli/configuration.md#usage-statistics)。

## Gemini CLI 常见问题（FAQ）

### 1. 我的代码（包括提示和答案）是否用于训练 Google 的模型？

您的代码（包括提示和答案）是否用于训练 Google 的模型取决于您使用的身份验证方法类型和您的账户类型。

- **使用面向个人的 Gemini Code Assist 的 Google 账户**：是的。当您使用个人 Google 账户时，适用 [面向个人的 Gemini Code Assist 隐私声明](https://developers.google.com/gemini-code-assist/resources/privacy-notice-gemini-code-assist-individuals)。在此声明下，您的**提示、答案和相关代码会被收集**，并可能用于改进 Google 的产品，包括用于模型训练。
- **使用面向 Workspace、标准版或企业版的 Gemini Code Assist 的 Google 账户**：不。对于这些账户，您的数据受 [Gemini Code Assist 隐私声明](https://cloud.google.com/gemini/docs/codeassist/security-privacy-compliance#standard_and_enterprise_data_protection_and_privacy) 条款约束，这些条款将您的输入视为机密。您的**提示、答案和相关代码不会被收集**，也不会用于训练模型。
- **通过 Gemini Developer API 的 Gemini API 密钥**：您的代码是否被收集或使用取决于您使用的是未付费还是付费服务。
  - **未付费服务**：是的。当您通过 Gemini Developer API 使用 Gemini API 密钥的未付费服务时，适用 [Gemini API 服务条款 - 未付费服务](https://ai.google.dev/gemini-api/terms#unpaid-services) 条款。在此声明下，您的**提示、答案和相关代码会被收集**，并可能用于改进 Google 的产品，包括用于模型训练。
  - **付费服务**：不。当您通过 Gemini Developer API 使用 Gemini API 密钥的付费服务时，适用 [Gemini API 服务条款 - 付费服务](https://ai.google.dev/gemini-api/terms#paid-services) 条款，这些条款将您的输入视为机密。您的**提示、答案和相关代码不会被收集**，也不会用于训练模型。
- **通过 Vertex AI GenAI API 的 Gemini API 密钥**：不。对于这些账户，您的数据受 [Google Cloud 隐私声明](https://cloud.google.com/terms/cloud-privacy-notice) 条款约束，这些条款将您的输入视为机密。您的**提示、答案和相关代码不会被收集**，也不会用于训练模型。

### 2. 什么是使用统计，选择退出控制什么？

**使用统计** 设置是 Gemini CLI 中所有可选数据收集的单一控制。

它收集的数据取决于您的账户和身份验证类型：

- **使用面向个人的 Gemini Code Assist 的 Google 账户**：启用时，此设置允许 Google 收集匿名遥测（例如，运行的命令和性能指标）和**您的提示和答案**以改进模型。
- **使用面向 Workspace、标准版或企业版的 Gemini Code Assist 的 Google 账户**：此设置仅控制匿名遥测的收集。无论此设置如何，您的提示和答案都不会被收集。
- **通过 Gemini Developer API 的 Gemini API 密钥**：
  **未付费服务**：启用时，此设置允许 Google 收集匿名遥测（如运行的命令和性能指标）和**您的提示和答案**以改进模型。禁用时，我们将按照 [Google 如何使用您的数据](https://ai.google.dev/gemini-api/terms#data-use-unpaid) 中描述的方式使用您的数据。
  **付费服务**：此设置仅控制匿名遥测的收集。Google 会在有限的时间内记录提示和响应，仅用于检测违反禁止使用政策的行为以及任何必需的法律或监管披露。
- **通过 Vertex AI GenAI API 的 Gemini API 密钥**：此设置仅控制匿名遥测的收集。无论此设置如何，您的提示和答案都不会被收集。

您可以按照 [使用统计配置](./cli/configuration.md#usage-statistics) 文档中的说明为任何账户类型禁用使用统计。 