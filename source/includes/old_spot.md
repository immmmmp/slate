---
title: API Reference

language_tabs: # must be one of https://git.io/vQNgJ
  - shell
  - python

toc_footers:
  - <a href='#'>Sign Up for a Developer Key</a>
  - <a href='https://github.com/slatedocs/slate'>Documentation Powered by Slate</a>

includes:
  - errors

search: true



---

# Overview

Welcome to Abit API doc. we provide REST、Websocket API to satisfy your programming need。you can find all example code on         .


# Identity Verification
```python example
签名方法说明:

注:数据接口不需要签名

Header:

1. access_key，sercet_key：从abit系统申请而来
2.发送的BODY数据格式是json，必须在json格式中添加”nonce”字段（表示时间戳）。将添加了nonce字段的json序列化成字符串作为发送消息body。
3.在请求头中添加“abit-ts” 字段,值为:当前UTC时间的时间戳,单位微秒
4.在请求头中添加“abit-sign” 字段，值为:md5(body+ sercet_key+ts(字符串))；
5.在请求中添加“abit-accesskey”字段，值位abit系统提供的access_key

Python 例子

def api_key_get(url, access_key, secret_key, params=''):
    param_data = urllib.parse.urlencode(params)

    _headers = headers.copy()
    ts = int64(time.time()*1000000)
    // 1541044393000000 当前UTC时间的时间戳,单位微秒
    abit_sign = secret_key+ts
    _headers['abit-ts'] = ts
    _headers["abit-sign"] = get_md5_value(_str=abit_sign)
    _headers['abit-accesskey'] = access_key
    _headers['abit-ExpiredTs'] = 合约云才需要,是对合约云的apiKey对应的apiSecrect加密后的超时时间,单位是微秒

    response = None
    try:
        response = requests.get(url, param_data, headers=_headers, timeout=5, verify=False)
        if response.status_code == 200:
            return response.json()
        else:
            return
    except BaseException as e:
        if response:
            print("httpGet failed, detail is:%s,%s" % (response.text, e))
        else:
            print("httpGet failed, detail is:%s" % e)
        return


def api_key_post(url, access_key, sercet_key, params):
    params['nonce'] = int(time.time())
    post_data = json.dumps(params)

    _headers = headers.copy()
    ts = int64(time.time()*1000000)
    // 1541044393000000 当前UTC时间的时间戳,单位微秒
    abit_sign = json.dumps(params) + sercet_key + ts
    _headers['abit-ts'] = ts
    _headers["abit-sign"] = get_md5_value(_str=abit_sign)
    _headers['abit-accesskey'] = access_key
    _headers['abit-ExpiredTs'] = 合约云才需要,是对合约云的apiKey对应的apiSecrect加密后的超时时间,单位是微秒
    response = None
    try:
        response = requests.post(url, post_data, headers=_headers, timeout=30, verify=False)
        if response.status_code == 200:
            return response.json()
        else:
            return
    except BaseException as e:
        if response:
            print("httpGet failed, detail is:%s,%s" % (response.text, e))
        else:
            print("httpGet failed, detail is:%s" % e)
        return


def get_md5_value(_str):
    file_md5 = hashlib.md5()
    file_md5.update(_str.encode(encoding="utf-8"))
    md5value = file_md5.hexdigest()
    return md5value

```


For authenticated requests, the following headers should be sent with the request:

.The encode string of MD5 :"{Body}+{secret_key}+{ts}".

- Body:   the params required by the interface,which is transfromed to JSON string .
- secret_key: your secret_key apply from the system.
- ts:Number of milliseconds since Unix epoch



|    param     |  type  |                         description                          |
| :----------: | :----: | :----------------------------------------------------------: |
| abit-accesskey | String |                         Your api key                         |
|    abit-ts     |  Long  |           Number of milliseconds since Unix epoch            |
|   abit-sign    | String | MD5 of the encode  string, using your API secret, as a hex string: |



