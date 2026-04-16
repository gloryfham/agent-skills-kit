# Glory Product Catalog MCP 工具参考

MCP Server 名称: `glory-product-catalog`
连接方式: SSE (`http://localhost:8076/sse`)

---

## 保险产品工具

### listInsuranceProducts
获取所有保险产品的列表概览。无参数。

---

### searchInsuranceProducts
根据关键词、产品类型、地区搜索保险产品。

| 参数 | 类型 | 必填 | 说明 |
|------|------|------|------|
| keyword | string | 是 | 搜索关键词，支持产品名称、简称、标签模糊匹配 |
| productType | string | 否 | 产品类型筛选，如 H01/H08/H09 |
| region | string | 否 | 产品地区筛选，如 HK/SG |

**产品类型对照**：
- `H01` — 分红险（Whole Life Participating）
- `H08` — 年金险（Annuity）
- `H09` — IUL（指数型万能寿险）

**产品分类对照**（`productClassify` 字段）：
- `06` — 分红险
- `07` — 年金险
- `01` — IUL

---

### getInsuranceProductDetail
根据产品编号获取保险产品详情。

| 参数 | 类型 | 必填 | 说明 |
|------|------|------|------|
| productCode | string | 是 | 产品编号，如 PRU_PACE、SGCL_HISP_S2 |

---

## 签证项目工具

### listVisaProjects
获取所有签证/移民项目的列表概览。无参数。

---

### searchVisaProjects
根据关键词搜索签证/移民项目。

| 参数 | 类型 | 必填 | 说明 |
|------|------|------|------|
| keyword | string | 是 | 搜索关键词，支持项目名称、国家、投资类型模糊匹配 |
| projectType | string | 否 | 项目类型筛选，如 传统国家/欧洲居留权/公民权 |

---

### getVisaProjectDetail
根据项目编号获取签证项目详情。

| 参数 | 类型 | 必填 | 说明 |
|------|------|------|------|
| projectCode | string | 是 | 项目编号，如 sg_prj_0001、sg_prj_0006 |

---

### getVisaProjectsByCountry
按指定国家列出所有签证/移民项目。

| 参数 | 类型 | 必填 | 说明 |
|------|------|------|------|
| country | string | 是 | 国家名称，如 美国/加拿大/希腊/马耳他/土耳其 |

---

## 签证项目类型

`projectTypeDesc` 字段值：
- `传统国家` — 如美国 EB5/EB1A/NIW、加拿大曼省/萨省
- `欧洲居留权` — 如希腊购房、西班牙购房、马耳他永居
- `公民权` — 如马耳他入籍、土耳其、多米尼克、安提瓜、圣基茨、格林纳达、圣卢西亚、瓦努阿图
