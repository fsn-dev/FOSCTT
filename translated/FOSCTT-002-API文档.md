## 欢迎使用Fusionapi的文档

Fusion是一个企业级金融基础设施区块链解决方案，简化了任何资产的完整数字生命周期。
在Ethreum的世界里，用户们都知道他们有一个钱包和余额。如果用户拥有ERC20或ERC721令牌，钱包应用程序将隐藏调用合同的技术细节以获取令牌的价值。
Fusion可以将资产及其余额转移到用户的以太坊钱包中。
无需智能合约，因为钱包中的链中可以包含多个具有唯一资产ID的余额。
通过简单地将资产放在用户帐户中，还消除了其他通货膨胀行为，例如创建过户押金和原子交换。
Fuison允许所有资产具有一定的时间段，这些时间段可以安全地借出资产。
因此，可以将1个Fusion Token的余额进行时间锁定30天，然后发送到另一个帐户。
30天后，其他帐户将无法再使用其钱包中的令牌。
所有这些都是在Fusion主网上完成的，没有智能合约的参与。

Fusion的目标是：

- 使资产（令牌）成为价值发送和接收的自然部分
    - 消除了对ERC20智能合约的需求
    - 消除了对ERC721智能合约的需求
    - 熟悉的from，to，value转变为 from，to，assetId，value
- 引入资产时间锁定的概念作为本地钱包功能
    - 将任何资产的使用分成任何时间
    - 允许将该资产的那部分时间安全地借给某人
- 量子交换-引入一种交易方法来在各方之间交换资产
    - 能够在区块链上的一次交易中在账户之间交换资产的能力而不会出错
    - 有时间限制的资产可以交换
    - 交换可以公开给所有用户或目标用户。
Fusionapi库的目的是采用流行的web3.js软件包并将其开放给Fusion链提供功能扩展。


### 入门

>注意:
该文档正在开发中，目前在beta中正在进行web3-fusion-extend。请随时通过右上角的github链接上的编辑提交拉取请求或问题。
### 开始
web3-fusion-extend是一组库，这些库使您可以使用HTTP或IPC连接与本地或远程融合节点进行交互。
Fuison提供了一种在区块链环境中代表价值的根本方法。
公共地址包含多个资产和这些资产的余额。
assetId是创建资产并可以对其执行操作时返回的ID。
资产创建者还具有增加和减少资产供应的能力。
这样可以构建跨链和跨功能的系统，从而实现资产交换。
资产也可以设置为时间锁定。当资产被时间锁定时，其所有权在指定期间内保持不变。在时间锁定期结束时，资产的权利返回给原始所有者。
通过资产的表示，安全交换资产的需求变得极为重要。

- Fusion协议引入了量子交换，该交换由三个功能组成：
    - makeSwap-告诉其他人您将如何收回资产以收回资产
    - recallSwap-取消交换
    - takeSwap-将您的资产交换为makeSwap中列出的其他方资产

该软件包扩展了以太坊兼容的JavaScript API，该API实现了通用JSON RPC规范以支持Fusion协议。

#### 安装
Node.js
```code
npm install web3-fusion-extend
```
Yarn
```code
yarn add web3-fusion-extend
```

#### 用法

照常创建一个web3对象，然后使用该对象调用web3FusionExtend。然后，web3将具有两个附加接口（fsn和fsntx）
```code
var web3FusionExtend = require('web3-fusion-extend')
web3 = new Web3(provider);
web3 = web3FusionExtend.extend(web3)
console.log(web3.fsn.consts.FSNToken);
```
```code
console.log(web3); // {fsn: .., fsntx: ...} // It's here!
```

到此为止，现在您可以使用它了：
```code
var balance = web3.eth.getBalance(coinbase);
web3.fsn
    .getAllBalances( web3.eth.coinbase ) // fsn supports multiple assets and balances on an address
    .then( balances => {
      console.log( balances )
      assert(  balances[web3.fsn.consts.FSNToken] , "there should be a balance for fusion tokens always"  )
      done();
    })
    .catch(err => {
      done(err);
    });
```

#### 贡献代码！
非常感谢您的任何贡献。


#### 要求
Node.js NPM

```code
# On Linux:
sudo apt-get update
sudo apt-get install nodejs
sudo apt-get install npm
sudo apt-get install nodejs-legacy
```
#### 测试（mocha）

测试到本地融合节点的连接字符串时，需要使用钱包地址作为环境变量
```code
CONNECT_STRING="ws://3.16.110.25:9001" WALLET_ADDRESS="0x4A5a7Aa4130e407d3708dE56db9000F059200C62" npm test
```

#### 社区
- Gitter
- Forum

#### 文档
- 总览
- fsn
- fsntx
- 合约
