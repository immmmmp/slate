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



For authenticated requests, the following headers should be sent with the request:

.The encode string of MD5 :"{Body}+{secret_key}+{ts}".

- Body:   the params required by the interface,which is transfromed to JSON string .
- secret_key: your secret_key apply from the system.
- ts:Number of milliseconds since Unix epoch



|    param     |  type  |                         description                          |
| :----------: | :----: | :----------------------------------------------------------: |
| EX-Accesskey | String |                         Your api key                         |
|    EX-Ts     |  Long  |           Number of milliseconds since Unix epoch            |
|   EX-Sign    | String | MD5 of the encode  string, using your API secret, as a hex string: |



# Contract Public Info-REST

REST endpoint URL: **https://host.com/**

## Obtain Data of Index Candlestick Chart

> curl request example:

```curl
curl --location --request GET 'https://host.com/swap/indexkline?indexID=11&startTime=1533686400&endTime=1533688400&unit=5&resolution=M'
```

> 返回数据:

```response-data
{
    "errno": "OK",
    "message": "Success",
    "data": [
        {
            "low": "130", // the lowest price in the unit
            "high": "130", // the highest price in the unit
            "open": "130", // the open price in the unit
            "close": "130", // the close price in the unit
            "last_px": "130", // the last price in the unit
            "avg_px": "130", // the avg price price in the unit
            "qty": "0", 
            "timestamp": 1532610000, 
            "change_rate": "0", 
            "change_value": "0" 
        }
    ]
}
```

### Request Url

`GET swap/indexkline`

### Params


|   param    |  type  |  example   |                         description                          |
| :--------: | :----: | :--------: | :----------------------------------------------------------: |
|  indexID   | String |     11     |                           index id                           |
| startTime  | String | 1533686400 |                                                              |
|  endTime   | String | 1533686400 |                                                              |
|    unit    |  Int   |     5      | 选择的时间跨度的长度，在示例中表示为5M，即5分钟为每根蜡烛图的时间跨度 |
| resolution | String |     M      |         表示获取K线数据的频率类型,M:分钟,H:小时;D:天         |

## Obtain Data of Contract Trading Records

> curl request example:

```curl
curl --location --request GET 'https://host.com/swap/trades?instrumentID=1'
```

> Response：

```
{
    "errno": "OK",
    "message": "Success",
    "data": {
        "trades": [
            {
                "oid": 10116361,        // taker order id 
                "tid": 10116363,        // trade id
                "instrument_id": 1,     
                "px": "16",   					
                "qty": "10",     				
                "make_fee": "0.04",   	// make fee
                "take_fee": "0.12",   	// take fee
                "created_at": null,   	
                "side": 5,             	// trade order side ，
                													the trade order is consisted of two side order.
             
             1 //buy order:taker,order side：open long buy,open short sell
             2 //buy order:taker,order side：open long buy,close long sell 
             3 //buy order:taker,order side：：close short buy , open short sell
             4 //buy order:taker,order side：：close short buy , close long sell
             5 //sell order:taker,order side：：open short buy,open long sell
             6 //sell order:taker,order side：：开空卖 平空买
             7 //sell order:taker,order side：：平多卖 开多买
             8 //sell order:taker,order side：：平多卖 平空买
                "change": "0"    				// 对行情的影响
                										如本次交易前的最新交易价yes10,本次交易的交易价yes11,则change为"1"
            }
        ]
    }

```

### Request Url

 `GET swap/trades`

### Params

|    param     |  type  | example | description |
| :----------: | :----: | :-----: | :---------: |
| instrumentID | String |   11    |             |



## Obtain ADL Sequence of Contract Position

>curl request example:

```curl
curl --location --request GET 'https://host.com/swap/pnls?instrumentID=1'
```

>Response：

```
{
    "errno": "OK",
    "message": "Success",
    "data": {
// 合约自动减仓排序表
"pnls": [
{
"instrument_id": 1,
"long_pnls": [
    {
        // pnl range :min~max
        "min_pnl": "-0.0379107725788900979",
        "max_pnl": "-0.0103298131866111136",
        "quan_tile": 40 // quan_tile lager,the possibility of close position lager					
    },
    {
        "min_pnl": "-0.0103298131866111136",
        "max_pnl": "0",
        "quan_tile": 60
    }
],

"short_pnls": [
    {
        "min_pnl": "-49962.3980439671407168788",
        "max_pnl": "0",
        "quan_tile": 20
    }
]
            }
        ]
    }
}
```

### Request Url 

`GET swap/pnls`

### Params

|    param     |  type  | example | description |
| :----------: | :----: | :-----: | :---------: |
| instrumentID | String |   11    |             |






## Obtain Index List

###Request Url 

`GET swap/indexes`

### Params

