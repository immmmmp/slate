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

# 概览

欢迎查看 Abit API 文档。 我们提供完整的 REST、Websocket API 以满足您的程序交易需求。您可以在    上找到每个连接选项的示例代码。

# 身份验证



API的身份验证通过Header上的参数进行加密验证。

具体加密算法为MD5加密。  待加密字段为"(Body)+(secret_key)+(ts)"，其中()+等符号不计入加密字符串。

Body为每个接口所要求的参数列表，转换为json字符串形式。

secret_key为向系统申请的个人账户加密私钥。

ts为utc时间戳，单位为毫秒。

以下为Header中的身份验证相关字段

|    参数名    | 参数类型 |          参数描述          |
| :----------: | :------: | :------------------------: |
| EX-Accesskey |  String  |          加密公钥          |
|    EX-Ts     |   Long   |   utc时间戳，单位为毫秒    |
|   EX-Sign    |  String  | 通过加密算法得到的加密签名 |



# 合约公共信息-REST

对于合约公共信息请求的EndPoint地址为：https://host.com/

## 获取指数k线数据

> curl请求示例:

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
            "low": "130", // 最低价
            "high": "130", // 最高价
            "open": "130", // 开盘价
            "close": "130", // 收盘价
            "last_px": "130", // 最后一次交易价
            "avg_px": "130", // 均价
            "qty": "0", // 交易量
            "timestamp": 1532610000, // 时间戳,单位秒
            "change_rate": "0", // 涨跌幅比例
            "change_value": "0" // 涨跌幅值
        }
    ]
}
```

### 请求Url

`GET swap/indexkline`

### 参数列表

|   参数名   |  参数类型  |       参数示例        |                           参数描述                           |
| :--------: | :--------: | :-------------------: | :----------------------------------------------------------: |
|  indexID   |   String   |          11           |                            指数id                            |
| startTime  | 1533686400 | 获取k线数据的起始时间 |                                                              |
|  endTime   | 1533688400 | 获取k线数据的结束时间 |                                                              |
|    unit    |    Int     |           5           | 选择的时间跨度的长度，在示例中表示为5M，即5分钟为每根蜡烛图的时间跨度 |
| resolution |   String   |           M           |         表示获取K线数据的频率类型,M:分钟,H:小时;D:天         |

## 获取合约交易记录

> curl请求示例:

```curl
curl --location --request GET 'https://host.com/swap/trades?instrumentID=1'
```

> Response：

```
{
    "errno": "OK",
    "message": "Success",
    "data": {
        // 交易记录列表,按创建时间由近及远排序
        "trades": [
            {
                "oid": 10116361,        // taker order id taker订单的id
                "tid": 10116363,        // trade id
                "instrument_id": 1,     // 合约ID
                "px": "16",   					// 成交价
                "qty": "10",     				// 成交的合约张数
                "make_fee": "0.04",   	// make fee
                "take_fee": "0.12",   	// take fee
                "created_at": null,   	// 创建时间
                "side": 5,             	// 订单交易方向，字段表示两笔订单成交所得到的交易记录的性质，
                																	1 //买单为taker,两笔订单方向：开多买 开空卖
                																	2 //买单为taker,两笔订单方向：开多买 平多卖
                                                  3 //买单为taker,两笔订单方向：平空买 开空卖
                                                  4 //买单为taker,两笔订单方向：平空买 平多卖
                                                  5 //卖单为taker,两笔订单方向：开空卖 开多买
                                                  6 //卖单为taker,两笔订单方向：开空卖 平空买
                                                  7 //卖单为taker,两笔订单方向：平多卖 开多买
                                                  8 //卖单为taker,两笔订单方向：平多卖 平空买
                "change": "0"    				// 对行情的影响
                										如本次交易前的最新交易价是10,本次交易的交易价是11,则change为"1"
            }
        ]
    }

```

### 请求Url

 `GET swap/trades`

### 参数列表

|    参数名    | 参数类型 | 参数示例 |                参数描述                |
| :----------: | :------: | :------: | :------------------------------------: |
| instrumentID |  String  |    11    | 合约id，具体可见获取合约交易对详情接口 |



## 获取合约仓位的自动减仓排序表

>curl请求示例:

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
// 合约看多仓位自动减仓排序表
"long_pnls": [
    {
        // 该分位的仓位盈利范围min~max
        "min_pnl": "-0.0379107725788900979",
        "max_pnl": "-0.0103298131866111136",
        "quan_tile": 40 // 五分位值,分别有20,40,60,80,100,quan_tile值越大,说明仓位发生自动减仓的可能													性越大.可以用这些值表示自动减仓警示灯的个数,20:1个灯,
        																											40:2个灯,
        																											60:3个灯,
        																											80:4个灯,
        																											100:5个灯
    },
    {
        "min_pnl": "-0.0103298131866111136",
        "max_pnl": "0",
        "quan_tile": 60 // 五分位值
    }
],
 // 合约看空仓位自动减仓排序表
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

### 请求Url 

`GET swap/pnls`

### 参数列表

|    参数名    | 参数类型 | 参数示例 |                参数描述                |
| :----------: | :------: | :------: | :------------------------------------: |
| instrumentID |  String  |    11    | 合约id，具体可见获取合约交易对详情接口 |






## 获取指数列表

###请求Url 

`GET swap/indexes`

### 参数列表

无

>curl请求示例:

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
            "index_id": 1, // 指数ID
            "name": "BTC", // 指数名称
            "base_coin": "BTC", // 指数基础币
            "quote_coin": "USDT", // 指数计价币
            "brief_en": "", // 英文简称
            "brief_zh": "", // 中文简称
            "px_unit": "0.000001", // 价格精度
            "created_at": "2018-07-30T16:04:08Z",
            // 指数成员
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
            // 合约列表
            "instruments": [
                {
                    // 跟指数关联的合约对象
                    // 数据与获取合约接口的结构一致
                },
                {

                }
            ]
        }
    ]
}
```

## 获取单个币种合约深度

>curl请求示例:

```curl
curl --location --request GET 'https://host.com/swap/depth?instrumentID=1&count=10'
```

>Response：

```
{
    "errno": "OK",
    "message": "Success",
    "data": {
         // 卖盘,按价格由小到大排序
        "asks": [
            [
                80000, // key
                "8",   // 价格
                "1",   // 量
                0      // 0:表示用户订单,1:表示包含系统订单
            ]...
        ],
        // 买盘,按价格由大到小排序
        "bids": [
            [
                74000, // key
                "7.4", // 价格
                "1",   // 量
                0
            ]...
        ]
    }
}
```

###请求Url

`GET swap/depth`

###参数列表

|    参数名    | 参数类型 | 参数示例 |                参数描述                |
| :----------: | :------: | :------: | :------------------------------------: |
| instrumentID |  String  |    11    | 合约id，具体可见获取合约交易对详情接口 |
|    count     |   Int    |    10    |   合约深度档数大小，不传表示获取全部   |



## 获取合约信息

>curl请求示例:

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
                "instrument_id": 7,                         //合约id
                "index_id": 16,															//指数id
                "symbol": "BCHUSDT",												//合约名称
                "name_zh": "BCHUSDT永续合约",                //合约中文名称
                "name_en": "BCHUSDT SWAP",									//合约英文名称
                "base_coin": "BCH",													//基础币种
                "quote_coin": "USDT",												//计价币种
                "margin_coin": "USDT",											//保证金币种
                "is_reverse": false,                        //是否是反向合约，usdt合约为正向合																																约，币本位合约为反向合约
                "market_name": "",                          //
                "face_value": "0.001",                      //单张合约面值，以基础币种为单位
                "begin_at": "2018-09-29T04:00:00Z",         //
                "settle_at": "2019-09-08T12:00:00Z",        //
                "settlement_interval": 28800,               //
                "min_leverage": "1.0",                      //最小支持杠杆
                "max_leverage": "50.0",                     //最大支持杠杆
                "position_type": 3,                       	//支持的仓位类型,1:全仓,2:逐仓,3:都																																支持
                "px_unit": "0.01",                       		//价格精度
                "qty_unit": "1",                       			//数量精度
                "value_unit": "0.0001",                     //价值精度
                "min_qty": "1",                     			  //单笔订单最小数量
                "max_qty": "100000",                        //单笔订单最大数量
                "underweight_type": 1,                      //穿仓补偿方式 1:ADL方式,2:盈利均摊																																						方式
                
                
                "status": 3,                      				  //合约状态
                																								1  // 审批中,系统内部使用
																																2  // 测试中,系统内部使用
																																3  // 可用,正在撮合的合约
																																4  // 暂停,合约可见,撮合暂停
																																5  // 交割中,期货合约到期后,暂停																																			撮合,开始结算
																																6  // 交割完成,结算完成
																																7  // 下线,合约生命周期结束
                "area": 2,                       						//1:USDT区,2:主区,3:创新区,4:模拟区
                "created_at": "2018-09-29T10:02:42Z",       //创建时间
                "depth_round": "1.001",                     //深度边框系数
                "base_coin_zh": "比特现金",                  //基础币种中文名称
                "base_coin_en": "BCH",                      //基础币种英文缩写
                "max_funding_rate": "0",                    //最大资金费率
                "min_funding_rate": "0",                    //最小资金费率
                "risk_limit_base": "100000",                //风险限额基础
                "risk_limit_step": "50000",                 //风险限额补偿
                "mmr": "0.005",                             //基本维持保证金率
                "imr": "0.01",                              //基本开仓保证金率
                "maker_fee_ratio": "0.00025",               //maker费率
                "taker_fee_ratio": "0.00075",               //taker费率
                "settle_fee_ratio": "0",                    //交割手续费率
                "plan_order_price_min_scope": "0",          //计划委托最小价格范围
                "plan_order_price_max_scope": "0",          //计划委托最大价格范围,如果为0,表示该																																合约不支持计划委托
                "plan_order_max_count": 0,                  //单用户计划委托最大数量
                "plan_order_min_life_cycle": 0,             //计划委托最小生命周期
                "plan_order_max_life_cycle": 0              //计划委托最大生命周期
            }
        ]
    }
}


