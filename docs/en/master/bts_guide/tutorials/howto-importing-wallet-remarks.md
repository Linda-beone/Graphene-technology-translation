  **原文链接**：[http://dev.bitshares.works/en/master/bts_guide/tutorials/howto-importing-wallet-remarks.html#migration-remarks](http://dev.bitshares.works/en/master/bts_guide/tutorials/howto-importing-wallet-remarks.html#migration-remarks)
 
 **译者**：[https://github.com/b-t-s-1](b-t-s-1)
 
 **校对者**： 
  
***  


# 迁移备注

## 关于私钥的备注

根据用户（交易）活动和投资者行为，请注意以下事项：

* **TITAN交易**：如果你上次打开钱包后可能收到TITAN交易（默认交易格式），则需要与BitShares 1.0区块链完全同步以捕获该交易并生成相应的私钥。之后，你可以使用石墨烯兼容钱包导出功能。
* **市场交易**：每个市场订单都与交易时产生的密钥相关联，因此是你钱包的一部分。请注意，对于向BitShares 2.0的过渡，所有未结订单（空头订单除外）将被关闭，资金将返还给其所有者。
* **冷存储资金**：只需将冷私钥导入BitShares 2.0即可申请冷存储资金。这将导致一个交易声明你的资金并将其存入你的帐户。
* **PTS/AGS捐赠者**：你可以直接将相应的私钥导入BitShares 2.0以申请资金。请注意，由于网络钱包无法解析你的*wallet.dat* 文件，因此可能需要使用第三方工具手动转储你的私钥。。如果你对此不满意，我们建议你将*wallet.dat*文件导入BitShares 1.0客户端，并导出可与BitShares 2.0网络钱包导入的石墨烯兼容钱包文件。

## 技术解释

BitShares 2.0钱包架构与BitShares 1.0截然不同。 在BitShares 1.0中，每个帐户包含数十个甚至数千个密钥，每个密钥控制一小部分余额，对于TITAN用户，与你的帐户关联的余额都不会与你的帐户使用相同的密钥。 在BitShares 2.0下，所有这些“余额”都成为唯一帐户而不是单个逻辑帐户。BitShares 2.0钱包有一个“导入”界面，允许你指定一组私钥以及你希望接收与这些密钥关联的所有资金的帐户名称。 然后，它会生成一个交易，该交易将与这些密钥关联的所有帐户的全部余额花费到新的统一BitShares 2.0帐户。 BitShares 0.9.x钱包提供了一个实用程序，用于转储与给定帐户关联的所有私钥，以简化迁移过程。