# Odaily RESTful-API

Domain
```
https://www.odaily.news/v1/
```
## 1 Specifications

### 1.1 Communication Protocol
HTTPS protocol

### 1.2 Request Method
All endpoints only support GET requests.

### 1.3 Character Encoding
UTF-8 character encoding is used for both HTTPS communication

### 1.4 Request Message Structure

### 1.4.1 Flash Details
- ** article **
- **Endpoint：** openapi/feeds
- **Endpoint：** openapi/feeds_en
- **Endpoint：** openapi/feeds_ja
- **Endpoint：** openapi/feeds_vi
- **Endpoint：** openapi/feeds_ko
- **Endpoint：** openapi/feeds_th
- **Endpoint：** openapi/feeds_zhtw

Sometimes we want to query previous data. Therefore the parameter "length" is added. "length" represents the number of days from a previous time to the current time. For example, the current date is 2018-01-02, we can call openapi/feeds?length=1 to query the data on 2018-01-01。 

### 1.5 Response Message Structure
#### 1.5.1 Structure Description
All endpoint responses are in JSON format. Unless specified otherwise, each response contains the following fields:

Parameter Name      |Description
:----       |:---
title       |Newsletter/Article title
id           |Newsletter/Article id
news_url     |Newsletter/Article reference link
link | Newsletter/Article  link in odaily
type      |Newsletter/Article type [newsflashes/post]
published_at | Newsletter/Article release time

#### 1.5.2 Response Message Example

```
{
    "code": 0,
    "data": {
        "arr_news": [
            {
                "title": "汇丰：区块链债券代币化平台HSBC Orion未来或将支持跨境交易",
                "news_url": "https://www1.hkej.com/dailynews/investment/article/3558797/%E4%BA%9E%E6%B4%B2%E7%B6%93%E6%BF%9F%E6%BD%9B%E5%8A%9B%E5%8E%9A+%E9%8A%80%E8%A1%8C%E5%89%B5%E6%96%B0%E8%BF%8E%E5%95%86%E6%A9%9F",
                "id": 335035,
                "link": "www.odaily.news/newsflashes/335035"
                "published_at": "2023-09-08 14:45:15",
                "description": "Odaily星球日报讯 汇丰环球银行业务亚太区主管兼总经理马智萍表示，汇丰推出的Orion平台供发行方通过区块链于卢森堡发行电子债券，并可在平台上以多种货币进行买卖，未来或将支持跨境交易。\n马智萍还指出，汇丰今年2月协助香港政府通过发行总值8亿港元的代币化绿色债券，糅合了数字金融、ESG，以及财富投资三大元素，充分展现了银行的未来发展。（信报）",
                "type": "newsflashes"
            }
        ],
        "Lang": "zh-cn",
        "Title": "Odaily星球日报 | 区块链新闻数据资讯媒体",
        "Desc": "36氪战略合作区块链媒体，致力于客观求是，助力行业发展，报道不收取费用，将新闻资讯、数据行情、技术解读、独家深度一网打尽，探索真实区块链。"
    }
}
```

## 2 Appendix - Response Code Description

Response Code |Description
:---- |:---
0  |Processed successfully
## Authors
- [@odaily.news](https://www.odaily.news)

## 3 Contact

For business cooperation, please email：marketing@odaily.email。

If you need business cooperation, please add us on WeChat for business

WeChat of our business QR code :
<img width="100" alt="496bd829f51cda8f4c8027daf0e6b543" src="https://piccdn.0daily.com/202212/02073313/n38zo2eckiajkarj.png">
<img width="100" alt="496bd829f51cda8f4c8027daf0e6b543" src="https://piccdn.0daily.com/202212/02073318/whh64xdbamdlspu2.png">

At the same time, we also have some communities, if you are interested, you can join our community.

	- Telegram: https://t.me/Odaily_CryptoPunk
	- Discord: https://discord.com/invite/nQrCTu9Ekd
	- twitter: https://twitter.com/OdailyChina