# Contract Public Info-REST

REST endpoint URL: **https://api.abit.com**

# Public Info-REST

The endpoint address of Request Url of contract public info：https://api.abit.com/v1/

## GET SYMBOL INFO

> curl example

```curl
curl --location --request GET 'https://api.abit.com/v1/ifmarket/stocks?stockCode=EOS/ETH' \
--header 'abit-ver: 1008' \
--header 'abit-dev: WEB' \
--header 'abit-ts: 1541044393000000'
```

> return data:

```response-data
{
    "errno": "OK",
    "message": "Success",
    "data": {
       
        "stocks": [ 
            {
              
                "stock": { 
                    "stock_id": 14,
                    "name": "EOS/ETH",
                    "base_coin": "EOS",
                    "quote_coin": "ETH",
                    "rank": 6,
                    "price_unit": "0.0000001",
                    "vol_unit": "0.001",
                    "tip_limit": "0.2",
                    "min_amount": "0.1",
                    "status": 1,
                    "is_support_market_order": true
                },
                
                "fee_configs": [
                    {
                        "coin_code": "ETH",
                        "stock_code": "EOS/ETH",
                        "fee_ratio": "0.001",
                        "status": 1,
                        "type": 3
                    }
                ],
               
                "market_order_config": {
                    "stock_code": "EOS/ETH",
                    "status": 1,
                    "type": 1,
                    "deal_scope": "0.2"
                }
            }
        ]
    }
}

```

### Request Url

`GET ifmarket/stocks`

### Params

|   Param   |  Type  | Example  | Description |
| :-------: | :----: | :------: | :---------: |
| stockCode | String | BTC/USDT |             |



## Get Symbol Depth

> curl example

```curl
curl --location --request GET 'https://api.abit.com/v1/ifmarket/queryDepth?stockCode=ETH/USDT' \
--header 'abit-ver: 1008' \
--header 'abit-dev: WEB' \
--header 'abit-ts: 1541044393000000'
```

> return data:

```response-data
{
    "errno": "OK",
    "message": "Success",
    "data": {
        "sells": [
            {
                "price": "135.76", 
                "vol": "0.5525"
            },
            {
                "price": "135.77",  
                "vol": "0.0222"
            },
            {
                "price": "135.78",  
                "vol": "1.0186"
            },
            {
                "price": "135.79",  
                "vol": "0.5818"
            },
            {
                "price": "135.8",    
                "vol": "0.0842"
            }
        ],
        "buys": [
            {
                "price": "135.37",  
                "vol": "0.0615"
            },
            {
                "price": "135.36",  
                "vol": "0.147"
            },
            {
                "price": "135.34",  
                "vol": "0.0799"
            },
            {
                "price": "135.33",  
                "vol": "0.0027"
            },
            {
                "price": "135.3",   
                "vol": "0.1035"
            }
        ]
    }
}
```

### Request Url

`GET ifmarket/queryDepth`

### Params

|   Param   |  Type  | Example  | Description |
| :-------: | :----: | :------: | :---------: |
| stockCode | String | BTC/USDT |             |



## Get Symbol Ticker

> curl example

```curl
curl --location --request GET 'https://api.abit.com/v1/ifmarket/spotTickers?stockCode=ETH/USDT' \
--header 'abit-ver: 1008' \
--header 'abit-dev: WEB' \
--header 'abit-ts: 1541044393000000'
```

> return data:

```response-data
{
    "errno": "OK",
    "message": "Success",
    "data": [ 
        {
            "stock_code": "ETH/USDT",
            "last_price": "135.5", 
            "open": "137.17",      
            "close": "135.5",     
            "low": "127.87",       
            "high": "137.38",      
            "avg_price": "134.838547255697", 
            "volume": "10.135", 
            "total_volume": "21244.5602", 
            "timestamp": 1551322866,
            "rise_fall_rate": "-0.012174673762", 
            "rise_fall_value": "-1.67", 
            "volume_day": "9789.7338", 
            "amount24": "2894738.555982"
        }
    ]
}
```

