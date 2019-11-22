###
### Place New Order

> Place Order - Successful ACK Response (Status 200, code 0)

```json
{
    "code": 0,
    "data": {
        "accountId": "cshQtyfq8XLAA9kcf19h8bXHbAwwoqDo",
        "action": "place-order",
        "info": {
            "id":        "16e607e2b83a8bXHbAwwoqDo55c166fa",
            "orderId":   "16e85b4d9b9a8bXHbAwwoqDoc3d66830",
            "orderType": "Market",
            "symbol":    "BTC/USDT",
            "timestamp":  1573576916201
        },
        "status": "Ack"
    }
}
```

> Place Order - Successful ACCEPT Response (Status 200, code 0)

```json
{
    "code": 0,
    "data": {
        "accountCategory":       "CASH",
        "accountId":             "cshQtyfq8XLAA9kcf19h8bXHbAwwoqDo",
        "avgPrice":              "0",
        "baseAvailableBalance":  "39.2010425",
        "baseTotalBalance":      "39.2010425",
        "errorCode":             "NULL_VAL",
        "execId":                "78271",
        "execInst":              "NULL_VAL",
        "fee":                   "0",
        "feeAsset":               "",
        "filledQty":             "0",
        "m":                     "order",
        "orderId":               "16e60fc831ba8bXHbAwwoqDo3d898cf3",
        "orderQty":              "0.11",
        "orderType":             "Market",
        "quoteAvailableBalance": "1113781.119315161",
        "quoteTotalBalance":     "1114852.305731461",
        "side":                  "Buy",
        "status":                "New",
        "symbol":                "BTC/USDT",
        "transactTime":           1573585191865
    }
}
```


> Place Order - Successful DONE Response (Status 200, code 0)

```json
{
    "code": 0,
    "data": {
        "accountCategory":       "CASH",
        "accountId":             "cshQtyfq8XLAA9kcf19h8bXHbAwwoqDo",
        "avgPrice":              "8846.25",
        "baseAvailableBalance":  "39.2010425",
        "baseTotalBalance":      "39.2010425",        
        "errorCode":             "NULL_VAL",
        "execId":                "78266",
        "execInst":              "NULL_VAL",
        "fee":                   "0.827124375",
        "feeAsset":              "USDT",
        "filledQty":             "0.11",
        "m":                     "order",
        "orderId":               "16e60f6601ba8bXHbAwwoqDo345548c3",
        "orderQty":              "0.11",
        "orderType":             "Market",
        "quoteAvailableBalance": "1114852.305731461",
        "quoteTotalBalance":     "1114852.305731461",
        "side":                  "Buy",
        "status":                "Filled",
        "symbol":                "BTC/USDT",
        "transactTime":           1573584789682
    }
}
```

> Place Order - Error Response (Status 200, code 0)

```json
{
    "code": 0,
    "data": {
        "accountId": "cshQtyfq8XLAA9kcf19h8bXHbAwwoqDo",
        "action": "place-order",
        "info": {
            "code":     300002,
            "id":      "ZPJNQSMGhGGse6y5e2Ft79MFEUCbBF9U",
            "message": "Order quantity is too small.",
            "reason":  "INVALID_QTY"
        },
        "status": "Err"
    }
}
```

Place a new order.

**HTTP Request**

`POST <account-group>/api/pro/{account-category}/order`

Set `account-category` to`cash` for cash account and `margin` for margin account. 

**Signature**

You should sign the message in header as specified in **Authenticate a RESTful Request** section.

**Prehash String**

`<timestamp>+order`


#### Request Parameters

Name       | Type   |Required| Value Range                          | Description
-----------|--------|------- | -------------------------------------|---------------
symbol     | String |  Yes   |                                      |
time       | Long   |  Yes   |milliseconds since UNIX epoch in UTC  |We do not process request placed more than 30 seconds ago.
orderQty   | String |  Yes   |                                      |Order size. Please set scale properly for each symbol.
orderType  | String |  Yes   |["market", "limit"]                   |Order type
side       | String |  Yes   |["buy", "sell"]                       |  
id         | String |  No    |32 chars(letter and digit number only)|Please generate unique ID for each trade, we echo it back to help you match response with request.
orderPrice | String |  No    |                                      |The limit price for limit order. Please set price scale properly.
stopPrice  | String |  No    |                                      |Trigger price of stop limit order
postOnly   | Boolean|  No    |["true", "false"]                     |
timeInForce| String |  No    |["GTC", "IOC"]                        |GTC: good-till-canceled; IOC: immediate-or-cancel. GTC by default.
respInst   | String |  No    |["ACK", "ACCEPT", "DONE"]             |Response instruction. Refer to "Response" below. "ACK" by default.  

The table below shows how to correctly configure order of different types: (o - required, x - optional)

Name        | Market | Limit | StopMarket | StopLimit
----------- | ------ | ----- | ---------- | ---------
id          | x      | x     | x          | x    
time        | o      | o     | o          | o
symbol      | o      | o     | o          | o
orderPrice  |        | o     |            | o
orderQty    | o      | o     | o          | o
orderType   | o      | o     | o          | o
side        | o      | o     | o          | o
postOnly    |        | x     |            | 
stopPrice   |        |       | o          | o
timeInForce |        | x     |            | 


#### Order Request Criteria

When placing a new **limit** order, the request parameters must meet all criteria defined in the [Products API](#list-all-products):

* The order notional must be within range `[minNotional, maxNotional]`. For limit orders, the order notional is defined as the product of `orderPrice` and `orderQty`.
* `orderPrice` and `stopPrice` must be multiples of `tickSize`.
* `orderQty` must be a multiple of `lotSize`.
* If you are trading using the margin account, `marginTradable` must be `true`. 
* For cash trading, you must have sufficient balance to fund the order. 
  * for buy orders, if `commissionType=Quote`, the quote asset balance must be no less than `orderPrice * orderQty * (1 + commissionReserveRate)`. For all other `commissionType`s (`Base` and `Received`), the quote asset balance must be no less than `orderPrice * orderQty`
  * for sell orders, if `commissionType=Base`, your base asset balance must be no less than `orderQty * (1 + commissionReserveRate)`, For other `commissionType`s (`Received` and `Quote`), the base asset balance must be no less than `orderQty`.
* For margin trading, you must make sure you have sufficient **max sellable amount** to fund the order. 


#### Response

In "Err" response, *id* is the id provided by user when placing order; for other responses, *orderId* is the id generated by server following "Order id generation method" above, and this is the id you should provide for future order query or cancel. 

In case you want to cancel an order before response from server, you could figure out the orderId following "Order id generation method" above.

*ACK*

Response with status 'Ack' to indicate new order request received by our server and passed some basic order field check. This is the default response type.

*ACCEPT*

Response with status "New" to indicate new order request is accepted by our match engine. Return normal 'Ack' response if no 'New' status update within 5 seconds.

*DONE*

Response with status "PartiallyFilled", "Filled", or "Rejected" to indicate the order request is partially filled, fully filled, or rejected by matching engine. Return normal 'Ack' response if no order status update within 5 seconds. 

*ERR*

Response with status "Err" to provide detailed error information on the order request. 


Error Response Messages (TODO: verify HTTP Status Code)

HTTP Status Code | Error Code | Reason           | Example
---------------- | ---------- | ---------------- | ----------------------------------------------------------------------
400              | xxx        | Parameter Error  | 


**Code Sample**

Refer to sample python code [place_order](https://github.com/gdm-exchange/bitmax-api-demo/blob/master/python/place_order.py)