```


###请求Url

`GET swap/instruments`


###参数列表

|    参数名    | 参数类型 | 参数示例 |                参数描述                |
| :----------: | :------: | :------: | :------------------------------------: |
| instrumentID |  String  |    11    | 合约id，具体可见获取合约交易对详情接口 |




## 获取合约信息

>curl请求示例:

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
            "rate": "-0.0060483184843194741", // 资金费率
            "timestamp": 1534320000 // 时间戳
        },
        {
            "rate": "0.1041766553400505803",
            "timestamp": 1534291200
        }
    ]
}
```


###请求Url

`GET swap/fundingrate`

###参数列表

|    参数名    | 参数类型 | 参数示例 |                参数描述                |
| :----------: | :------: | :------: | :------------------------------------: |
| instrumentID |  String  |    11    | 合约id，具体可见获取合约交易对详情接口 |




## 获取合约k线数据



>curl请求示例:

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
            "low": "130", 				// 最低价
            "high": "130", 				// 最高价
            "open": "130", 				// 开盘价
            "close": "130", 			// 收盘价
            "last_px": "130", 		// 最后一次交易价
            "avg_px": "130", 			// 均价
            "qty": "10", 					// 交易量,单位张数
            "base_coin_qty":"163.9", 				// 基础币的交易量(币值对前面的币为基础币)
            "quote_coin_qty":"34555.3812", 	// 计价币的交易量(币值对后面的币为计价币)
            "timestamp": 1532610000, 				// 时间戳,单位秒
            "change_rate": "0", 						// 涨跌幅比例
            "change_value": "0" 						// 涨跌幅值
        }
    ]
}
```

###请求Url

`GET swap/kline`


###参数列表  

|   参数名   | 参数类型 |  参数示例  |                           参数描述                           |
| :--------: | :------: | :--------: | :----------------------------------------------------------: |
|  indexID   |  String  |     11     |                            合约id                            |
| startTime  |    ts    | 1533686400 |                    获取k线数据的起始时间                     |
|  endTime   |    ts    | 1533688400 |                    获取k线数据的结束时间                     |
|    unit    |   Int    |     5      | 选择的时间跨度的长度，在示例中表示为5M，即5分钟为每根蜡烛图的时间跨度 |
| resolution |  String  |     M      |         表示获取K线数据的频率类型,M:分钟,H:小时;D:天         |




## 获取合约ticker数据

###请求Url    

`GET swap/tickers`


###参数列表

|    参数名    | 参数类型 | 参数示例 | 参数描述 |
| :----------: | :------: | :------: | :------: |
| instrumentID |  String  |    11    |  合约id  |



>curl请求示例:

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
                "last_px": "6277.5", 					// 最后成交价
                "open": "6074.5", 						// 当天的开盘价
                "close": "6277.5", 						// 当前的收盘价
                "low": "6261.3", 							// 当天的最低价
                "high": "6279", 							// 当天的最高价
                "avg_px": "6274.67",					// 均价
                "last_qty": "20000", 					// 最后一笔交易量,单位张数,如果要显示btc的交易量,																											需要last_qty乘以合约的大小
                "qty24": "45462", 						// 24小时总量,单位张数
                "base_coin_qty":"163.9", 			// 基础币的交易量(币值对前面的币为基础币)
                "quote_coin_qty":"34555.381", // 计价币的交易量(币值对后面的币为计价币)
                "timestamp": 1534315695, 			// 时间戳
                "change_rate": "0.033", 			// 涨幅比例
                "change_value": "203", 				// 涨幅值
                "instrument_id": 1, 					// 合约id
                "position_size": "374266", 		// 未平仓位量
                "qty_day": "45270", 					// 当天交易量
                "amount24":"28520.77363", 		// 24小时交易额
                "index_px": "6406.53", 				// 指数价格
                "fair_basis": "0.000000690", 	// 基差率
                "fair_value": "0.00442673", 	// 基差
                "fair_px": "6406.5344267", 		// 标记价
                "rate": {
                    "quote_rate": "0.0003", 	// 计价币借贷利率
                    "base_rate": "0.0006", 		// 基础币借贷利率
                    "interest_rate": "-0.00009999" // 利率
                },
                "premium_index": "-0.0309530479798782534", 	// 溢价指数
                "funding_rate": "0.0001", 									// 当前资金费率
                "next_funding_rate": "-0.0304530", 					// 下一个预计资金费率
                "next_funding_at": "2018-08-15T08:00:01Z" 	// 下一个结算时间(UTC时间)
            }
        ]
    }
}
```