### Request Url

`GET ifmarket/spotTickers`

### Params

|   Param   |  Type  | Example  |                 Description                  |
| :-------: | :----: | :------: | :------------------------------------------: |
| stockCode | String | BTC/USDT | optional ,if not pass,return all the tickers |



## Get Trade Records

> curl example

```curl
curl --location --request POST 'https://api.abit.com/v1/ifmarket/getStockTrades' \
--data-raw '{
    "stock_code":"ETH/USDT",
    "offset":0,
    "size":10
}'
```

> return data:

```response-data
{
    "errno":"OK",
    "message":"Success",
    "data":{
        "trades":[
            {
                "oid":100039430677,  //taker order id
                "tid":100039430678,  //trade id 
                "symbol":"BTC/ETH",
                "px":"0.06",
                "qty":"21",
                "fee":"0.0000000126",
                "fee_coin_code":"ETH",
                "created_at":"2018-04-19T03:27:21.62635069Z",  //utc time
                "side":2,    //taker order side
                "change":"0"  //the influence to the market
            }
        ]
```

### Request Url

`GET ifmarket/getStockTrades`

### Params

|   Param    |  Type  | Example  | Description |
| :--------: | :----: | :------: | :---------: |
| stock_code | String | BTC/USDT |             |
|   offset   |  Int   |    0     |             |
|    size    |  Int   |    20    |             |



## Get k-Line Data

> curl example

```curl
curl --location --request GET 'https://api.abit.com/v1/ifmarket/v2/spot?stockCode=ETH/USDT&startTime=1532610072&endTime=1581143854&unit=5&resolution=M'
```

> return data:

```response-data
{
    "errno": "OK",
    "message": "Success",
    "data": [ // array
        {
            "last_px": "135.5", 
            "open": "137.17",      
            "close": "135.5",      
            "low": "127.87",      
            "high": "137.38",      
            "avg_px": "134.838547255697", 
            "last_qty": "10.135", // last trade qty
            "timestamp": 1551322866,
            "change_rate": "-0.012174673762", 
            "change_value": "-1.67", 
        }
    ]
}
```

### Request Url

`GET ifmarket/v2/spot`

### Params

|   Param    |  Type  |  Example   | Description |
| :--------: | :----: | :--------: | :---------: |
|   symbol   | String |  BTC/USDT  |             |
| startTime  |  Int   |     0      |             |
|  endTime   |  Int   | 2532656524 |             |
|    unit    |  Int   |     5      |             |
| resolution | String |     M      |             |

# Trade API



## Submit Order



> curl example

```curl
curl --location --request POST 'https://api.abit.com/v1/ifmarket/submitOrder' \
--header 'abit-ver: 1008' \
--header 'abit-dev: api' \
--header 'abit-sign: d1701e739a359ee4c7a5003fe39ef27065c019f687b982a6e98bd375a673ec42' \
--header 'abit-ts: 1532428054000000' \
--header 'abit-accesskey: 34234423sdfsfsfwerwerwerwrwerwer' \
--data-raw '{
   "stock_code":"EOS/ETH",
   "price":"0.01",
   "vol":"10.2",
   "way":2,
   "category":1,
   "nonce":1545820222
}'
```

> Response：

```response-data
{
	"errno": "OK",
	"message": "Success",
	"data": {
		"oid": 10540013  //order id
	}
}
fail: {
	"errno": "failed",
	"message": "失败原因",
	
}
```

### Request Url    

`POST ifmarket/submitOrder`

### Params Body 

|   Param    |  Type   |  Example   |                     Description                      |
| :--------: | :-----: | :--------: | :--------------------------------------------------: |
| stock_code | String  |  BTC/USDT  |                                                      |
|   price    | Decimal |    7293    |                        Price                         |
|    vol     | Decimal |    10.2    |                         Qty                          |
|    way     |   Int   |     2      |                     1:buy 2:sell                     |
|  category  |   Int   |     1      | 1:limit price order,2:market order,price must be "0" |
|   nonce    |   int   | 1545820222 |                      Timestamp                       |



