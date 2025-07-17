# 示例代理脚本

以下是一个可以与 `GEMINI_SANDBOX_PROXY_COMMAND` 环境变量一起使用的代理脚本示例。此脚本仅允许对 `example.com:443` 的 `HTTPS` 连接，并拒绝所有其他请求。

```javascript
#!/usr/bin/env node

/**
 * @license
 * Copyright 2025 Google LLC
 * SPDX-License-Identifier: Apache-2.0
 */

// 示例代理服务器，监听 :::8877 并仅允许对 example.com 的 HTTPS 连接。
// 设置 `GEMINI_SANDBOX_PROXY_COMMAND=scripts/example-proxy.js` 来与沙箱一起运行代理
// 在沙箱内通过 `curl https://example.com` 进行测试（在 shell 模式或通过 shell 工具）

import http from 'http';
import net from 'net';
import { URL } from 'url';
import console from 'console';

const PROXY_PORT = 8877;
const ALLOWED_DOMAINS = ['example.com', 'googleapis.com'];
const ALLOWED_PORT = '443';

const server = http.createServer((req, res) => {
  // 拒绝除 CONNECT 以外的所有 HTTPS 请求
  console.log(
    `[PROXY] 拒绝对以下的非 CONNECT 请求: ${req.method} ${req.url}`,
  );
  res.writeHead(405, { 'Content-Type': 'text/plain' });
  res.end('Method Not Allowed');
});

server.on('connect', (req, clientSocket, head) => {
  // req.url 对于 CONNECT 请求将采用 "hostname:port" 格式。
  const { port, hostname } = new URL(`http://${req.url}`);

  console.log(`[PROXY] 拦截对以下的 CONNECT 请求: ${hostname}:${port}`);

  if (
    ALLOWED_DOMAINS.some(
      (domain) => hostname == domain || hostname.endsWith(`.${domain}`),
    ) &&
    port === ALLOWED_PORT
  ) {
    console.log(`[PROXY] 允许连接到 ${hostname}:${port}`);

    // 建立到原始目标的 TCP 连接。
    const serverSocket = net.connect(port, hostname, () => {
      clientSocket.write('HTTP/1.1 200 Connection Established\r\n\r\n');
      // 通过在客户端和目标服务器之间传输数据来创建隧道。
      serverSocket.write(head);
      serverSocket.pipe(clientSocket);
      clientSocket.pipe(serverSocket);
    });

    serverSocket.on('error', (err) => {
      console.error(`[PROXY] 连接到目标时出错: ${err.message}`);
      clientSocket.end(`HTTP/1.1 502 Bad Gateway\r\n\r\n`);
    });
  } else {
    console.log(`[PROXY] 拒绝连接到 ${hostname}:${port}`);
    clientSocket.end('HTTP/1.1 403 Forbidden\r\n\r\n');
  }

  clientSocket.on('error', (err) => {
    // 如果客户端挂起，这可能会发生。
    console.error(`[PROXY] 客户端套接字错误: ${err.message}`);
  });
});

server.listen(PROXY_PORT, () => {
  const address = server.address();
  console.log(`[PROXY] 代理监听在 ${address.address}:${address.port}`);
  console.log(
    `[PROXY] 允许对以下域名的 HTTPS 连接: ${ALLOWED_DOMAINS.join(', ')}`,
  );
});
``` 