无

>curl request example:

```curl
curl --location --request GET 'https://host.com/swap/indexes'
```

>Response：

```
{
    "errno": "OK",
    "message": "Success",
    "data": [
        {
            "index_id": 1, 
            "name": "BTC", 
            "base_coin": "BTC", 
            "quote_coin": "USDT", 
            "brief_en": "",
            "brief_zh": "", 
            "px_unit": "0.000001", // price decimal
            "created_at": "2018-07-30T16:04:08Z",
          
            "members": [
                {
                    "index_id": 11,
                    "coin_code": "BTC",
                    "weight": "1",
                    "market": [
                        {
                            "market_name": "Bitstamp",
                            "weight": "1"
                        },
                        {
                            "market_name": "Coinbase.Pro",
                            "weight": "1"
                        }
                    ]
                }
            ],

                {

                }
            ]
        }
    ]
}
```

## Obtain Contract Depth of a Coin

>curl request example:

```curl
curl --location --request GET 'https://host.com/swap/depth?instrumentID=1&count=10'
```

>Response：

```
{
    "errno": "OK",
    "message": "Success",
    "data": {
      
        "asks": [
            [
                80000, //key
                "8",   // price
                "1",   // qty
                0      // 0:user order,1:system order
            ]...
        ],
        
        "bids": [
            [
                74000, 
                "7.4", 
                "1",   
                0
            ]...
        ]
    }
}
```

###Request Url

`GET swap/depth`

###Params

|    param     |  type  | example | description |
| :----------: | :----: | :-----: | :---------: |
| instrumentID | String |   11    |             |
|    count     |  Int   |   10    |   option    |



## Obtain Contract Info

>curl request example:

```curl
curl --location --request GET 'https://host.com/swap/instruments?instrumentID=11'
```

>Response：

```
{
    "errno": "OK",
    "message": "Success",
    "data": {
        "instruments": [
            {
                "instrument_id": 7,                         
                "index_id": 16,															
                "symbol": "BCHUSDT",												
                "name_zh": "BCHUSDT永续合约",                
                "name_en": "BCHUSDT SWAP",									
                "base_coin": "BCH",													
                "quote_coin": "USDT",												
                "margin_coin": "USDT",											
                "is_reverse": false,                        
                "market_name": "",                          
                "face_value": "0.001",                      //the unit is the base coin 
                "begin_at": "2018-09-29T04:00:00Z",         //
                "settle_at": "2019-09-08T12:00:00Z",        //
                "settlement_interval": 28800,               //
                "min_leverage": "1.0",                      
                "max_leverage": "50.0",                     
                "position_type": 3,                       	//1:cross,2:fixed,3:both
                "px_unit": "0.01",                       		
                "qty_unit": "1",                       			
                "value_unit": "0.0001",                     
                "min_qty": "1",                     			  
                "max_qty": "100000",                        
                "underweight_type": 1,                      // 1:ADL ,2:avg pnl
                
                
                "status": 3,                      				  //
																																3  // available
																																4  // suspend
																																5  // settling
																																6  // settled
																																
                "area": 2,                       						//1:USDT区,2:主区,3:创新区,4:模拟区
                "created_at": "2018-09-29T10:02:42Z",       
                "depth_round": "1.001",                     
                "base_coin_zh": "比特现金",                  
                "base_coin_en": "BCH",                     
                "max_funding_rate": "0",                    
                "min_funding_rate": "0",                    
                "risk_limit_base": "100000",                
                "risk_limit_step": "50000",                 
                "mmr": "0.005",                             
                "imr": "0.01",                              
                "maker_fee_ratio": "0.00025",               //maker fee
                "taker_fee_ratio": "0.00075",               //taker fee
                "settle_fee_ratio": "0",                    
                "plan_order_price_min_scope": "0",          
                "plan_order_price_max_scope": "0",          
                "plan_order_max_count": 0,                  
                "plan_order_min_life_cycle": 0,             
                "plan_order_max_life_cycle": 0              
            }
        ]
    }
}


```


###Request Url

`GET swap/instruments`


###Params

|    param     |  type  | example | description |
| :----------: | :----: | :-----: | :---------: |
| instrumentID | String |   11    |             |




## Obtain Single Contract Info

>curl request example:

```curl
curl --location --request GET 'https://host.com/swap/fundingrate?instrumentID=11'
```

>Response：

```
{
    "errno": "OK",
    "message": "Success",
    "data": [
        {
            "rate": "-0.0060483184843194741", // refund fee rate
            "timestamp": 1534320000 
        },
        {
            "rate": "0.1041766553400505803",
            "timestamp": 1534291200
        }
    ]
}
```


###Request Url

`GET swap/fundingrate`

###Params

