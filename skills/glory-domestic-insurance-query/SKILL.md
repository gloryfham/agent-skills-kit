---
name: glory-domestic-insurance-query
description: 查询国内保险产品列表（互联网+线下渠道），支持按产品类型、公司、渠道、标签筛选。当用户提到国内保险、大陆产品、国内重疾、国内医疗、互联网保险时，触发此 skill。
license: MIT
metadata:
  author: Glory
  version: "1.0.0"
  homepage: "https://github.com/gloryfham/mcp-domestic-insurance"
  agent:
    requires:
      bins: ["gloryfham-mcp-domestic-insurance"]
    install:
      - id: npm
        kind: node
        package: "@gloryfham/mcp-domestic-insurance"
        bins: ["gloryfham-mcp-domestic-insurance"]
        label: "Install Glory Domestic Insurance MCP Server"
---

# Glory 国内保险产品查询 Skill

通过 MCP Server `gloryfham-mcp-domestic-insurance` 查询国内保险产品数据（111个产品，31家保险公司）。

## 产品类型对照

| 代码 | 类型说明 | 示例产品 |
|------|----------|----------|
| P01 | 终身寿险 | 中英人寿福满佳C款（悦享版）终身寿险 |
| P02 | 两全保险 | 中宏悦华金生传承版 |
| P03 | 年金保险 | 中英人寿悦活人生年金保险B款 |
| P04 | 重大疾病保险 | 君龙人寿超级玛丽16号重大疾病保险 |
| P05 | 医疗保险 | 各类医疗保险产品 |
| P07 | 两全保险 | 含生存+身故双重保障 |
| P08 | 增额终身寿险 | 保额逐年增长的终身寿险 |
| P13 | 定期寿险 | 中意人寿擎天柱12号定期寿险 |
| P14 | 宠物保险 | 平安毛孩子宠物医疗险 |
| P15 | 意外伤害保险 | 意外保障类产品 |
| P16 | 其他类型 | 其他特殊保险产品 |

## 热门标签

| 代码 | 含义 | 说明 |
|------|------|------|
| H01 | 分红型 | 保单享有分红权益 |
| H02 | 成长型 | 保额/现金价值持续增长 |
| H03 | 年金早领 | 较早开始领取年金 |
| H04 | 热销产品 | 市场热销产品 |
| H06 | 推荐产品 | 平台推荐产品 |
| H08 | 特色产品 | 具有特色责任的产品 |

## 可用工具

### 产品列表查询

| 工具 | 参数 | 用途 |
|------|------|------|
| `listDomesticProducts` | channel(可选) | 获取所有国内产品列表 |

**channel 枚举**：
- `internet` — 互联网渠道（53个产品，支持在线投保）
- `offline` — 线下渠道（58个产品，需顾问协助）
- `all` — 全部渠道（默认）

### 产品搜索

| 工具 | 参数 | 用途 |
|------|------|------|
| `searchDomesticProducts` | keyword(可选), productType(可选), channel(可选), company(可选), hotLabel(可选) | 多条件搜索产品 |

**搜索示例**：
- 搜索重疾险：`searchDomesticProducts(productType="P04")`
- 搜索平安健康产品：`searchDomesticProducts(company="平安健康")`
- 搜索互联网热销产品：`searchDomesticProducts(channel="internet", hotLabel="H04")`

### 产品详情

| 工具 | 参数 | 用途 |
|------|------|------|
| `getDomesticProductDetail` | productCode(必填) | 获取产品详情 |

**产品编号示例**：`AM100000588`, `179403`, `NQF`, `PAN`

### 分类体系

| 工具 | 参数 | 用途 |
|------|------|------|
| `getDomesticProductCategories` | 无 | 获取产品类型/标签/公司/渠道统计 |

---

## 工作流程

```
1. 需求识别 → 2. 调用 MCP 工具 → 3. 结构化输出
```

### 第一步：需求识别

当客户提出以下任一需求时触发：
- "国内有什么保险产品"
- "我想看重疾险产品"
- "平安健康的产品有哪些"
- "互联网渠道可以投保什么"
- "给孩子买个国内保险"

### 第二步：信息澄清（对话收集）

通过对话收集以下关键信息（不要求一次全部）：
1. **产品类型** — 重疾/医疗/寿险/年金/意外
2. **投保渠道** — 互联网（在线投保）还是线下（顾问协助）
3. **保险公司** — 是否有偏好的保险公司
4. **预算范围** — 可接受的保费区间
5. **被保人情况** — 成人/少儿/老人

### 第三步：产品搜索

根据收集的信息，调用相应的搜索工具：
- `listDomesticProducts(channel="...")` — 按渠道获取产品列表
- `searchDomesticProducts(...)` — 多条件精准搜索
- `getDomesticProductCategories()` — 了解分类体系

### 第四步：产品详情补充

根据搜索结果，调用详情工具获取具体产品的完整信息：
- `getDomesticProductDetail(productCode="...")`

### 第五步：结构化输出

输出格式：
1. **产品概览** — 用表格呈现产品名称、公司、类型、简介
2. **产品详情** — 完整产品信息（如被要求）
3. **渠道说明** — 互联网 vs 线下投保方式差异
4. **下一步行动** — 具体可执行的建议

---

## 输出规范

1. **禁止直接输出 JSON** — 必须解析、格式化后呈现
2. **优先使用表格** 呈现可对比数据
3. **信息缺失标注** "数据暂未披露"
4. **渠道说明**
   - 互联网渠道 (productChannel=1): 支持在线投保，流程便捷
   - 线下渠道 (productChannel=2): 需顾问协助投保，服务更全面
5. **产品状态** productStatus=3 表示在售状态

---

## 数据概览

- **总计产品**: 111 个
- **互联网渠道**: 53 个产品
- **线下渠道**: 58 个产品
- **保险公司**: 31 家
- **产品类型**: 16 种 (P01-P16)
- **热门标签**: 6 种 (H01/H02/H03/H04/H06/H08)

---

## 创造性能力说明

本 Skill 的核心价值在于：**把国内 111 个保险产品从"产品列表"变成"客户需求驱动的精准匹配引擎"**。

客户看到的是"符合我需求的产品推荐"，顾问拿到的是"可对比、可追问、可转化"的结构化产品数据和下一步动作。这样既能提升产品查询效率，也能帮助团队快速响应客户的国内保险需求。