# 合约交易接口

在此类别中，所有接口都需要进行身份验证。

## 撤销合约订单

### 请求Url    

`POST swap/cancelOrders`

### 参数列表 

|    参数名    | 参数类型 | 参数示例  | 参数描述 |
| :----------: | :------: | :-------: | :------: |
| instrumentID |  String  |    11     |  合约id  |
|    orders    | 整型列表 | [1,2,3,4] |          |

> curl请求示例:

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
                "last_px": "6277.5", 					// 最后成交价
                "open": "6074.5", 						// 当天的开盘价
                "close": "6277.5", 						// 当前的收盘价
                "low": "6261.3", 							// 当天的最低价
                "high": "6279", 							// 当天的最高价
                "avg_px": "6274.67",					// 均价
                "last_qty": "20000", 					// 最后一笔交易量,单位张数,如果要显示btc的交易量,																											需要last_qty乘以合约的大小
                "qty24": "45462", 						// 24小时总量,单位张数
                "base_coin_qty":"163.9", 			// 基础币的交易量(币值对前面的币为基础币)
                "quote_coin_qty":"34555.381", // 计价币的交易量(币值对后面的币为计价币)
                "timestamp": 1534315695, 			// 时间戳
                "change_rate": "0.033", 			// 涨幅比例
                "change_value": "203", 				// 涨幅值
                "instrument_id": 1, 					// 合约id
                "position_size": "374266", 		// 未平仓位量
                "qty_day": "45270", 					// 当天交易量
                "amount24":"28520.77363", 		// 24小时交易额
                "index_px": "6406.53", 				// 指数价格
                "fair_basis": "0.000000690", 	// 基差率
                "fair_value": "0.00442673", 	// 基差
                "fair_px": "6406.5344267", 		// 标记价
                "rate": {
                    "quote_rate": "0.0003", 	// 计价币借贷利率
                    "base_rate": "0.0006", 		// 基础币借贷利率
                    "interest_rate": "-0.00009999" // 利率
                },
                "premium_index": "-0.0309530479798782534", 	// 溢价指数
                "funding_rate": "0.0001", 									// 当前资金费率
                "next_funding_rate": "-0.0304530", 					// 下一个预计资金费率
                "next_funding_at": "2018-08-15T08:00:01Z" 	// 下一个结算时间(UTC时间)
            }
        ]
    }
}
```



## 批量提交订单



> curl请求示例:

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


### 请求Url    

`POST swap/batchOrders`

### 请求body 

| 参数名 | 参数类型 |                           参数示例                           |   参数描述    |
| :----: | :------: | :----------------------------------------------------------: | :-----------: |
| orders |  order   | [{"instrument_id":3,"category":1,"client_id":1,"side":4,"position_type":1, "leverage":100,"px":"998","qty":"10","hide_qty":"0"},...] | 批量订单列表  |
| nonce  |   int    |                          1533876299                          | 当前utc时间戳 |

### order

| 参数名        | 参数类型 | 参数示例 | 参数描述                             | 是否必须 |
| ------------- | -------- | -------- | ------------------------------------ | -------- |
| Instrument_id | Int      | 3        | 合约id                               | 是       |
| category      | Int      | 1        | 类型,1:限价,2:市价,3:被动委托        | 是       |
| client_id     | int      | 1        |                                      | 是       |
| side          | int      | 1        | 订单方向,1:开多,2:开空,3:平多,4:平空 | 是       |
| position_type | 开仓方式 | 1        | 开仓方式,1:逐仓,2:全仓。平仓不必传   | 是       |
| leverage      | decimal  | 10       | 杠杆倍数                             | 是       |
| px            | decimal  |          | 价格                                 | 是       |
| qty           | decimal  |          | 数量                                 | 是       |

### Header

| 参数名       | 参数类型 | 参数示例         | 参数描述                            | 是否必须 |
| ------------ | -------- | ---------------- | ----------------------------------- | -------- |
| Content-Type | String   | application/json | 定值                                | 是       |
| EX-Accesskey | String   |                  |                                     | 是       |
| EX-Sign      | int      |                  | MD5({postdata}+{secretKey}+{EX-Ts}) | 是       |
| EX-Ver       | int      | 1008             | 定值                                | 是       |
| EX-Dev       | String   | WEB              | 定值                                | 是       |
| EX-Ts        | Long     | 1541044393000000 | utc时间戳-微妙                      | 是       |



## 单次提交订单

> curl请求示例:

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

### 请求Url    

`POST swap/submitOrder`

### 请求body 

| 参数名        | 参数类型 | 参数示例   | 参数描述                             | 是否必须 |
| ------------- | -------- | ---------- | ------------------------------------ | -------- |
| Instrument_id | Int      | 3          | 合约id                               | 是       |
| category      | Int      | 1          | 类型,1:限价,2:市价,3:被动委托        | 是       |
| client_id     | int      | 1          |                                      | 是       |
| side          | int      | 1          | 订单方向,1:开多,2:开空,3:平多,4:平空 | 是       |
| position_type | 开仓方式 | 1          | 开放方式,1:逐仓,2:全仓,平仓不必传    | 是       |
| leverage      | decimal  | 10         | 杠杆倍数                             | 是       |
| px            | decimal  |            | 价格                                 | 是       |
| qty           | decimal  |            | 数量                                 | 是       |
| nonce         | int      | 1533873979 | 当前时间戳                           | 是       |



## 获取合约账户信息

> curl请求示例:

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
                "account_id": 10, // 账号ID
                "coin_code": "USDT", // 代币名称
                "freeze_vol": "1201.8", // 冻结量
                "available_vol": "8397.65", // 可用余额
                "cash_vol": "0", // 净现金余额
                "realised_vol": "-0.5", // 已实现盈亏
                "earnings_vol": "-0.5", // 已结算收益
                "created_at":  // 创建时间 "2018-07-13T16:48:49+08:00",
                "updated_at":  // 修改时间 "2018-07-13T18:34:45.900387+08:00"
            }
        ]
```