|    param     |  type  | example | description |
| :----------: | :----: | :-----: | :---------: |
| instrumentID | String |   11    |             |




## Obtain Data of Contract Candlestick Chart



>curl request example:

```curl
https://host.com/swap/kline?instrumentID=1&startTime=1532610072&endTime=1532656524&unit=5&resolution=M

```

>Response：

```
ENVIRONMENT LAYOUT 
LANGUAGE 
Public
{
    "errno": "OK",
    "message": "Success",
    "data": [
        {
            "low": "130", 				
            "high": "130", 				
            "open": "130", 				
            "close": "130", 			
            "last_px": "130", 		
            "avg_px": "130", 			
            "qty": "10", 					
            "base_coin_qty":"163.9", 				
            "quote_coin_qty":"34555.3812", 	
            "timestamp": 1532610000, 				
            "change_rate": "0", 						
            "change_value": "0" 						
        }
    ]
}
```

###Request Url

`GET swap/kline`


###Params  

|   param    |  type  |  example   |                         description                          |
| :--------: | :----: | :--------: | :----------------------------------------------------------: |
|  indexID   | String |     11     |                                                              |
| startTime  |   ts   | 1533686400 |                                                              |
|  endTime   |   ts   | 1533688400 |                                                              |
|    unit    |  Int   |     5      | 选择的时间跨度的长度，在示例中表示为5M，即5分钟为每根蜡烛图的时间跨度 |
| resolution | String |     M      |         表示获取K线数据的频率类型,M:分钟,H:小时;D:天         |




## Obtain Data of Contract Ticker

###Request Url    

`GET swap/tickers`


###Params

|    param     |  type  | example | description |
| :----------: | :----: | :-----: | :---------: |
| instrumentID | String |   11    |             |



>curl request example:

```curl
curl --location --request GET 'https://host.com/swap/tickers?instrumentID=1'
```

>Response：

```
{
    "errno": "OK",
    "message": "Success",
    "data": {
        "tickers": [
            {
                "last_px": "6277.5", 					// last price
                "open": "6074.5", 						
                "close": "6277.5", 						
                "low": "6261.3", 							
                "high": "6279", 							
                "avg_px": "6274.67",					
                "last_qty": "20000", 					
                "qty24": "45462", 					
                "base_coin_qty":"163.9", 			
                "quote_coin_qty":"34555.381", 
                "timestamp": 1534315695, 			
                "change_rate": "0.033", 			
                "change_value": "203", 				
                "instrument_id": 1, 					
                "position_size": "374266", 	
                "qty_day": "45270", 					
                "amount24":"28520.77363", 		
                "index_px": "6406.53", 				
                "fair_basis": "0.000000690", 	
                "fair_value": "0.00442673", 	
                "fair_px": "6406.5344267", 		
                "rate": {
                    "quote_rate": "0.0003", 	
                    "base_rate": "0.0006", 		
                    "interest_rate": "-0.00009999" 
                },
                "premium_index": "-0.0309530479798782534", 	
                "funding_rate": "0.0001", 									
                "next_funding_rate": "-0.0304530", 				
                "next_funding_at": "2018-08-15T08:00:01Z" 
            }
        ]
    }
}
```

# Contract Trading API

## Cancel Contract Order

### Request Url    

`POST swap/cancelOrders`

### Params 

|    param     |   type   |  example  | description |
| :----------: | :------: | :-------: | :---------: |
| instrumentID |  String  |    11     |             |
|    orders    | int list | [1,2,3,4] |             |

> curl request example:

```curl
curl --location --request GET 'https://host.com/swap/tickers?instrumentID=1'
```

> Response：

```response-data
{
    "errno": "OK",
    "message": "Success",
    "data": {
        "tickers": [
            {
                "last_px": "6277.5", 					
                "open": "6074.5", 						
                "close": "6277.5", 						
                "low": "6261.3", 							
                "high": "6279", 							
                "avg_px": "6274.67",					
                "last_qty": "20000", 					
                "qty24": "45462", 						
                "base_coin_qty":"163.9", 			
                "quote_coin_qty":"34555.381", 
                "timestamp": 1534315695, 			
                "change_rate": "0.033", 			
                "change_value": "203", 				
                "instrument_id": 1, 					
                "position_size": "374266", 		
                "qty_day": "45270", 					
                "amount24":"28520.77363", 		
                "index_px": "6406.53", 				
                "fair_basis": "0.000000690", 	
                "fair_value": "0.00442673", 	
                "fair_px": "6406.5344267", 	
                "rate": {
                    "quote_rate": "0.0003", 	
                    "base_rate": "0.0006", 		
                    "interest_rate": "-0.00009999" 
                },
                "premium_index": "-0.0309530479798782534", 	
                "funding_rate": "0.0001", 									
                "next_funding_rate": "-0.0304530", 					
                "next_funding_at": "2018-08-15T08:00:01Z" 	
            }
        ]
    }
}
```



