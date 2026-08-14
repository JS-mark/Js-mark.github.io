---
title: 别再为 AI Agent 重写一遍你的系统：我为什么做 Bridgent AI
description: "Bridgent AI 可以复用 OpenAPI、Prisma、Drizzle、tRPC 与 Zod 定义，生成 Claude Code、Cursor、Codex 等客户端可调用的 MCP Tools。"
type: categories
author: Mark
comments: true
external_link:
  enable: true
date: 2026-08-14 19:38:51
tags:
  - MCP
  - AI Agent
  - TypeScript
  - Node.js
  - Open Source
categories:

---

# 别再为 AI Agent 重写一遍你的系统：我为什么做 Bridgent AI

![Bridgent AI：复用现有 Schema 生成 MCP Tools](bridgent-blog-hero.png)

AI coding agent 正从“帮我补几行代码”，逐渐变成能够完成真实工程任务的协作者。

但只要把它放进实际项目，很快就会遇到一道墙：它可以理解代码，却看不见团队内部的 API、数据库和业务函数。要让 Claude Code、Cursor、Codex 或 Gemini CLI 调用这些能力，通常还得手写一套 MCP Server。
<!-- more -->
问题是，我们的系统明明已经被描述过一次了。

OpenAPI 描述了 HTTP API，Prisma 和 Drizzle 描述了数据模型，tRPC router 描述了应用过程，Zod 描述了函数输入。为了 MCP 再维护一份 tool schema，不仅重复劳动，也很容易出现两边定义逐渐漂移的问题。

这就是我开始做 **Bridgent AI** 的原因：

> 不重新发明一套描述，让项目里已经存在的 schema 直接成为 MCP Tools。

