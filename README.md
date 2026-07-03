# LLM API Mock 文档站

Mintlify 文档站 + Prism Mock Server，用于 Mintlify 框架调研与 Playground 练手。

## 目录结构

```
site/
├── docs.json              # Mintlify 站点配置
├── introduction.mdx       # API 介绍
├── quickstart.mdx         # 快速开始
├── pricing.mdx            # Mock 定价
├── webhooks.mdx           # Webhook 占位
├── mcp-server.mdx         # MCP Server 占位
├── agent-skills.mdx       # Agent Skills 占位
├── openapi/
│   └── openapi.yaml       # OpenAPI 3.1 spec
├── images/logo/           # Logo 资源
├── package.json
└── .mintignore
```

## 前置条件

- Node.js 20.17+
- pnpm

Mintlify CLI 已作为项目 devDependency 安装，无需全局安装。若希望全局使用，可执行 `pnpm add -g mint`。

## 启动方式（双终端）

### 终端 1：Mock API Server

```bash
cd site
pnpm install
pnpm mock:api
```

Mock Server 监听 `http://127.0.0.1:4010`，基于 OpenAPI spec 返回预设响应。

### 终端 2：文档预览

```bash
cd site
pnpm dev:docs
```

浏览器访问 `http://localhost:3000`。

## Playground 用法

1. 确保 Mock Server 已启动（终端 1）
2. 在文档站切换到 **API** Tab，选择任意 endpoint
3. 在 Playground 的 Authorization 字段填入：`Bearer sk-mock-test-key`
4. 点击 **Send** 发送请求

Playground 已设置 `proxy: false`，浏览器直连 `http://127.0.0.1:4010`（Prism 默认开启 CORS）。

## 命令行验证

```bash
curl http://127.0.0.1:4010/v1/models \
  -H "Authorization: Bearer sk-mock-test-key"
```

## 注意事项

### 本地 vs 线上

- OpenAPI spec 中 `servers.url` 为 `http://localhost:4010`，仅适用于本地开发
- 部署到 Mintlify 线上后，Playground 默认仍指向 `localhost:4010`，**线上用户无法直接测试**
- 如需在线 Playground，需：
  - 部署可公网访问的 Mock/沙箱 API，并修改 `openapi/openapi.yaml` 中的 `servers.url`
  - 或设置 `api.playground.proxy: false` 并确保后端配置 CORS

### Mock 数据

- 所有响应均为 OpenAPI examples 中的固定数据
- 认证接受任意 `Bearer sk-mock-*` 格式的 Key
- Webhooks、MCP Server、Agent Skills 页面为占位文档，无真实实现

### 校验命令

```powershell
cd site
pnpm validate
pnpm broken-links
```

## 相关文档

- 调研文档：仓库根目录 `docs/`
- Mintlify 官方文档：https://www.mintlify.com/docs