## Batch Order Submission



> curl request example:

```curl
curl --location --request POST 'https://host.com/swap/batchOrders' \
--header 'EX-Accesskey: 123123123213' \
--header 'EX-Sign: MD5(postdata+secretKey+EX-Ts)' \
--header 'EX-Ver: 1008' \
--header 'EX-Dev: WEB' \
--header 'EX-Ts: 1541044393000000' \
--data-raw '{
	"orders":[
		{
		   "instrument_id":3,
		   "category":1,
		   "client_id":1,
		   "side":4,
		   "position_type":1,
		   "leverage":100,
		   "px":"998",
		   "qty":"10",
		   "hide_qty":"0"
		},
		{
		   "instrument_id":3,
		   "category":1,
		   "client_id":2,
		   "side":4,
		   "position_type":1,
		   "leverage":100,
		   "qty":"10",
		   "hide_qty":"0"
		}
	],
   "nonce":1533876299
}'
```

> Response   ：

```response-data
#成功
{
  "errno": "OK",
  "message": "Success",
  "data": {
      "orders": [
          {
              "client_id": 1,
              "oid": 10540013
          },
          {
              "client_id": 2,
              "oid": 10540014
          }
      ]
  }
}
#失败
{
  "errno": "OK",
  "message": "Success",
  "data": {
      "orders": [
          {
              "client_id": 1,
              "err": {
                  "http_err":405,
                  "err_code":"LIQUIDATE_ORDER",
                  "err_msg":"订单将触发强平"
              }
          },
          {
              "client_id": 2,
              "oid": 10540014
          }
      ]
  }
}
```


### Request Url    

`POST swap/batchOrders`

### Request Body 

| param  | type  |                           example                            | description |
| :----: | :---: | :----------------------------------------------------------: | :---------: |
| orders | order | [{"instrument_id":3,"category":1,"client_id":1,"side":4,"position_type":1, "leverage":100,"px":"998","qty":"10","hide_qty":"0"},...] | Order list  |
| nonce  |  int  |                          1533876299                          |             |

### order

| param         | type     | example | description                                          | is required |
| ------------- | -------- | ------- | ---------------------------------------------------- | ----------- |
| Instrument_id | Int      | 3       | 合约id                                               | yes         |
| category      | Int      | 1       | 1:limit price,2:market price                         | yes         |
| client_id     | int      | 1       |                                                      | yes         |
| side          | int      | 1       | 1:open long ,2:open short,3:close long,4:close short | yes         |
| position_type | 开仓方式 | 1       | 1:fixed,2:cross                                      | yes         |
| leverage      | decimal  | 10      |                                                      | yes         |
| px            | decimal  |         | Price                                                | yes         |
| qty           | decimal  |         |                                                      | yes         |

### Header

| param        | type   | example          | description                         | is required |
| ------------ | ------ | ---------------- | ----------------------------------- | ----------- |
| Content-Type | String | application/json | 定值                                | yes         |
| EX-Accesskey | String |                  |                                     | yes         |
| EX-Sign      | int    |                  | MD5({postdata}+{secretKey}+{EX-Ts}) | yes         |
| EX-Ver       | int    | 1008             | 定值                                | yes         |
| EX-Dev       | String | WEB              | 定值                                | yes         |
| EX-Ts        | Long   | 1541044393000000 | utc时间戳-微妙                      | yes         |



## Single Order Submission

> curl request example:

```curl
curl --location --request POST 'https://host.com/swap/submitOrder' \
--header 'EX-Ver: 1.0.0' \
--header 'EX-Dev: web' \
--header 'EX-Sign: d1701e739a359ee4c7a5003fe39ef27065c019f687b982a6e98bd375a673ec42' \
--header 'EX-Ts: 1532428054000000' \
--header 'EX-Uid: 100039428965' \
--data-raw '{
   "instrument_id":3,
   "category":1,
   "side":4,
   "position_type":1,
   "leverage":100,
   "px":998,
   "qty":10,
   "nonce":1533873979
}'
```

> Response   ：

```response-data
#成功
{
  "errno": "OK",
  "message": "Success",
  "data":
          {
              "client_id": 1,
              "oid": 10540013
          }
}
```

### Request Url    

`POST swap/submitOrder`

### Request Body 

