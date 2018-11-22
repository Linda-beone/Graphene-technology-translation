  **原文链接**：[https://github.com/bitshares/bitshares-ui/wiki/Gateway-Integration-Requirements](https://github.com/bitshares/bitshares-ui/wiki/Gateway-Integration-Requirements)
 
 **译者**：[https://github.com/b-t-s-1](b-t-s-1)
 
 **校对者**： 

***  
  
### **请注意，这是一项正在进行的工作，目前需要进行验证**

# 网关集成

本文档将描述交易所如何以简单且受控的方式将其服务集成到Bitshares UI Reference Wallet。 交易所必须提供一些API调用，本文档将满足这些要求。

## 必需的API调用

需要五个API调用。

### 1. 可用代币

返回包含所有资产和支持资产的JSON数组。

例如，如果网关交易所支持比特币，我们称之为DAC.xxx资产，它应该包括以下两个条目。

#### 笔记
实际资产上的walletSymbol需要与支持资产名称具有相同的名称。 示例`：walletSymbol：BTC`对应于支持的资产`DAC.BTC`

```
object = {
    coinType: "dac.btc"
    walletName: null
    name: "Exchange Backed BTC"
    symbol: "DAC.BTC"
    walletSymbol: "DAC.BTC"
    walletType: null
    transactionFee: "0"
    precision: "100000000"
    supportsOutputMemos: false
    restricted: false
    authorized: null
    notAuthorizedReasons: null
    backingCoinType: "btc"
    gateFee: "0.000000"
    minAmount: "0.003"
    maxAmount: "999999999"
    intermediateAccount: "dac-dex"
    coinPriora: 0
    maintenanceReason: null
}

object = {
    coinType: "btc"
    walletName: "BTC"
    name: "BTC"
    symbol: "BTC"
    walletSymbol: "BTC"
    walletType: "btc"
    transactionFee: "0"
    precision: "100000000"
    supportsOutputMemos: false
    restricted: false
    authorized: null
    notAuthorizedReasons: null
    backingCoinType: null
    gateFee: "0.000900"
    minAmount: "0.00000001"
    maxAmount: "999999999"
    intermediateAccount: "dac-dex"
    coinPriora: 0
    maintenanceReason: ""
}
```

### 2. 可用的可交易资产

我们要求的第二个API调用是相应可交易资产的列表。 这也应该返回代币和支持代币的一个条目。

```
object = {
    inputCoinType: "dac.btc"
    outputCoinType: "btc"
    rateFee: "0."
} 

object = {
    inputCoinType: "btc"
    outputCoinType: "dac.btc"
    rateFee: "0."
}
```

### 3. 可用钱包

我们要求的第三个API调用是可用代币钱包列表。 这将确保仅启用此列表中具有`walletType`条目的资产。 这使网关交易所可以禁用钱包，并确保用户在维护期间不能存入或取出资产。

```
["btc","muse","incnt","esc","xdrac"]
```

### 4. 地址验证 

需要的第四个API调用是地址验证

地址应位于 `/wallets/<walletType>/address-validator?address=<address>`

- `walletType` element是代币列表中的walletType。
- `address` element是UI所需的验证提供的用户地址。

如果地址有效，则响应应返回以下内容

```
isValid: true
```


### 5. 地址生成器

需要的第五个API调用是地址生成器

该地址应位于`/simple-api/initiate-trade` 并接受POST查询。

POST查询将采用以下格式

```
inputCoinType: html
outputAddress: userBitSharesAccountName
outputCoinType: open.html
```

响应应返回代币钱包地址供用户存入。 如果使用备注，也应该包括备注。

```
{
  "inputAddress": "HoRNZ6opWQ4P9vAADn3cufNWf3yDPQD8m3",
  "inputMemo": null,
  "inputCoinType": "html",
  "outputAddress": "userBisSharesAccountName",
  "outputCoinType": "open.html",
  "refundAddress": null,
  "comment": ""
}
```
### 6. 提现备注
当用户启动提款时，钱包将生成TX，其中包含包含所选资产和用户地址的字符串的备注，例如`btc：<my-address>`。

如果为网关设置了`useFullAssetName`，则所选资产也将包含网关名称，例如`brdige.btc：<address>`

## 配置交易网关

### apiConfig.js

该文件包含API调用地址。

让我们假设我们希望添加一个名为“DAC”的新交易。

``` 
export const dacAPIs = {
    BASE: "https://api.dacexchange.info/api/v1",
    COINS_LIST: "/coins",
    ACTIVE_WALLETS: "/active-wallets",
    TRADING_PAIRS: "/trading-pairs",
};
```

然后我们将它添加到gateways.js文件中，如下所示

- 对象名称和`ID`应始终与支持的资产前置名称相同。
- `name` 可以是全名
- `baseAPI` 与apiConfig.js中的const相同
- `isEnabled` 可以设置为false以禁用网关
- `selected` 已弃用，可以忽略
- `options{}` 是可视占位符，将在运行时更改。

```
    DAC: {
        id: "DAC",
        name: "DAC Exchange Gateway",
        baseAPI: dacAPIs,
        isEnabled: true,
        selected: false,
        options: {
            enabled: false,
            selected: false
        },
    },
```

------

# 集成网关如何在参考钱包中运行

以下文档介绍了代码如何处理本文档上一节中的各种API调用。 本节旨在用于开发和集成理解。

## 生成可用的支持资产

通过各种API，参考钱包建立了通过提供的网关交易服务存放或提取的资产列表。 由此服务提供商提供正确的响应以确保此列表保持准确和有效。

### 1. 查询

我们查询`/coins`, `/trading-pairs` 和`/active-wallets` APIs. 

1. 构建一个数组 `tradeableCoins[inputCoinType][outputCoinType]`
2. 剔除所有代币并检查我们是否可以存入/取出它们
- a. 如果 `tradeableCoins[coin.backingCoinType][coin.coinType]` 存在, 则启用存款
- b. 如果 `tradeableCoins[coin.coinType][coin.backingCoinType]` 存在, 则启用提现
3. 通过所有支持的代币和检查
- a. 代币有支持者名称 (DAC.xxx)
- b. 代币有backingCoinType字符串
- c. 那是一个相应的资产朋友 (DAC.BTC要求BTC展示)
4. 确保已设置walletType 并且名称存在于 `/active-wallets` 列表中.
- 这将设置 `isAvailable: true`标志

如果所有条件都有效，则资产将作为以下对象结构推送到支持的资产数组。

```
object = {
    "name":"Bitcoin",
    "intermediateAccount":"dac-dex",
    "gateFee":"0.000000",
    "walletType":"btc",
    "backingCoinType":"bitcoin",
    "symbol":"DAC.BTC",
    "minAmount": "0.003"
    "maxAmount": "999999999"
    "supportsMemos":true,
    "depositAllowed":true,
    "withdrawalAllowed":true,
    "isAvailable":true
}
``` 

# 其他情况

在其他情况下，网关已经以“更轻”的方式集成，或者通过一些调整来实现。

### RUDEX
Rudex通过“简单”系统实现，只需要一次API调用。 API在一次推送中返回所有细节，这限制了UI，也是RuDex的支持资产对象看起来有点不同的原因。

### 加密网桥
Crypto Bridge的实现没有`active-wallets`的要求，限制了UI能够感知资产何时无法存入/取出。

### BlockTrades
BlockTrades通过`fetchBackingAsset（）`调用单独实现，但使用的是网关交易的相同技术。