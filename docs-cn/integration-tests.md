# 集成测试

本文档提供了关于本项目中使用的集成测试框架的信息。

## 概述

集成测试旨在验证 Gemini CLI 的端到端功能。它们在受控环境中执行构建的二进制文件，并验证其在与文件系统交互时是否按预期运行。

这些测试位于 `integration-tests` 目录中，使用自定义测试运行器运行。

## 运行测试

集成测试不作为默认 `npm run test` 命令的一部分运行。必须使用 `npm run test:integration:all` 脚本显式运行它们。

集成测试也可以使用以下快捷方式运行：

```bash
npm run test:e2e
```

## 运行特定测试集

要运行一个测试文件子集，您可以使用 `npm run <integration test command> <file_name1> ....`，其中 `<integration test command>` 是 `test:e2e` 或 `test:integration*` 之一，`<file_name>` 是 `integration-tests/` 目录中的任何 `.test.js` 文件。例如，以下命令运行 `list_directory.test.js` 和 `write_file.test.js`：

```bash
npm run test:e2e list_directory write_file
```

### 按名称运行单个测试

要按名称运行单个测试，使用 `--test-name-pattern` 标志：

```bash
npm run test:e2e -- --test-name-pattern "reads a file"
```

### 运行所有测试

要运行整个集成测试套件，使用以下命令：

```bash
npm run test:integration:all
```

### 沙箱矩阵

`all` 命令将为 `no sandboxing`、`docker` 和 `podman` 运行测试。
每个单独的类型可以使用以下命令运行：

```bash
npm run test:integration:sandbox:none
```

```bash
npm run test:integration:sandbox:docker
```

```bash
npm run test:integration:sandbox:podman
```

## 诊断

集成测试运行器提供了几个诊断选项来帮助跟踪测试失败。

### 保留测试输出

您可以保留测试运行期间创建的临时文件以供检查。这对于调试文件系统操作问题很有用。

要保留测试输出，您可以使用 `--keep-output` 标志或将 `KEEP_OUTPUT` 环境变量设置为 `true`。

```bash
# 使用标志
npm run test:integration:sandbox:none -- --keep-output

# 使用环境变量
KEEP_OUTPUT=true npm run test:integration:sandbox:none
```

当输出被保留时，测试运行器将打印测试运行的唯一目录路径。

### 详细输出

对于更详细的调试，`--verbose` 标志将 `gemini` 命令的实时输出流式传输到控制台。

```bash
npm run test:integration:sandbox:none -- --verbose
```

当在同一命令中使用 `--verbose` 和 `--keep-output` 时，输出被流式传输到控制台，并且也保存到测试临时目录中的日志文件中。

详细输出被格式化以清楚地识别日志来源：

```
--- TEST: <file-name-without-js>:<test-name> ---
... output from the gemini command ...
--- END TEST: <file-name-without-js>:<test-name> ---
```

## 代码检查和格式化

为了确保代码质量和一致性，集成测试文件作为主要构建过程的一部分进行检查。您也可以手动运行代码检查器和自动修复器。

### 运行代码检查器

要检查代码检查错误，运行以下命令：

```bash
npm run lint
```

您可以在命令中包含 `--fix` 标志来自动修复任何可修复的检查错误：

```bash
npm run lint --fix
```

## 目录结构

集成测试在 `.integration-tests` 目录内为每个测试运行创建唯一的目录。在这个目录中，为每个测试文件创建一个子目录，在其中，为每个单独的测试用例创建一个子目录。

这种结构使得很容易找到特定测试运行、文件或用例的工件。

```
.integration-tests/
└── <run-id>/
    └── <test-file-name>.test.js/
        └── <test-case-name>/
            ├── output.log
            └── ...other test artifacts...
```

## 持续集成

为了确保集成测试始终运行，在 `.github/workflows/e2e.yml` 中定义了 GitHub Actions 工作流。此工作流自动在每个拉取请求和推送到 `main` 分支时运行集成测试。

工作流在不同的沙箱环境中运行测试，以确保 Gemini CLI 在每个环境中都得到测试：

- `sandbox:none`：在没有任何沙箱的情况下运行测试。
- `sandbox:docker`：在 Docker 容器中运行测试。
- `sandbox:podman`：在 Podman 容器中运行测试。 