| param         | type     | example    | description                                          | is required |
| ------------- | -------- | ---------- | ---------------------------------------------------- | ----------- |
| Instrument_id | Int      | 3          |                                                      | yes         |
| category      | Int      | 1          |                                                      | yes         |
| client_id     | int      | 1          |                                                      | yes         |
| side          | int      | 1          | 1:open long ,2:open short,3:close long,4:close short | yes         |
| position_type | 开仓方式 | 1          | 1:fixed,2:cross                                      | yes         |
| leverage      | decimal  | 10         |                                                      | yes         |
| px            | decimal  |            |                                                      | yes         |
| qty           | decimal  |            |                                                      | yes         |
| nonce         | int      | 1533873979 | 当前时间戳                                           | yes         |



## Obtain Contract Account Info

> curl request example:

```curl
curl --location --request GET 'https://host.com/swap/accounts?coinCode=USDT' \
--header 'EX-Accesskey: 123123123213' \
--header 'EX-Sign: MD5({secretKey}+{EX-Ts})' \
--header 'EX-Ver: 1008' \
--header 'EX-Dev: WEB' \
--header 'EX-Ts: 1541044393000000'
```

> Response   ：

```response-data
#成功
合约账号列表
"accounts": [
            {
                "account_id": 10, 
                "coin_code": "USDT", 
                "freeze_vol": "1201.8", 
                "available_vol": "8397.65", 
                "cash_vol": "0", 
                "realised_vol": "-0.5", 
                "earnings_vol": "-0.5",
                "created_at":  // 创建时间 "2018-07-13T16:48:49+08:00",
                "updated_at":  // 修改时间 "2018-07-13T18:34:45.900387+08:00"
            }
        ]
```

### Request Url    

` GET swap/accounts`

### Request Params

> params pass by urlparam  

| param         | type   | example | description | is required |
| ------------- | ------ | ------- | ----------- | ----------- |
| instrument_id | Int    | 3       |             | no          |
| coinCode      | String | USDT    |             | no          |



## Obtain Contract Position of User

> curl request example:

```curl
curl --location --request GET 'https://host.com/swap/userPositions?coinCode=USDT&status=3&offset=1&size=0' \
--header 'EX-Accesskey: 123123123213' \
--header 'EX-Sign: MD5(secretKey+EX-Ts)' \
--header 'EX-Ver: 1008' \
--header 'EX-Dev: WEB' \
--header 'EX-Ts: 1541044393000000'
```

> Response   ：

```response-data
#成功
{
    "errno": "OK",
    "message": "Success",
    "data": {
        "positions": [
            {
                "pid": 10116365,              
                "uid": 10,										
                "instrument_id": 1,
                "cur_qty": "10",							
                "freeze_qty": "0",						
                "close_qty": "0",							
                "avg_cost_px": "16",					
                "avg_open_px": "16",          
                "avg_close_px": "0",         
                "oim": "100.075",             //origin open  position margin
                "im": "100.075",							//open position margin
                "mm": "50",                   //margin
                "realised_pnl": "-0.075",     
                "earnings": "-0.075",         
                "tax": "0",                   //refund fee cost
                "position_type": 1,           
                "side": 2,                    
                "status": 1,                  //状态  1:hold position  2:system  4:closed
                "errno": 0,                   //close position reason
                															1:closing position
																							2:breaking up 
																							3:closed position
																							4:broke up
																							5:boom position
																							6:auto close position(positive)
																							7:auto close position(negative)
                "created_at": "2018-07-17T03:04:26.108983Z",
                "updated_at": "2018-07-17T03:04:26.098404Z"
            }
        ]
    }
}
```

### Request Url    

` GET swap/userPositions`

### Request Params

> params pass by urlparam  

| param    | type   | example | description                                           | is required |
| -------- | ------ | ------- | ----------------------------------------------------- | ----------- |
| coinCode | String | USDT    |                                                       | yes         |
| status   | int    | 1       | 1:holding position 2:system  4:already close position | yes         |
| offset   | int    | 1       |                                                       | yes         |
| size     | int    | 0       |                                                       | no          |



## Obtain Funding Cost of Position

> curl request example:

```curl
curl --location --request GET 'https://host.com/swap/positionTax?instrumentID=32785&pid=2154205116' \
--header 'EX-Accesskey:  a3f056e5-0041-43c3-a9df-574b25a2ab03' \
--header 'EX-Dev: api' \
--header 'EX-Ts: 1547626071000000' \
--header 'EX-Ver: 1008' \
--header 'EX-Sign: 56ea9ec5355d1dde381e1296bda42db0'
```

> Response   ：

```response-data
{
    "errno": "OK",
    "message": "Success",
    
    "data": [
        {
            "instrument_id": 32785,
            "uid": 2071026819, 
            "pid": 2154205116, // position id
            "fair_px": "0.849940605285", 
            "funding_rate": "0",
            "tax": "-149.8912195248", 
            "qty": "1053468", 
            "created_at": "2019-01-15T00:00:00.708979Z" 
        },
        {
            "instrument_id": 32785,
            "uid": 2071026819,
            "pid": 2154205116,
            "fair_px": "0.849940605285",
            "funding_rate": "0",
            "tax": "-261.432832752",
            "qty": "1053468",
            "created_at": "2019-01-14T16:00:01.183355Z"
        }
    ]
}
```

