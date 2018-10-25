**原文链接**：[http://dev.bitshares.works/en/master/development/use_cases/uc_traders.html](http://dev.bitshares.works/en/master/development/use_cases/uc_traders.html)
 
 **译者**：[https://github.com/b-t-s-1](b-t-s-1)
 
 **校对者**： 
  
***
# 交易者

**交易者** 交易者利用提供的API与石墨烯网络（例如比特股）进行交互，并提供市商和流动性。 API可以很容易地用于实现基于交易算法的自动化机器人。

基于区块链的去中心化交易所（DEX）与中心化交易所略有不同，因此，通过API以程序化方式处理DEX也不同于中心化的方法。但是，我们的开发人员已经付出了很大的努力使DEX像中心化一样易于使用，并为非常相似的公共交交换数据提供API。但是，私人交换API不同，因为除了你自己以外的任何实体都无法访问你的资金。因此，在DEX中进行交易要求你在应用程序中安装了你的帐户私钥，该应用程序可以为你构建和签署相应的交易。其中一个应用程序是：ref：`cli-wallet`，在安装和配置之后，它提供了你自己的私有API。

对于交易，我们还建议阅读：ref：`what-is-different`和：ref：`often-used-calls`。

## 公共API


获取市场公开数据的最佳方式是通过websocket连接到为交易者提供的公共全节点

  - 一个市场
  - 交易记录
  - 交易历史
  - 更多.


关于如何与基于石墨烯区块链的接口的详细描述（例如比特股）和可用的调用列表
如下：

  - `websocket-calls`
  - `blockchain-api`

-----

## 私有API

如上所述，在DEX上的程序化交易需要你运行自己的`cli-wallet`。以下教程给出了一个简要介绍如何使用CLI钱包并对其进行配置正确，以便它可以用作**私有API服务器**：

  - `如何为交易准备CLI钱包
    <how-to-prepare-cliwallet-traiding>`

安装和配置私有API后，我们可以使用RPC创建订单，取消订单，创建和调整订单，以及更多。 CLI钱包提供可用于管理你的帐户的各种调用并在DEX中交易：`wallet-api-calls`

已经开发了只与两者相互作用的库，全节点和CLI钱包使得与区块链，DEX交互非常简单：`support-libraries`