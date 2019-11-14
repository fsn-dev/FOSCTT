# FUSION JSON RPC API

FUSION RPC 与以太坊的[JSON-RPC](https://github.com/ethereum/wiki/wiki/JSON-RPC)和[web.js](https://github.com/ethereum/web3.js)API是兼容的。除此之外，FUSION还扩展了API，扩展的API包括票证，资产，时间所，USAN，交换，抵押等。

## JSON RPC

[JSON](http://json.org/) 是一个轻量级的数据交换格式。它能够表示数字，字符串，值的有序序列以及名字/值的集合。

[JSON-RPC](http://www.jsonrpc.org/specification) 是一个无状态的轻量级远程调用（RPC）协议。首先，它定义了一些数据结构机器处理规则。它与传输无关，可以在同一个调用中，使用sockets，通过HTTP或者在许多各种消息传递环境中使用。它使用JSON ([RFC 4627](http://www.ietf.org/rfc/rfc4627.txt))作为数据格式。


## JavaScript API

要从javasc应用程序内部与fusion节点进行交互，请使用[web3.js](https://github.com/ethereum/web3.js)库，它为RPC方法提供了一个方便的接口。FUSION扩展了Javascript库，更多详情请查看[JavaScript API](https://fusionapi.readthedocs.io/)

## JSON-RPC Endpoint

Online RPC service:
在线RPC服务

测试网: https://testnet.fsn.dev/api

主网: https://fsn.dev/api

默认JOSN-RPC端口

| 客户端 | URL |
|-------|:------------:|
| Go |http://localhost:9000 |


你可以使用`--rpc`开启HTTP JSON-RPC服务

```bash
efsn --rpc
```

将默认端口（9000）和监听地址（localhost）更改为：

```bash
efsn --rpc --rpcaddr <ip> --rpcport <portnumber>
```

如果从浏览器访问RPC，需要启动CROS。否则，Javascript调用将收到同源策略的限制，请求将失败。


```bash
efsn --rpc --rpccorsdomain "http://localhost:9000"
```

The JSON RPC can also be started from the efsn console using the `admin.startRPC(addr, port)` command.
JSON RPC服务也可以在efsn 的控制台使用`admin.startRPC(addr,port)`命令开启

## Curl Examples Explained

下面的curl选项可能返回一个响应不兼容的内容类型，这是因为--data选项将内容设置成了application/x-www-form-urlencoded。如果你的节点经常出现这种问题，在开始curl请求的时候加上"Content-Type: application/json"

以下示例不包括必须以curl e.x. 127.0.0.1:9000结尾的这种URL/IP + port的组合，
The examples also do not include the URL/IP & port combination which must be the last argument given to curl e.x. 127.0.0.1:9000

## JSON-RPC methods

* [fsn_ticketPrice](#fsn_ticketPrice)
* [fsn_getStakeInfo](#fsn_getStakeInfo)
* [fsn_getBlockReward](#fsn_getBlockReward)
* [fsntx_buyTicket](#fsntx_buyTicket)
* [fsn_allTickets](#fsn_allTickets)
* [fsn_allTicketsByAddress](#fsn_allTicketsByAddress)
* [fsn_totalNumberOfTickets](#fsn_totalNumberOfTickets)
* [fsn_totalNumberOfTicketsByAddress](#fsn_totalNumberOfTicketsByAddress)
* [fsntx_assetToTimeLock](#fsntx_assetToTimeLock)
* [fsntx_timeLockToTimeLock](#fsntx_timeLockToTimeLock)
* [fsntx_timeLockToAsset](#fsntx_timeLockToAsset)
* [fsn_getTimeLockBalance](#fsn_getTimeLockBalance)
* [fsn_getAllTimeLockBalances](#fsn_getAllTimeLockBalances)
* [fsntx_genAsset](#fsntx_genAsset)
* [fsntx_decAsset](#fsntx_decAsset)
* [fsntx_incAsset](#fsntx_incAsset)
* [fsntx_sendAsset](#fsntx_sendAsset)
* [fsn_getAllBalances](#fsn_getAllBalances)
* [fsn_getLatestNotation](#fsn_getLatestNotation)
* [fsntx_genNotation](#fsntx_genNotation)
* [fsn_getNotation](#fsn_getNotation)
* [fsn_getAddressByNotation](#fsn_getAddressByNotation)
* [fsn_getAsset](#fsn_getAsset)
* [fsn_getBalance](#fsn_getBalance)
* [fsntx_makeSwap](#fsntx_makeSwap)
* [fsn_getSwap](#fsn_getSwap)
* [fsntx_takeSwap](#fsntx_takeSwap)
* [fsntx_recallSwap](#fsntx_recallSwap)
* [fsntx_makeMultiSwap](#fsntx_makeMultiSwap)
* [fsn_getMultiSwap](#fsn_getMultiSwap)
* [fsntx_takeMultiSwap](#fsntx_takeMultiSwap)
* [fsntx_recallMultiSwap](#fsntx_recallMultiSwap)
* [fsntx_isAutoBuyTicket](#fsntx_isAutoBuyTicket)
* [miner_startAutoBuyTicket](#miner_startAutoBuyTicket)
* [miner_stopAutoBuyTicket](#miner_stopAutoBuyTicket)
* [fsn_getTransactionAndReceipt](#fsn_getTransactionAndReceipt)
* [fsn_allInfoByAddress](#fsn_allInfoByAddress)

## JSON RPC API Reference

***

#### fsn_ticketPrice

返回当前区块高度的票价

##### Parameters

`String|HexNumber|TAG` - 区块高度的整数类型，或者是`"latest"`, `"earliest"` 和 `"pending"`字符串类型


```js
params: [
  "latest"  // 最新区块状态
]
```

##### Return

`QUANTITY` - 以wei为单位的当前的整数票价.

##### Example
```js
// 请求
curl -X POST -H "Content-Type":application/json --data '{"jsonrpc":"2.0","method":"fsn_ticketPrice","params":["latest"],"id":1}'

// 结果
{
  "jsonrpc": "2.0",
  "id": 1,
  "result": "5000000000000000000000"
}

```

***

#### fsn_getStakeInfo

返回一些矿工的信息

##### 参数

`String|HexNumber|TAG` - 区块高度的整数类型，或者是`"latest"`, `"earliest"` 和 `"pending"`字符串类型

```js
params: [
  "latest"  // 最新区块状态
]
```

##### 返回

`DATA`  - all miners' address, total number of miners and tickets.
`DATA`  - 所有矿工的地址，矿工的数量以及票数

##### 例子
```js
// 请求
curl -X POST -H "Content-Type":application/json --data '{"jsonrpc":"2.0","method":"fsn_getStakeInfo","params":["latest"],"id":1}'

// 结果
{
  "jsonrpc": "2.0",
  "id": 1,
  "result": {
    "stakeInfo": {
      "0x3a1b3b81ed061581558a81f11d63e03129347437": 2
    },
    "summary": {
      "totalMiners": 1,
      "totalTickets": 2
    }
  }
}

```
#### fsn_getBlockReward 

返回区块奖励

##### Parameters

`String|HexNumber|TAG` - 区块高度的整数类型，或者是`"latest"`, `"earliest"` 和 `"pending"`字符串类型

```js
params: [
  "latest"  // 最新区块的状态
]
```

##### 返回值

`DATA`  - 需要查询的区块的奖励

##### 例子
```js
// 请求
curl -X POST -H "Content-Type":application/json --data '{"jsonrpc":"2.0","method":"fsn_getBlockReward","params":["latest"],"id":1}'

// 返回结果
{
  "jsonrpc":"2.0",
  "id":1,
  "result":"2500021952000000000"
}

```
***

#### fsntx_buyTicket

Return `hash` if successful ticket purchase. (Account has been unlocked)
如果买票成功，将会返回交易的哈希值（账户已解锁状态）

##### 参数

`DATA`, from - 需要买票的账户

```js
params: [{"from":"0x5b15a29274c74cd7cae59cabf656873a0ea706ac"}]
```

##### 返回值

`DATA`, 32字节hash - 交易哈希

##### 例子
```js
// 请求
curl -X POST -H "Content-Type":application/json --data '{"jsonrpc":"2.0","method":"fsntx_buyTicket","params":[{"from":"0x5b15a29274c74cd7cae59cabf656873a0ea706ac"}],"id":1}'

// 结果
{
  "jsonrpc":"2.0",
  "id":1,
  "result":"0x7bee2c9931da61d53da0cfbbc35b7614dd0283959f2e6023dc7792baeafe97f5"
}

```

***

#### fsn_allTickets

返回当前所有账户以及各个账户所拥有的总票数

##### Parameters

`String|HexNumber|TAG` - 区块高度的整数类型，或者是`"latest"`, `"earliest"` 和 `"pending"`字符串类型

```js
params: [
  "latest"  // 最新区块状态
]
```

##### 返回值

`DATA` - All tickets.

##### 示例
```js
// 请求
curl -X POST -H "Content-Type":application/json --data '{"jsonrpc":"2.0","method":"fsn_allTickets","params":["latest"],"id":1}'

// 返回结果
{
  "jsonrpc":"2.0",
  "id":1,
  "result":{
    "0x07d523bfcfc6d1cb5973403e799b7808f94ea0a6b3fca745a9d814c3813015ca":
    {
      "Owner":"0x3a1b3b81ed061581558a81f11d63e03129347437",
      "Height":132,
      "StartTime":1567405502,
      "ExpireTime":1569997502,
      "Value":5000000000000000000000
      },
    "0x0932192a10e7be74ea33ebd83b4cdffa613c0bd896803ac03b67b52f06cc20d7":
    {
      "Owner":"0x0963a18ea497b7724340fdfe4ff6e060d3f9e388",
      "Height":113,
      "StartTime":1567405255,
      "ExpireTime":1569997255,
      "Value":5000000000000000000000
      }
    }
}
```

***

#### fsn_allTicketsByAddress

返回需要查询地址的所有票数

##### 参数

1. `DATA`, address - 将要查询票数的地址
2. `String|HexNumber|TAG` - 区块高度的整数类型，或者是`"latest"`, `"earliest"` 和 `"pending"`字符串类型

```js
params: [
  "0x5b15a29274c74cd7cae59cabf656873a0ea706ac",
  "latest"  // state at the latest block
]
```

##### 返回值

`DATA` - 改地址的所有票数

##### Example
```js
// 请求
curl -X POST -H "Content-Type":application/json --data '{"jsonrpc":"2.0","method":"fsn_allTicketsByAddress","params":["0x5b15a29274c74cd7cae59cabf656873a0ea706ac","latest"],"id":1}

// 返回结果
{
  "jsonrpc":"2.0",
  "id":1,
  "result":
  {
    "0x5bd28f3dbe5d56294c4f8db4f6db86df2210544e1d17d127930742a1f0b7edda":
    {
      "Owner":"0x5b15a29274c74cd7cae59cabf656873a0ea706ac",
      "Height":180,
      "StartTime":1567406170,
      "ExpireTime":1569998170,
      "Value":5000000000000000000000
      }
    }
}
```

***

#### fsn_totalNumberOfTickets

返回所有地址的总票数

##### 参数

`String|HexNumber|TAG` - 区块高度的整数类型，或者是`"latest"`, `"earliest"` 和 `"pending"`字符串类型

```js
params: [
  "latest"  // 最新区块状态
]
```

##### 返回值

`Number` - The total number of all tickets.

##### 示例
```js
// 请求
curl -X POST -H "Content-Type":application/json --data '{"jsonrpc":"2.0","method":"fsn_totalNumberOfTickets","params":["latest"],"id":1}'

// 返回结果
{
  "jsonrpc":"2.0",
  "id":1,
  "result":67
}
```

***

#### fsn_totalNumberOfTicketsByAddress

Return the total number of tickets by address.
返回某个地址的总票数

##### Parameters

1. `String|Address`, 20长度的字节 - 需要查询的账户的地址
2. `String|HexNumber|TAG` - 区块高度的整数类型，或者是`"latest"`, `"earliest"` 和 `"pending"`字符串类型

```js
params: [
  "0x5b15a29274c74cd7cae59cabf656873a0ea706ac",
  "latest"  // 最新的区块状态
]
```

##### Return

`Number` - 该地址所有的票数

##### Example
```js
// Request
curl -X POST -H "Content-Type":application/json --data '{"jsonrpc":"2.0","method":"fsn_totalNumberOfTicketsByAddress","params":["0x5b15a29274c74cd7cae59cabf656873a0ea706ac","latest"],"id":1}'

// Result
{
  "jsonrpc":"2.0",
  "id":1,
  "result":1
}
```

***

#### fsntx_assetToTimeLock

Convert assets to timelock balance.(Account has been unlocked)
将资产转换为时间锁余额。（账户已解锁）

##### Parameters

1. `String|Address`, from - 发送账户的地址
2. `Number`, gas -  (可选，默认值：待定) 此次交易需要消耗的gas数量 (退换未使用的gas).
3. `String|BigNumber`, gasPrice - (可选) 此次交易中需要消耗gas的价格.
4. `Number`, nonce -  (可选) nonce是一个整数类型.允许一笔pending状态的交易使用相同的nonce.
5. `String|Hash`, asset -资产的哈希.
6. `String|Address|Number`, to|toUSAN - 接收方的地址 | 接收方的短地址，整数类型.
7. `String|BigNumber`, value - 需要发送的值.
8. `String|HexNumber`, start - (可选)时间锁代币的开始时间
9. `Number`, end - (可选) 时间锁代币的结束时间.


```js
1.params: [{
  "asset":"0xffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffff",
  "from":"0x5b15a29274c74cd7cae59cabf656873a0ea706ac",
  "to":"0x5b15a29274c74cd7cae59cabf656873a0ea706ac",
  "start":"0x5D6CD2FC", //2019/9/2 16:29:48
  "end":"0x5D945FFC", //2019/10/2 16:29:48
  "value":"0x1BC16D674EC80000" //2,000,000,000,000,000,000
}]
2.params: [{
  "asset":"0xffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffff",
  "from":"0x5b15a29274c74cd7cae59cabf656873a0ea706ac",
  "toUSAN":104,
  "start":"0x5D6CD2FC", //2019/9/2 16:29:48
  "end":"0x5D945FFC", //2019/10/2 16:29:48
  "value":"0x1BC16D674EC80000" //2,000,000,000,000,000,000
}]
```

##### Return

`DATA`, 32字节，返回交易哈希

##### Example
```js
// Request
1.curl -X POST -H "Content-Type":application/json --data '{"jsonrpc":"2.0","method":"fsntx_assetToTimeLock","params":[{"asset":"0xffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffff","from":"0x5b15a29274c74cd7cae59cabf656873a0ea706ac","to":"0x5b15a29274c74cd7cae59cabf656873a0ea706ac","start":"0x5D6CD2FC","end":"0x5D945FFC","value":"0x1BC16D674EC80000"}],"id":1}'

2.curl -X POST -H "Content-Type":application/json --data '{"jsonrpc":"2.0","method":"fsntx_assetToTimeLock","params":[{"asset":"0xffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffff","from":"0x5b15a29274c74cd7cae59cabf656873a0ea706ac","toUSAN":104,"start":"0x5D6CD2FC","end":"0x5D945FFC","value":"0x1BC16D674EC80000"}],"id":1}'
// Result
{
  "jsonrpc":"2.0",
  "id":1,
  "result":"0xa5c08546790e19974fbbb554878289147e439fff9f57e12cb40e63d1e3ea8096"
}

```

***

#### fsntx_timeLockToTimeLock

Send a time locked asset to another account.(Account has been unlocked)
发送一个时间锁资产给另外一个账户（发送方账户要解锁）

##### Parameters

1. `String|Address`, from - 发送方的账户地址
2. `Number`, gas -  (可选，默认值：待定) 此次交易需要消耗的gas数量 (退换未使用的gas).
3. `String|BigNumber`, (可选) 此次交易中需要消耗gas的价格.
4. `Number`, nonce -  (可选) nonce是一个整数类型.允许一笔pending状态的交易使用相同的nonce.
5. `String|Hash`, asset -  资产的哈希.
6. `String|Address|Number`, to|toUSAN - 接收方的地址 | 接收方的短地址，整数类型.
7. `String|BigNumber`, value -  需要发送的值.
8. `String|HexNumber`, start -  (可选)时间锁代币的开始时间
9. `String|HexNumber`, end - 时间锁代币的结束时间.


```js
1. params: [{
  "asset":"0xffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffff",
  "from":"0x5b15a29274c74cd7cae59cabf656873a0ea706ac",
  "to":"0x07718f21f889b84451727ada8c65952a597b2e78",
  "start":"0x5D6CD2FC",  //2019/9/2 16:29:48
  "end":"0x5D945FFC",   //2019/10/2 16:29:48
  "value":"0x1BC16D674EC80000"  //2,000,000,000,000,000,000‬
  }]

2. params: [{
  "asset":"0xffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffff",
  "from":"0x5b15a29274c74cd7cae59cabf656873a0ea706ac",
  "toUSAN":104,
  "start":"0x5D6CD2FC",  //2019/9/2 16:29:48
  "end":"0x5D945FFC",   //2019/10/2 16:29:48
  "value":"0x1BC16D674EC80000"  //2,000,000,000,000,000,000‬
  }]
```

##### Return

`DATA`, 32字节，返回交易哈希

##### Example
```js
// Request
1. curl -X POST -H "Content-Type":application/json --data '{"jsonrpc":"2.0","method":"fsntx_timeLockToTimeLock","params":[{"asset":"0xffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffff","from":"0x5b15a29274c74cd7cae59cabf656873a0ea706ac","to":"0x07718f21f889b84451727ada8c65952a597b2e78","start":"0x5D6CD2FC","end":"0x5D945FFC","value":"0x1BC16D674EC80000"}],"id":1}'

2. curl -X POST -H "Content-Type":application/json --data '{"jsonrpc":"2.0","method":"fsntx_timeLockToTimeLock","params":[{"asset":"0xffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffff","from":"0x5b15a29274c74cd7cae59cabf656873a0ea706ac","toUSAN":104,"start":"0x5D6CD2FC","end":"0x5D945FFC","value":"0x1BC16D674EC80000"}],"id":1}'

// Result
{
  "jsonrpc":"2.0",
  "id":1,
  "result":"0x7ca4fa378a8ece6f2db1fe41521c80edcda45fff270b0bc12c81ab269c651d27"
}
```

***

#### fsntx_timeLockToAsset

Convert timelock balance to asset.(Account has been unlocked)
将timeLock余额转换为资产（账户已解锁）

##### Parameters

1. `String|Address`, from - 发送账户的地址
2. `Number`, gas -  (可选，默认值：待定) 此次交易需要消耗的gas数量 (退换未使用的gas).
3. `String|BigNumber`, gasPrice -  (可选) 此次交易中需要消耗gas的价格.
4. `Number`, nonce -  ( (可选) nonce是一个整数类型.允许一笔pending状态的交易使用相同的nonce.
5. `String|Hash`, asset -  资产的哈希.
6. `String|Address|Number`, to|toUSAN - 接收方的地址 | 接收方的短地址，整数类型.
7. `String|BigNumber`, value -  需要发送的值.
8. `String|HexNumber`, start - (可选)时间锁代币的开始时间
9. `String|HexNumber`, end - (可选) 时间锁代币的结束时间.

```js
1. params: [{
  "asset":"0xffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffff",
  "from":"0x5b15a29274c74cd7cae59cabf656873a0ea706ac",
  "to":"0x5b15a29274c74cd7cae59cabf656873a0ea706ac",
  "start":"0x5D6CD2FC",  //2019/9/2 16:29:48
  "end":"0x5D945FFC",   //2019/10/2 16:29:48
  "value":"0x1BC16D674EC80000"  //2,000,000,000,000,000,000‬
  }]

2. params: [{
  "asset":"0xffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffff",
  "from":"0x5b15a29274c74cd7cae59cabf656873a0ea706ac",
  "toUSAN":104,
  "start":"0x5D6CD2FC",  //2019/9/2 16:29:48
  "end":"0x5D945FFC",   //2019/10/2 16:29:48
  "value":"0x1BC16D674EC80000"  //2,000,000,000,000,000,000‬
  }]
```

##### Return

`DATA`, 32字节，返回交易的哈希

##### Example
```js
// Request
1.curl -X POST -H "Content-Type":application/json --data '{"jsonrpc":"2.0","method":"fsntx_timeLockToAsset","params":[{"asset":"0xffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffff","from":"0x5b15a29274c74cd7cae59cabf656873a0ea706ac","to":"0x5b15a29274c74cd7cae59cabf656873a0ea706ac","start":"0x5D6CD2FC","end":"0x5D945FFC","value":"0x1BC16D674EC80000"}],"id":1}'

2.curl -X POST -H "Content-Type":application/json --data '{"jsonrpc":"2.0","method":"fsntx_timeLockToAsset","params":[{"asset":"0xffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffff","from":"0x5b15a29274c74cd7cae59cabf656873a0ea706ac","toUSAN":104,"start":"0x5D6CD2FC","end":"0x5D945FFC","value":"0x1BC16D674EC80000"}],"id":1}'

// Result
{
  "jsonrpc":"2.0",
  "id":1,
  "result":"0xb8242235c1f4fa1bcbc22b65bdfd2ab19db97cdfdc3b804fa302f87eafcd69c2"
}
```

***

#### fsn_getTimeLockBalance

返回资产的锁定余额

##### Parameters

1. `String|Hash`, assetID -资产的哈希.
2. `String|Address`, address - 账户的地址
3. `String|HexNumber|TAG` -  区块高度的整数类型，或者是`"latest"`, `"earliest"` 和 `"pending"`字符串类型


```js
params: [{
  "0xffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffff",
  "0x5b15a29274c74cd7cae59cabf656873a0ea706ac",
  "latest"  // 最新区块的状态
  }]
```

##### Return

`DATA`, 该时间锁定资产的详细情况

##### Example
```js
// Request
curl -X POST -H "Content-Type":application/json --data '{"jsonrpc":"2.0","method":"fsn_getTimeLockBalance","params":["0xffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffff","0x5b15a29274c74cd7cae59cabf656873a0ea706ac","latest"],"id":1}'

// Result
{
  "jsonrpc":"2.0",
  "id":1,
  "result":
  {
    "Items":
    [{
      "StartTime":1570002245,
      "EndTime":18446744073709551615,
      "Value":"5000000000000000000000"
      }]
    }
}
```

***

#### fsn_getAllTimeLockBalances

返回所有时间锁资产余额

##### Parameters

1. `DATA`, 20字节，该账户的地址
2. `String|HexNumber|TAG` -  区块高度的整数类型，或者是`"latest"`, `"earliest"` 和 `"pending"`字符串类型

```js
params: [{
  "0x5b15a29274c74cd7cae59cabf656873a0ea706ac",
  "latest"  // 最新区块的状态
  }]
```

##### Return

`DATA`, 时间锁资产的详细情况

##### Example
```js
// Request
curl -X POST -H "Content-Type":application/json --data '{"jsonrpc":"2.0","method":"fsn_getAllTimeLockBalances","params":["0x5b15a29274c74cd7cae59cabf656873a0ea706ac","latest"],"id":1}'

// Result
{
  "jsonrpc":"2.0",
  "id":1,
  "result":
  {
    "0xffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffff":
    {
      "Items":
      [{
        "StartTime":1570002245,
        "EndTime":18446744073709551615,
        "Value":"5000000000000000000000"
        }]
        }
      }
}

```

***

#### fsntx_genAsset

 生成新的资产（账户已解锁）

##### Parameters

1. `String|Address`, from - 该账户的地址
2. `Number`, gas -  (可选，默认值：待定) 此次交易需要消耗的gas数量 (退换未使用的gas).
3. `String|BigNumber`, gasPrice - (可选) 此次交易中需要消耗gas的价格.
4. `Number`, nonce -  (可选) nonce是一个整数类型.允许一笔pending状态的交易使用相同的nonce.
5. `String`, name - 资产的名字
6. `String`, symbol -  资产的符号
7. `Number`, decimals -  资产的小数
8. `String|BigNumber`, total -  资产的总价值
9. `bool`, canChange -  资产是否可以更改

```js
params: [{
  "from":"0x5b15a29274c74cd7cae59cabf656873a0ea706ac",
  "name":"FusionTest",
  "symbol":"FT",
  "decimals":1,
  "total":"0x200",
  "canChange":true
  }]
```

##### Return

`DATA`, 32字节，返回交易的哈希

##### Example
```js
// Request
curl -X POST -H "Content-Type":application/json --data '{"jsonrpc":"2.0","method":"fsntx_genAsset","params":[{"from":"0x5b15a29274c74cd7cae59cabf656873a0ea706ac","name":"FusionTest","symbol":"FT","decimals":1,"total":"0x200","canChange":true}],"id":1}'

// Result
{
  "jsonrpc":"2.0",
  "id":1,
  "result":"0x6e566d3de4da6bd8628ddc228dd571ddcc7e708ff11d02152f810570fb0d0b9a"
}

```

***

#### fsntx_decAsset

减少资产总数量（账户必须解锁并且创建时的“canChange”（可改变）为真）

##### Parameters

1. `String|Address`, from - 账户的地址
2. `Number`, gas -  (可选，默认值：待定) 此次交易需要消耗的gas数量 (退换未使用的gas).
3. `String|BigNumber`, (可选) 此次交易中需要消耗gas的价格.
4. `Number`, nonce -  (可选) nonce是一个整数类型.允许一笔pending状态的交易使用相同的nonce.
5. `String|Hash`, asset - 资产的哈希.
6. `String|Address`, to - 接受方账户的地址
7. `String|BigNumber`, value - 将要减少的数量
8. `String`, transacData - （可选）交易的额外数据


```js
params: [{
  "from":"0x5b15a29274c74cd7cae59cabf656873a0ea706ac",
  "to":"0x5b15a29274c74cd7cae59cabf656873a0ea706ac",
  "value":"0x10",
  "asset":"0x530566afdbc2e3e4192fb561a1032fba189571bd65abb823e0b0d0ae023dfbbd"
  }]
```

##### Return

`DATA`, 32字节，返回交易哈希

##### Example
```js
// Request
curl -X POST -H "Content-Type":application/json --data '{"jsonrpc":"2.0","method":"fsntx_decAsset","params":[{"from":"0x5b15a29274c74cd7cae59cabf656873a0ea706ac","to":"0x5b15a29274c74cd7cae59cabf656873a0ea706ac","value":"0x10","asset":"0x530566afdbc2e3e4192fb561a1032fba189571bd65abb823e0b0d0ae023dfbbd"}],"id":1}'

// Result
{
  "jsonrpc":"2.0",
  "id":1,
  "result":"0x85d95f61676e99046053346419129afec9d7a8fee2924d9062419aee253f17a4"
}
```

***

#### fsntx_incAsset

增加资产的总数（资产必须解锁并且“canChange”(可修改)为真）

##### Parameters

1. `String|Address`, from - 账户的地址
2. `Number`, gas -  (可选，默认值：待定) 此次交易需要消耗的gas数量 (退换未使用的gas).
3. `String|BigNumber`, gasPrice - (可选) 此次交易中需要消耗gas的价格.
4. `Number`, nonce -   (可选) nonce是一个整数类型.允许一笔pending状态的交易使用相同的nonce.
5. `String|Hash`, asset - 资产的哈希
6. `String|Address`, to - 接受方的地址
7. `String|BigNumber`, value - 将要增加的值
8. `String`, transacData - （可选）交易的额外字段


```js
params: [{
  "from":"0x5b15a29274c74cd7cae59cabf656873a0ea706ac",
  "to":"0x5b15a29274c74cd7cae59cabf656873a0ea706ac",
  "value":"0x10",
  "asset":"0x530566afdbc2e3e4192fb561a1032fba189571bd65abb823e0b0d0ae023dfbbd"
  }]
```

##### Return

`DATA`,32字节，返回交易的哈希

##### Example
```js
// Request
 curl -X POST -H "Content-Type":application/json --data '{"jsonrpc":"2.0","method":"fsntx_incAsset","params":[{"from":"0x5b15a29274c74cd7cae59cabf656873a0ea706ac","to":"0x5b15a29274c74cd7cae59cabf656873a0ea706ac","value":"0x10","asset":"0x530566afdbc2e3e4192fb561a1032fba189571bd65abb823e0b0d0ae023dfbbd"}],"id":1}'

// Result
{
  "jsonrpc":"2.0",
  "id":1,
  "result":"0xdb48fa3fdad4ad6c85b3700d89c37e776cb4fe61487425c752b863c25fe8a7d8"
}
```

***

#### fsntx_sendAsset

发送资产到另外一个账户（账户需要解锁）

##### Parameters

1. `String|Address`, from - 发送方账户的地址
2. `Number`, gas -  (可选，默认值：待定) 此次交易需要消耗的gas数量 (退换未使用的gas).
3. `String|BigNumber`, gasPrice -  (可选) 此次交易中需要消耗gas的价格.
4. `Number`, nonce -   (可选) nonce是一个整数类型.允许一笔pending状态的交易使用相同的nonce.
5. `String|Hash`, asset - 资产的哈希.
6. `String|Address|Number`, to|toUSAN -  接收方的地址 | 接收方的短地址，整数类型.
7. `String|BigNumber`, value - 将要发送的值


```js
1. params: [{
  "from":"0x5b15a29274c74cd7cae59cabf656873a0ea706ac",
  "to":"0x07718f21f889b84451727ada8c65952a597b2e78",
  "value":"0x10",
  "asset":"0x530566afdbc2e3e4192fb561a1032fba189571bd65abb823e0b0d0ae023dfbbd"
  }]
2. params:[{
  "from":"0x5b15a29274c74cd7cae59cabf656873a0ea706ac",
  "toUSAN":"207",
  "value":"0x10",
  "asset":"0x530566afdbc2e3e4192fb561a1032fba189571bd65abb823e0b0d0ae023dfbbd"
}]

```

##### Return

`DATA`, 32字节，返回交易的哈希

##### Example
```js
// Request
1. curl -X POST -H "Content-Type":application/json --data '{"jsonrpc":"2.0","method":"fsntx_sendAsset","params":[{"from":"0x5b15a29274c74cd7cae59cabf656873a0ea706ac","to":"0x07718f21f889b84451727ada8c65952a597b2e78","value":"0x10","asset":"0x530566afdbc2e3e4192fb561a1032fba189571bd65abb823e0b0d0ae023dfbbd"}],"id":1}'
2. curl -X POST -H "Content-Type":application/json --data '{"jsonrpc":"2.0","method":"fsntx_sendAsset","params":[{"from":"0x5b15a29274c74cd7cae59cabf656873a0ea706ac","toUSAN":"207","value":"0x10","asset":"0x530566afdbc2e3e4192fb561a1032fba189571bd65abb823e0b0d0ae023dfbbd"}],"id":1}'

// Result
{
  "jsonrpc":"2.0",
  "id":1,
  "result":"0xbb9651c88974a0f79cde02e763f7999f0da36df751719934b40d04814b0640af"
}
```

***

#### fsn_getAllBalances

获取某个账户的所有资产余额

##### Parameters

1. `String|Address`, address - 账户的地址
2. `String|HexNumber|TAG` - 区块高度的整数类型，或者是`"latest"`, `"earliest"` 和 `"pending"`字符串类型


```js
params: [
   "0x5b15a29274c74cd7cae59cabf656873a0ea706ac",
   "latest"  // 最新的状态区块
]
```

##### Return

`QUANTITY` - 当前用wei做单位的整数



##### Example
```js
// Request
curl -X POST -H "Content-Type":application/json --data '{"jsonrpc":"2.0","method":"fsn_getAllBalances","params":["0x5b15a29274c74cd7cae59cabf656873a0ea706ac","latest"],"id":1}'

// Result
{
  "jsonrpc":"2.0",
  "id":1,
  "result":{"0xffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffff":"3499978048000000000"}}
```

***

#### fsn_getLatestNotation   

获取区块链中最新短地址

##### Parameters

1. `String|HexNumber|TAG` - 区块高度的整数类型，或者是`"latest"`, `"earliest"` 和 `"pending"`字符串类型


```js
params: [
  "latest"  // 最新的状态区块
]
```

##### Return

`DATA` - 最新的短地址


##### Example
```js
// Request
curl -X POST -H "Content-Type":application/json --data '{"jsonrpc":"2.0","method":"fsn_getLatestNotation","params":["latest"],"id":1}'

// Result
{
  "jsonrpc":"2.0",
  "id":1,
  "result":104
}
```

***

#### fsntx_genNotation

给一个账户创造一个短地址（账户已经解锁）


##### Parameters

1. `String|Address`, from - 该账户的地址
2. `Number` gas - (可选，默认值：待定) 此次交易需要消耗的gas数量 (退换未使用的gas).
3. `String|BigNumber`, gasPrice - (可选) 此次交易中需要消耗gas的价格.
4. `Number` nonce - (可选) nonce是一个整数类型.允许一笔pending状态的交易使用相同的nonce.


```js
params: [{"from":"0x5b15a29274c74cd7cae59cabf656873a0ea706ac"}]
```

##### Return

`DATA`, 32字节-交易的哈希


##### Example
```js
// Request
curl -X POST -H "Content-Type":application/json --data '{"jsonrpc":"2.0","method":"fsntx_genNotation","params":[{"from":"0x5b15a29274c74cd7cae59cabf656873a0ea706ac"}],"id":1}'

// Result
{
  "jsonrpc":"2.0",
  "id":1,
  "result":"0xae4b4949f9e2ad46343df4e11d5a245306281b60cb6d217dd10c011d18638445"
}
```

***

#### fsn_getNotation

获取一个账户的短地址

##### Parameters

1. `String|Address` - 需要查询账户的地址
2. `String|HexNumber|TAG` - 区块高度的整数类型，或者是`"latest"`, `"earliest"` 和 `"pending"`字符串类型

```js
params: [
   "0x5b15a29274c74cd7cae59cabf656873a0ea706ac",
   "latest"  // state at the latest block
]
```

##### Return

`Number` - 短地址


##### Example
```js
// Request
curl -X POST -H "Content-Type":application/json --data '{"jsonrpc":"2.0","method":"fsn_getNotation","params":["0x5b15a29274c74cd7cae59cabf656873a0ea706ac","latest"],"id":1}'

// Result
{
  "jsonrpc":"2.0",
  "id":1,
  "result":104
}
```

***

#### fsn_getAddressByNotation

返回改地址的短地址

##### Parameters

1. `Number`, notation - 短地址
2. `String|HexNumber|TAG` - 区块高度的整数类型，或者是`"latest"`, `"earliest"` 和 `"pending"`字符串类型

```js
params: [
   104,  //Notation
   "latest" // state at the latest block
]
```

##### Return

`DATA` - 20字节-改地址的短地址

##### Example
```js
// Request
curl -X POST -H "Content-Type":application/json --data '{"jsonrpc":"2.0","method":"fsn_getAddressByNotation","params":[104,"latest"],"id":1}'

// Result
{
  "jsonrpc":"2.0",
  "id":1,
  "result":"0x5b15a29274c74cd7cae59cabf656873a0ea706ac"
}
```

***

#### fsn_getAsset

获取资产信息

##### Parameters

1. `String|Hash`, assetID - 资产的哈希
2. `String|HexNumber|TAG`, blockNr - 区块高度的整数类型，或者是`"latest"`, `"earliest"` 和 `"pending"`字符串类型


```js
params: [
   "0xffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffff", //AssetId
   "latest"  // 最新区块的状态
]
```

##### Return

`DATA` - 资产的详细情况


##### Example
```js
// Request
 curl -X POST -H "Content-Type":application/json --data '{"jsonrpc":"2.0","method":"fsn_getAsset","params":["0xffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffff","latest"],"id":1}'

// Result
{
  "jsonrpc":"2.0",
  "id":1,
  "result":
  {
    "ID":"0xffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffff",
    "Owner":"0x0000000000000000000000000000000000000000",
    "Name":"Fusion",
    "Symbol":"FSN",
    "Decimals":18,
    "Total":"81920000000000000000000000",
    "CanChange":false,
    "Description":"https://fusion.org"
  }
}
```
***

#### fsn_getBalance

获取账户的余额


##### Parameters

1. `String|Number`, assetID - 该账户的资产id
2. `String|Address`, address - 该账户的地址
3. `String|HexNumber|TAG`, blockNr - 区块高度的整数类型，或者是`"latest"`, `"earliest"` 和 `"pending"`字符串类型


```js
params: [
   "0xffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffff", //AssetId
   "0x5b15a29274c74cd7cae59cabf656873a0ea706ac",
   "latest"  // 最新区块的状态
]
```

##### Return

`QUANTITY`  - 当前以wei为单位的账户余额


##### Example
```js
// Request
  curl -X POST -H "Content-Type":application/json --data '{"jsonrpc":"2.0","method":"fsn_getBalance","params":["0xffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffff","0x5b15a29274c74cd7cae59cabf656873a0ea706ac","latest"],"id":1}'

// Result
{
  "jsonrpc":"2.0",
  "id":1,
  "result":"5999978048000000000"
}
```

***

#### fsntx_makeSwap

创建一个交换订单（账户需要解锁）

##### Parameters

1. `String|Address`, from - 发送方账户地址
2. `Number`, gas - (可选，默认值：待定) 此次交易需要消耗的gas数量 (退换未使用的gas).
3. `String|BigNumber`, gasPrice- (可选) 此次交易中需要消耗gas的价格.
4. `Number`, (可选) nonce是一个整数类型.允许一笔pending状态的交易使用相同的nonce.
5. `String|Number`, FromAssetID - 发送资产的ID
6. `String|HexNumber`, FromStartTime - (可选) 发送账户的开始时间
7. `String|HexNumber`, FromEndTime - (可选) 发送账户的结束时间
8. `String|BigNumber`, MinFromAmount - 将要发送的金额
9. `String|Number`,  ToAssetID - 该账户的资产ID.
10. `String|HexNumber`,  ToStartTime - (可选) 接收到该账户的开始时间
11. `String|HexNumber`, ToEndTime - (可选) 接收到该账户的结束时间.
12. `String|BigNumber`, MinToAmount - 接收到该账户的最小余额.
13. `Number`, SwapSize - 交换的大小.
14. `Array String|Address`, Targes - 只有这些地址可以交换


```js
params: [
   {
    "from":"0x5b15a29274c74cd7cae59cabf656873a0ea706ac",
    "FromAssetID":"0xffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffff", //If FromAssetID is "0xfffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffe",it means make a Notation swap
    "ToAssetID":"0xffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffff",
    "MinToAmount":"0x1BC16D674EC80000",    //2000000000000000000
    "MinFromAmount":"0x29A2241AF62C0000",  //3000000000000000000
    "FromStartTime":"0x5D6CE866",          //2019/9/2 18:1:10
    "FromEndTime":"0x5D9475B9",            //2019/10/2 18:2:33
    "SwapSize":2,
    "Targes":["0x07718f21f889b84451727ada8c65952a597b2e78"]
  }
]
```

##### Return

`DATA`, 32字节-交易的哈希


##### Example
```js
// Request
curl -X POST -H "Content-Type":application/json --data '{"jsonrpc":"2.0","method":"fsntx_makeSwap","params":[{"from":"0x5b15a29274c74cd7cae59cabf656873a0ea706ac","FromAssetID":"0xffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffff","ToAssetID":"0xffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffff","MinToAmount":"0x1BC16D674EC80000","MinFromAmount":"0x29A2241AF62C0000","FromStartTime":"0x5D6CE866","FromEndTime":"0x5D9475B9","SwapSize":2,"Targes":["0x07718f21f889b84451727ada8c65952a597b2e78"]}],"id":1}'

// Result
{
  "jsonrpc":"2.0",
  "id":1,
  "result":"0xfbd0eb501114d23aa42d92be50d705a3e987bda658772a92283e1483fcc6027a"
}
```

***

#### fsn_getSwap

获取MakeSwap交易的详细信息

##### Parameters

1. `String|Hash` - MakeSwap交易的哈希
2. `String|HexNumber|TAG`, blockNr - 区块高度的整数类型，或者是`"latest"`, `"earliest"` 和 `"pending"`字符串类型

```js
params: [
  "0x2bd4929f673c53bd4ff90ee640f8d3ccd6b756977893009ca6ef5561e54af535",
  "latest"
]
```

##### Return

`DATA` - MakeSwap交易的详细信息


##### Example
```js
// Request
curl -X POST -H "Content-Type":application/json --data '{"jsonrpc":"2.0","method":"fsn_getSwap","params":["0x2bd4929f673c53bd4ff90ee640f8d3ccd6b756977893009ca6ef5561e54af535","latest"],"id":1}'

// Result
{
  "jsonrpc": "2.0",
  "id": 1,
  "result": {
    "ID": "0xaac295a8b71c4698615d2ceb08e205355ad5f0bb622020fe1bac145e47a24141",
    "Owner": "0x3a1b3b81ed061581558a81f11d63e03129347437",
    "FromAssetID": "0xffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffff",
    "FromStartTime": 1567418470,
    "FromEndTime": 1570010553,
    "MinFromAmount": 3e+18,
    "ToAssetID": "0xffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffff",
    "ToStartTime": 0,
    "ToEndTime": 18446744073709552000,
    "MinToAmount": 2e+18,
    "SwapSize": 2,
    "Targes": [
      "0xb49edfcd6ab3dac4cc908f31fc4b3f7772773113"
    ],
    "Time": 1569381182,
    "Description": "",
    "Notation": 0
  }
}
```

***

#### fsntx_takeSwap

买下这笔swap订单（账户已经解锁）

##### Parameters

1. `String|Address`, 发送方账户地址
2. `Number`, gas - (可选，默认值：待定) 此次交易需要消耗的gas数量 (退换未使用的gas).
3. `String|BigNumber`, gasPrice - (可选) 此次交易中需要消耗gas的价格.
4. `Number`, nonce - (可选) nonce是一个整数类型.允许一笔pending状态的交易使用相同的nonce.
5. `String|Hash`, SwapID - swap交易的哈希
6. `Number`, Size - swap交易的大小


```js
params: [{
  "from":"0x07718f21f889b84451727ada8c65952a597b2e78",
  "SwapID":"0x3be968fd7368b73d2cd8ccac12fedb0a9a3b85b8b86f10bdb69b4a8e3285dbab",
  "Size":1
  }]
```

##### Return

`DATA`, 32字节-交易哈希


##### Example
```js
// Request
curl -X POST -H "Content-Type":application/json --data '{"jsonrpc":"2.0","method":"fsntx_takeSwap","params":[{"from":"0x07718f21f889b84451727ada8c65952a597b2e78","SwapID":"0x3be968fd7368b73d2cd8ccac12fedb0a9a3b85b8b86f10bdb69b4a8e3285dbab","Size":1}],"id":1}'

// Result
{
  "jsonrpc":"2.0",
  "id":1,
  "result":"0x61923c6b6c5bf635130b5f2d093754a70fc7a9986cc2acc8c4fcfc87a580c25d"
}
```

***

#### fsntx_recallSwap

销毁这笔交换订单，返回资产类型

##### Parameters

1. `String|Address`, from - 账户的地址.
2. `Number`, gas - (可选，默认值：待定) 此次交易需要消耗的gas数量 (退换未使用的gas).
3. `String|BigNumber`, gasPrice -  (可选) 此次交易中需要消耗gas的价格.
4. `Number`, nonce - (可选) nonce是一个整数类型.允许一笔pending状态的交易使用相同的nonce.
5. `String|Hash`, SwapID - swap交易的哈希


```js
params: [{
  "from":"0x5b15a29274c74cd7cae59cabf656873a0ea706ac",
  "SwapID":"0x3be968fd7368b73d2cd8ccac12fedb0a9a3b85b8b86f10bdb69b4a8e3285dbab"
  }]
```

##### Return

`DATA`, 32字节-交易哈希


##### Example
```js
// Request
curl -X POST -H "Content-Type":application/json --data '{"jsonrpc":"2.0","method":"fsntx_recallSwap","params":[{"from":"0x5b15a29274c74cd7cae59cabf656873a0ea706ac","SwapID":"0x3be968fd7368b73d2cd8ccac12fedb0a9a3b85b8b86f10bdb69b4a8e3285dbab"}],"id":1}'

// Result
{
  "jsonrpc":"2.0",
  "id":1,
  "result":"0x4270a81d7377fffb7e4aa4785f729baab5ca6711b9ae6df8777916a6f036285d"
}
```

***
#### fsntx_makeMultiSwap

创建多个交换订单（账户已解锁）

##### Parameters

1. `String|Address`, from - 发送方账户地址
2. `Number`, gas - (可选，默认值：待定) 此次交易需要消耗的gas数量 (退换未使用的gas).
3. `String|BigNumber`, (可选) 此次交易中需要消耗gas的价格.
5. `String|Number`, FromAssetID - 该账户的资产ID
6. `String|HexNumber`, FromStartTime - (可选)发送账户的开始时间.
7. `String|HexNumber`, FromEndTime - (可选) 发送账户的结束时间.
8. `String|BigNumber`, MinFromAmount - 发送账户的金额.
9. `String|Number`,  ToAssetID - 账户的资产ID.
10. `String|HexNumber`,  ToStartTime - (可选) 接收账户的开始时间.
11. `String|HexNumber`, ToEndTime - (可选) 接收账户的结束时间.
12. `String|BigNumber`, MinToAmount - 接收账户的数量.
13. `Number`, SwapSize - swap的大小.
14. `Array String|Address`, Targes - 只有这些地址可以被交易.


```js
params: [
  {
    "from":"0x3a1b3b81ed061581558a81f11d63e03129347437",
    "FromAssetID":[
      "0xffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffff",
      "0xffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffff"],
      "ToAssetID":[
      "0xffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffff",
      "0xffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffff"],
      "MinToAmount":["0x1BC16D674EC80000","0x1BC16D674EC80000"],
      "MinFromAmount":["0x29A2241AF62C0000","0x29A2241AF62C0000"],
      "FromStartTime":["0x5D6CE866","0x5D6CE866"],
      "FromEndTime":["0x5D9475B9","0x5D9475B9"],
      "SwapSize":2,
      "Targes":[]
  }
]
```

##### Return

`DATA`, 32字节-交易的哈希


##### Example
```js
// Request
curl -X POST -H "Content-Type":application/json --data '{"jsonrpc":"2.0","method":"fsntx_makeMultiSwap","params":[{"from":"0x3a1b3b81ed061581558a81f11d63e03129347437","FromAssetID":["0xffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffff","0xffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffff"],"ToAssetID":["0xffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffff","0xffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffff"],"MinToAmount":["0x1BC16D674EC80000","0x1BC16D674EC80000"],"MinFromAmount":["0x29A2241AF62C0000","0x29A2241AF62C0000"],"FromStartTime":["0x5D6CE866","0x5D6CE866"],"FromEndTime":["0x5D9475B9","0x5D9475B9"],"SwapSize":2,"Targes":[]}],"id":1}'

// Result
{
  "jsonrpc":"2.0",
  "id":1,
  "result":"0x89e699deac578e491675f1e4fe2233966e79fd388f9725035479268605801c87"
}
```

***

#### fsn_getMultiSwap

获取multi swap交易的详细情况

##### Parameters

1. `String|Hash` - multi交易的哈希
2. `String|HexNumber|TAG` - 区块高度的整数类型，或者是`"latest"`, `"earliest"` 和 `"pending"`字符串类型

```js
params: [
  "0xf616d50440414ce2bfd2204ace993a34e53e58cc656c82b339be935670f2070e",
  "latest"
  ]
```

##### Return

`DATA`, multi swap交易的哈希


##### Example
```js
// Request
curl -X POST -H "Content-Type":application/json --data '{"jsonrpc":"2.0","method":"fsn_getMultiSwap","params":["0xf616d50440414ce2bfd2204ace993a34e53e58cc656c82b339be935670f2070e","latest"],"id":1}'

// Result
{
  "jsonrpc": "2.0",
  "id": 1,
  "result": {
    "ID": "0xac05ff3d0d7ad5440eba0ee751ba19f876105b790dec63051c4db0b434cefa47",
    "Owner": "0x3a1b3b81ed061581558a81f11d63e03129347437",
    "FromAssetID": [
      "0xffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffff",
      "0xffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffff"
    ],
    "FromStartTime": [
      1567418470,
      1567418470
    ],
    "FromEndTime": [
      1570010553,
      1570010553
    ],
    "MinFromAmount": [
      3e+18,
      3e+18
    ],
    "ToAssetID": [
      "0xffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffff",
      "0xffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffff"
    ],
    "ToStartTime": [
      0,
      0
    ],
    "ToEndTime": [
      18446744073709552000,
      18446744073709552000
    ],
    "MinToAmount": [
      2e+18,
      2e+18
    ],
    "SwapSize": 2,
    "Targes": [],
    "Time": 1569387880,
    "Description": "",
    "Notation": 0
  }
}
```

***

#### fsntx_takeMultiSwap  

买下这笔混合资产交换订单（账户已解锁）

##### Parameters

1. `String|Address`, from - 发送方账户的地址
2. `Number`, gas -  (可选，默认值：待定) 此次交易需要消耗的gas数量 (退换未使用的gas).
3. `String|BigNumber`, gasPrice - (可选) 此次交易中需要消耗gas的价格.
4. `Number`, nonce - (可选) nonce是一个整数类型.允许一笔pending状态的交易使用相同的nonce.
5. `String|Hash`, SwapID - multi swap交易的哈希
6. `Number`, Size - multi swap 交易大小.


```js
params: [{
  "from":"0xb49edfcd6ab3dac4cc908f31fc4b3f7772773113",
  "SwapID":"0xac05ff3d0d7ad5440eba0ee751ba19f876105b790dec63051c4db0b434cefa47",
  "Size":1
  }]
```

##### Return

`DATA`, 32字节-交易的哈希


##### Example
```js
// Request
curl -X POST -H "Content-Type":application/json --data '{"jsonrpc":"2.0","method":"fsntx_takeMultiSwap","params":[{"from":"0xb49edfcd6ab3dac4cc908f31fc4b3f7772773113","SwapID":"0xac05ff3d0d7ad5440eba0ee751ba19f876105b790dec63051c4db0b434cefa47","Size":1}],"id":1}'

// Result
{
  "jsonrpc":"2.0",
  "id":1,
  "result":"0x75ebbd003ae590e1df879ebc931d34c6c494f9479b828017baecf60b44e2c59c"
}
```

***

#### fsn_recallMultiSwap 

销毁混合资产订单并且返回资产（账户已解锁）

##### Parameters

1. `String|Address`, from - 账户的地址.
2. `Number`, gas - (可选，默认值：待定) 此次交易需要消耗的gas数量 (退换未使用的gas).
3. `String|BigNumber`, gasPrice - (可选) 此次交易中需要消耗gas的价格.
4. `Number`, nonce - (可选) nonce是一个整数类型.允许一笔pending状态的交易使用相同的nonce.
5. `String|Number`, SwapID - 混合资产交换哈希.


```js
params: [{
  "from":"0x3a1b3b81ed061581558a81f11d63e03129347437",
  "SwapID":"0xac05ff3d0d7ad5440eba0ee751ba19f876105b790dec63051c4db0b434cefa47"
  }]
```

##### Return

`DATA`, 32字节-交易的哈希


##### Example
```js
// Request
curl -X POST -H "Content-Type":application/json --data '{"jsonrpc":"2.0","method":"fsntx_recallMultiSwap","params":[{"from":"0x3a1b3b81ed061581558a81f11d63e03129347437","SwapID":"0xac05ff3d0d7ad5440eba0ee751ba19f876105b790dec63051c4db0b434cefa47"}],"id":1}'

// Result
{
  "jsonrpc":"2.0",
  "id":1,
  "result":"0xe2dd257d22e2717f9591d207d18e493acb5aae5d19dc94a3f95ded7227f7c825"
}
```

***
#### fsntx_isAutoBuyTicket

如果是自动买票返回“true”（账户已经解锁）

##### Parameters

`String|HexNumber|TAG`, blockNr - 区块高度的整数类型，或者是`"latest"`, `"earliest"` 和 `"pending"`字符串类型

```js
params: ["latest"]
```

##### Return

`bool`, 如果是自动买票，将会返回“true”

##### Example
```js
// Request
curl -X POST -H "Content-Type":application/json --data '{"jsonrpc":"2.0","method":"fsntx_isAutoBuyTicket","params":["latest"],"id":1}'

// Result
{
  "jsonrpc":"2.0",
  "id":1,
  "result":true
}

```

***
#### miner_startAutoBuyTicket

开始自动买票（账户已经解锁）

##### Parameters

无

##### Return

`String`, 无返回.

##### Example
```js
// Request
curl -X POST -H "Content-Type":application/json --data '{"jsonrpc":"2.0","method":"miner_startAutoBuyTicket","params":[],"id":1}'

// Result
{
  "jsonrpc":"2.0",
  "id":1,
  "result":null
}

```

***
#### miner_stopAutoBuyTicket

关闭自动买票（账户已解锁）

##### Parameters

无

##### Return

`String`, 无返回

##### Example
```js
// Request
curl -X POST -H "Content-Type":application/json --data '{"jsonrpc":"2.0","method":"miner_stopAutoBuyTicket","params":[],"id":1}'

// Result
{
  "jsonrpc":"2.0",
  "id":1,
  "result":null
}

```

***
#### fsn_getTransactionAndReceipt

返回交易详情和回执

##### Parameters

`DATA`, 32字节-交易的哈希

```js
params: ["0x8700056ef2896b47760e661902b21d8f294a80bff87c7e4108d7bbd5bce4ce6d"]
```

##### Return

`DATA` - 交易详情和回执

##### Example
```js
// Request
curl -X POST -H "Content-Type":application/json --data '{"jsonrpc":"2.0","method":"fsn_getTransactionAndReceipt","params":["0x8700056ef2896b47760e661902b21d8f294a80bff87c7e4108d7bbd5bce4ce6d"],"id":1}'

// Result
{
  "jsonrpc": "2.0",
  "id": 1,
  "result": {
    "fsnTxInput": {
      "FuncType": "SendAssetFunc",
      "FuncParam": {
        "AssetID": "0xffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffff",
        "To": "0x37a200388caa75edcc53a2bd329f7e9563c6acb6",
        "Value": 1e+18
      }
    },
    "tx": {
      "blockHash": "0xd8d4b5f054cb398b1f0b5bb5d4add5e80d10a432f2c15226f620609577536b6b",
      "blockNumber": "0xad1a5",
      "from": "0x0122bf3930c1201a21133937ad5c83eb4ded1b08",
      "gas": "0x15f90",
      "gasPrice": "0x3b9aca00",
      "hash": "0x8700056ef2896b47760e661902b21d8f294a80bff87c7e4108d7bbd5bce4ce6d",
      "input": "0xf84402b841f83fa0ffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffff9437a200388caa75edcc53a2bd329f7e9563c6acb6880de0b6b3a7640000",
      "nonce": "0xa7cc",
      "to": "0xffffffffffffffffffffffffffffffffffffffff",
      "transactionIndex": "0x2",
      "value": "0x0",
      "v": "0x16ce3",
      "r": "0x8244e44f720023b240faafab08bb401b1b3167087f2882fa6b8f4fc87b59bdfc",
      "s": "0x277635df431668f4a8b8c8b0702077634dd404db9a1139539d3c276651d3d1ce"
    },
    "receipt": {
      "blockHash": "0xd8d4b5f054cb398b1f0b5bb5d4add5e80d10a432f2c15226f620609577536b6b",
      "blockNumber": "0xad1a5",
      "contractAddress": null,
      "cumulativeGasUsed": "0x10f60",
      "from": "0x0122bf3930c1201a21133937ad5c83eb4ded1b08",
      "fsnLogData": {
        "AssetID": "0xffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffff",
        "To": "0x37a200388caa75edcc53a2bd329f7e9563c6acb6",
        "Value": 1e+18
      },
      "fsnLogTopic": "SendAssetFunc",
      "gasUsed": "0x63e0",
      "logs": [
        {
          "address": "0xffffffffffffffffffffffffffffffffffffffff",
          "topics": [
            "0x0000000000000000000000000000000000000000000000000000000000000002"
          ],
          "data": "0x7b2241737365744944223a22307866666666666666666666666666666666666666666666666666666666666666666666666666666666666666666666666666666666666666666666666666666666222c22546f223a22307833376132303033383863616137356564636335336132626433323966376539353633633661636236222c2256616c7565223a313030303030303030303030303030303030307d",
          "blockNumber": "0xad1a5",
          "transactionHash": "0x8700056ef2896b47760e661902b21d8f294a80bff87c7e4108d7bbd5bce4ce6d",
          "transactionIndex": "0x2",
          "blockHash": "0xd8d4b5f054cb398b1f0b5bb5d4add5e80d10a432f2c15226f620609577536b6b",
          "logIndex": "0x2",
          "removed": false
        }
      ],
      "logsBloom": "0x04000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000080000000000000002000000000000000000000000000000000000000000000000000000000000000000100000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000010000000000000000000000000000000000000000000008000000000000000000000",
      "status": "0x1",
      "to": "0xffffffffffffffffffffffffffffffffffffffff",
      "transactionHash": "0x8700056ef2896b47760e661902b21d8f294a80bff87c7e4108d7bbd5bce4ce6d",
      "transactionIndex": "0x2"
    },
    "receiptFound": true
  }
}
```

***
#### fsn_allInfoByAddress

返回有关地址的所有信息

##### Parameters

1. `String|Address`, 20字节-要查询的地址
2. `String|HexNumber|TAG`, blockNr - 区块高度的整数类型，或者是`"latest"`, `"earliest"` 和 `"pending"`字符串类型

```js
params: ["0x3a1b3b81ed061581558a81f11d63e03129347437","latest"]
```
##### Return

`DATA` - 有关地址的所有信息

##### Example
```js
// Request
curl -X POST -H "Content-Type":application/json --data '{"jsonrpc":"2.0","method":"fsn_allInfoByAddress","params":["0x3a1b3b81ed061581558a81f11d63e03129347437","latest"],"id":1}'

// Result
{
  "jsonrpc": "2.0",
  "id": 1,
  "result": {
    "tickets": {
      "0x8fd87180c90cc7133be48b83b03781810c8ae4ad86446870dbed6b1a9a3193a8": {
        "Owner": "0x3a1b3b81ed061581558a81f11d63e03129347437",
        "Height": 251,
        "StartTime": 1569381648,
        "ExpireTime": 1571973648,
        "Value": 5e+21
      },
      "0xf9972189b9c077207cc05ab81d2e35bfe4ce6db28683d68c9ebfff6ff640722e": {
        "Owner": "0x3a1b3b81ed061581558a81f11d63e03129347437",
        "Height": 255,
        "StartTime": 1569381700,
        "ExpireTime": 1571973700,
        "Value": 5e+21
      }
    },
    "balances": {
      "0xffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffff": "98980637500000000000000000"
    },
    "timeLockBalances": {
      "0xffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffff": {
        "Items": [
          {
            "StartTime": 1569381700,
            "EndTime": 18446744073709552000,
            "Value": "9994000000000000000000"
          },
          {
            "StartTime": 1571973649,
            "EndTime": 18446744073709552000,
            "Value": "5000000000000000000000"
          },
          {
            "StartTime": 1571973701,
            "EndTime": 18446744073709552000,
            "Value": "5000000000000000000000"
          },
          {
            "StartTime": 1570010554,
            "EndTime": 18446744073709552000,
            "Value": "6000000000000000000"
          }
        ]
      }
    },
    "notation": 0
  }
}
```

***