### Request Url    

` GET swap/positionTax`

### Request Params

> params pass by urlparam  

| param        | type | example    | description | is required |
| ------------ | ---- | ---------- | ----------- | ----------- |
| instrumentID | Int  | 2          |             | yes         |
| pid          | int  | 2154205116 |             | yes         |



## Obtain Transaction History of User

> curl request example:

```curl
curl --location --request GET 'https://host.com/swap/userTrades?instrumentID=1&offset=0&size=100' \
--header 'EX-Accesskey: 123123123213' \
--header 'EX-Sign: MD5(secretKey+EX-Ts)' \
--header 'EX-Ver: 1008' \
--header 'EX-Dev: WEB' \
--header 'EX-Ts: 1541044393000000'
```

> Response   ：

```response-data
{
    "errno": "OK",
    "message": "Success",
    "data": {
        "trades": [
            {
                "oid": 10116361, 					// taker order id
                "tid": 10116363, 					// trade id
                "instrument_id": 1,     	
                "px": "16",   						
                "qty": "10",     					
                "make_fee": "0.04",   		
                "take_fee": "0.12",   		
                "created_at": null,   		
                "side": 5,             		
                "change": "0"    					
            }
        ]
    }
}
```

### Request Url    

` GET swap/userTrades`

### Request Params

> params pass by urlparam  

| param        | type | example | description | is required |
| ------------ | ---- | ------- | ----------- | ----------- |
| instrumentID | Int  | 2       |             | yes         |
| offset       | Int  | 0       |             | yes         |
| size         | int  | 60      | default 60  | no          |



## Add or Decrease Isolated Margin

> curl request example:

```curl
curl --location --request POST 'https://host.com/swap/changeMargin' \
--header 'EX-Accesskey:  a3f056e5-0041-43c3-a9df-574b25a2ab03' \
--header 'EX-Dev: api' \
--header 'EX-Ts: 1547626071000000' \
--header 'EX-Ver: 1008' \
--header 'EX-Sign: 56ea9ec5355d1dde381e1296bda42db0' \
--data-raw '{
   "pid":10539974,
   "instrument_id":1,
   "vol":10,
   "side":1,
   "nonce":1533871871
}'
```

> Response   ：

```response-data
？
```

### Request Url    

` POST swap/changeMargin`

### Request Params

> params pass by urlparam  

| param         | type | example    | description      | is required |
| ------------- | ---- | ---------- | ---------------- | ----------- |
| pid           | Int  | 2          |                  | yes         |
| instrument_id | Int  | 0          |                  | yes         |
| side          | int  | 1          | 1:add,2:decrease | yes         |
| vol           | int  | 10         |                  | yes         |
| nonce         | int  | 1533871871 |                  | yes         |



## User Order Records

> curl request example:

```curl
curl --location --request GET 'https://host.com/swap/userOrders?instrumentID=1&offset=0&size=0&status=0' \
--header 'EX-Accesskey: 123123123213' \
--header 'EX-Sign: MD5(secretKey+EX-Ts)' \
--header 'EX-Ver: 1008' \
--header 'EX-Dev: WEB' \
--header 'EX-Ts: 1541044393000000'
```

> Response   ：

```response-data
{
    "errno": "OK",
    "message": "Success",
    "data": {
        "orders": [
            {
                "oid": 10539098, 
                "instrument_id": 1, 
                "pid": 10539088, 
                "uid": 10, 
                "px": "16", 
                "qty": "1", 
                "hide_qty": "0", 
                "avg_px": "16", 
                "cum_qty": "1", 
                "side": 3,   
                "category": 1, 
                "make_fee": "0.00025", 
                "take_fee": "0.012", 
                "origin": "",
                "created_at": "2018-07-23T11:55:56.715305Z",
                "finished_at": "2018-07-23T11:55:56.763941Z",
                "status": 4, 
                "errno": 0,  /
                "position_type": 2, 
                "time_in_force": 1, 
                "mfr": "0.00025", 
                "tfr": "0.00075", 
                "self_mfr": "0.00025", 
                "self_tfr": "0.00075", 
                "leverage": "10", 
                "client_id": 123123, 
            }
        ]
    }
}
```

### Request Url    

` GET swap/userOrders`

### Request Params

> params pass by urlparam  

| param         | type | example | description                                                  | is required |
| ------------- | ---- | ------- | ------------------------------------------------------------ | ----------- |
| offset        | Int  | 2       |                                                              | yes         |
| instrument_id | Int  | 0       |                                                              | yes         |
| size          | int  | 1       |                                                              | yes         |
| status        | int  | 10      | 1:apply 2:sending 4:done ,3:include sending and done order,0:all | yes         |



