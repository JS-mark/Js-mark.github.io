---
title: Bridgent AI：为什么我选择复用 Schema，而不是继续手写 MCP Server
type: categories
author: Mark
comments: true
external_link:
  enable: true
date: 2026-08-14 19:38:51
tags:
categories:
---
![logo](logo.png)

Bridgent AI 是一个 TypeScript 开源工具，可将 OpenAPI、Prisma、Drizzle、tRPC 与 Zod 定义转换为 Claude Code、Cursor、Codex 等客户端可用的 MCP tools。


AI coding agent 正在从“帮我补几行代码”走向“替我完成一段真实工作流”。但当它需要调用团队内部 API、查询业务数据库或执行已有服务能力时，事情往往卡在了最后一公里：**Agent 不知道这些能力存在，也没有安全、结构化的调用入口。**

<!-- more -->

MCP 为工具接入提供了统一协议，却没有消除每个团队重复建设工具层的成本。我们仍要重新描述参数、编写执行函数、选择 transport、处理异常，并为数据库补上访问限制与审计。

这也是我开始做 Bridgent AI 的原因。

### 系统已经被描述过一次了

一个成熟的 TypeScript 项目通常已经拥有多种机器可读定义：OpenAPI 描述 HTTP API，Prisma 与 Drizzle 描述数据模型，tRPC router 描述应用过程，Zod 描述输入约束。

如果这些定义已经是项目事实，就不应该为了接入 AI Agent 再维护一套容易漂移的 tool schema。

Bridgent AI 让这些已有定义直接生成 MCP tools：

- OpenAPI operation 成为可调用工具
- Prisma model 成为默认只读的查询工具
- Drizzle table 成为受限的只读工具
- tRPC query 成为 MCP read tool，mutation 保持默认关闭
- Zod schema 与函数可以组成自定义 typed tool

这条路径减少的不只是代码量，更重要的是减少了“API 已经变化，但 MCP 工具还停留在旧参数”的双重维护问题。

### 保留开发者对运行时的控制

Bridgent AI 没有试图成为另一个 Agent framework。它不管理 prompt、memory、chain、planning，也不决定你使用哪个模型。

开发者在一个明确的 server file 中组合 source adapter，再选择 stdio、Streamable HTTP 或 Web Standard handler。鉴权上下文、依赖注入和暴露范围仍由应用控制，因此它可以逐步进入现有工程，而不是要求团队迁移整个 AI 技术栈。

### 安全应该是默认值，不是文档里的提醒

数据库接入最容易出现“演示很顺，生产不敢用”的落差。

因此 Bridgent AI 的数据库 source 默认只读，限制返回行数与查询时间，不提供 raw SQL。Prisma 写操作只有在显式开启、加入 allowlist 并配置 audit 后才会出现；执行采用 dry-run 与 preview token 两步确认，并可使用幂等键降低宿主重试导致的重复写入风险。

这些机制不能替代业务系统本身的身份认证和授权，但它们让危险能力不再因为一次无意配置就默认暴露。

### 现在可以做什么

Bridgent AI 当前处于 Alpha，已提供：

- OpenAPI 3.x、Prisma 6.x、Drizzle、tRPC v10/v11、Zod source
- stdio、Streamable HTTP、Web Standard fetch handler
- `bridgent init / dev / serve / inspect` CLI
- Claude Code、Cursor、Codex、Gemini CLI 的配置文档
- 可运行示例与三种 transport 的协议级测试

下一阶段会聚焦本地 policy enforcement，让 generated tool surface 不只携带 capability metadata，还能在执行前应用明确策略。

如果你正在尝试让 AI Agent 接入真实业务系统，欢迎试用 Bridgent AI。项目仍早，也正因为早，真实的集成反馈会直接影响它接下来支持的数据源和安全边界。

GitHub：https://github.com/JS-mark/Bridgent

文档：https://js-mark.com/Bridgent/