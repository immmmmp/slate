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

> returned data:

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
|    unit    |  Int   |     5      | It indicates the selected time length. For example, 5M means that it takes 5 minutes to present a new candlestick on the candlestick chart. |
| resolution | String |     M      | It indicates the frequency type of obtaining candlestick chart data. M: minute, H: hour, D: day |

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
             3 //buy order:taker,order side：close short buy , open short sell
             4 //buy order:taker,order side：close short buy , close long sell
             5 //sell order:taker,order side：open short buy,open long sell
             6 //sell order:taker,order side：open short sell, close short buy
             7 //sell order:taker,order side：close long sell, open long buy
             8 //sell order:taker,order side：close long sell, close short buy
                "change": "0"    				// the influence to the market
                										if the latest price before this transaction is 10 and the latest price after this transaction is 11, then the change is "1".
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
// ADL sequence of contract position
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
																																
                "area": 2,                       						//1:USDT section,2:main section,3:innovation section,4:demo section
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
|    unit    |  Int   |     5      | It indicates the selected time length. For example, 5M means that it takes 5 minutes to present a new candlestick on the candlestick chart. |
| resolution | String |     M      | It indicates the frequency type of obtaining candlestick chart data. M: minute, H: hour, D: day |




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
                  "err_msg":"Order is about to trigger the forced close-out"
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

| param         | type    | example | description                                                | is required |
| ------------- | ------- | ------- | ---------------------------------------------------------- | ----------- |
| Instrument_id | Int     | 3       | instrument id                                              | yes         |
| category      | Int     | 1       | 1: limit price, 2: market price                            | yes         |
| client_id     | int     | 1       |                                                            | yes         |
| side          | int     | 1       | 1: open long, 2: open short, 3: close long, 4: close short | yes         |
| position_type | int     | 1       | 1: fixed, 2: cross                                         | yes         |
| leverage      | decimal | 10      |                                                            | yes         |
| px            | decimal |         | Price                                                      | yes         |
| qty           | decimal |         |                                                            | yes         |

### Header

| param        | type   | example          | description                         | is required |
| ------------ | ------ | ---------------- | ----------------------------------- | ----------- |
| Content-Type | String | application/json | fixed value                         | yes         |
| EX-Accesskey | String |                  |                                     | yes         |
| EX-Sign      | int    |                  | MD5({postdata}+{secretKey}+{EX-Ts}) | yes         |
| EX-Ver       | int    | 1008             | fixed value                         | yes         |
| EX-Dev       | String | WEB              | fixed value                         | yes         |
| EX-Ts        | Long   | 1541044393000000 | utc timestamp-microsecond           | yes         |



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

