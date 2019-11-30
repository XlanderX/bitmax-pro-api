### WS: Place Order 

> Request to place new order

```json
{
    "op": "req",
    "action": "place-Order",
    "args": {
        "time":        1573772908483,
        "coid":       "11eb9a8355fc41bd9bf5b08bc0d18f6b",
        "symbol":     "EOS/USDT",
        "orderPrice": "3.27255",
        "orderQty":   "30.557210737803853",
        "orderType":  "limit",
        "side":       "buy",
        "postOnly":    false,
        "respInst":   "ACK"
    }
}
```

> Successful ACK message

```json
{
    "m": "order",
    "accountId": "cshQtyfq8XLAA9kcf19h8bXHbAwwoqDo",
    "action": "place-order",
    "status": "Ack",
    "info": {
        "symbol":    "ETC/USDT",
        "orderType": "Market",
        "timestamp":  1573623067556,
        "id":        "8a7612a36ab44766abec4bbb45fd84ea",
        "orderId":   "16e83845dcdsimtrader00008c645f67"
    }
}
```

> Error response message

```json
{
    "m": "order",
    "accountId": "cshQtyfq8XLAA9kcf19h8bXHbAwwoqDo",
    "action": "place-order",
    "status": "Err",
    "info": {
        "id":      "69c482a3f29540a0b0d83e00551bb623",
        "symbol":  "ETH/USDT",
        "code":     300011,
        "message": "Not Enough Account Balance",
        "reason":  "INVALID_BALANCE"
    }
}
```

Place order via websocket 

**Request**

Make new order request follow the general websocket request rule, with proper place new order parameters as specified in rest api for *args* field.

see [placing order via RESTful API](#place-new-order).

**Response**

Respond with *m* field as *order*, and *action* field as *place-order*; *status* field to indicate if this is a successful *Ack* or failed *Err*.

*ACK* 

With *status* field as *Ack* to indicate this new order request pass some basic sanity check, and has been sent to matching engine. 

*info* field provide some detail: if you provide *id* in your request, it will be echoed back as *id* to help you identify; we also provide server side generated *orderId*, which is the id you should use for future track or action on the order.  


*ERR* 

With *status* field as *Err* to indicate there is some obvisous errors in your order. 

*info* field provide some detail: if you provide *id* in your request, it will be echoed back as *id* to help you identify; we also provide error *code*, *reason*, and *message* detail.