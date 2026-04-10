
# Odaily RSS API 接口文档

## 概览

| 项目 | 说明 |
|------|------|
| Base URL | `https://api.odaily.news` |
| 协议 | HTTPS |
| 数据格式 | JSON |
| 字符编码 | UTF-8 |
| 认证方式 | 无（公开接口） |

---

## 通用规范

### 请求头

| Header | 值 | 说明 |
|--------|----|------|
| `Accept` | `application/json` | 建议携带 |
| `Content-Type` | `application/json` | POST 请求时必填，当前所有接口均为 GET |

### 公共查询参数

以下参数在所有列表接口中均适用：

| 参数名 | 类型 | 必填 | 默认值 | 说明 |
|--------|------|------|--------|------|
| `page` | int | 否 | 1 | 页码，从 1 开始 |
| `size` | int | 否 | 10 | 每页条数 |
| `lang` | string | 否 | `zh-cn` | 语言，见[语言参数](#语言参数) |

### 语言参数

| 值 | 语言 |
|----|------|
| `zh-cn` | 简体中文 |
| `zh-tw` | 繁体中文 |
| `en` | 英语 |
| `ja` | 日语 |
| `ko` | 韩语 |
| `vi` | 越南语 |
| `th` | 泰语 |

### 通用响应结构

所有接口均返回统一的顶层结构：

| 字段 | 类型 | 说明 |
|------|------|------|
| `code` | int | 状态码，`200` 表示成功 |
| `msg` | string | 消息描述 |
| `success` | boolean | 请求是否成功 |
| `data` | object | 业务数据，结构因接口而异 |
| `serverTimestamp` | long | 服务器时间戳（ms） |

**列表类接口的 `data` 结构：**

| 字段 | 类型 | 说明 |
|------|------|------|
| `data.total` | long | 总条数 |
| `data.totalPage` | long | 总页数 |
| `data.pageSize` | long | 每页条数 |
| `data.pageNum` | long | 当前页码 |
| `data.hasMore` | boolean | 是否有下一页 |
| `data.empty` | boolean | 当前页是否为空 |
| `data.list` | array | 数据列表 |

### 错误响应

请求失败时，HTTP 状态码为非 2xx，响应体结构如下：

```json
{
  "code": 400,
  "msg": "参数错误",
  "success": false,
  "data": null,
  "serverTimestamp": 1699999999999
}
```

| HTTP 状态码 | code | 说明 |
|------------|------|------|
| 400 | 400 | 请求参数错误 |
| 401 | 401 | 未授权 |
| 403 | 403 | 无权限 |
| 404 | 404 | 资源不存在 |
| 429 | 429 | 请求频率超限 |
| 500 | 500 | 服务器内部错误 |

---

## 快讯接口

### 快讯对象字段

所有快讯接口的 `data.list` 数组元素结构一致：

| 字段 | 类型 | 说明 |
|------|------|------|
| `id` | long | 快讯 ID |
| `title` | string | 标题 |
| `content` | string | 正文（HTML 格式） |
| `images` | array\<string\> | 图片 URL 列表 |
| `isImportant` | boolean | 是否重要快讯 |
| `publishTimestamp` | long | 发布时间戳（ms） |
| `publishDate` | string | 发布时间，格式 `yyyy-MM-dd HH:mm:ss` |
| `sourceUrl` | string | 原文外链，可能为空字符串 |
| `link` | string | Odaily 站内详情页 URL |

---

### 获取快讯列表

`GET /api/v1/newsflash`

返回全量快讯，按发布时间倒序排列。

**请求参数**

| 参数名 | 类型 | 必填 | 说明 |
|--------|------|------|------|
| `page` | int | 否 | 页码，默认 1 |
| `size` | int | 否 | 每页条数，默认 10 |
| `lang` | string | 否 | 语言，默认 `zh-cn` |
| `isImportant` | boolean | 否 | 为 `true` 时只返回重要快讯 |

**请求示例**

```bash
curl "https://api.odaily.news/api/v1/newsflash?page=1&size=10&lang=zh-cn"
```

**响应示例**

```json
{
  "code": 200,
  "msg": "操作成功",
  "success": true,
  "data": {
    "total": 349437,
    "totalPage": 34944,
    "pageSize": 10,
    "pageNum": 1,
    "hasMore": true,
    "empty": false,
    "list": [
      {
        "id": 476146,
        "title": "汇丰银行、渣打银行在香港获得稳定币牌照",
        "content": "<p>Odaily星球日报讯 ...</p>",
        "images": [],
        "isImportant": true,
        "publishTimestamp": 1775812659000,
        "publishDate": "2026-04-10 17:17:39",
        "sourceUrl": "https://www.hkma.gov.hk/...",
        "link": "https://www.odaily.news/zh-CN/newsflash/476146"
      }
    ]
  },
  "serverTimestamp": 1775813803078
}
```

---

### 24 小时快讯

`GET /api/v1/newsflash/24h`

返回最近 24 小时内发布的快讯。

**请求参数**

| 参数名 | 类型 | 必填 | 说明 |
|--------|------|------|------|
| `page` | int | 否 | 页码，默认 1 |
| `size` | int | 否 | 每页条数，默认 10 |
| `lang` | string | 否 | 语言，默认 `zh-cn` |

**请求示例**

```bash
curl "https://api.odaily.news/api/v1/newsflash/24h?page=1&size=10"
```

**响应结构** 同[快讯对象字段](#快讯对象字段)。

---

### 24 小时热门快讯

`GET /api/v1/newsflash/24h-hot`

返回最近 24 小时内热度最高的快讯。

**请求参数**

| 参数名 | 类型 | 必填 | 说明 |
|--------|------|------|------|
| `page` | int | 否 | 页码，默认 1 |
| `size` | int | 否 | 每页条数，默认 10 |
| `lang` | string | 否 | 语言，默认 `zh-cn` |

**请求示例**

```bash
curl "https://api.odaily.news/api/v1/newsflash/24h-hot?page=1&size=10"
```

**响应结构** 同[快讯对象字段](#快讯对象字段)。

---

### AI 快讯

`GET /api/v1/newsflash/ai`

返回 AI 相关分类快讯。

**请求参数**

| 参数名 | 类型 | 必填 | 说明 |
|--------|------|------|------|
| `page` | int | 否 | 页码，默认 1 |
| `size` | int | 否 | 每页条数，默认 10 |
| `lang` | string | 否 | 语言，默认 `zh-cn` |

**请求示例**

```bash
curl "https://api.odaily.news/api/v1/newsflash/ai?page=1&size=10"
```

**响应结构** 同[快讯对象字段](#快讯对象字段)。

---

### 预测市场快讯

`GET /api/v1/newsflash/prediction-market`

返回预测市场相关快讯。

**请求参数**

| 参数名 | 类型 | 必填 | 说明 |
|--------|------|------|------|
| `page` | int | 否 | 页码，默认 1 |
| `size` | int | 否 | 每页条数，默认 10 |
| `lang` | string | 否 | 语言，默认 `zh-cn` |

**请求示例**

```bash
curl "https://api.odaily.news/api/v1/newsflash/prediction-market?page=1&size=10"
```

**响应结构** 同[快讯对象字段](#快讯对象字段)。

---

### 链上数据快讯

`GET /api/v1/newsflash/on-chain-data`

返回链上数据相关快讯。

**请求参数**

| 参数名 | 类型 | 必填 | 说明 |
|--------|------|------|------|
| `page` | int | 否 | 页码，默认 1 |
| `size` | int | 否 | 每页条数，默认 10 |
| `lang` | string | 否 | 语言，默认 `zh-cn` |

**请求示例**

```bash
curl "https://api.odaily.news/api/v1/newsflash/on-chain-data?page=1&size=10"
```

**响应结构** 同[快讯对象字段](#快讯对象字段)。

---

### 行情播报

`GET /api/v1/newsflash/market-report`

返回行情播报类快讯。

**请求参数**

| 参数名 | 类型 | 必填 | 说明 |
|--------|------|------|------|
| `page` | int | 否 | 页码，默认 1 |
| `size` | int | 否 | 每页条数，默认 10 |
| `lang` | string | 否 | 语言，默认 `zh-cn` |

**请求示例**

```bash
curl "https://api.odaily.news/api/v1/newsflash/market-report?page=1&size=10"
```

**响应结构** 同[快讯对象字段](#快讯对象字段)。

---

### 交易所公告

`GET /api/v1/newsflash/exchange-announcement`

返回交易所官方公告类快讯。

**请求参数**

| 参数名 | 类型 | 必填 | 说明 |
|--------|------|------|------|
| `page` | int | 否 | 页码，默认 1 |
| `size` | int | 否 | 每页条数，默认 10 |
| `lang` | string | 否 | 语言，默认 `zh-cn` |

**请求示例**

```bash
curl "https://api.odaily.news/api/v1/newsflash/exchange-announcement?page=1&size=10"
```

**响应结构** 同[快讯对象字段](#快讯对象字段)。

---

### 项目动向

`GET /api/v1/newsflash/project-trend`

返回区块链项目动态类快讯。

**请求参数**

| 参数名 | 类型 | 必填 | 说明 |
|--------|------|------|------|
| `page` | int | 否 | 页码，默认 1 |
| `size` | int | 否 | 每页条数，默认 10 |
| `lang` | string | 否 | 语言，默认 `zh-cn` |

**请求示例**

```bash
curl "https://api.odaily.news/api/v1/newsflash/project-trend?page=1&size=10"
```

**响应结构** 同[快讯对象字段](#快讯对象字段)。

---

### 名人观点

`GET /api/v1/newsflash/celebrity-opinion`

返回行业名人言论类快讯。

**请求参数**

| 参数名 | 类型 | 必填 | 说明 |
|--------|------|------|------|
| `page` | int | 否 | 页码，默认 1 |
| `size` | int | 否 | 每页条数，默认 10 |
| `lang` | string | 否 | 语言，默认 `zh-cn` |

**请求示例**

```bash
curl "https://api.odaily.news/api/v1/newsflash/celebrity-opinion?page=1&size=10"
```

**响应结构** 同[快讯对象字段](#快讯对象字段)。

---

### 币股动态

`GET /api/v1/newsflash/crypto-stock`

返回加密货币与股票相关联动类快讯。

**请求参数**

| 参数名 | 类型 | 必填 | 说明 |
|--------|------|------|------|
| `page` | int | 否 | 页码，默认 1 |
| `size` | int | 否 | 每页条数，默认 10 |
| `lang` | string | 否 | 语言，默认 `zh-cn` |

**请求示例**

```bash
curl "https://api.odaily.news/api/v1/newsflash/crypto-stock?page=1&size=10"
```

**响应结构** 同[快讯对象字段](#快讯对象字段)。

---

### 融资信息

`GET /api/v1/newsflash/funding`

返回融资投资相关快讯。

**请求参数**

| 参数名 | 类型 | 必填 | 说明 |
|--------|------|------|------|
| `page` | int | 否 | 页码，默认 1 |
| `size` | int | 否 | 每页条数，默认 10 |
| `lang` | string | 否 | 语言，默认 `zh-cn` |

**请求示例**

```bash
curl "https://api.odaily.news/api/v1/newsflash/funding?page=1&size=10"
```

**响应结构** 同[快讯对象字段](#快讯对象字段)。

---

### 宏观政策

`GET /api/v1/newsflash/macro-policy`

返回宏观经济与监管政策类快讯。

**请求参数**

| 参数名 | 类型 | 必填 | 说明 |
|--------|------|------|------|
| `page` | int | 否 | 页码，默认 1 |
| `size` | int | 否 | 每页条数，默认 10 |
| `lang` | string | 否 | 语言，默认 `zh-cn` |

**请求示例**

```bash
curl "https://api.odaily.news/api/v1/newsflash/macro-policy?page=1&size=10"
```

**响应结构** 同[快讯对象字段](#快讯对象字段)。

---

## 文章接口

### 文章对象字段

所有文章接口的 `data.list` 数组元素结构一致：

| 字段 | 类型 | 说明 |
|------|------|------|
| `id` | long | 文章 ID |
| `title` | string | 标题 |
| `cover` | string | 封面图 URL |
| `summary` | string | 摘要 |
| `content` | string | 正文（HTML 格式） |
| `publishTimestamp` | long | 发布时间戳（ms） |
| `publishDate` | string | 发布时间，格式 `yyyy-MM-dd HH:mm:ss` |
| `sourceUrl` | string | 原文外链，可为 `null` |
| `link` | string | Odaily 站内详情页 URL |

---

### 获取文章列表

`GET /api/v1/article`

返回全量文章，按发布时间倒序排列。

**请求参数**

| 参数名 | 类型 | 必填 | 说明 |
|--------|------|------|------|
| `page` | int | 否 | 页码，默认 1 |
| `size` | int | 否 | 每页条数，默认 10 |
| `lang` | string | 否 | 语言，默认 `zh-cn` |

**请求示例**

```bash
curl "https://api.odaily.news/api/v1/article?page=1&size=10&lang=zh-cn"
```

**响应示例**

```json
{
  "code": 200,
  "msg": "操作成功",
  "success": true,
  "data": {
    "total": 50000,
    "totalPage": 5000,
    "pageSize": 10,
    "pageNum": 1,
    "hasMore": true,
    "empty": false,
    "list": [
      {
        "id": 5210174,
        "title": "黄金重回4800美元，今年顶部在哪？",
        "cover": "https://oss.odaily.top/image/2026/04/09/xxx.jpg",
        "summary": "黄金的价格上限取决于你的风险承受能力上限。",
        "content": "<p>...</p>",
        "publishTimestamp": 1775731018000,
        "publishDate": "2026-04-09 18:36:58",
        "sourceUrl": null,
        "link": "https://www.odaily.news/zh-CN/post/5210174"
      }
    ]
  },
  "serverTimestamp": 1775813803078
}
```

---

### 24 小时文章

`GET /api/v1/article/24h`

返回最近 24 小时内发布的文章。

**请求参数**

| 参数名 | 类型 | 必填 | 说明 |
|--------|------|------|------|
| `page` | int | 否 | 页码，默认 1 |
| `size` | int | 否 | 每页条数，默认 10 |
| `lang` | string | 否 | 语言，默认 `zh-cn` |

**请求示例**

```bash
curl "https://api.odaily.news/api/v1/article/24h?page=1&size=10"
```

**响应结构** 同[文章对象字段](#文章对象字段)。

---

### 24 小时热门文章

`GET /api/v1/article/24h-hot`

返回最近 24 小时内热度最高的文章。

**请求参数**

| 参数名 | 类型 | 必填 | 说明 |
|--------|------|------|------|
| `page` | int | 否 | 页码，默认 1 |
| `size` | int | 否 | 每页条数，默认 10 |
| `lang` | string | 否 | 语言，默认 `zh-cn` |

**请求示例**

```bash
curl "https://api.odaily.news/api/v1/article/24h-hot?page=1&size=10"
```

**响应结构** 同[文章对象字段](#文章对象字段)。

---

### 深度文章

`GET /api/v1/article/depth`

返回深度分析类文章。

**请求参数**

| 参数名 | 类型 | 必填 | 说明 |
|--------|------|------|------|
| `page` | int | 否 | 页码，默认 1 |
| `size` | int | 否 | 每页条数，默认 10 |
| `lang` | string | 否 | 语言，默认 `zh-cn` |

**请求示例**

```bash
curl "https://api.odaily.news/api/v1/article/depth?page=1&size=10"
```

**响应结构** 同[文章对象字段](#文章对象字段)。

---

### 重要文章

`GET /api/v1/article/important`

返回被标记为推送过的重要文章。

**请求参数**

| 参数名 | 类型 | 必填 | 说明 |
|--------|------|------|------|
| `page` | int | 否 | 页码，默认 1 |
| `size` | int | 否 | 每页条数，默认 10 |
| `lang` | string | 否 | 语言，默认 `zh-cn` |

**请求示例**

```bash
curl "https://api.odaily.news/api/v1/article/important?page=1&size=10"
```

**响应结构** 同[文章对象字段](#文章对象字段)。

---

## 搜索接口

### 搜索快讯

`GET /api/v1/search/newsflash`

按关键词全文搜索快讯。

> **注意**：`keyword` 参数中的中文需进行 URL 编码，例如 `比特币` → `%E6%AF%94%E7%89%B9%E5%B8%81`。

**请求参数**

| 参数名 | 类型 | 必填 | 说明 |
|--------|------|------|------|
| `keyword` | string | 是 | 搜索关键词（需 URL 编码） |
| `page` | int | 否 | 页码，默认 1 |
| `size` | int | 否 | 每页条数，默认 10 |
| `lang` | string | 否 | 语言，默认 `zh-cn` |

**请求示例**

```bash
curl "https://api.odaily.news/api/v1/search/newsflash?keyword=%E6%AF%94%E7%89%B9%E5%B8%81&page=1&size=10"
```

**响应示例**

```json
{
  "code": 200,
  "msg": "操作成功",
  "success": true,
  "data": {
    "total": 61079,
    "totalPage": 6108,
    "pageSize": 10,
    "pageNum": 1,
    "hasMore": true,
    "empty": false,
    "list": [
      {
        "id": 476123,
        "title": "BIT：比特币已处于明显超卖区间",
        "content": "<p>...</p>",
        "images": [],
        "isImportant": true,
        "publishTimestamp": 1775802131000,
        "publishDate": "2026-04-10 14:22:11",
        "sourceUrl": "https://x.com/...",
        "link": "https://www.odaily.news/zh-CN/newsflash/476123"
      }
    ]
  },
  "serverTimestamp": 1775813857129
}
```

---

### 搜索文章

`GET /api/v1/search/article`

按关键词全文搜索文章。

> **注意**：`keyword` 参数中的中文需进行 URL 编码。

**请求参数**

| 参数名 | 类型 | 必填 | 说明 |
|--------|------|------|------|
| `keyword` | string | 是 | 搜索关键词（需 URL 编码） |
| `page` | int | 否 | 页码，默认 1 |
| `size` | int | 否 | 每页条数，默认 10 |
| `lang` | string | 否 | 语言，默认 `zh-cn` |

**请求示例**

```bash
curl "https://api.odaily.news/api/v1/search/article?keyword=%E6%AF%94%E7%89%B9%E5%B8%81&page=1&size=10"
```

**响应结构** 同[文章对象字段](#文章对象字段)。
