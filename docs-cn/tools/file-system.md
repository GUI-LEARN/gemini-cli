# Gemini CLI 文件系统工具

Gemini CLI 提供了一套用于与本地文件系统交互的综合工具。这些工具允许 Gemini 模型读取、写入、列出、搜索和修改文件和目录，都在您的控制下，通常对敏感操作需要确认。

**注意：** 所有文件系统工具出于安全考虑都在 `rootDirectory`（通常是您启动 CLI 的当前工作目录）内操作。您提供给这些工具的路径通常应该是绝对路径或相对于此根目录解析。

## 1. `list_directory`（ReadFolder）

`list_directory` 列出指定目录路径内的文件和子目录名称。它可以选择性地忽略与提供的 glob 模式匹配的条目。

- **工具名称：** `list_directory`
- **显示名称：** ReadFolder
- **文件：** `ls.ts`
- **参数：**
  - `path`（字符串，必需）：要列出的目录的绝对路径。
  - `ignore`（字符串数组，可选）：要从列表中排除的 glob 模式列表（例如，`["*.log", ".git"]`）。
  - `respect_git_ignore`（布尔值，可选）：列出文件时是否遵循 `.gitignore` 模式。默认为 `true`。
- **行为：**
  - 返回文件和目录名称列表。
  - 指示每个条目是否为目录。
  - 对条目进行排序，目录在前，然后按字母顺序。
- **输出（`llmContent`）：** 类似于：`Directory listing for /path/to/your/folder:\n[DIR] subfolder1\nfile1.txt\nfile2.png`
- **确认：** 无。

## 2. `read_file`（ReadFile）

`read_file` 读取并返回指定文件的内容。此工具处理文本、图像（PNG、JPG、GIF、WEBP、SVG、BMP）和 PDF 文件。对于文本文件，它可以读取特定的行范围。其他二进制文件类型通常被跳过。

- **工具名称：** `read_file`
- **显示名称：** ReadFile
- **文件：** `read-file.ts`
- **参数：**
  - `path`（字符串，必需）：要读取的文件的绝对路径。
  - `offset`（数字，可选）：对于文本文件，开始读取的基于 0 的行号。需要设置 `limit`。
  - `limit`（数字，可选）：对于文本文件，读取的最大行数。如果省略，读取默认最大值（例如，2000 行）或整个文件（如果可行）。
- **行为：**
  - 对于文本文件：返回内容。如果使用 `offset` 和 `limit`，则只返回那些行的切片。如果由于行限制或行长度限制而截断内容，则进行指示。
  - 对于图像和 PDF 文件：返回文件内容作为适合模型消费的 base64 编码数据结构。
  - 对于其他二进制文件：尝试识别并跳过它们，返回指示它是通用二进制文件的消息。
- **输出：**（`llmContent`）：
  - 对于文本文件：文件内容，可能以截断消息为前缀（例如，`[File content truncated: showing lines 1-100 of 500 total lines...]\nActual file content...`）。
  - 对于图像/PDF 文件：包含 `inlineData` 的对象，其中包含 `mimeType` 和 base64 `data`（例如，`{ inlineData: { mimeType: 'image/png', data: 'base64encodedstring' } }`）。
  - 对于其他二进制文件：类似于 `Cannot display content of binary file: /path/to/data.bin` 的消息。
- **确认：** 无。

## 3. `write_file`（WriteFile）

`write_file` 将内容写入指定文件。如果文件存在，它将被覆盖。如果文件不存在，它（以及任何必要的父目录）将被创建。

- **工具名称：** `write_file` 