### 请求Url    

` GET swap/accounts`

### 请求参数

> 参数通过urlparam形式传入 

| 参数名        | 参数类型 | 参数示例           | 参数描述 | 是否必须 |
| ------------- | -------- | ------------------ | -------- | -------- |
| instrument_id | Int      | 3                  | 合约id   | 否       |
| String        | USDT     | 合约的计价币种名称 | 否       |          |



## 获取用户仓位

> curl请求示例:

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
                "pid": 10116365,              //仓位id
                "uid": 10,										//用户id
                "instrument_id": 1,
                "cur_qty": "10",							//当前持有量
                "freeze_qty": "0",						//冻结数量
                "close_qty": "0",							//已经平仓量
                "avg_cost_px": "16",					//持仓均价
                "avg_open_px": "16",          //开仓均价
                "avg_close_px": "0",          //平仓均价
                "oim": "100.075",             //原始开仓保证金
                "im": "100.075",							//开仓保证金
                "mm": "50",                   //维持保证金
                "realised_pnl": "-0.075",     //已实现盈亏
                "earnings": "-0.075",         //已实现收益
                "tax": "0",                   //持仓产生的资金费用
                "position_type": 1,           //开仓类型
                "side": 2,                    //仓位方向
                "status": 1,                  //状态  1:持仓  2:系统托管  4:已经平仓
                "errno": 0,                   //平仓原因 
                															1:平仓委托中
																							2:破产委托中
																							3:平仓委托结束
																							4:破产委托结束
																							5:爆仓
																							6:自动减仓(主动发起方)
																							7:自动减仓(被动接收方)
                "created_at": "2018-07-17T03:04:26.108983Z",
                "updated_at": "2018-07-17T03:04:26.098404Z"
            }
        ]
    }
}
```

### 请求Url    

` GET swap/userPositions`

### 请求参数

> 参数通过urlparam形式传入 

| 参数名   | 参数类型 | 参数示例                                                     | 参数描述           | 是否必须 |
| -------- | -------- | ------------------------------------------------------------ | ------------------ | -------- |
| coinCode | String   | USDT                                                         | 合约的计价币种名称 | 是       |
| int      | 1        | 仓位状态 1:持仓中 2:系统委托中 4:已平仓 如果请求参数中的status值为3,标识同时请求持仓中和系统委托中的仓位 如果请求参数中的status值为0或者7,标识同时请求所有状态的仓位 | 是                 |          |
| offset   | int      |                                                              | 1                  |          |
| size     | int      |                                                              | 0                  |          |



## 获取仓位资金费用

> curl请求示例:

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
    // 记录数组按照时间由近及远排序
    "data": [
        {
            "instrument_id": 32785, // 合约ID
            "uid": 2071026819, // 用户ID
            "pid": 2154205116, // 仓位ID
            "fair_px": "0.849940605285", // 产生仓位费用时的合理价格
            "funding_rate": "0", // 产生仓位费用时的资金费率
            "tax": "-149.8912195248", // 手续费,负数表示赚的,正数表示付出费
            "qty": "1053468", // 产生仓位费用时的仓位持仓量
            "created_at": "2019-01-15T00:00:00.708979Z" // 产生仓位费用的时间,UTC时间
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

### 请求Url    

` GET swap/positionTax`

### 请求参数

> 参数通过urlparam形式传入 

| 参数名   | 参数类型   | 参数示例 | 参数描述 | 是否必须 |
| -------- | ---------- | -------- | -------- | -------- |
| coinCode | Int        | 2        | 合约id   | 是       |
| int      | 2154205116 | 仓位id   | 是       |          |



## 获取用户的交易记录

> curl请求示例:

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
        // 交易记录列表,按创建时间由近及远排序
        "trades": [
            {
                "oid": 10116361, 					// taker order id
                "tid": 10116363, 					// trade id
                "instrument_id": 1,     	// 合约ID
                "px": "16",   						// 成交价
                "qty": "10",     					// 成交量
                "make_fee": "0.04",   		// maker 费率
                "take_fee": "0.12",   		// taker 费率
                "created_at": null,   		// 创建时间
                "side": 5,             		// 交易方向
                "change": "0"    					// 如本次交易前的最新交易价是10,本次交易的交易价是11,则																							fluctuation为"1" 
            }
        ]
    }
}
```

