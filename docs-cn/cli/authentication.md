# 身份验证设置

Gemini CLI 要求您使用 Google 的 AI 服务进行身份验证。在初始启动时，您需要配置以下身份验证方法中的**一种**：

1. **使用 Google 登录（Gemini Code Assist）：**
   - 使用此选项登录您的 Google 账户。
   - 在初始启动期间，Gemini CLI 会引导您到一个网页进行身份验证。一旦通过身份验证，您的凭据将在本地缓存，以便在后续运行中可以跳过 web 登录。
   - 请注意，web 登录必须在可以与运行 Gemini CLI 的机器通信的浏览器中完成。（具体来说，浏览器将被重定向到 Gemini CLI 将监听的 localhost URL）。
   - <a id="workspace-gca">用户可能需要指定 GOOGLE_CLOUD_PROJECT 如果：</a>
     1. 您有 Google Workspace 账户。Google Workspace 是为企业和组织提供的付费服务，提供一套生产力工具，包括自定义电子邮件域（例如 your-name@your-company.com）、增强的安全功能和管理控制。这些账户通常由雇主或学校管理。
     2. 您通过 [Google 开发者计划](https://developers.google.com/program/plans-and-pricing) 获得了免费的 Code Assist 许可证（包括合格的 Google 开发者专家）
     3. 您已被分配到当前 Gemini Code Assist 标准版或企业版订阅的许可证。
     4. 您在免费个人使用的 [支持区域](https://developers.google.com/gemini-code-assist/resources/available-locations) 之外使用产品。
     5. 您是 18 岁以下的 Google 账户持有者
     - 如果您属于这些类别之一，您必须首先配置要使用的 Google Cloud Project Id，[启用 Gemini for Cloud API](https://cloud.google.com/gemini/docs/discover/set-up-gemini#enable-api) 和 [配置访问权限](https://cloud.google.com/gemini/docs/discover/set-up-gemini#grant-iam)。

     您可以使用以下命令在当前 shell 会话中临时设置环境变量：

     ```bash
     export GOOGLE_CLOUD_PROJECT="YOUR_PROJECT_ID"
     ```
     - 对于重复使用，您可以将环境变量添加到您的 [.env 文件](#使用-env-文件持久化环境变量) 或 shell 配置文件（如 `~/.bashrc`、`~/.zshrc` 或 `~/.profile`）。例如，以下命令将环境变量添加到 `~/.bashrc` 文件：

     ```bash
     echo 'export GOOGLE_CLOUD_PROJECT="YOUR_PROJECT_ID"' >> ~/.bashrc
     source ~/.bashrc
     ```

2. **<a id="gemini-api-key"></a>Gemini API 密钥：**
   - 从 Google AI Studio 获取您的 API 密钥：[https://aistudio.google.com/app/apikey](https://aistudio.google.com/app/apikey)
   - 设置 `GEMINI_API_KEY` 环境变量。在以下方法中，将 `YOUR_GEMINI_API_KEY` 替换为您从 Google AI Studio 获得的 API 密钥：
     - 您可以使用以下命令在当前 shell 会话中临时设置环境变量：
       ```bash
       export GEMINI_API_KEY="YOUR_GEMINI_API_KEY"
       ```
     - 对于重复使用，您可以将环境变量添加到您的 [.env 文件](#使用-env-文件持久化环境变量) 或 shell 配置文件（如 `~/.bashrc`、`~/.zshrc` 或 `~/.profile`）。例如，以下命令将环境变量添加到 `~/.bashrc` 文件：
       ```bash
       echo 'export GEMINI_API_KEY="YOUR_GEMINI_API_KEY"' >> ~/.bashrc
       source ~/.bashrc
       ```

3. **Vertex AI：**
   - 获取您的 Google Cloud API 密钥：[获取 API 密钥](https://cloud.google.com/vertex-ai/generative-ai/docs/start/api-keys?usertype=newuser)
     - 设置 `GOOGLE_API_KEY` 环境变量。在以下方法中，将 `YOUR_GOOGLE_API_KEY` 替换为您的 Vertex AI API 密钥：
       - 您可以使用以下命令在当前 shell 会话中临时设置这些环境变量：
         ```bash
         export GOOGLE_API_KEY="YOUR_GOOGLE_API_KEY"
         ```
       - 对于重复使用，您可以将环境变量添加到您的 [.env 文件](#使用-env-文件持久化环境变量) 或 shell 配置文件（如 `~/.bashrc`、`~/.zshrc` 或 `~/.profile`）。例如，以下命令将环境变量添加到 `~/.bashrc` 文件：
         ```bash
         echo 'export GOOGLE_API_KEY="YOUR_GOOGLE_API_KEY"' >> ~/.bashrc
         source ~/.bashrc
         ```
   - 要使用应用程序默认凭据（ADC），请使用以下命令：
     - 确保您有 Google Cloud 项目并已启用 Vertex AI API。
       ```bash
       gcloud auth application-default login
       ```
       有关更多信息，请参阅 [为 Google Cloud 设置应用程序默认凭据](https://cloud.google.com/docs/authentication/provide-credentials-adc)。
     - 设置 `GOOGLE_CLOUD_PROJECT` 和 `GOOGLE_CLOUD_LOCATION` 环境变量。在以下方法中，将 `YOUR_PROJECT_ID` 和 `YOUR_PROJECT_LOCATION` 替换为您项目的相关值：
       - 您可以使用以下命令在当前 shell 会话中临时设置这些环境变量：
         ```bash
         export GOOGLE_CLOUD_PROJECT="YOUR_PROJECT_ID"
         export GOOGLE_CLOUD_LOCATION="YOUR_PROJECT_LOCATION" # 例如，us-central1
         ```
       - 对于重复使用，您可以将环境变量添加到您的 [.env 文件](#使用-env-文件持久化环境变量) 或 shell 配置文件（如 `~/.bashrc`、`~/.zshrc` 或 `~/.profile`）。例如，以下命令将环境变量添加到 `~/.bashrc` 文件：
         ```bash
         echo 'export GOOGLE_CLOUD_PROJECT="YOUR_PROJECT_ID"' >> ~/.bashrc
         echo 'export GOOGLE_CLOUD_LOCATION="YOUR_PROJECT_LOCATION"' >> ~/.bashrc
         source ~/.bashrc
         ```

4. **Cloud Shell：**
   - 此选项仅在 Google Cloud Shell 环境中运行时可用。
   - 它自动使用 Cloud Shell 环境中登录用户的凭据。
   - 当在 Cloud Shell 中运行且没有配置其他方法时，这是默认的身份验证方法。

### 使用 `.env` 文件持久化环境变量

您可以在项目目录或主目录中创建 **`.gemini/.env`** 文件。创建普通的 **`.env`** 文件也可以，但建议使用 `.gemini/.env` 以保持 Gemini 变量与其他工具隔离。

Gemini CLI 会自动从找到的**第一个** `.env` 文件加载环境变量，使用以下搜索顺序：

1. 从**当前目录**开始向上移动到 `/`，对于每个目录检查：
   1. `.gemini/.env`
   2. `.env`
2. 如果没有找到文件，则回退到您的**主目录**：
   - `~/.gemini/.env`
   - `~/.env`

> **重要：** 搜索在遇到**第一个**文件时停止——变量**不会**在多个文件之间合并。

#### 示例

**项目特定的覆盖**（当您在项目内时优先）：

```bash
mkdir -p .gemini
echo 'GOOGLE_CLOUD_PROJECT="your-project-id"' >> .gemini/.env
```

**用户范围的设置**（在每个目录中都可用）：

```bash
mkdir -p ~/.gemini
cat >> ~/.gemini/.env <<'EOF'
GOOGLE_CLOUD_PROJECT="your-project-id"
GEMINI_API_KEY="your-gemini-api-key"
EOF
``` 