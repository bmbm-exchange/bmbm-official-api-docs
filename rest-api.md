# Public Rest API for BMBM
# General API Information
* The base endpoint is: **http://api.bmbm.com**
* All endpoints return either a JSON object or array.
* All time and timestamp related fields are in milliseconds.
* HTTP `4XX` return codes are used for for malformed requests; the issue is on the sender's side.
* HTTP `5XX` return codes are used for internal errors; the issue is on BMBM's side.
  It is important to **NOT** treat this as a failure operation; the execution status is **UNKNOWN** and could have been a success.
* Any endpoint can return an ERROR; the error payload is as follows:
```javascript
{
  "code": 1102,
  "msg": "Invalid symbol."
}
```

* Specific error codes and messages defined in another document.
* For `GET` endpoints, parameters must be sent as a `query string`.
* For `POST`, `PUT`, and `DELETE` endpoints, the parameters may be sent as a
  `query string` or in the `request body` with content type
  `application/x-www-form-urlencoded`. You may mix parameters between both the
  `query string` and `request body` if you wish to do so.
* Parameters may be sent in any order.
* If a parameter sent in both the `query string` and `request body`, the
  `query string` parameter will be used.
  
# Public API Endpoints
## Terminology
* `base asset` refers to the asset that is the `quantity` of a symbol.
* `quote asset` refers to the asset that is the `price` of a symbol.
## ENUM definitions
**Kline/Candlestick chart intervals:**
m -> minutes; h -> hours; d -> days; w -> weeks;
* 1m
* 3m
* 5m
* 15m
* 30m
* 1h
* 2h
* 4h
* 6h
* 12h
* 1d
* 1w
## General endpoints
### Check server time
```
GET /api/v1/time
```
Test connectivity to the Rest API and get the current server time.

**Parameters:**
NONE

**Response:**
```javascript
{
  "serverTime": 1534149050000
}
```

## Market Data endpoints
### Order book
```
GET /api/v1/depth
```

**Parameters:**

Name | Type | Mandatory | Description
------------ | ------------ | ------------ | ------------
symbol | STRING | YES |
limit | INT | NO | Default 100; max 500.

**Response:**

```
{
  "lastUpdateId": 45,
  "bids": [
    [
      "1.00007927",     // PRICE
      "1.00000000"      // QTY
    ]
  ],
  "asks": [
    [
      "1.00007926",
      "1.00000000"
    ]
  ]
}
```



### Recent trades list
```
GET /api/v1/trades
```
Get recent trades (up to last 500).

**Parameters:**

Name | Type | Mandatory | Description
------------ | ------------ | ------------ | ------------
symbol | STRING | YES |
limit | INT | NO | Default 500; max 1000.

**Response:**

```
[
  {
    "id": 42,
    "price": "0.00006500",
    "qty": "8.00000000",
    "time": 1525593971456,
    "isBuyerMaker": true
  }
]
```

### Kline
```
GET /api/v1/klines
```

**Parameters:**

Name | Type | Mandatory | Description
------------ | ------------ | ----------- | ------------
symbol | STRING | YES |
interval | ENUM | YES | 
since | LONG | NO | 
limit | INT | NO | Default 500; max 1000.

**Response:**

```
[
  [
    "1499040000000", // Open time
    "0.01634790", // Open
    "0.80000000", // High
    "0.01575800", // Low
    "0.01577100", // Close
    "148976.11427815", // Volume	
    "14.11427815" // Taker buy volume
  ]
]
```

### 24hr ticker price change statistics
```
GET /api/v1/ticker/24hr
```

**Parameters:**

Name | Type | Mandatory | Description
------------ | ------------ | ----------- | ------------
symbol | STRING | YES |

**Response:**

```
{
  "symbol": "CZRETH",
  "priceChange": "0.00003000",
  "priceChangePercent": "50",
  "weightedAvgPrice": "0.00005438",
  "lastPrice": "0.00009000",
  "bidPrice": "0.00007926",
  "askPrice": "0.00009000",
  "openPrice": "0.00006000",
  "highPrice": "0.00009000",
  "lowPrice": "0.00005401",
  "volume": "113533.21330000",
  "total": "6.17348378",
  "takerBuyVolume": "1000.00000000",
  "takerBuyTotal": "0.05000000",
  "count": 30
}
OR
[
  {
   "symbol": "CZRETH",
   "priceChange": "0.00003000",
   "priceChangePercent": "50",
   "weightedAvgPrice": "0.00005438",
   "lastPrice": "0.00009000",
   "bidPrice": "0.00007926",
   "askPrice": "0.00009000",
   "openPrice": "0.00006000",
   "highPrice": "0.00009000",
   "lowPrice": "0.00005401",
   "volume": "113533.21330000",
   "total": "6.17348378",
   "takerBuyVolume": "1000.00000000",
   "takerBuyTotal": "0.05000000",
   "count": 30
 }
]
```
### 24hr mini ticker price change statistics
```
GET /api/v1/miniticker/24hr
```

**Parameters:**

Name | Type | Mandatory | Description
------------ | ------------ | ----------- | ------------
symbol | STRING | YES |

**Response:**

```
{
  "symbol": "CZRETH",
  "lastPrice": "0.00009000",
  "openPrice": "0.00006000",
  "highPrice": "0.00009000",
  "lowPrice": "0.00005401",
  "volume": "113533.21330000", //基础货币24小时成交量
  "total": "0.05000000", //计价货币货币24小时成交量
  "takerBuyVolume": "1000.00000000", //基础货币24小时流入
  "takerBuyTotal": "0.05000000" //计价货币24小时流入
}
OR
[
  {
    "symbol": "CZRETH",
    "lastPrice": "0.00009000",
    "openPrice": "0.00006000",
    "highPrice": "0.00009000",
    "lowPrice": "0.00005401",
    "volume": "113533.21330000",
    "total": "0.05000000",
    "takerBuyVolume": "1000.00000000",
    "takerBuyTotal": "0.05000000"
  }
]
```