| param         | type    | example    | description                                          | is required |
| ------------- | ------- | ---------- | ---------------------------------------------------- | ----------- |
| Instrument_id | Int     | 3          |                                                      | yes         |
| category      | Int     | 1          |                                                      | yes         |
| client_id     | int     | 1          |                                                      | yes         |
| side          | int     | 1          | 1:open long ,2:open short,3:close long,4:close short | yes         |
| position_type | int     | 1          | 1:fixed,2:cross                                      | yes         |
| leverage      | decimal | 10         |                                                      | yes         |
| px            | decimal |            |                                                      | yes         |
| qty           | decimal |            |                                                      | yes         |
| nonce         | int     | 1533873979 | current timestamp                                    | yes         |



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
#success
contract accounts list
"accounts": [
            {
                "account_id": 10, 
                "coin_code": "USDT", 
                "freeze_vol": "1201.8", 
                "available_vol": "8397.65", 
                "cash_vol": "0", 
                "realised_vol": "-0.5", 
                "earnings_vol": "-0.5",
                "created_at":  // created time "2018-07-13T16:48:49+08:00",
                "updated_at":  // updated time "2018-07-13T18:34:45.900387+08:00"
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
#success
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
                "status": 1,                  //status  1:hold position  2:system  4:closed
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
        // order info
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
                                  A bit-by-bit binary representation applies to this field
                                  The 0-5th bit represents the type of the order; the 6th bit is reserved; value 1 of 7th bit represents forced close-out order; value 1 of 8th bit represents blow-up order; value 1 of 9th bit represents ADL order
                "make_fee": "0.00025", // make fee
                "take_fee": "0.012", // take fee
                "origin": "",
                "created_at": "2018-07-23T11:55:56.715305Z",
                "finished_at": "2018-07-23T11:55:56.763941Z",
                "status": 4, // status
                "errno": 0,  // reason for the order finish
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

| param         | type    | example    | description                                                | is required |
| ------------- | ------- | ---------- | ---------------------------------------------------------- | ----------- |
| instrument_id | Int     | 3          | instrument id                                              | yes         |
| category      | Int     | 1          | 1: limit, 2: market                                        | yes         |
| client_id     | int     | 1          |                                                            | yes         |
| side          | int     | 1          | 1: open long, 2: open short, 3: close long, 4: close short | yes         |
| position_type | int     | 1          | 1: fixed, 2: cross                                         | yes         |
| leverage      | decimal | 10         |                                                            | yes         |
| px            | decimal |            | Trigger price                                              | yes         |
| qty           | decimal |            |                                                            | yes         |
| nonce         | int     | 1533873979 |                                                            | yes         |
| trigger_type  | int     |            | 1: trade price, 2: target price; 4: index price            | yes         |
| trend         | int     | 1          | 1: long, 2: short                                          | yes         |
| exec_px       | int     | 980        |                                                            |             |
| cycle         | Int     | 10         |                                                            |             |



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
                "errno": 1   // 0:success 1:cancle 2:over time 3:not enough balance 4:order is aboout to trigger forced close-out 5:invalid order 6:user asset is under custody 7:position not exist 8:position cant reach required 9:position has been canceled 10:the instrument has suspended
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




#Websocket data subscription

#Websocket data push


`GET real time market data(WebSocket)`

wss://host.com/swap/realTime

### Address

- test environment `wss://host.com/swap/realTime`

### Command

-  basic command sending format `{"action":"","args":["arg1", "arg2", "arg3"]}`

- command return format

  ```undefined
  {
    "action":"<command>",
    "success":true,         
    "group":"<group>",
    "request":{
        // origin request
    },
    "error":""  // if having this field when failed, return specific cause of failure

  }
  ```

- support command list

  ```undefined
  subscribe   // subscribe
  unsubscribe //unsubscribe
  ```

### Theme

- theme list

  ```undefined
  universal theme list (no need to do authorization verification)
  Trade       //lastest trade
  Ticker      //real time price
  OrderBook   //depth
  QuoteBin1m  //1 minute quotation data

  QuoteBin5m  //5 minute quotation data
  QuoteBin30m //30 minute quotation data
  QuoteBin1h  //1 hour quotation data
  QuoteBin2h  //2 hour quotation data
  QuoteBin4h  //4 hour quotation data
  QuoteBin6h  //6 hour quotation data
  QuoteBin12h //12 hour quotation data
  QuoteBin1d  //daily quotation data
  QuoteBin1w  //weekly quotation data|
  IndexBin1m  //1 minute index quotation data|
  IndexBin5m  //5 minute index quotation data|
  IndexBin30m //30 minute index quotation data|
  IndexBin1h  //1 hour index quotation data|
  IndexBin2h  //2 hour index quotation data|
  IndexBin4h  //4 hour index quotation data|
  IndexBin6h  //6 hour index quotation data|
  IndexBin12h //12 hour index quotation data|
  IndexBin1d  //daily index quotation data|
  IndexBin1w  //weekly index quotation data|
  ```

- explanation

1. Except ticker, all other themes are related to InstrumentID
2. request subscription command， composition of themes in theme list are <theme:InstrumentID>,such as the command for subscribing spot trading pair EOS/ETH’s real time depth and 5 minute quotation is` {"action":"subscribe","args":["Depth:EOS/ETH","QuoteBin5m:EOS/ETH"]}`
3. orderbook is updated upon data difference. When action is 1, it stands for full data. When action is 2, it stands for incremental data. They are both sorted according to the orderbook.  {"group":"OrderBook:1","action":1,"data":{"asks":[[862810,"8628.1","2666",0]]}} [862810, // Integer key, easy for sorting  "8628.1", // price "2666", // volume, if value is 0 then it means the book is deleted  0] // if value is 1 then it means the book contains blow-up order




### Format Of Subscribing Data

- base format is as followed

  ```undefined
  {
    "group":"",
    "data":{
    }
  }
  ```

### verification

```if need to subscribe private information such as data related to user’s self-information etc., then there are 2 ways to do ws verification
  1.verification through user name and password
  // referred to Api Sign calculation method （the sequence of parameters is very important, don’t mess up with the sequence）
  {
      "action":"access",
      "args":[
          "uid",       // user ID, must be string
          "web",              // device type. same as Dev(abit-dev) must be string
          "1.0.0",            // client end version number,Version(abit-ver)must be string
          "Sign",              // encrypted string,same as Sign(abit-sign),referred to Api Sign calculation method. must be string 
          "1540286461000000",  // same as Ts(abit-ts) unit: microseconds, must be string 
      ] 
  }
  2.verification through accessKey
  // referred to Api Sign calculation method （the sequence of parameters is very important, don’t mess up with the sequence）
  {
      "action":"access",
      "args":[
          "Accesskey",       // Accesskey of the user, must be string
          "api",              // Dev(abit-dev) must be string
          "1.0.0",            // Version(abit-ver)must be string
          "Sign",              // Sign(abit-sign) must be string
          "1540286461000000",  // Ts(abit-ts) unit: microseconds, must be string 
      ] 
  }
  
  
  parameter value of Sign is:md5(sercet_key+Ts(string))

```

### subscribe private data


```// when subscribe is finished （passed verification）, private data in unicast theme can be received. There is only one unicast theme temporarily. All the private data will be returned in this theme
   {
       "action":"subscribe",
       "args":["unicast"]
   }
   
   // unicast theme,format of returned data
   {
       "group":"SUD",
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
   "group":"SUD", indicating the user’s spot trading data. Driven by backend operation. Push user data updates. One push may include multiple business operations, so data is in the form of an array, starting from the head of the array, a set of user operations stored in order of operations. The elements of each group of data includes: action (operation Type), order (order information), s_assets (spot asset information, note it is the array of spot assets), c_assets (contract asset information). For order information, spot asset information, contract asset information, only when the operation of these information generated updates, can each set of data elements contain that information.
   action: operation types include
   1: Matching.
      Possible impact: order update, position update, contract asset update
   2: Submit order
      Possible impact: order update, contract asset update
   3: Order cancellation:
      Possible impact: order update, position update, contract asset update
   11: Transfer from contract asset to spot assets
      Possible impact: contract asset update, spot asset update
   12: Transfer from spot asset to contract account
```

### heartbeat

- The server will send a PingFrame to the client end every 10 seconds. Under normal circumstances, the client end will reply with a PongFrame. If the server does not receive a response for five consecutive PingFrames, and during this period, no other data from the client is received, the server will actively disconnect the link. After receiving PingFrame, most browsers will automatically respond with PongFrame without the need of business layer implementation.


- The server implements a PingMessage Handle in the business layer. After receiving the PingMessage, it will automatically reply with a PongMessage. If the bottom layer of the client end has no way to handle the ping / pong frame of the websocket protocol layer, the ping / pong message of the business layer can determine whether the link is healthy.  The specific message is as follows


  ```undefined
  // ping
  {"action":"ping"}
  // pong
  {"group":"System","data":"pong"}
  ```

### testing


- Websocket client ends for any standard can be used for testing


- Chrome websocket test plugin

  ```undefined
  https://chrome.google.com/webstore/search/WebSocket%20Test%20Client?utm_source=chrome-ntp-icon
  ```







# Spot Trading




# 公共信息-REST

对于合约公共信息Request Url的EndPoint地址为：https://host.com/

## GET SYMBOL INFO

> curl example

```curl
curl --location --request GET 'https://api.host.com/spot/instruments?symbol=BTC/USDT'
```

> return data:

```response-data
{
    "errno": "OK",
    "message": "Success",
    "data": {
        "instruments": [
            {
            
            "instrument_id": 22,  // spot symbol ID
            "symbol": "BTC/USDT", 
            "base_coin": "BTC",   
            "quote_coin": "USDT", 
            "rank": 5,            // System rank
            "px_unit": "0.0001",  // price decimal
            "qty_unit": "0.0001", // qyt decimal
            "tip_limit": "0.2",   // price invalid range
            "min_qty": "0.0001",  
            "status": 1,          //    1  // online
                                        2  // judging 
                                        3  // testing
                                        5  // suspend
                                        6  // offline
            "big_icon": "",       
            "small_icon": "",
            "gray_icon": "",
            "created_at": null,
            "market_order_risk_type": 1,//1:按order book价格幅度约束,
            													2:按order book档位约束,
            													0:don’t support market order
            											
            "market_order_risk_deal_scope": "0.2",
            "fee_configs": [
                {
                    "coin_code": "BTC", // which coin to pay fee
                    "fee_ratio": "0.1",
                    "type": 3  //1:only take buy order fee,2:only take sell order fee,3:take both order fee
                }
            ] 
            }
        ]
    }
}

```

### Request Url

`GET spot/instruments`

### Params

| Param  |  Type  | Example  | Description |
| :----: | :----: | :------: | :---------: |
| symbol | String | BTC/USDT |             |



## Get Symbol Depth

> curl example

```curl
curl --location --request GET 'https://api.host.com/spot/depth?symbol=BTC/USDT' \
--header 'Ex-Ver: 1008' \
--header 'Ex-Dev: WEB' \
--header 'Ex-Ts: 1541044393000000'
```

> return data:

```response-data
{
    "errno": "OK",
    "message": "Success",
    "data": {

        "asks": [
            [
                80000, // key
                "8",   // price
                "1",   // qty
                0      
            ],
            [
                82000,
                "8.2",
                "1",
                0
            ],
        // 买盘,按价格由大到小排序
        "bids": [
            [
                74000, // key
                "7.4", // price 
                "1",   // qty
                0
            ],
            [
                73000,
                "7.3",
                "1",
                0
            ],
        ]
    }
}
```

### Request Url

`GET spot/depth`

### Params

| Param  |  Type  | Example  | Description |
| :----: | :----: | :------: | :---------: |
| symbol | String | BTC/USDT |             |



## Get Symbol Ticker

> curl example

```curl
curl --location --request GET 'https://api.host.com/spot/tickers?symbol=BTC/USDT'
```

> return data:

```response-data
{
    "errno": "OK",
    "message": "Success",
    "data": [ // ticker数组
        {
            "symbol": "ETH/USDT",
            "last_px": "135.5", 
            "open": "137.17",      
            "close": "135.5",      
            "low": "127.87",       
            "high": "137.38",      
            "avg_px": "134.838547255697", 
            "last_qty": "10.135", 
            "qty24": "21244.5602", 
            "timestamp": 1551322866,
            "change_rate": "-0.012174673762",
            "change_value": "-1.67", 
            "qty_day": "9789.7338",
            "amount24": "2894738.555982" 
        }
    ]
}
```

### Request Url

`GET spot/ticker`

### Params

| Param  |  Type  | Example  | Description |
| :----: | :----: | :------: | :---------: |
| symbol | String | BTC/USDT |  现货名称   |



## Get Trade Records

> curl example

```curl
curl --location --request GET 'https://api.host.com/spot/trades?symbol=BTC/USDT&offset=0&size=10' 
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

`GET spot/trades`

### Params

| Param  |  Type  | Example  | Description |
| :----: | :----: | :------: | :---------: |
| symbol | String | BTC/USDT |             |
| offset |  Int   |    0     |             |
|  size  |  Int   |    20    |             |



## Get k-Line Data

> curl example

```curl
curl --location --request GET 'https://api.host.com/spot/kline?symbol=BTC/USDT&startTime=0&endTime=2532656524&unit=5&resolution=M'
```

> return data:

```response-data
{
    "errno": "OK",
    "message": "Success",
    "data": [ // 数组
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

`GET spot/kline`

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
curl --location --request POST 'https://api.host.com/spot/submitOrder' \
--header 'Ex-Ver: 1008' \
--header 'Ex-Dev: api' \
--header 'Ex-Sign: d1701e739a359ee4c7a5003fe39ef27065c019f687b982a6e98bd375a673ec42' \
--header 'Ex-Ts: 1532428054000000' \
--header 'Ex-Accesskey: 34234423sdfsfsfwerwerwerwrwerwer' \
--data-raw '{
   "symbol":"EOS/ETH",
   "px":"0.01",
   "qty":"10.2",
   "side":2,
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

`POST swap/submitOrder`

### Params Body 

|  Param   |  Type   |  Example   |                     Description                      |
| :------: | :-----: | :--------: | :--------------------------------------------------: |
|  symbol  | String  |  BTC/USDT  |                                                      |
|    px    | Decimal |    7293    |                        Price                         |
|   qty    | Decimal |    10.2    |                         Qty                          |
|   side   |   Int   |     2      |                     1:buy 2:sell                     |
| category |   Int   |     1      | 1:limit price order,2:market order,price must be "0" |
|  nonce   |   int   | 1545820222 |                      Timestamp                       |



## Cancel Order

> curl example

```curl
curl --location --request POST 'https://api.host.com/spot/cancelOrder' \
--header 'Ex-Ver: 1008' \
--header 'Ex-Dev: api' \
--header 'Ex-Sign: d1701e739a359ee4c7a5003fe39ef27065c019f687b982a6e98bd375a673ec42' \
--header 'Ex-Ts: 1532428054000000' \
--header 'Ex-Accesskey: 34234423sdfsfsfwerwerwerwrwerwer' \
--data-raw '{"oid":14369318,"symbol":"EOS/ETH","nonce":1545735606}'
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
    "message": "失败原因",
}
```

### Request Url    

`POST spot/cancelOrder`

### Params Body 

| Param  |  Type  |  Example   |      Description      |
| :----: | :----: | :--------: | :-------------------: |
| symbol | String |  BTC/USDT  |                       |
|  oid   |  int   |    7293    | id of order to cancel |
| nonce  |  int   | 1545820222 |       Timestamp       |



## Batch Submit Order

> curl example

```curl
curl --location --request POST 'https://api.host.com/spot/batchOrders' \
--header 'Ex-Accesskey: 123123123213' \
--header 'Ex-Sign: MD5(postdata+secretKey+Ex-Ts)' \
--header 'Ex-Ver: 1008' \
--header 'Ex-Dev: api' \
--header 'Ex-Ts: 1541044393000000' \
--data-raw '{
	"orders":[
		{
		   "symbol":"EOS/ETH",
		   "px":"0.01",
		   "qty":"10.2",
		   "side":2,
		   "category":1,
		   "client_id":1
		},
		{
		   "symbol":"EOS/ETH",
		   "px":"0.01",
		   "qty":"10.2",
		   "side":2,
		   "category":1,
		   "client_id":2
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
partly success:
{
    "errno": "OK",
    "message": "Success",
    "data": {
        "orders": [
            {
                "client_id": 1,
                "err": {
                    "http_err":405,
                    "err_code":"code",
                    "err_msg":"失败原因"
                }
            },
            {
                "client_id": 2,
                "oid": 10540014
            }
        ]
    }
}
fail:
{
    "errno": "FAILED",
    "message": "失败原因"
}
```

### Request Url    

`POST spot/batchOrders`

### Params Body 

|  Param   | Type |  Example   | Description |
| :------: | :--: | :--------: | :---------: |
| BTC/USDT |      |            |             |
|  nonce   | int  | 1545820222 | Stimestamp  |

### order  

|  Param   |  Type   | Example  |                       Description                        |
| :------: | :-----: | :------: | :------------------------------------------------------: |
|  symbol  | String  | BTC/USDT |                                                          |
|    px    | Decimal |   7293   |                          Price                           |
|   qty    | Decimal |   10.2   |                           Qty                            |
|   side   |   Int   |    2     |                       1:buy 2:sell                       |
| category |   Int   |    1     | 1:limit price order,2:market order,the price must be "0" |
|   int    |    3    |          |                                                          |

## Batch Cancel Order

> curl example

```curl
curl --location --request POST 'https://api.host.com/spot/cancelOrders' \
--header 'Ex-Accesskey: 123123123213' \
--header 'Ex-Sign: MD5(postdata+secretKey+Ex-Ts)' \
--header 'Ex-Ver: 1008' \
--header 'Ex-Dev: api' \
--header 'Ex-Ts: 1541044393000000' \
--data-raw '{
    "orders":[
		{
		   "symbol":"EOS/ETH",
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
        // successfully canceled list 
        "succeed": [ 
            10116356,
            10116357
        ],
        
        "failed": null
    }
}
```

### Request Url    

`POST spot/cancelOrders`

### Params Body 

| Param  |     Type     |                           Example                            | Description |
| :----: | :----------: | :----------------------------------------------------------: | :---------: |
| orders | cancelOrders | { 		   "symbol":"EOS/ETH", 		   "orders":[ 				   10116356, 				   10116357 		    ] 		} |             |
| nonce  |     int      |                          1545820222                          |             |

### cancelOrders说明

| Param  |   Type   | Example | Description |
| :----: | :------: | :-----: | :---------: |
| String | ETH/USDT |         |             |
|  int   | [1,2,3]  |         |             |

## Get User Order

> curl example

```curl
curl --location --request GET 'https://api.host.com/spot/userOrders?symbol=EOS/ETH&status=2&offset=1&size=2' \
--header 'Ex-Ver: 1008' \
--header 'Ex-Dev: api' \
--header 'Ex-Sign: d1701e739a359ee4c7a5003fe39ef27065c019f687b982a6e98bd375a673ec42' \
--header 'Ex-Ts: 1532428054000000' \
--header 'Ex-Accesskey: 100039428965' \
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
                "oid": 10284160,
                "uid":23249999,
                "symbol": "EOS/ETH",
                "px": "8",
                "qty": "4",
                "cum_qty": "0",
                "swap_qty": "0",
                "side": 1,
                "category": 1,
                "fee": "0",
                "fee_ratio": "0",
                "fee_coin_code": "0",
                "cum_fee": "", 
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

`GET spot/userOrders`

### Params

| Param  |  Type  | Example  |                         Description                          |
| :----: | :----: | :------: | :----------------------------------------------------------: |
| symbol | String | BTC/USDT |                                                              |
| status |  int   |    2     | 状态,1:申报中的订单 2:委托和申报中的订单 3:已经完成的订单 如果status字段不传则返回所有状态的订单 |
| offset |  int   |    0     |                                                              |
|  size  |  int   |    20    |                                                              |
|        |        |          |                                                              |

## Get User Balance

> curl example

```curl
curl --location --request POST 'https://api.host.com/v1/ifaccount/users/vipMe' \
--header 'Ex-Ver: 1008' \
--header 'Ex-Dev: api' \
--header 'Ex-Sign: d1701e739a359ee4c7a5003fe39ef27065c019f687b982a6e98bd375a673ec42' \
--header 'Ex-Ts: 1532428054000000' \
--header 'Ex-Accesskey: 34234423sdfsfsfwerwerwerwrwerwer' \
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

`GET /v1/ifaccount/users/vipMe`



## Get User Order Detail

> curl example

```curl
curl --location --request POST 'https://api.host.com/spot/queryOrder' \
--header 'Ex-Accesskey: 123123123213' \
--header 'Ex-Sign: MD5(secretKey+Ex-Ts)' \
--header 'Ex-Ver: 1008' \
--header 'Ex-Dev: api' \
--header 'Ex-Ts: 1541044393000000' \
--data-raw '{
	"symbol":"ETH/USDT",
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
                "oid":2192079529,
                "symbol":"XDAG/ETH",
                "px":"1.1",  // order price 
                "qty":"1",        
                "cum_qty":"1", // done qty
                "swap_qty":"1.1",  
                "side":1,  // 1:buy,2:sell
                "category":1,  // 1:limit price,2:market price
                "fee":"0.0011", 
                "fee_ratio":"0.001", 
                "fee_coin_code":"ETH", 
                "cum_fee":"0.0011",  
                "uid":2000628412, 
                "created_at":"2019-02-28T10:28:14.01776Z",
                "finished_at":"2019-02-28T10:28:14.040836Z",
                "status":3, 
                "errno":0 
            }
        ]
    }
}
```

### Request Url

`POST spot/queryOrder`

### Params

| Param  |  Type  | Example  |  Description  |
| :----: | :----: | :------: | :-----------: |
| symbol | String | BTC/USDT |               |
| status |  int   | [1,23,4] | order id list |
| offset |  int   |    0     |               |
|  size  |  int   |    20    |               |



## Get User Trade Records

> curl example

```curl
curl --location --request GET 'https://api.host.com/spot/userTrades?symbol=ETH/USDT&offset=0&size=60' \
--header 'Ex-Accesskey: 123123123213' \
--header 'Ex-Sign: MD5(secretKey+Ex-Ts)' \
--header 'Ex-Ver: 1008' \
--header 'Ex-Dev: api' \
--header 'Ex-Ts: 1541044393000000'
```

> return data:

```response-data
{
    "errno":"OK",
    "message":"Success",
    "data":{
        // 交易记录列表,按创建时间由近及远排序
        "trades":[
            {
                "oid":100039430677,
                "tid":100039430678,
                "symbol":"BTC/ETH",
                "px":"0.06",
                "qty":"21",
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

`GET spot/orderTrades`

### Params

| Param  |  Type  | Example  | Description |
| :----: | :----: | :------: | :---------: |
| symbol | String | BTC/USDT |             |
| offset |  int   |    0     |             |
|  size  |  int   |    20    |             |

## Get User Trade Record By Order

> curl example

```curl
curl --location --request GET 'https://api.host.com/spot/orderTrades?symbol=ETH/USDT&oid=1000121232' \
--header 'Ex-Accesskey: 123123123213' \
--header 'Ex-Sign: MD5(secretKey+Ex-Ts)' \
--header 'Ex-Ver: 1008' \
--header 'Ex-Dev: api' \
--header 'Ex-Ts: 1541044393000000'
```

> return data:

```response-data
{
    "errno":"OK",
    "message":"Success",
    "data":{
        // 交易记录列表,按创建时间由近及远排序
        "trades":[
            {
                "oid":100039430677,  //该交易记录的taker order id
                "tid":100039430678,  //trade id
                "symbol":"BTC/ETH",
                "px":"0.06",
                "qty":"21",
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

`GET spot/orderTrades`

### Params

| Param  |  Type  | Example  | Description  |
| :----: | :----: | :------: | :----------: |
| symbol | String | BTC/USDT |   现货名称   |
| offset |  int   |    1     | 查询的订单id |

# SPOT-RealTime(WebSocket)

### 地址

- 测试环境 `wss://api.host.com/v1/ifspot/realTime`
- 生产环境 `wss://api.host.com/v1/ifspot/realTime`

### 命令

- 基本命令格式发送格式 `{"action":"","args":["arg1", "arg2", "arg3"]}` 如:客户端订阅EOS/ETH,USDT/BTC现货对的ticker的命令 {"action":"subscribe","args":["Ticker:EOS/ETH", "Ticker:USDT/BTC"]} 客户单取消这两个现货对的订阅 {"action":"unsubscribe","args":["Ticker:EOS/ETH", "Ticker:USDT/BTC"]}

- 命令返回格式

  ```undefined
  {
    "action":"<command>",
    "success":true,         // 成功-true, 失败-false
    "group":"<group>",
    "request":{
        // 原始Request Url
    },
    "error":""  // 失败时有这个字段返回具体错误原因
  }
  ```

- 支持命令列表

  ```
  subscribe   // 订阅
  unsubscribe //取消订阅
  ```

### 主题

- 通用主题列表(不需要做授权认证)

  ```undefined
  这些主题列表是开放主题,不需要做ws认证.
  Trade       //最新成交
  Ticker      //实时价格
  OrderBook   //深度
  QuoteBin1m  //1分钟行情数据
  QuoteBin5m  //5分钟行情数据
  QuoteBin30m //30分钟行情数据
  QuoteBin1h  //1小时行情数据
  QuoteBin2h  //2小时行情数据
  QuoteBin4h  //4小时行情数据
  QuoteBin6h  //6小时行情数据
  QuoteBin12h //12小时行情数据
  QuoteBin1d  //日行情数据
  QuoteBin1w  //周行情数据|
  IndexBin1m  //1分钟指数行情数据|
  IndexBin5m  //5分钟指数行情数据|
  IndexBin30m //30分钟指数行情数据|
  IndexBin1h  //1小时指数行情数据|
  IndexBin2h  //2小时指数行情数据|
  IndexBin4h  //4小时指数行情数据|
  IndexBin6h  //6小时指数行情数据|
  IndexBin12h //12小时指数行情数据|
  IndexBin1d  //日指数行情数据|
  IndexBin1w  //周指数行情数据|
  ```

- 说明

1. Request Url订阅命令，主题列表主题的构成方式为<主题:现货对的Code(区分大小写)>,例如需要订阅现货EOS/ETH的实时深度和5分钟行情的命令为 `{"action":"subscribe","args":["OrderBook:EOS/ETH","QuoteBin5m:EOS/ETH"]}`

### 订阅数据格式

- 基本格式如下

  ```undefined
  {
    "group":"",
    "action":1,
    "data":{
    }
  }
  // 订阅主题不同，data字段格式不同。data的具体以接口返回为准，Request Url输入对应主题的订阅命令获取
  action:表示更新的数据类型
  1:全量数据更新,
  2:差量数据更新
  3:插入数据更新
  4:删除数据更新
  ```

### 认证

```undefined
如果需要订阅跟用户自己信息相关的数据等私有信息,需要做ws认证方式有两种
1.通过用户名密码方式认证
// 参考Api接口Sign计算方式(参数顺序很重要，别搞错顺序了)
{
    "action":"access",
    "args":[
        "uid",       // 用户ID,必须是字符串
        "web",              // 设备类型.同Dev(Ex-Dev) 必须是字符串
        "1.0.0",            // 客户端版本号,Version(Ex-Ver）必须是字符串
        "Sign",              // 加密串,同Sign(Ex-Sign),参考Api接口Sign计算方式. 必须是字符串
        "1540286461000000",  // 同Ts(Ex-Ts) 单位:微秒,必须是字符串
    ] 
}
2.通过accessKey认证
// 参考Api接口Sign计算方式(参数顺序很重要，别搞错顺序了)
{
    "action":"access",
    "args":[
        "Accesskey",       // 用户的Accesskey,必须是字符串
        "api",              // Dev(Ex-Dev) 必须是字符串
        "1.0.0",            // Version(Ex-Ver）必须是字符串
        "Sign",              // Sign(Ex-Sign) 必须是字符串
        "1540286461000000",  // Ts(Ex-Ts) 单位:微秒,必须是字符串
    ] 
}


Sign参数的值为:md5(sercet_key+Ts(字符串))
```

### 订阅私有数据

```undefined
// 订阅完成后(认证通过)就可以收到UserProperty主题的私有数据了,暂时只有一个UserProperty主题，所有私有数据都在这个主题返回
{
    "action":"subscribe",
    "args":["UserProperty"]
}

// UserProperty主题,返回的数据格式
{
    "group":"UserProperty",
    "data":[
        {
            "action":1, // 操作类型
            "order":{   // 订单信息

            },
            "s_assets":[  // 现货资产列表

            ],
            "c_assets":{  // 合约资产

            }
        }
    ]
}
"group":"UserProperty",表示该用户的的现货数据.以后台业务操作为驱动,推送用户数据的更新.一次推送可能包括多次业务操作,所以data是以数组形式,从数组的头开始,按操作先后顺序存放的用户的一组操作.每组数据的元素包括:action(操作类型),order(订单信息),s_assets(现货资产信息,注意是现货资产数组),c_assets(合约资产信息).对于订单信息,现货资产信息,合约资产信息,只有当操作这些信息产生了更新,每组数据的元素才会包含该信息.
action:操作类型有
1 :撮合.
   可能产生的影响:订单更新,仓位更新,合约资产更新
2 :提交订单
   可能产生的影响:订单更新,合约资产更新
3 :取消订单
   可能产生的影响:订单更新,仓位更新,合约资产更新
11 :从合约资产化出到现货资产
   可能产生的影响:合约资产更新,现货资产更新
12 :从现货资产化出到合约账户
```

### 心跳

- 服务端会每隔10秒发送一个PingFrame到客户端，正常情况下客户端均会回复一个PongFrame。如果服务端连续5个PingFrame均没有收到应答。并且在此期间没有收到客户端的其他数据，服务端会主动断开链接。大部分浏览器收到PingFrame后均会自动给以PongFrame应答，不需要业务层实现。

- 服务端在业务层实现了一个PingMessage Handle，收到PingMessage后会自动回复一个PongMessage，客户端底层如果没有办法处理websocket协议层的ping/pong frame可以通过业务层的ping/pong message判断链接是否健康。具体消息如下

  ```undefined
  // ping
  {"action":"ping"}
  // pong
  {"group":"System","data":"pong"}
  ```

### 测试

- 任何标准的Websocket客户端都可以用来测试

- Chrome websocket测试插件

  ```undefined
  https://chrome.google.com/webstore/search/WebSocket%20Test%20Client?utm_source=chrome-ntp-icon
  ```