### 请求Url    

` GET swap/userTrades`

### 请求参数

> 参数通过urlparam形式传入 

| 参数名   | 参数类型 | 参数示例 | 参数描述        | 是否必须 |
| -------- | -------- | -------- | --------------- | -------- |
| coinCode | Int      | 2        | 合约id          | 是       |
| offset   | Int      | 0        | 偏移量          | 是       |
| size     | int      | 60       | 记录条数,默认60 | 否       |



## 逐仓仓位追加或减少保证金

> curl请求示例:

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

### 请求Url    

` POST swap/changeMargin`

### 请求参数

> 参数通过urlparam形式传入 

| 参数名        | 参数类型 | 参数示例   | 参数描述                  | 是否必须 |
| ------------- | -------- | ---------- | ------------------------- | -------- |
| pid           | Int      | 2          | 仓位id                    | 是       |
| instrument_id | Int      | 0          | 合约id                    | 是       |
| side          | int      | 1          | 1:追加保证金,2:减少保证金 | 是       |
| vol           | int      | 10         | 保证金数量                | 是       |
| nonce         | int      | 1533871871 | 时间戳 单位秒             | 是       |



## 用户订单记录

> curl请求示例:

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
                "oid": 10539098, // 订单id
                "instrument_id": 1, // 合约id
                "pid": 10539088, // 平仓为id
                "uid": 10, // 用户id
                "px": "16", // 订单价格
                "qty": "1", // 订单总量
                "hide_qty": "0", // 订单隐藏量
                "avg_px": "16", // 成交均价
                "cum_qty": "1", // 成交量
                "side": 3,   // 订单方向
                "category": 1, // 订单类型
                "make_fee": "0.00025", // make fee
                "take_fee": "0.012", // take fee
                "origin": "", // 来源
                "created_at": "2018-07-23T11:55:56.715305Z",
                "finished_at": "2018-07-23T11:55:56.763941Z",
                "status": 4, // 状态
                "errno": 0,  // 订单结束原因
                "position_type": 2, // 1:逐仓,2:全仓
                "time_in_force": 1, // 1:普通,2:FOK,3:IOC
                "mfr": "0.00025", // 系统的make手续费率
                "tfr": "0.00075", // 系统的take手续费率
                "self_mfr": "0.00025", // 用户自己的make手续费率,如果没有返回该字段,或者值为0,表示用户的手续费率以系统的配置为准
                "self_tfr": "0.00075", // 用户自己的take手续费率,如果没有返回该字段,或者值为0,表示用户的手续费率以系统的配置为准
                "leverage": "10", // 订单的杠杆大小,
                "client_id": 123123, // 前端自定义id
            }
        ]
    }
}
```

### 请求Url    

` GET swap/userOrders`

### 请求参数

> 参数通过urlparam形式传入 

| 参数名        | 参数类型 | 参数示例 | 参数描述                                                     | 是否必须 |
| ------------- | -------- | -------- | ------------------------------------------------------------ | -------- |
| offset        | Int      | 2        | 偏移量                                                       | 是       |
| instrument_id | Int      | 0        | 合约id                                                       | 是       |
| ize           | int      | 1        | 记录数量                                                     | 是       |
| status        | int      | 10       | 订单状态 1:申报中 2:委托中 4:完成 如果请求参数status=3,标识同时请求申报中和委托中的订单,如果请求参数status=0或者7,标识同时请求所有状态的订单 | 是       |



## 查询订单详情

> curl请求示例:

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
                "oid": 10539098, // 订单id
                "instrument_id": 1, // 合约id
                "pid": 10539088, // 平仓为id
                "uid": 10, // 用户id
                "px": "16", // 订单价格
                "qty": "1", // 订单总量
                "hide_qty": "0", // 订单隐藏量
                "avg_px": "16", // 成交均价
                "cum_qty": "1", // 成交量
                "side": 3,   // 订单方向
                "category": 1, // 订单类型 
                                  该字段采用二进制按位表示法
                                  0~5位表示订单的基本类型,第6位预留,
                                  第7位为1表示:强平委托单
                                  第8位为1表示:爆仓委托单
                                  第9位为1表示:自动减仓委托单
                "make_fee": "0.00025", // make fee
                "take_fee": "0.012", // take fee
                "origin": "", // 来源
                "created_at": "2018-07-23T11:55:56.715305Z",
                "finished_at": "2018-07-23T11:55:56.763941Z",
                "status": 4, // 状态
                "errno": 0,  // 订单结束原因
                								1:用户取消
																2:超时,(暂时没有用)
																3:用户资产不够,转撤销
                                4:用户冻结资产不够
                                5:系统部分转撤销
                                6:部分平仓导致的部分转撤销
                                7:自动减仓导致的部分转撤销
                                8:盈利补偿导致的部分转撤销(暂时没有用)
                                9:仓位错误导致的部分转撤销
                                10:类型非法
                                11:反方向订单存在
                "position_type": 2, // 1:逐仓,2:全仓
                "time_in_force": 1, // 1:普通,2:FOK,3:IOC
                "mfr": "0.00025", // 系统的make手续费率
                "tfr": "0.00075", // 系统的take手续费率
                "self_mfr": "0.00025", // 用户自己的make手续费率,如果没有返回该字段,或者值为0,表示用户的手续费率以系统的配置为准
                "self_tfr": "0.00075", // 用户自己的take手续费率,如果没有返回该字段,或者值为0,表示用户的手续费率以系统的配置为准
                "leverage": "10", // 订单的杠杆大小,
                "client_id": 123123, // 前端自定义id
            }
        ]
    }
}
```

