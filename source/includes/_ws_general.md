# WebSocket 


## URL 

`<account-group>/api/pro/stream`

In order to authorize the sessionm you must include `<account-group>` in the URL. Without `<account-group>`, you can 
only subscribe to public data. 



## Keep the Connection Alive via Ping/Pong

In order to keep the websocket connection alive, you have two options, detailed below.


#### Method 1: Responding to Server ping messages

> Server pushed ping message 

```
{ "op": "ping" }
```

> Client responds to server pushed ping 

```
{ "op": "pong" }
```

If the server doesn't receive any client message after a while, it will send a `ping` message to the client. Once the `ping` message is received,
the client should promptly send a `pong` message to the server. If you missed two consecutive `ping` messages, the session will be disconnected. 


#### Method 2: Sending ping messages to Server 

> Client initiated ping message 

```
{ "op": "ping" }
```

> Server responds to client ping 

```
{ "m":"pong", "code":0, "ts":1574260701259 }
``` 

You can also send `ping` message to the server every 15 seconds to keep the connection alive. The server will stop sending `ping` message 
for 30 seconds if a client initiated `ping` message is received. 




## Data Channels 


## Subscription 

> Use `wscat` from Node.js to connect to websocket data.

```bash
# install wscat if you haven't
npm install -g wscat

# Connect to websocket
wscat -c wss://bitmax.io/0/api/pro/stream -x '{"op":"sub", "ch": "depth:BTMX/USDT"}'
```

> You can also setup authorized session

```bash
@ToDo
```

### Request Body Schema 

The standard messages to subscribe(`sub`)/unsubscribe(`unsub`) to a data channel is an JSON object with fields:

 Field | Type               | Description                                                                                    
-------| ------------------ | ---------------------------------------------------------------------------------------------- 
 `op`  | `String`           | `sub` or `unsub`                                                                               
 `id`  | `Optional[String]` | user specified UUID, if provided, the server will echo back this value in the response message 
 `ch`  | `String`           | name of the data channel with optional arguments, see below for details                        


###  Customize Channel content with `ch`

You can subscription to different data channels by setting `ch` to `name:<arg1>[:<arg2>]` with `arg1` being the required argument 
and `arg2` being optinal:

 Type    | Value                        | Description                                      
-------- | ---------------------------- | ------------------------------------------------ 
 public  | `depth:<symbol>`             |                                                  
 public  | `bbo:<symbol>`               |                                                  
 public  | `trade:<symbol>`             |                                                  
 public  | `bar:<symbol>`               |                                                  
 public  | `ticker:<symbol>`            |                                                  
 public  | `ref-px:<symbol>`            | Reference prices used by margin risk Calculation 
 Private | `order:<account>`            | Order Update Stream                              
 Private | `order:<account>[:<symbol>]` |                                                  

You can subscribe/unsubscribe one channel per subscription message. You can subscribe to multiple data channels by sending multiple 
subscription messages. However, the exchange limits the total number of data channels per client (**NOT per session**) according to 
the following rules:

@ToDo rule: maximum number of channels 


## Operation or Data Request

You could place/cancel order and take snapshot for some data via websocket.

### Request Schema

All operation or data request follow the same uniform format:

 Field   | Type                 | Description                                                                                    
---------| ---------------------| ---------------------------------------------------------------------------------------------- 
 `op`    |`String`              | `req`                                                                               
 `id`    |`Optional[String]`    | user specified UUID, if provided, the server will echo back this value in the response message 
 `action`|`String`              | name of the request action with optional arguments, see below for details    
 `args`  |`List of [key: value]`| Each `action` has different args, please see each action for detail.


### Response 

Response follow the following schema

 Field | Type               | Description                                                                                    
-------| ------------------ | ---------------------------------------------------------------------------------------------- 
 `m`  | `String`            | action field from request                                                                               
 `id`  | `Optional[String]` | user specified UUID, if provided, the server will echo back this value in the response message 
 `action`| `String`           | name of the request action with optional arguments, see below for details    
 `args` | `List of [key: value]` | Each `action` has different args, please see each action for detail.


*Ack or Data*

*Error*
