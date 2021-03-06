### WS: Query Open Orders on Symbol

> Requesting open orders on symbol BTC/USDT

```json
{
    "op": "req", 
    "action": "open-order", 
    "id": "abdad113", 
    "account": "cash", 
    "args": {
        "symbols": "BTC/USDT"
    }
}
```

> Open orders response

```json
{
    "m": "open-order", 
    "id": "abdad113", 
    "accountId": "cshQtyfq8XLAA9kcf19h8bXHbAwwoqDo", 
    "ac": "CASH", 
    "data": [
        {
            "sn": 103, 
            "orderId": "a16edce82c6e0866943712UKrJP1HCA", 
            "s": "BTC/USDT", 
            "ot": "Limit", 
            "t": 1575664233699, 
            "p": "9000", 
            "q": "0.01", 
            "sd": "Buy", 
            "st": "New", 
            "ap": "0", 
            "cfq": "0", 
            "sp": "0", 
            "err": "NULL_VAL", 
            "btb": "0", 
            "bab": "0", 
            "qtb": "0", 
            "qab": "0", 
            "cf": "0", 
            "fa": "USDT"}, 
            ...
    ]
}
```

You can request the open order via websocket by an `open-order` request. 

The request schema:

 Name          | Data Type           | Description                
-------------- | ------------------- | -------------------------- 
 `op`          | `String`            | `req`                      
 `action`      | `String`            | `open-order`  
 `id`          | `String`            | for result match purpose
 `account`     | `String`            | `cash`, `margin`         
 `args:symbols`| `String`            | 


The response schema:

 Name               | Data Type             | Description                   
--------------------| --------------------- | ----------------------------- 
 `m`                | `String`              | `open-order`
 `accountId`        | `String`              | account  
 `ac`               | `String`              | `cash`, `margin`
 `id`               | `String`              | echo id in request
 `data`             | `Order Json Array`    | A list of open order json objects        