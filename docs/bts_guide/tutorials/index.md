::: {#dev-guides}

------------------------------------------------------------------------
:::

**原文链接**：
<http://dev.bitshares.works/en/master/bts_guide/tutorials/index.html>

**译者**：https://github.com/ArcticCat1220 （北极猫）

**校对者**：

============

开发者指南(教程)
================

本节主要负责收集"如何实现"的步骤，以此展现一些执行程序的示例。

------------------------------------------------------------------------

账号
----

-   `我应该如何通过命令行注册账号？ <create-account-dev-cli>`{.interpreted-text
    role="ref"}
-   `我应该如何改变云钱包的密码？ <howto-change-cloud-wallet-pwd>`{.interpreted-text
    role="ref"}
-   `我应该如何导入GUI钱包的账号信息到命令行钱包中去？<howto-import-gui-wallet-account-cli>`{.interpreted-text
    role="ref"}

**股份结算**

-   `我应该如何查核股份结算？ <list-vesting-balances>`{.interpreted-text
    role="ref"}
-   `我应该如何认领股份结算？ <claiming-vesting-balance>`{.interpreted-text
    role="ref"}

API
---

-   `[信息交换是如果从它们的多个服务器统一到比特股UI钱包的？` -
    网关集成需求](https://github.com/bitshares/bitshares-ui/wiki/Gateway-Integration-Requirements)
    : 来自比特股UI钱包
-   `我应该如何使RPC能够登录？ <rpc-logging>`{.interpreted-text
    role="ref"}
-   `如何将Websockets写成脚本？ `{.interpreted-text role="ref"}

资产
----

-   `我应该如何用GUI创建一个新的UIA？ <creating-new-uia-gui>`{.interpreted-text
    role="ref"}
-   `我应该如何手动（命令行）创建UIA?？<uia-create-manual>`{.interpreted-text
    role="ref"}
-   `我应该如何手动（命令行）创建MPA？ <mpa-create-manual>`{.interpreted-text
    role="ref"}
-   `我应该如何（通过命令行）更新/改变一个已经存在的UIA？ <uia-update-manual>`{.interpreted-text
    role="ref"}
-   `我应该如何（通过命令行）发布一个喂价？ <publish-feed>`{.interpreted-text
    role="ref"}

市场预测
--------

-   `什么使市场预测？ <pm>`{.interpreted-text role="ref"}
-   `我应该如何（通过命令行）创建一个市场预测？ <pm-create-manual>`{.interpreted-text
    role="ref"}
-   `我应该如何（通过命令行）关闭/设立一个市场预测？ <pm-close-manual>`{.interpreted-text
    role="ref"}

比特股分布式数据交换(DEX)
-------------------------

-   `我应该如何运行自己的分布式数据交换？(DEX) <distributed-access-to-dex>`{.interpreted-text
    role="ref"}
-   `我应该如何准备比特股数据交换？(单节点版) <exchange-single-node>`{.interpreted-text
    role="ref"}

委员会指南
----------

-   `我应该如何创建一个新的委员会成员？ <committee-create>`{.interpreted-text
    role="ref"}
-   `我应该如何创建一个修改费率的提案？ <committee-fee-change>`{.interpreted-text
    role="ref"}
-   `如何同意/不同意一个提案？ <committee-approve-proposal>`{.interpreted-text
    role="ref"}
-   `如何提案委员会行动？ <committee-propose-action>`{.interpreted-text
    role="ref"}

监控 {#monitoring_support}
----

-   `如何监控账号存款？ - Python <monitoring-account-deposits-python>`{.interpreted-text
    role="ref"}
-   `如何监控账号账单历史明细？ - NodeJS <nodejs-example>`{.interpreted-text
    role="ref"}

性能
----

-   `如何进行性能测试？`(https://github.com/bitshares/bitshares-core/tree/develop/tests/performance)

插件
----

-   `检索插件：如何将账号历史数据存储到检索数据库？ <elastic-search-plugin>`{.interpreted-text
    role="ref"}
-   `节点降存：通过使用witness_node可执行选项，可帮助显著减少对RAM的使用 <memory-nodes>`{.interpreted-text
    role="ref"}
-   `[插件模板-（创建一个API小样hello）]`(https://github.com/bitshares/bitshares-core/blob/hello_plugin/libraries/plugins/hello/README.md)

测试网络
--------

-   `代码覆盖率测试 (维基) <how-to-testing-bts>`{.interpreted-text
    role="ref"}
-   `如何准备一个公共测试网络 - 快速开始指南 <public-testnet-details>`{.interpreted-text
    role="ref"}
-   `如何搭建/部署一个私人测试网络(见证人节点) <private-testnet-guide>`{.interpreted-text
    role="ref"}
-   `如何搭建一个公共测试网络 <public-testnet-guide>`{.interpreted-text
    role="ref"}
-   `如何搭建Python库 <how-to-setup-python-lib>`{.interpreted-text
    role="ref"}
-   `如何进行代码覆盖率测试 <how-to-testing-bts>`{.interpreted-text
    role="ref"}
-   `如何搭建Faucet <how-to setup-faucet>`{.interpreted-text role="ref"}
-   `如何搭建Nignx <how-to-setup-nignx>`{.interpreted-text role="ref"}

转账/交易
---------

-   `比特股交易平均大小是多少字节？`{.interpreted-text role="ref"}
-   `什么交易类型是天然被支持的？`{.interpreted-text role="ref"}
-   `多重签名是如何实现的？ <bts-multi-sign>`{.interpreted-text
    role="ref"}
-   `如何用命令行钱包实现私密转账 <confidential-transactions-guide>`{.interpreted-text
    role="ref"}
-   `如何手动构建交易 <manually-construct-transaction>`{.interpreted-text
    role="ref"}
-   `如何创建、提出、允许交易 <proposing-transaction>`{.interpreted-text
    role="ref"}
-   `如何使用命令行钱包在比特股实现秘密转账（维基） <w-stealth-transfers>`{.interpreted-text
    role="ref"}
-   `提议交易和多重签名 <proposed-tran>`{.interpreted-text role="ref"}

钱包 / 命令行 {#wallet-cli-tutorials}
-------------

-   `如何连接并使用命令行钱包 <run-cli-wallet-steps>`{.interpreted-text
    role="ref"}
-   `如何部署网络和配置钱包 <network-setups>`{.interpreted-text
    role="ref"}
-   `如何使用命令行钱包转账 <transfering-funds-cli-wallet>`{.interpreted-text
    role="ref"}
-   `如何将GUI-Wallet账号导入到命令行钱包中 <howto-import-gui-wallet-account-cli>`{.interpreted-text
    role="ref"}
-   `哪里可以查看比特股公共全节点`(https://github.com/bitshares/bitshares-ui/blob/staging/app/api/apiConfig.js)

见证人节点（全节点）指南
------------------------

-   `如何连接到自己的全节点(GUI) <howto-connect-own-full-node-gui>`{.interpreted-text
    role="ref"}
-   `如何改变您见证人的签名密钥 <change-witness-key>`{.interpreted-text
    role="ref"}
-   `系统需求是怎样的 <system-requirements-node>`{.interpreted-text
    role="ref"} (更新时间: 2018-07-02)
-   `如何运行并使用全节点 <how-to-run-full-node2>`{.interpreted-text
    role="ref"}
-   `如何变成活跃见证人 <howto-become-active-witness>`{.interpreted-text
    role="ref"}
-   `如何以GNU屏幕为背景运行节点 <manage-gun-screen>`{.interpreted-text
    role="ref"}
-   `如何运行比特股API节点 <run-api-node-guide>`{.interpreted-text
    role="ref"}
-   `如何确认块的产生 <veryfy_block_production>`{.interpreted-text
    role="ref"}
-   `如何备份一个服务器 <witness-backup-server>`{.interpreted-text
    role="ref"}
-   `见证人喂价是如何实现的 <witness-price-feeds>`{.interpreted-text
    role="ref"}

矿工指南
--------

-   `如何创建矿工 <worker-create>`{.interpreted-text role="ref"}
-   `如何查看规定的矿工收益 <worker-budget>`{.interpreted-text
    role="ref"}
-   `区块链矿工系统是如何运作的？`(https://bitshares.org/doxygen/group__workers.html)
    (\*打开一个配置文件)

迁移 (来自BitShares1.0版本)
---------------------------

-   `导出您的钱包 <howto-exporting-wallet>`{.interpreted-text
    role="ref"}
-   `导入您的钱包 <howto-importing-wallet>`{.interpreted-text
    role="ref"}
-   `迁移备注`{.interpreted-text role="ref"}

| 

------------------------------------------------------------------------