## Cancel Order

> curl example

```curl
curl --location --request POST 'https://api.abit.com/v1/ifmarket/cancelOrder' \
--header 'abit-ver: 1008' \
--header 'abit-dev: api' \
--header 'abit-sign: d1701e739a359ee4c7a5003fe39ef27065c019f687b982a6e98bd375a673ec42' \
--header 'abit-ts: 1532428054000000' \
--header 'abit-accesskey: 34234423sdfsfsfwerwerwerwrwerwer' \
--data-raw '{"order_id":14369318,"stock_code":"EOS/ETH","nonce":1545735606}'
```

> Response：

```response-data
success
{
    "errno": "OK",
    "message": "Success",
}
fail
{
    "errno": "FAILED",
    "message": "failure reason",
}
```

### Request Url    

`POST ifmarket/cancelOrder`

### Params Body 

|   Param    |  Type  |  Example   |      Description      |
| :--------: | :----: | :--------: | :-------------------: |
| stock_code | String |  BTC/USDT  |                       |
|  order_id  |  int   |    7293    | id of order to cancel |
|   nonce    |  int   | 1545820222 |       Timestamp       |



## Batch Submit Order

> curl example

```curl
curl --location --request POST 'https://api.abit.com/v1/ifmarket/batchOrders' \
--header 'abit-accesskey: 123123123213' \
--header 'abit-sign: MD5(postdata+secretKey+abit-ts)' \
--header 'abit-ver: 1008' \
--header 'abit-dev: api' \
--header 'abit-ts: 1541044393000000' \
--data-raw '{
	"orders":[
		{
		   "stock_code":"EOS/ETH",
		   "price":"0.01",
		   "vol":"10.2",
		   "way":2,
		   "category":1,
		   "custom_id":1
		},
		{
		   "stock_code":"EOS/ETH",
		   "price":"0.01",
		   "vol":"10.2",
		   "way":2,
		   "category":1,
		   "custom_id":2
		}
	],
   "nonce":1533876299
}'
```

> Response：

```response-data
success:
{
    "errno": "OK",
    "message": "Success",
    "data": {
        "orders": [
            {
                "custom_id": 1,
                "order_id": 10540013
            },
            {
                "custom_id": 2,
                "order_id": 10540014
            }
        ]
    }
}
partly success:
{
    "errno": "OK",
    "message": "Success",
    "data": {
        "orders": [
            {
                "custom_id": 1,
                "err": {
                    "http_err":405,
                    "err_code":"code",
                    "err_msg":"failed reason"
                }
            },
            {
                "custom_id": 2,
                "order_id": 10540014
            }
        ]
    }
}
fail:
{
    "errno": "FAILED",
    "message": "failed reason"
}
```

### Request Url    

`POST ifmarket/batchOrders`

### Params Body 

| Param  | Type  |  Example   | Description |
| :----: | :---: | :--------: | :---------: |
| orders | order |            |             |
| nonce  |  int  | 1545820222 | Stimestamp  |

### order  

|   Param    |  Type   | Example  |                       Description                        |
| :--------: | :-----: | :------: | :------------------------------------------------------: |
| stock_code | String  | BTC/USDT |                                                          |
|   price    | Decimal |   7293   |                          Price                           |
|    vol     | Decimal |   10.2   |                           Qty                            |
|    way     |   Int   |    2     |                       1:buy 2:sell                       |
|  category  |   Int   |    1     | 1:limit price order,2:market order,the price must be "0" |
|   nonce    |   int   |    3     |                                                          |

## Batch Cancel Order

> curl example