### 请求Url    

` GET swap/queryOrder`

### 请求参数

> 参数通过urlparam形式传入 

| 参数名        | 参数类型 | 参数示例 | 参数描述 | 是否必须 |
| ------------- | -------- | -------- | -------- | -------- |
| instrument_id | Int      | 0        | 合约id   | 是       |
| oid           | Int      | 0        | 订单id   | 是       |



## 查询单笔交易记录详情

> curl请求示例:

```curl
curl --location --request GET 'https://host.com/swap/orderTrades?instrumentID=1&orderID=2064344648'
```

> Response   ：

```response-data
{
    "errno": "OK",
    "message": "Success",
    "data": {
        // 交易记录列表,按创建时间由近及远排序
        "trades": [
            {
                "oid": 10116361, 			// taker order id
                "tid": 10116363, 			// trade id
                "instrument_id": 1,   // 合约ID
                "px": "16",         	// 成交价
                "qty": "10",        	// 成交量
                "make_fee": "0.04",   // make fee
                "take_fee": "0.12",   // take fee
                "created_at": null,   // 创建时间
                "side": 5,            // 交易方向
                "change": "0"    			// 对行情的影响
            }
        ]
    }
}
```

### 请求Url    

` GET swap/orderTrades`

### 请求参数

> 参数通过urlparam形式传入 

