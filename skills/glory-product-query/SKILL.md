---
name: glory-product-query
description: 查询保险产品/销售计划列表，支持按状态、渠道筛选，并对结果进行分析统计。当用户提到查询产品、产品列表、销售计划、wecom计划、产品分析、在售产品、保险产品、产品搜索、产品统计时自动触发。
---

# Glory 产品查询 Skill

通过 MCP 协议查询保险产品销售计划列表，支持按状态、渠道、关键词筛选，并具备数据分析能力。

## 前置条件

此 skill 依赖 `glory-product-query` MCP Server。请确认已完成以下安装步骤：

```bash
# 1. 安装
npm install @gloryfham/glory-product-query-mcp

# 2. 初始化配置（baseUrl 向管理员获取）
npx glory-product-query-mcp config init --baseUrl <API地址>

# 3. 配置客户端（根据使用的工具选择一个）
npx glory-product-query-mcp setup claude-code      # Claude Code
npx glory-product-query-mcp setup qoder            # Qoder
npx glory-product-query-mcp setup claude-desktop   # Claude Desktop
npx glory-product-query-mcp setup kiro             # Kiro
npx glory-product-query-mcp setup opencode         # OpenCode
```

如 MCP 工具调用报错 `未配置 baseUrl`，说明第 2 步未完成，请引导用户执行 `config init`。

## 触发条件

当用户的意图涉及以下场景时，应主动调用 MCP 工具：

- 查询/搜索保险产品或销售计划
- 查看在售产品列表或产品详情
- 按关键词搜索特定产品（如"医疗"、"重疾"、"储蓄"）
- 统计或分析产品数据（如数量、分布、对比）
- 提及 wecom、企微、渠道相关的产品查询

## 首次使用引导

当 skill 被触发但 MCP 工具不可用时（即 `mcp__glory-product-query__queryWecomPlanList` 不在可用工具列表中），**不要尝试调用工具**，而是输出以下引导信息：

> **glory-product-query MCP Server 尚未配置**
>
> 请按以下步骤完成安装：
>
> ```bash
> # 1. 安装 MCP Server
> npm install @gloryfham/glory-product-query-mcp
>
> # 2. 初始化配置（baseUrl 向管理员获取）
> npx glory-product-query-mcp config init --baseUrl <API地址>
>
> # 3. 配置当前客户端
> npx glory-product-query-mcp setup qoder    # 根据实际客户端选择
> ```
>
> 完成后请重启客户端，然后重新发起查询。

如果工具存在但调用时返回 `未配置 baseUrl` 错误，说明第 2 步未完成，引导用户执行：

```bash
npx glory-product-query-mcp config init --baseUrl <API地址>
```

## MCP 工具

### queryWecomPlanList

查询企微渠道保险产品销售计划列表。

**调用方式**: `mcp__glory-product-query__queryWecomPlanList`

**参数说明**:

| 参数 | 类型 | 必填 | 默认值 | 说明 |
|------|------|------|--------|------|
| currentPage | number | 否 | 1 | 查询页码 |
| pageSize | number | 否 | 1000 | 每页数量，建议首次查询用大值获取全量 |
| statusCodes | string[] | 否 | ["2"] | 销售计划状态码（见下方参考值） |
| channelCodes | string[] | 否 | ["WECOM_GLORY"] | 销售渠道码（见下方参考值） |
| keyword | string | 否 | - | 按关键词全文搜索（匹配所有文本字段） |

**statusCodes 参考值**:

| 值 | 含义 |
|----|------|
| "1" | 待上架 |
| "2" | 已上架（在售） |
| "3" | 已下架 |

**channelCodes 参考值**:

| 值 | 含义 |
|----|------|
| "WECOM_GLORY" | 企微 Glory 渠道（默认） |

**返回格式**: Markdown 表格，包含产品名称、状态、渠道等字段，最多展示前 20 条，超出会提示总数。

## 使用指引

### 基本查询

当用户说"查询产品列表"或"有什么在售产品"时：

```
调用 queryWecomPlanList，使用默认参数即可获取所有在售产品
```

### 关键词搜索

当用户说"搜索医疗相关产品"或"有没有重疾险"时：

```
调用 queryWecomPlanList，设置 keyword = "医疗" 或 "重疾"
```

### 查询特定状态

当用户说"查看已下架的产品"时：

```
调用 queryWecomPlanList，设置 statusCodes = ["3"]
```

### 查询全部状态

当用户说"查看所有产品（含下架）"时：

```
调用 queryWecomPlanList，设置 statusCodes = ["1", "2", "3"]
```

## 数据分析能力

获取产品数据后，应主动进行以下分析（无需用户额外要求）：

1. **汇总统计**: 告知用户查询到的产品总数
2. **关键词搜索结果**: 如果使用了 keyword，说明匹配了多少条记录
3. **字段概览**: 列出返回数据包含哪些主要字段

当用户明确要求分析时，还可以进行：

1. **分类统计**: 按产品类型、状态等维度进行分组统计
2. **数据对比**: 对比不同渠道或不同状态的产品差异
3. **趋势分析**: 如有时间字段，分析产品上下架时间分布
4. **字段提取**: 从返回结果中提取特定字段，用于后续处理或导出

## 错误处理

| 错误信息 | 原因 | 解决方案 |
|----------|------|----------|
| 未配置 baseUrl | 未执行 config init | 引导用户执行 `npx glory-product-query-mcp config init --baseUrl <url>` |
| HTTP 4xx/5xx | API 地址错误或服务异常 | 建议用户检查 baseUrl 配置是否正确 |
| 请求超时 | 网络问题或服务无响应 | 建议用户检查网络连接，稍后重试 |
| 未查询到数据 | 筛选条件过严或无匹配 | 尝试放宽 statusCodes 或移除 keyword 再次查询 |

## 注意事项

- 默认只查询已上架状态（statusCode=2）的 WECOM_GLORY 渠道产品
- 返回结果最多展示前 20 条记录，超出部分会提示总数
- 首次查询建议使用默认参数获取全量数据，再根据需要筛选
- keyword 搜索为全文模糊匹配，会搜索所有文本类型字段