```curl
curl --location --request POST 'https://api.abit.com/v1/ifmarket/cancelOrders' \
--header 'abit-accesskey: 123123123213' \
--header 'abit-sign: MD5(postdata+secretKey+abit-ts)' \
--header 'abit-ver: 1008' \
--header 'abit-dev: api' \
--header 'abit-ts: 1541044393000000' \
--data-raw '{
    "orders":[
		{
		   "stock_code":"EOS/ETH",
		   "orders":[
				   10116356,
				   10116357
		    ]
		}
     ],
    "nonce":1531968125
}'
```

> Response：

```response-data
{
    "errno": "OK",
    "message": "Success",
    "data": {
        "succeed": [ 
            10116356,
            10116357
        ],
        "failed": null
    }
}
```

### Request Url    

`POST ifmarket/cancelOrders`

### Params Body 

| Param  |     Type     |                           Example                            | Description |
| :----: | :----------: | :----------------------------------------------------------: | :---------: |
| orders | cancelOrders | { 		   "symbol":"EOS/ETH", 		   "orders":[ 				   10116356, 				   10116357 		    ] 		} |             |
| nonce  |     int      |                          1545820222                          |             |

### cancelOrders说明

|   Param    |   Type   | Example  |   Description    |
| :--------: | :------: | :------: | :--------------: |
|   orders   | int list | [1,2,3]  | Cancel order ids |
| stock_code |  String  | BTC/USDT |                  |

## Get User Order

> curl example

```curl
curl --location --request GET 'https://api.abit.com/v1/ifmarket/getOrders?stockCode=EOS/ETH&status=2&offset=1&size=2' \
--header 'abit-ver: 1008' \
--header 'abit-dev: api' \
--header 'abit-sign: d1701e739a359ee4c7a5003fe39ef27065c019f687b982a6e98bd375a673ec42' \
--header 'abit-ts: 1532428054000000' \
--header 'abit-accesskey: 100039428965' \
--data-raw ''
```

> return data:

```response-data
{
    "errno": "OK",
    "message": "Success",
    "data": {
        "orders": [
            {
                "order_id": 10284160,
                "account_id":23249999,
                "stock_code": "EOS/ETH",
                "price": "8",
                "vol": "4",
                "done_vol": "0",
                "swap_vol": "0",
                "way": 1,
                "category": 1,
                "fee": "0",
                "fee_ratio": "0",
                "fee_coin_code": "0",
                "done_fee": "", 
                "created_at": "2018-07-17T07:24:13.410507Z",
                "finished_at": null,
                "status": 2,
                "errno": 0
            }
        ]
    }
}
```

### Request Url

`GET ifmarket/getOrders`

### Params

|   Param   |  Type  | Example  |                         Description                          |
| :-------: | :----: | :------: | :----------------------------------------------------------: |
| stockCode | String | BTC/USDT |                                                              |
|  status   |  int   |    2     | status,1: requested orders 2:requested and opened orders 3: completed orders   if status field is not transferred then orders under all statuses will be returned |
|  offset   |  int   |    0     |                                                              |
|   size    |  int   |    20    |                                                              |
|           |        |          |                                                              |

## Get User Balance

> curl example

```curl
curl --location --request POST 'https://api.abit.com/v1/ifaccount/users/vipMe' \
--header 'abit-ver: 1008' \
--header 'abit-dev: api' \
--header 'abit-sign: d1701e739a359ee4c7a5003fe39ef27065c019f687b982a6e98bd375a673ec42' \
--header 'abit-ts: 1532428054000000' \
--header 'abit-accesskey: 34234423sdfsfsfwerwerwerwrwerwer' \
--data-raw ''
```

> return data:

```response-data
{
    "errno": "OK",
    "message": "Success",
    "data": {
        "account_id": 100039428965,                 
        "email": "",                                
        "phone":"",                                 
        "account_type": 1,                          
        "status": 2,                               
        "asset_password_effective_time": -2,        
        "ga_key": "unbound",                        
        "kyc_type": 0,                              
        "kyc_status": 1,                            
        "user_assets":[
            {
                "coin_code":"",
                "freeze_vol":"",
                "available_vol":""
            }
        ]
        "created_at": "2018-04-08T04:04:25.26468Z",
        "updated_at": "2018-04-23T03:25:16.408297Z"
    }
}
```

