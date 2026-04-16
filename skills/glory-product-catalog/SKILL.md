---
name: glory-product-catalog
description: 查询公司产品库信息，包括保险产品搜索与详情、签证/移民项目搜索与详情。当用户提到产品相关需求、询问保险产品、签证项目、移民项目时，立即触发此 skill。
license: MIT
metadata:
  author: Glory
  version: "1.0.0"
  homepage: "https://github.com/gloryfham/agent-skills"
  agent:
    requires:
      mcp: ["glory-product-catalog"]
---

# Glory Product Catalog — 产品库查询

通过 MCP Server `glory-product-catalog` 查询公司保险产品库和签证/移民项目数据。

---

## 数据源

MCP Server: `glory-product-catalog`（SSE 连接，本地启动于 business-hk-web 服务）

可用工具分为两类：

| 类别 | 工具 | 用途 |
|------|------|------|
| 保险产品 | `listInsuranceProducts` | 获取所有保险产品列表 |
| 保险产品 | `searchInsuranceProducts(keyword, productType, region)` | 搜索保险产品 |
| 保险产品 | `getInsuranceProductDetail(productCode)` | 获取保险产品详情 |
| 签证项目 | `listVisaProjects` | 获取所有签证/移民项目列表 |
| 签证项目 | `searchVisaProjects(keyword, projectType)` | 搜索签证项目 |
| 签证项目 | `getVisaProjectDetail(projectCode)` | 获取签证项目详情 |
| 签证项目 | `getVisaProjectsByCountry(country)` | 按国家列出签证项目 |

工具参数详情见 `references/tools.md`。

---

## 工作流程

```
1. 识别场景  →  2. 调用 MCP 工具获取数据  →  3. 结构化输出
```

---

## 场景与输出规范

### 场景一：保险产品查询

**触发**：用户询问保险产品、保险计划、保险方案等。

**操作**：
1. 如有具体产品名称，调用 `searchInsuranceProducts(keyword)`
2. 如需产品详情，调用 `getInsuranceProductDetail(productCode)`
3. 如需浏览全部产品，调用 `listInsuranceProducts()`

**输出格式**：

#### 【产品列表】
```
| 产品编号 | 产品名称 | 英文名 | 简称 | 类型 | 标签 | 地区 | 公司 |
|--------|--------|--------|------|------|------|------|------|
|        |        |        |      |      |      |      |      |
```

#### 【产品详情】
当获取到详情数据时，输出：

```
【产品详情 — {salesPlanName}】

| 字段 | 内容 |
|------|------|
| 产品编号 | {salesPlanCode} |
| 产品名称 | {salesPlanName} |
| 英文名 | {salesPlanNameEn} |
| 简称 | {salesPlanShortName} |
| 产品类型 | {salesPlanType} |
| 标签 | {salesPlanLabel} |
| 地区 | {placeToBelong} |
| 公司编码 | {companyCode} |
| 产品分类 | {productClassify} |
| 简介 | {planBriefDesc} |
```

---

### 场景二：签证/移民项目查询

**触发**：用户询问签证项目、移民项目、身份规划、某国移民等。

**操作**：
1. 如有具体国家，调用 `getVisaProjectsByCountry(country)`
2. 如有具体项目编号，调用 `getVisaProjectDetail(projectCode)`
3. 关键词搜索调用 `searchVisaProjects(keyword, projectType)`
4. 浏览全部调用 `listVisaProjects()`

**输出格式**：

#### 【项目列表】
```
| 项目编号 | 项目名称 | 国家 | 类型 | 身份类型 | 投资金额 | 居住要求 | 负责人 |
|--------|--------|------|------|--------|--------|--------|--------|
|        |        |      |      |        |        |        |        |
```

#### 【项目详情】
```
【项目详情 — {projectName}】

| 字段 | 内容 |
|------|------|
| 项目编号 | {projectCode} |
| 项目名称 | {projectName} |
| 国家 | {country} |
| 项目类型 | {projectTypeDesc} |
| 投资类型 | {investType} |
| 身份类型 | {identityType} |
| 投资金额 | {investAmountRequirement} |
| 居住要求 | {residenceRequirement} |
| 项目负责人 | {projectManager} |
| 项目描述 | {projectDescribe} |
```

---

### 场景三：产品对比

**触发**：用户提到"对比"、"哪个更好"、"A 和 B 的区别"等。

**操作**：分别调用详情工具获取各产品信息，输出对比矩阵。

```
【对比矩阵】
| 维度 | 产品 A | 产品 B |
|------|--------|--------|
| 类型 |        |        |
| 地区 |        |        |
| 标签 |        |        |
| 简介 |        |        |
```

---

## 输出通用规范

1. **禁止直接输出 JSON 原文**——所有数据必须解析、格式化后呈现
2. **优先使用表格**呈现可对比数据；用文字段落进行解读
3. **信息缺失处理**：如某字段 API 未返回或为空，直接标注"数据暂未披露"
4. **数据字段说明**：
   - 保险产品 `salesPlanType`：H01=分红险, H08=年金险, H09=IUL
   - 保险产品 `productClassify`：06=分红险, 07=年金险, 01=IUL
   - 签证项目 `projectTypeDesc`：传统国家 / 欧洲居留权 / 公民权