| 参数名        | 参数类型 | 参数示例 | 参数描述 | 是否必须 |
| ------------- | -------- | -------- | -------- | -------- |
| instrument_id | Int      | 0        | 合约id   | 是       |
| orderID       | int      | 20644322 | 订单id   | 是       |



## 创建合约账户

> curl请求示例:

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
    "errno": "OK",  // 创建成功
    "message": "Success"
}
```

### 请求Url    

` GET swap/createAccount`

### 请求参数

> 参数通过urlparam形式传入 

| 参数名        | 参数类型 | 参数示例 | 参数描述 | 是否必须 |
| ------------- | -------- | -------- | -------- | -------- |
| instrument_id | Int      | 0        | 合约id   | 是       |



## 获取用户爆仓记录

> curl请求示例:

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
    "type":1, // 爆仓类型,1:部分强平,2:破产,3:ADL主动发起,4:ADL被动发起
    "trigger_px":"16", // 触发价
    "order_px":"16", // 委托价
    "mmr":"0.0013", // 爆仓时的维持保证金率
    "subsidies":"0.018", // 破产系统补贴额度
    "created_at": "2018-07-23T11:55:56.715305Z"
    }
]
    }
}
```

### 请求Url    

` GET swap/userLiqRecords`

### 请求参数

> 参数通过urlparam形式传入 

| 参数名       | 参数类型 | 参数示例 | 参数描述 | 是否必须 |
| ------------ | -------- | -------- | -------- | -------- |
| instrumentID | Int      | 0        | 合约id   | 否       |
| oid          | int      | 1000232  | 订单id   | 否       |



## 提交计划委托

> curl请求示例:

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

### 请求Url    

`POST swap/submitPlanOrder`

### 请求body 

| 参数名        | 参数类型 | 参数示例   | 参数描述                                | 是否必须 |
| ------------- | -------- | ---------- | --------------------------------------- | -------- |
| instrument_id | Int      | 3          | 合约id                                  | 是       |
| category      | Int      | 1          | 类型,1:限价,2:市价,3:被动委托           | 是       |
| client_id     | int      | 1          |                                         | 是       |
| side          | int      | 1          | 订单方向,1:开多,2:开空,3:平多,4:平空    | 是       |
| position_type | 开仓方式 | 1          | 开放方式,1:逐仓,2:全仓,平仓不必传       | 是       |
| leverage      | decimal  | 10         | 杠杆倍数                                | 是       |
| px            | decimal  |            | 触发价格                                | 是       |
| qty           | decimal  |            | 数量                                    | 是       |
| nonce         | int      | 1533873979 | 当前时间戳                              | 是       |
| trigger_type  | int      |            | 价格类型,1:交易价,2:标记价;4:指数价     | 是       |
| trend         | int      | 1          | 价格方向,1:看涨,2:看跌,否则返回无效参数 | 是       |
| exec_px       | int      | 980        | 执行价格                                |          |
| cycle         | Int      | 10         | 委托周期,单位小时                       |          |



## 取消计划委托

> curl请求示例:

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
        // 取消成功的列表
        "succeed": [ 
            10116356,
            10116357
        ],
        // 取消失败的列表
        "failed": null
    }
}
```

### 请求Url    

`POST swap/cancelPlanOrders`

### 请求body 

| 参数名        | 参数类型 | 参数示例 | 参数描述       | 是否必须 |
| ------------- | -------- | -------- | -------------- | -------- |
| instrument_id | Int      | 3        | 合约id         | 是       |
| orders        | Int list | [1,2,3]  | 合约订单id列表 | 是       |



## 获取用户计划委托记录

> curl请求示例:

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
                "errno": 1   //错误码 0:触发成功 1:用户取消 2:超时 3:用户资产不够 4:订单将触发强平 5:订单无效 6:用户资产正处在托管中 7:仓位不存在 8:仓位量不够 9:仓位已经被关闭 10:合约已经暂停交易 11:反方向订单存在 12:合约已经下线 13:未知错误
            }
        ]
    }
}
```