项目地址：[github.com/JS-mark/Bridgent](https://github.com/JS-mark/Bridgent)

使用文档：[js-mark.com/Bridgent](https://js-mark.com/Bridgent/)

## MCP 接入真正麻烦的，不只是写一个工具

一个最简单的 MCP demo 并不复杂。但当它进入真实项目，事情很快会变多：

- 为每个能力重新定义名称、说明和输入 schema；
- 把 OpenAPI、ORM model 或业务函数转换成工具；
- 选择 stdio、HTTP 或 Web runtime；
- 处理鉴权上下文、异常、超时和结果序列化；
- 限制数据库查询范围，避免大结果塞满模型上下文；
- 防止 raw SQL 或未审计的写操作被意外暴露。

单独看，每一项都不算难。问题在于，每个项目都需要再做一遍。

Bridgent AI 想减少的正是这层重复工程。它不是另一个 Agent framework，不管理 prompt、memory、planning，也不绑定某一种模型。它只专注于一件事：**把现有系统能力转换为边界清晰、可以被 MCP host 调用的工具。**

## Bridgent AI 的工作方式

![Bridgent AI 技术架构图](bridgent-architecture.png)

*图：已有 Source 经过 Bridgent 的 adapter 与统一 Tool Runtime，运行在不同 transport 上，最终供多个 MCP host 调用。*

这套架构分为四层：

### 1. 复用现有 Source

当前 Alpha 已支持：

- **OpenAPI 3.x**：将 operation 转换为 MCP tool；
- **Prisma 6.x**：从 model 生成查询工具，并支持显式开启的审计写工具；
- **Drizzle**：从 tables 生成只读查询工具；
- **tRPC v10/v11**：query procedure 默认成为读工具，mutation 默认隐藏；
- **Zod + TypeScript function**：用于定义项目自己的 typed tool。

这些 adapter 的目标不是把所有差异抹掉，而是保留原有定义的类型信息，并在不支持的输入形态上明确失败，而不是悄悄退化成宽松的 `any`。

### 2. 统一为 Bridgent Tools

不同 source 最终统一为包含 `description`、`inputSchema` 与 `run` 的工具结构，并可携带 source、读写能力、审计和限制等 metadata。

CLI 可以利用这些 metadata，在启动 MCP Inspector 前给出 source 与 capability hints。开发者因此能更早发现“生成工具过多”或“开启了写能力”之类的风险信号。

### 3. 选择合适的 Transport

同一套 tools 可以运行在：

- **stdio**：适合本地 Claude Code、Cursor、Codex、Gemini CLI；
- **Streamable HTTP**：适合自托管的共享 MCP Server；
- **Web Standard fetch handler**：适合 Cloudflare Workers、Deno、Bun 等 fetch-compatible runtime。

Bridgent AI 使用显式 server-file 模型。依赖注入、业务鉴权上下文和最终暴露范围仍掌握在开发者手中，而不是被隐藏在一个远端控制台里。

### 4. 交给任何兼容 MCP 的 Host

项目为 Claude Code、Cursor、OpenAI Codex CLI 和 Gemini CLI 提供了可直接复制的配置文档。仓库还使用官方 MCP SDK Client 对 stdio、HTTP 与 Web handler 做协议级测试。

## 五分钟跑起第一个 Server

环境需要 Node.js `>= 22.18`。先安装 CLI、core 与 Zod：

```bash
pnpm add -D @bridgent/cli @bridgent/core zod
bridgent init ./server.ts
```

也可以直接编写一个最小的 `server.ts`：

```ts
import { createStdioServer, defineTool } from '@bridgent/core'
import { z } from 'zod'

await createStdioServer({
  name: 'hello',
  version: '0.0.1',
  tools: [
    defineTool({
      name: 'add',
      description: 'Add two numbers',
      inputSchema: z.object({
        a: z.number(),
        b: z.number(),
      }),
      run: ({ a, b }) => a + b,
    }),
  ],
})
```

然后启动开发模式：

```bash
bridgent dev ./server.ts
```

如果项目已经有 tRPC router，也可以直接复用：

```ts
import { createStdioServer } from '@bridgent/core'
import { fromTrpc } from '@bridgent/source-trpc'
import { appRouter } from './router'

await createStdioServer({
  name: 'app',
  version: '0.0.1',
  tools: fromTrpc({
    router: appRouter,
    createContext: () => ({ userId: 'demo' }),
  }),
})
```

tRPC query 会生成读工具；mutation 和 subscription 不会默认暴露。要开放 mutation，必须同时显式开启写能力，并将最终工具名加入 allowlist。

## 数据库接入，安全默认比功能数量更重要

![Bridgent AI 数据库安全机制示意图](bridgent-safety.png)

*图：Agent 的数据库请求依次经过只读边界、结果限制、提交确认和审计记录。*

“能让 Agent 查数据库”很适合做演示，但真正决定它能否进入项目的，是默认边界。

Bridgent AI 当前采用以下策略：

1. **默认只读**：Prisma 与 Drizzle 不会因为接入 adapter 就自动获得写权限；
2. **查询受限**：结果行数有上限，并提供 query timeout；
3. **不暴露 raw SQL**：避免工具绕过模型和字段边界；
4. **写操作显式开放**：Prisma writes 需要开启 mutating、配置工具 allowlist 与 audit；
5. **两步提交**：先 dry-run 获取一次性 preview token，再使用相同参数 commit；
6. **降低重复写入风险**：支持 audit sink 与同进程幂等键，应对 MCP host 的重试行为。

这些机制不会替代业务系统自己的认证和授权。`createContext`、用户身份、租户隔离等仍应由宿主应用负责。Bridgent AI 提供的是工具层的安全默认值，而不是绕过业务边界的万能入口。

## 为什么我没有把它做成一个“大而全”的 Agent 平台

在 AI 工具领域，很容易不断加入 prompt 管理、memory、workflow、模型路由和托管控制台。但这些能力并不是 Bridgent AI 当前最该解决的问题。

它现阶段坚持三件事：

- **本地运行**：工具与业务代码放在一起；
- **显式组合**：开发者能够看到并控制 server file；
- **专注连接层**：只解决 Source → MCP Tools，而不是接管完整 Agent runtime。

这种边界让 Bridgent AI 更容易进入现有工程，也让项目可以把精力放在 adapter 正确性、transport 兼容性和安全控制上。

## 当前状态与下一步

Bridgent AI 仍处于 **Alpha**，现在已经具备：

- OpenAPI、Prisma、Drizzle、tRPC、Zod 五类 source；
- stdio、Streamable HTTP、Web Standard handler 三类 transport；
- `bridgent init`、`dev`、`serve`、`inspect` CLI；
- 主流 MCP host 的配置文档；
- 示例项目和协议级测试。

下一阶段计划聚焦 generated tool surface 的本地 policy enforcement，让 metadata 不只是用于提示，还能在工具执行前真正限制 allowlist、只读模式、最大工具数量，以及写工具必须具备的安全条件。

GraphQL、托管控制台与 registry/distribution workflow 仍属于更后续的方向，不会为了功能列表好看而提前承诺。

## 写在最后

我做 Bridgent AI，不是因为 MCP Server 完全写不出来，而是因为团队不应该为同一套系统定义反复付出维护成本。

如果你的项目已经有 OpenAPI、Prisma、Drizzle、tRPC 或 Zod，那么它们本来就应该成为 AI Agent 理解和调用系统的入口。

项目现在还早，真实使用反馈会直接影响它接下来支持什么。如果 Bridgent AI 刚好解决了你的问题，欢迎试用、提交 Issue，或者点一个 Star，让我知道这条路值得继续走下去。

- GitHub：[https://github.com/JS-mark/Bridgent](https://github.com/JS-mark/Bridgent)
- 文档：[https://js-mark.com/Bridgent/](https://js-mark.com/Bridgent/)
- License：MIT