## Query Order Details

> curl request example:

```curl
curl --location --request GET 'https://host.com/swap/queryOrder?instrumentID=1&oid=10539098' \
--header 'EX-Accesskey: 123123123213' \
--header 'EX-Sign: MD5(secretKey+EX-Ts)' \
--header 'EX-Ver: 1008' \
--header 'EX-Dev: WEB' \
--header 'EX-Ts: 1541044393000000'
```

> Response   ：

```response-data
{
    "errno": "OK",
    "message": "Success",
    "data": {
        // 订单信息
        "orders": [
            {
                "oid": 10539098, 
                "instrument_id": 1, 
                "pid": 10539088, 
                "uid": 10, 
                "px": "16", 
                "qty": "1", 
                "hide_qty": "0", 
                "avg_px": "16", 
                "cum_qty": "1", 
                "side": 3,   
                "category": 1, //  
                                  该字段采用二进制按位表示法
                                  0~5位表示订单的基本类型,第6位预留,
                                  第7位为1表示:强平委托单
                                  第8位为1表示:爆仓委托单
                                  第9位为1表示:自动减仓委托单
                "make_fee": "0.00025", // make fee
                "take_fee": "0.012", // take fee
                "origin": "",
                "created_at": "2018-07-23T11:55:56.715305Z",
                "finished_at": "2018-07-23T11:55:56.763941Z",
                "status": 4, // 状态
                "errno": 0,  // 订单结束原因
                								1:user cancel
																2:over time limit
																3:don't have enough balance when place order
                                4:not enough freeze balance
                                5:system order to cancel
                                6:a part of close positioin order 
                                7:auto close position order to cancel
             
                                10:invalid
                                
                "position_type": 2, // 1:fixed marigin,2:cross margin
                "time_in_force": 1, // 1:normal,2:FOK,3:IOC
                "mfr": "0.00025", // system maker fee
                "tfr": "0.00075", // system taker fee
                "self_mfr": "0.00025", // user maker fee 
                "self_tfr": "0.00075", // user taker fee
                "leverage": "10", ,
                "client_id": 123123, 
            }
        ]
    }
}
```

### Request Url    

` GET swap/queryOrder`

### Request Params

> params pass by urlparam  

| param         | type | example | description | is required |
| ------------- | ---- | ------- | ----------- | ----------- |
| instrument_id | Int  | 0       |             | yes         |
| oid           | Int  | 0       |             | yes         |



## Query Single Transaction History

> curl request example:

```curl
curl --location --request GET 'https://host.com/swap/orderTrades?instrumentID=1&orderID=2064344648'
```

> Response   ：

```response-data
{
    "errno": "OK",
    "message": "Success",
    "data": {
 
        "trades": [
            {
                "oid": 10116361, 			// taker order id
                "tid": 10116363, 			// trade id
                "instrument_id": 1,   
                "px": "16",         	
                "qty": "10",        	
                "make_fee": "0.04",   // make fee
                "take_fee": "0.12",   // take fee
                "created_at": null,   
                "side": 5,            
                "change": "0"    			//influence to the market
            }
        ]
    }
}
```

### Request Url    

` GET swap/orderTrades`

### Request Params

> params pass by urlparam  

| param         | type | example  | description | is required |
| ------------- | ---- | -------- | ----------- | ----------- |
| instrument_id | Int  | 0        |             | yes         |
| orderID       | int  | 20644322 |             | yes         |



## Create Contract Account

> curl request example:

```curl
curl --location --request GET 'https://host.com/swap/createAccount?instrumentID=1' \
--header 'EX-Accesskey:  a3f056e5-0041-43c3-a9df-574b25a2ab03' \
--header 'EX-Dev: api' \
--header 'EX-Ts: 1547626071000000' \
--header 'EX-Ver: 1008' \
--header 'EX-Sign: 56ea9ec5355d1dde381e1296bda42db0'
```

> Response   ：

```response-data
{
    "errno": "OK",  
    "message": "Success"
}
```

### Request Url    

` GET swap/createAccount`

### Request Params

> params pass by urlparam  

| param         | type | example | description | is required |
| ------------- | ---- | ------- | ----------- | ----------- |
| instrument_id | Int  | 0       |             | yes         |



## Obtain Liquidation Records of User

> curl request example:

```curl
curl --location --request GET 'https://host.com/swap/userLiqRecords?instrumentID=1&oid=1000232'
```

> Response   ：

