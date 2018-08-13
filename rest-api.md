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


