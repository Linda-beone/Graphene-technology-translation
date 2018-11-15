  **原文链接**：[http://how.bitshares.works/en/master/technology/graphene.html](http://how.bitshares.works/en/master/technology/graphene.html)
 
 **译者**：[https://github.com/b-t-s-1](b-t-s-1)
 
 **校对者**： 
  
***

# 石墨烯

## 欢迎来到石墨烯文档

BitShares的开发人员组建了Cryptonomex，以便在他们开发和运营的前两年积累的技术，经验，声誉和良好意愿中获益。其中大部分技术都体现在石墨烯中，这是一种工业强度软件平台，用于部署第三代加密安全分布式账本，称为区块链。

基于石墨烯的系统比第一代比特币衍生系统或甚至构成我们当前最接近的竞争对手的第二代“比特币2.0”系统具有更高的性能。基于石墨烯的系统不仅仅是“支票账簿”式支付，而是提供广泛的金融服务，其特点是透明度和固有的不可回溯性。

本页介绍了[Cryptonomex](http://cryptonomex.com)构建的[石墨烯](https://github.com/cryptonomex/graphene)技术。 您可以将石墨烯视为实时区块链的工具包。 为方便起见，我们将文档分成较小的部分，以便于相关信息的放置。

### 最近更新

* `17/04/04`压力测试结果可在bitshares/papers/index中找到
* `16/06/29`bitshares/tutorials/distributed-access-hosting
* `16/04/07`入门
* `16/03/17`整合/交易者/指数，整合/库/指数
* `16/03/15` bitshares/investor/index,bitshares/investor/claim,bitshares/migration/legacy-blockchain
* `16/03/02` bitshares/user/referral-program
* `16/03/01` bitshares/tutorials/cli-wallet-usage,bitshares/tutorials/transfer-funds-cli,bitshares/user/voting,bitshares/tutorials/voting
* `16/02/13` [Public API](https://docs.readthedocs.io/en/latest/api/index.html)的巨大改进
* `16/02/08</span` bitshares/user/committee,bitshares/tutorials/committee-approve-proposal,bitshares/user/vesting
* `16/02/01` 集成/商家/商家协议，添加了对导航的搜索
* `16/01/19` 测试网/index,bitshares/tutorials/pm-create-manual,bitshares/user/eba
* `16/01/13` bitshares/tutorials/uia-update-manual,bitshares/tutorials/uia-create-manual,bitshares/tutorials/uia-create-gui,integration/network-setup,integration/tutorials/index
* `16/01/12` bitshares/user/assets,bitshares/tutorials/uia-create-manual bitshares/tutorials/mpa-create-manual,bitshares/user/assets-faq,bitshares/user/privbta,bitshares/tutorials/publish-feed,bitshares/user/pm,bitshares/tutorials/pm-create-manual,bitshares/tutorials/pm-close-manual

### 区块链规范指南

石墨烯技术已经应用于几个区块链。 BitShares 2.0是石墨烯技术的第一个应用程序，您将能够找到BitShares 2.0中实现的几乎所有功能。 进一步的区块链将独立添加。

BitShares 2.0是一个金融智能合约平台，可以实现数字资产的交易，并且具有跟踪其基础资产价值的市场挂钩资产（例如跟踪美元的bitUSD）。