```response-data
{
    "errno": "OK",
    "message": "Success",
    "data": {
"records": [
    {
    "oid": 10539098,
    "instrument_id": 1,
    "pid": 10539088,
    "uid": 10,
    "type":1, // over book type ,1:part force close position,2:bankrupt position,3:ADL positive action ,4:ADL negative action
    "trigger_px":"16", 
    "order_px":"16",
    "mmr":"0.0013", 
    "subsidies":"0.018", 
    "created_at": "2018-07-23T11:55:56.715305Z"
    }
]
    }
}
```

### Request Url    

` GET swap/userLiqRecords`

### Request Params

> params pass by urlparam  

| param        | type | example | description | is required |
| ------------ | ---- | ------- | ----------- | ----------- |
| instrumentID | Int  | 0       |             | no          |
| oid          | int  | 1000232 |             | no          |



## Submit Trigger Order

> curl request example:

```curl
curl --location --request POST 'http://host.com/swap/submitPlanOrder' \
--header 'EX-Ver: 1.0.0' \
--header 'EX-Dev: web' \
--header 'EX-Sign: d1701e739a359ee4c7a5003fe39ef27065c019f687b982a6e98bd375a673ec42' \
--header 'EX-Ts: 1532428054000000' \
--header 'EX-Uid: 100039428965' \
--data-raw '{
   "instrument_id":3,
   "category":1,
   "trigger_type":1,
   "trend":1,
   "position_type":1,
   "leverage":100,
   "px":998,
   "qty":10,
   "nonce":1537326290
}'
```

> Response   ：

```response-data

```

### Request Url    

`POST swap/submitPlanOrder`

### Request Body 

| param         | type     | example    | description                                         | is required |
| ------------- | -------- | ---------- | --------------------------------------------------- | ----------- |
| instrument_id | Int      | 3          | 合约id                                              | yes         |
| category      | Int      | 1          | 1:limit,2:market                                    | yes         |
| client_id     | int      | 1          |                                                     | yes         |
| side          | int      | 1          | 1:open long,2:open short,3:close long,4:close short | yes         |
| position_type | 开仓方式 | 1          | 1:fixed,2:cross                                     | yes         |
| leverage      | decimal  | 10         |                                                     | yes         |
| px            | decimal  |            | Trigger price                                       | yes         |
| qty           | decimal  |            |                                                     | yes         |
| nonce         | int      | 1533873979 |                                                     | yes         |
| trigger_type  | int      |            | 1:trade price ,2:target price;4:index price         | yes         |
| trend         | int      | 1          | 1:long,2:short                                      | yes         |
| exec_px       | int      | 980        |                                                     |             |
| cycle         | Int      | 10         |                                                     |             |



## Cancel Trigger Order

> curl request example:

```curl
curl --location --request POST 'http://host.com/swap/cancelPlanOrders' \
--header 'Content-Type: application/json' \
--data-raw '{
    "orders":[
		{
		   "instrument_id":1,
		   "orders":[
				   10116356,
				   10116357
		    ]
		}
     ],
    "nonce":1531968125
}'
```

> Response   ：

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

`POST swap/cancelPlanOrders`

### Request Body 

| param         | type     | example | description | is required |
| ------------- | -------- | ------- | ----------- | ----------- |
| instrument_id | Int      | 3       |             | yes         |
| orders        | Int list | [1,2,3] |             | yes         |



## Obtain Trigger Order Records of User

> curl request example:

```curl
curl --location --request GET 'http://host.com/swap/userPlanOrders?instrumentID=1&offset=0&size=0&status=0'
```

> Response   ：

```response-data
{
    "errno": "OK",
    "message": "Success",
    "data": {
        "orders": [
            {
                "oid": 10540133,
                "instrument_id": 3,
                "uid": 10,
                "px": "998",
                "qty": "10",
                "side": 1,
                "trigger_type": 1,
                "trend": 1,
                "category": 1,
                "cycle": 24,
                "position_type": 1,
                "leverage": "100",
                "origin": "web",
                "created_at": "2018-09-19T03:23:57.444639Z",
                "finished_at": "2018-09-19T03:43:11.948255Z",
                "status": 4,
                "errno": 1   // 0:success 1:cancle 2:over time 3:not enough balance 4:订单将触发强平 5:invalid order 6:用户资产正处在托管中 7:position not exist 8:position cant reach required 9:position has been canceled 10:the instrument has suspended
            }
        ]
    }
}
```

### Request Url    

` GET swap/userLiqRecords`

### Request Params

> params pass by urlparam  

| param        | type | example | description | is required |
| ------------ | ---- | ------- | ----------- | ----------- |
| instrumentID | Int  | 0       |             | yes         |
| offset       | int  | 0       |             | yes         |
| size         | int  | 10      |             | no          |
| status       | int  | 1       |             |             |
|              |      |         |             |             |













# 现货交易