### Request Url

`GET ifaccount/users/vipMe`



## Get User Order Detail

> curl example

```curl
curl --location --request POST 'https://api.abit.com/v1/ifmarket/queryOrder' \
--header 'abit-accesskey: 123123123213' \
--header 'abit-sign: MD5(secretKey+abit-ts)' \
--header 'abit-ver: 1008' \
--header 'abit-dev: api' \
--header 'abit-ts: 1541044393000000' \
--data-raw '{
	"stock_code":"ETH/USDT",
	"orders":[
		10000234234,
		10000343434
	]
}'
```

> return data:

```response-data
{
    "errno":"OK",
    "message":"Success",
    "data":{
        "orders":[
            {
                "order_id":2192079529,
                "stock_code":"XDAG/ETH",
                "price":"1.1",  // 订单申报价
                "vol":"1",      // 申报量  
                "done_vol":"1", // 成交量
                "swap_vol":"1.1",  // 交易额量
                "way":1,  // 订单方向,1:买,2:卖
                "category":1,  // 订单类型,1:限价,2:市价
                "fee":"0.0011", // 订单手续费冻结量
                "fee_ratio":"0.001", // 订单手续费比例
                "fee_coin_code":"ETH", // 订单手续费code
                "done_fee":"0.0011",  // 订单成交产生手续费
                "account_id":2000628412, // 用户id
                "created_at":"2019-02-28T10:28:14.01776Z",
                "finished_at":"2019-02-28T10:28:14.040836Z",
                "status":3, // 状态
                "errno":0 
            },
            {
                "order_id":2192079156,
                "stock_code":"XDAG/ETH",
                "price":"1.1",
                "vol":"1",
                "done_vol":"1",
                "swap_vol":"1.1",
                "way":1,
                "category":1,
                "fee":"0.0011",
                "fee_ratio":"0.001",
                "fee_coin_code":"ETH",
                "done_fee":"0.0011",
                "account_id":2000628412,
                "created_at":"2019-02-28T09:53:42.866192Z",
                "finished_at":"2019-02-28T09:53:42.884625Z",
                "status":3,
                "errno":0
            }
        ]
    }
}
```

### Request Url

`POST ifmarket/queryOrder`

### Params

|   Param    |  Type  | Example  |  Description  |
| :--------: | :----: | :------: | :-----------: |
| stock_code | String | BTC/USDT |               |
|   orders   |  int   | [1,23,4] | order id list |



## Get User Trade Records

> curl example

```curl
curl --location --request GET 'https://api.abit.com/v1/ifmarket/getOrders?stockCode=ETH/USDT&offset=0&size=0&status=0' \
--header 'abit-accesskey: 123123123213' \
--header 'abit-sign: MD5(secretKey+abit-ts)' \
--header 'abit-ver: 1008' \
--header 'abit-dev: api' \
--header 'abit-ts: 1541044393000000'
```

> return data:

```response-data
{
    "errno":"OK",
    "message":"Success",
    "data":{
        // trade records list, sorted by created time from latest to farest
        "trades":[
            {
                "oid":100039430677,
                "tid":100039430678,
                "stock_code":"BTC/ETH",
                "price":"0.06",
                "vol":"21",
                "fee":"0.0000000126",
                "fee_coin_code":"ETH",
                "created_at":"2018-04-19T03:27:21.62635069Z",
                "side":2,
                "change":"0"
            }
        ]
    }
}
```

### Request Url

`GET ifmarket/getOrders`

### Params

|   Param   |  Type  | Example  |            Description            |
| :-------: | :----: | :------: | :-------------------------------: |
| stockCode | String | BTC/USDT |                                   |
|  offset   |  int   |    0     |                                   |
|   size    |  int   |    20    |                                   |
|  status   |  int   |    0     | 0:all the status,2:pending,3:done |

