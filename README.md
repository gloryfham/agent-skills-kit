# Agent Skills Kit

Claude Code **Agent Skills** for Glory global planning workflows: structured instructions and tool references for the **荣耀全球路径规划师** (Glory Global Path Planner).

This repository is a **[Claude Code plugin](https://code.claude.com/docs/en/plugins)** (manifest under [`.claude-plugin/plugin.json`](.claude-plugin/plugin.json), skills in [`skills/`](skills/)).

## 版本

**当前版本**: v2.0.0

## 布局

- [`.claude-plugin/plugin.json`](.claude-plugin/plugin.json) — Claude Code plugin 标识（`name`, `version`, …）。Plugin id `glory-agent-kit` 命名空间化 skills。
- [`.claude-plugin/marketplace.json`](.claude-plugin/marketplace.json) — 通过 GitHub marketplace 进行团队分发的目录。

每个 skill 位于 `skills/<skill-id>/` 目录下：

- `SKILL.md` — front matter（`name`, `description`）加上 agent 必须遵循的完整工作流程。

## 本仓库的 Skills

| Skill | 版本 | 用途 |
|--------|------|------|
| [glory-global-path-planner](skills/glory-global-path-planner/SKILL.md) | v2.0.0 | 荣耀全球路径规划师 — 联动国内/海外保险、移民签证与信托传承的家庭未来规划智能助手。 |
| [glory-product-query](skills/glory-product-query/SKILL.md) | v1.0.0 | 查询保险产品/销售计划列表，支持按状态、渠道筛选，并对结果进行分析统计。 |
| [glory-domestic-insurance-query](skills/glory-domestic-insurance-query/SKILL.md) | v1.0.0 | 查询国内保险产品列表（互联网+线下渠道），支持按产品类型、公司、渠道、标签筛选。 |

## 使用这些 Skills

### Claude Code (plugin)

1. **本地 checkout（开发）**:

   ```bash
   claude --plugin-dir /path/to/agent-skills-kit
   ```

   然后如果更改文件，运行 `/reload-plugins`。

2. **通过 marketplace 安装（团队或 GitHub）**:

   ```text
   /plugin marketplace add gloryfham/agent-skills-kit
   /plugin install glory-agent-kit@glory-agent-kit
   /reload-plugins
   ```

3. **直接从 GitHub 安装**:

   ```bash
   npx skills add gloryfham/agent-skills-kit
   ```

## MCP Servers

### 1. glory-global-path-planner (v2.0.0)

联动**国内/海外保险**、移民签证与信托传承的家庭未来规划智能助手。

- **npm**: https://www.npmjs.com/package/@gloryfham/mcp-global-planner
- **GitHub**: https://github.com/gloryfham/mcp-global-planner
- **安装**: `npm install -g @gloryfham/mcp-global-planner`
- **Bin**: `gloryfham-mcp-global-planner`

#### 可用工具

**国内保险查询**:

| 工具 | 参数 | 用途 |
|------|------|------|
| `listDomesticInsuranceProducts(channel?)` | channel(可选) | 获取所有国内产品列表 |
| `searchDomesticInsuranceProducts(...)` | 全部可选 | 搜索国内保险产品 |
| `getDomesticInsuranceProductDetail(productCode)` | productCode(必填) | 获取国内产品详情 |

**海外保险查询**:

| 工具 | 参数 | 用途 |
|------|------|------|
| `listInsuranceProducts` | 无 | 获取所有海外保险产品列表 |
| `searchInsuranceProducts(...)` | 全部可选 | 搜索海外保险产品 |
| `getInsuranceProductDetail(productCode)` | productCode(必填) | 获取海外产品详情 |

**签证/移民查询**:

| 工具 | 参数 | 用途 |
|------|------|------|
| `listVisaProjectTypes` | 无 | 获取所有项目类型分类 |
| `listAllVisaProjects` | 无 | 获取所有签证/移民项目列表 |
| `searchVisaProjects(...)` | 全部可选 | 搜索签证/移民项目 |
| `getVisaProjectDetail(projectCode)` | projectCode(必填) | 获取项目详情 |

**信托查询**:

| 工具 | 参数 | 用途 |
|------|------|------|
| `listTrustServiceTypes` | 无 | 获取信托服务类型分类 |
| `listTrustJurisdictions` | 无 | 获取所有司法管辖区及法律特征对比 |
| `getTrustJurisdictionDetail(jurisdictionCode)` | jurisdictionCode(必填) | 获取司法管辖区详情 |
| `listTrustProducts` | 无 | 获取所有信托产品列表 |
| `searchTrustProducts(...)` | 全部可选 | 搜索信托产品 |
| `getTrustProductDetail(productCode)` | productCode(必填) | 获取信托产品详情 |

**核心路径规划**:

| 工具 | 用途 |
|------|------|
| **`generateFamilyPathPlan`** | **生成家庭身份+保障+传承联动路径建议** |

参数：
- `primaryGoal` (必填) — 主要目标：`identity`(身份配置) | `insurance`(保障优先) | `education`(子女教育) | `retirement`(养老规划) | `comprehensive`(全面规划)
- `targetCountries` (可选) — 目标国家/地区列表
- `familyStructure` (可选) — 家庭结构描述
- `timeWindow` (可选) — 期望完成时间窗口
- `hasExistingVisa` (可选) — 是否已有海外身份
- `hasExistingInsurance` (可选) — 是否已有商业保险
- `budget` (可选) — 预算范围

---

### 2. glory-domestic-insurance-query (v1.0.0)

国内保险产品查询服务，覆盖互联网和线下双渠道。

- **npm**: https://www.npmjs.com/package/@gloryfham/mcp-domestic-insurance
- **GitHub**: https://github.com/gloryfham/mcp-domestic-insurance
- **安装**: `npm install -g @gloryfham/mcp-domestic-insurance`
- **Bin**: `gloryfham-mcp-domestic-insurance`

#### 数据概览

- **产品总数**: 111 个（互联网 53 + 线下 58）
- **保险公司**: 31 家
- **产品类型**: 16 种（终身寿险、重疾险、医疗险、意外险、年金险等）
- **热门标签**: 6 种（保证续保、保证领取、高性价比等）

#### 可用工具

| 工具 | 参数 | 用途 |
|------|------|------|
| `listDomesticProducts` | 无 | 获取所有国内产品列表 |
| `searchDomesticProducts(...)` | 全部可选 | 搜索国内保险产品 |
| `getDomesticProductDetail(productCode)` | productCode(必填) | 获取产品详情 |
| `getDomesticProductCategories` | 无 | 获取产品分类统计 |

---

### 3. glory-product-query (v1.0.0)

查询保险产品/销售计划列表，支持按状态、渠道筛选，并对结果进行分析统计。

- **npm**: https://www.npmjs.com/package/@gloryfham/glory-product-query-mcp
- **安装**: `npm install @gloryfham/glory-product-query-mcp`

> **重要**: 安装 skill 后，必须完成以下 MCP Server 配置才能正常使用产品查询功能。  
> 仅执行 `npx skills add` 只会注册 skill 定义，不会自动安装或配置 MCP Server。

#### 完整安装步骤

```bash
# 1. 安装 MCP Server
npm install @gloryfham/glory-product-query-mcp

# 2. 初始化配置（baseUrl 向管理员获取，必须完成此步）
npx glory-product-query-mcp config init --baseUrl <API地址>

# 3. 配置客户端（根据使用的工具选择一个）
npx glory-product-query-mcp setup claude-code      # Claude Code
npx glory-product-query-mcp setup qoder            # Qoder
npx glory-product-query-mcp setup claude-desktop   # Claude Desktop
npx glory-product-query-mcp setup kiro             # Kiro
npx glory-product-query-mcp setup opencode         # OpenCode
```

#### 可用工具

| 工具 | 用途 |
|------|------|
| `queryWecomPlanList` | 查询企微渠道保险产品销售计划列表，支持按状态、渠道、关键词筛选 |

---

## 业务条线

Glory Agent Kit 覆盖四大业务条线：

1. **国内保险** — 111个产品，31家公司，互联网+线下双渠道
2. **海外保险** — 分红险、年金险、IUL 等完整产品线
3. **签证/移民** — 传统国家、欧洲、亚洲等全球项目
4. **信托传承** — 香港、新加坡、BVI、开曼等多司法管辖区

## License

MIT