### 请求Url    

` GET swap/userLiqRecords`

### 请求参数

> 参数通过urlparam形式传入 

| 参数名       | 参数类型 | 参数示例 | 参数描述 | 是否必须 |
| ------------ | -------- | -------- | -------- | -------- |
| instrumentID | Int      | 0        | 合约id   | 是       |
| offset       | int      | 0        | 偏移量   | 是       |
| size         | int      | 10       | 大小     | 否       |
| status       | int      |    1      |   状态       |     是   |





#Websocket 数据推送


`GET 实时数据(WebSocket)`

wss://host.com/swap/realTime

### 地址

- 测试环境 `wss://host.com/swap/realTime`

### 命令

- 基本命令格式发送格式 `{"action":"","args":["arg1", "arg2", "arg3"]}`

- 命令返回格式

  ```undefined
  {
    "action":"<command>",
    "success":true,         // 成功-true, 失败-false
    "group":"<group>",
    "request":{
        // 原始请求
    },
    "error":""  // 失败时有这个字段返回具体错误原因
  }
  ```

- 支持命令列表

  ```undefined
  subscribe   // 订阅
  unsubscribe //取消订阅
  ```

### 主题

- 主题列表

  ```undefined
  这些主题列表是开放主题,不需要做ws认证.
  Trade       //最新成交
  PNL         //自动减仓排名
  Ticker      //实时价格
  OrderBook       //深度
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

1. 除Ticker之外目前所有主题都跟合约ID相关
2. 请求订阅命令，主题列表主题的构成方式为<主题:合约ID>,例如需要订阅合约(2)的实时深度和5分钟行情的命令为 `{"action":"subscribe","args":["OrderBook:2","QuoteBin5m:2"]}`
3. orderbook是差量更新的,当消息的action为1表示全量数据,action为2表示增量数据.不管是全量数据,还是增量数据都是按照盘口排好序的. {"group":"OrderBook:1","action":1,"data":{"asks":[[862810,"8628.1","2666",0]]}} [862810, // 整型数key,方便排序用的 "8628.1", // 价格 "2666", // 量,如果量为0表示该book被删除了 0] // 1表示book中包含爆仓订单

### 订阅数据格式

- 基本格式如下

  ```undefined
  {
    "group":"",
    "data":{
    }
  }
  // 订阅主题不同，data字段格式不同。data的具体以接口返回为准，请求输入对应主题的订阅命令获取
  ```

### 认证

```undefined
如果需要订阅跟用户自己信息相关的数据等私有信息,需要做ws认证
// 参考Api接口Sign计算方式(参数顺序很重要，别搞错顺序了)
{
    "action":"access",
    "args":[
        "Accesskey",       // 用户的Accesskey,必须是字符串
        "api",              // Dev(EX-Dev) 必须是字符串
        "1.0.0",            // Version(EX-Ver）必须是字符串
        "Sign",              // Sign(EX-Sign) 必须是字符串
        "1540286461000000",  // Ts(EX-Ts) 单位:微秒,必须是字符串
        "36000", // 超时时间,只有是合约云接口认证需要      
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
            "position":{ // 仓位信息

            },
            "c_assets":{  // 合约资产

            },
            "s_assets":{  // 现货资产

            }
        }
    ]
}
"group":"UserProperty",表示该用户的的合约数据.以后台业务操作为驱动,推送用户数据的更新.一次推送可能包括多次业务操作,所以data是以数组形式,从数组的头开始,按操作先后顺序存放的用户的一组操作.每组数据的元素包括:action(操作类型),order(订单信息),position(仓位信息),c_assets(合约资产信息),s_assets(现货资产信息).对于订单信息,仓位信息,合约资产信息,现货资产信息,只有当操作这些信息产生了更新,每组数据的元素才会包含该信息.
action:操作类型有
1 :撮合.
   可能产生的影响:订单更新,仓位更新,合约资产更新
2 :提交订单
   可能产生的影响:订单更新,合约资产更新
3 :取消订单
   可能产生的影响:订单更新,仓位更新,合约资产更新
4 :强平取消订单
   可能产生的影响:订单更新,仓位更新,合约资产更新
5 :被动ADL取消订单
   可能产生的影响:订单更新,仓位更新,合约资产更新
6 :部分强平
   可能产生的影响:订单更新,仓位更新,合约资产更新
7 :破产委托
   可能产生的影响:新增订单,仓位更新,合约资产更新
8 :被动ADL撮合成交
   可能产生的影响:订单更新,仓位更新,合约资产更新
9 :主动ADL撮合成交
   可能产生的影响:订单更新,仓位更新,合约资产更新
10 :从现货资产化入到合约资产
   可能产生的影响:合约资产更新,现货资产更新
11 :从合约资产化出到现货资产
   可能产生的影响:合约资产更新,现货资产更新
12 :追加保证金
   可能产生的影响:仓位更新,合约资产更新
13 :减少保证金
   可能产生的影响:仓位更新,合约资产更新
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




# 现货交易