## Get User Trade Record By Order

> curl example

```curl
curl --location --request GET 'https://api.abit.com/v1/ifmarket/getOrderTrades?stockCode=ETH/USDT&orderId=1000121232' \
--header 'abit-accesskey: 123123123213' \
--header 'abit-sign: MD5(secretKey+abit-ts)' \
--header 'abit-ver: 1008' \
--header 'abit-dev: api' \
--header 'abit-ts: 1541044393000000'
```

> return data:

```response-data
{
    "errno":"OK",
    "message":"Success",
    "data":{
       
        "trades":[
            {
                "order_id":100039430677,
                "trade_id":100039430678,
                "stock_code":"abit/ETH",
                "deal_price":"0.06",
                "deal_vol":"21",
                "fee":"0.0000000126",
                "fee_coin_code":"ETH",
                "created_at":"2018-04-19T03:27:21.62635069Z",
                "way":2,
                "fluctuation":"0"
            }
        ]
    }
}


```

### Request Url

`GET ifmarket/getOrderTrades`

### Params

|  Param  |  Type  | Example  |    Description     |
| :-----: | :----: | :------: | :----------------: |
| symbol  | String | BTC/USDT |  spot stock name   |
| orderid |  int   |    1     | query the order id |

# SPOT-RealTime(WebSocket)

### Address

- test environment `wss://devapi.abit.com/v1/ifspot/realTime`
- production environment `wss://api.abit.com/v1/ifspot/realTime`

### Command

- The sending format of basic command format `{"action":"","args":["arg1", "arg2", "arg3"]}`  eg. the command for subscribing the tickers of EOS/ETH,USDT/BTC spot trading pair {"action":"subscribe","args":["Ticker:EOS/ETH", "Ticker:USDT/BTC"]}   Client end unsubscribes the tickers of these two pairs {"action":"unsubscribe","args":["Ticker:EOS/ETH", "Ticker:USDT/BTC"]}

- Returning format of command

  ```undefined
  {
    "action":"<command>",
    "success":true,         // success-true, failed-false
    "group":"<group>",
    "request":{
        // Original Request Url
    },
    "error":""  // error reason will be returned via this field when failed
  }
  ```

- Supported command list

  ```
  subscribe   // subscribe
  unsubscribe // unsubscribe
  ```

### Theme

- General theme list (no authorization verification required)

  ```undefined
  The following are open themes, no ws verification required.
  Trade       //latest dealed
  Ticker      //real time price实时价格
  OrderBook   //depth
  QuoteBin1m  //1 minute quote data
  QuoteBin5m  //5 minutes quote data
  QuoteBin30m //30 minutes quote data
  QuoteBin1h  //1 hour quote data
  QuoteBin2h  //2 hours quote data
  QuoteBin4h  //4 hours quote data
  QuoteBin6h  //6 hours quote data
  QuoteBin12h //12 hours quote data
  QuoteBin1d  //daily quote data
  QuoteBin1w  //weekly quote price
  IndexBin1m  //1 minute index quote data
  IndexBin5m  //5 minutes index quote data
  IndexBin30m //30 minutes index quote data
  IndexBin1h  //1 hour index quote data
  IndexBin2h  //2 hours index quote data
  IndexBin4h  //4 hours index quote data
  IndexBin6h  //6 hours index quote data
  IndexBin12h //12 hours index quote data
  IndexBin1d  //daily index quote data
  IndexBin1w  //weekly index quote data
  ```

- Instructions

1. Request Url subscribe command，composition of themes in the theme list is <theme: code of spot trading pairs (case sensitive)>, such as the command for subscribing the spot trading pair EOS/ETH's real time depth and 5 minutes quote data is `{"action":"subscribe","args":["OrderBook:EOS/ETH","QuoteBin5m:EOS/ETH"]}`

### Format of subscribed data

