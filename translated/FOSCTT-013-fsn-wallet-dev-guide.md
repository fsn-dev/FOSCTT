# FSN wallet development guide

This document mainly describes the interfaces such as deposit, withdrawal, transfer and query involved in the development of FSN for exchanges, staking pools and wallets, as well as how to one-click deploy FSN node.

## Deploy FSN node

FSN node supports two deployment methods:

1. one-click deployment via docker image. docker image address: https://hub.docker.com/u/fusionnetwork

2. source code compilation and deployment

#### 1. Docker deployment

Run the command on a Linux system:

`bash -c "$(curl -fsSL https://raw.githubusercontent.com/FUSIONFoundation/efsn/master/QuickNodeSetup/fsnNode.sh)"`

If you select miner during the deployment process, you need to enter the keystore file content and password. For details, please refer to: https://fusionnetworks.zendesk.com/hc/en-us/categories/360001967614-Staking-On-Fusion-MainNet

#### 2. Source code compilation and deployment

1. Synchronize code

`git clone https://github.com/FUSIONFoundation/efsn.git`

2. Compile the source (golang > 1.11):

`cd efsn && make efsn`

3. Run node

`./build/bin/efsn console`

4. As a backend synchronized node, open the RPC interface

`nohup ./build/bin/efsn --datadir ./node1/ --rpc --rpcaddr 0.0.0.0 --rpcapi net,fsn,eth,web3 --rpcport 9001 --rpccorsdomain "*" &`

For test network, please add the `--testnet` parameter.

As a synchronized node, it is necessary to open the `--gcmode=archive` parameter to query all historical data. At block height 770,000, it occupies more than 100G of hard disk space. In this mode, it is necessary to prepare server storage space in advance (>300G is recommended).The default non-archive mode runs about 1G, but it is not possible to query some historical data.

## FSN wallet interface

FSN node code is forked from [go - ethereum](https://github.com/ethereum/go-ethereum). The RPC interface is compatible with ETH. Upper application interface is compatible with [web3.js](https://github.com/ethereum/web3.js). FSN's Ticket, Asset, Timelock USAN, Swap, Staking functions provides [RPC extension interface](https://github.com/FUSIONFoundation/efsn/wiki/FSN-RPC-API) and [web3 extension interface](https://github.com/FUSIONFoundation/web3-fusion-extend).

#### Deposit recognition

FSN network supports two types of transfer transactions, both can deposit.

- By Default [sendAsset](https://github.com/FUSIONFoundation/efsn/wiki/FSN-RPC-API#fsntx_sendAsset)

- Eth-compatible [sendtransaction](https://github.com/ethereum/wiki/wiki/JSON-RPC#eth_sendtransaction)

Eth-compatible sendtransaction can use the same Deposit recognition code as ethereum. Generally, by monitoring the latest block, all the transactions in the block are obtained, and then the transaction list is traversed to identify whether the to address is a deposit address. If yes, it is a despoit transaction.

The default sendAsset transaction is similar to erc20 smart contract transaction, and a piece of code needs to be added to recognize the deposit. The difference is that the actual 'to' address and transfer amount for this kind of transaction need to be obtained by parsing the data parameter of the receipt transaction. Interface uses [getTransactionAndReceipt](https://github.com/FUSIONFoundation/efsn/wiki/FSN-RPC-API#fsn_getTransactionAndReceipt). The transaction type is recognized through the parameter: ` "fsnLogTopic" : "SendAssetFunc" `. The actual 'to' address and the Value of the transfer amount is recognized through this parameter:

```
"fsnLogData": {
        "AssetID": "0xffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffff",
        "To": "0x37a200388caa75edcc53a2bd329f7e9563c6acb6",
        "Value": 1e+18
      }
```

It is suggested that the number of blocks to confirm a deposit should be more than 30.

#### Withdrawal transaction

Sending withdrawal transactions can use ethereum compatible withdrawal codes. After the transaction is signed offline, it is sent to the FSN node RPC interface through the [sendrawtransaction](https://github.com/ethereum/wiki/wiki/JSON-RPC#eth_sendrawtransaction) interface.

When transaction is signed, FSN mainet chainid=32659, testnet chainid=3

### Dev community
If you have development problems, please join the development community: https://fsn.dev/group/