- Basic format

  ```undefined
  {
    "group":"",
    "action":1,
    "data":{
    }
  }
  // When subscribed theme is different, data field format is different. Details of data rely on api return，obtained by entering corresponding theme command in Request Url
  action: indicates the type of updated data
  1:full data update
  2:changed data update
  3:inserted data update
  4:deleted data update
  ```

### Verification

```undefined
if need to subscribe information such as data related to user's self-information, etc., then there are 2 ways to do ws verification.
1.verification through username and password
// refer to Api Sign calculation method (the sequence of parameters is important, please don't mess up with the sequence参考Api)
{
    "action":"access",
    "args":[
        "uid",       // user ID, must be string
        "web",              // device type.same as Dev(Ex-Dev), must be string
        "1.0.0",            // client end version number, Version(Ex-Ver）, must be string
        "Sign",              // encrypted string, same as Sign(Ex-Sign), refer to Api Sign calculation method, must be string
        "1540286461000000",  // same as Ts(Ex-Ts)  unit:microsecond, must be string
    ] 
}
2.verification through accessKey
// refer to Api Sign Calcultation methode. The sequence of parameters is very important. Please don't mess up with the sequence.)
{
    "action":"access",
    "args":[
        "Accesskey",       // Accesskey of user,must be string
        "api",              // Dev(Ex-Dev), must be string
        "1.0.0",            // Version(Ex-Ver）, must be sting
        "Sign",              // Sign(Ex-Sign), must be string
        "1540286461000000",  // Ts(Ex-Ts), unit:microsecond, must be string
    ] 
}


parameter value of Sign is: md5(sercet_key+Ts(string))
```

### Subscribe Private Data

```undefined
// when subscription is completed (verification passed), private data of UserProperty theme can be received. Currently there is only one UserProperty theme. All private data will be returned in this theme.
{
    "action":"subscribe",
    "args":["UserProperty"]
}

// UserProperty theme,the format of returned data
{
    "group":"UserProperty",
    "data":[
        {
            "action":1, // operation type
            "order":{   // order information

            },
            "s_assets":[  // spot asset list

            ],
            "c_assets":{  // contract asset

            }
        }
    ]
}
"group":"UserProperty", indicating the user's spot trading data. Driven by back-end operation. Push user data updates. One push may include multiple business operations, so data is in the form of an array, starting from the head of the array, a set of user operations stored in order of operations. The elements of each group of data includes: action(operation type), order(order information), s_assets(spot asset information, note it is the array of spot assets), c_assets(contract asset information). For order information, spot asset information, contract asset information, only when the operation of these information has generated updates, can each set of data elements contain that information. 
action: operation types include
1 :matching
   possible impact: order update, position update, contract asset update
2 :submit order
   possible impact: order update, contract asset update
3 :cancel order
   possible impact: order update, position update, contract asset update
11 :transfer from contract asset to spot asset
   possible impact:contract asset update, spot asset update
12 :trasfer from spot asset to contract account
```

### Heartbeat

- The server will send a PingFrame to client end every 10 seconds. Under normal circumstances, the client end will reply with a PongFrame. If the server does not receive a response for 5 consecutive PingFrames, and during this period, no other data from the client is received, the server will actively disconnect the link. After receiving PingFrame, most browsers will automatically respond with PongFrame without the need of business layer implementation. 

- The server implements a PingMessage Handle in the business layer. After receiving the PingMessage, it will automatically reply with a PongMessage. If the bottom layer of the client end has no way to handle the ping/pong frame of the websocket protocol layer, the ping/pong message of the business layer can determine whether the link is healthy. The specific message is as follows.

  ```undefined
  // ping
  {"action":"ping"}
  // pong
  {"group":"System","data":"pong"}
  ```

### Testing

- Websocket client ends of any standards can be used for testing

- Chrome websocket test plugin

  ```undefined
  https://chrome.google.com/webstore/search/WebSocket%20Test%20Client?utm_source=chrome-ntp-icon
  ```

