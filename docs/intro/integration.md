链接：http://dev.bitshares.works/en/master/development/integration.html
译者：bt666



#集成指南

银行，交易所和商家已经集成了石墨烯技术，为即时跨境汇款，企业支付，投票和去中心化交易提供支持。 本节提供了技术信息讲解集成基于石墨烯的技术，从而从以下几个方面获利：
- 实时区块链技术
- 现有的用户基础
- 网络效应
- 现有的生态系统

## 基础知识
如果你还没看过[比特股用户文档](http://how.bitshares.works/en/latest)，建议你先通读一下从而了解比特股的基础知识点。

在此我们提供了一些必要信息，以确保作为商户、交易商、交易所或者菲亚特网关的安全运行。

- [什么是比特股](http://how.bitshares.works/en/latest/)
- [区块链交互](http://dev.bitshares.works/en/master/development/interaction.html#blockchain-interaction-top)
- [常用的API调用](http://dev.bitshares.works/en/master/api/often-used-calls.html#often-used-calls)
- [网络与钱包配置](http://dev.bitshares.works/en/master/development/apps/network.html)
	- [组件](http://dev.bitshares.works/en/master/development/apps/network.html#components)
	- [通用的网络与钱包配置](http://dev.bitshares.works/en/master/development/apps/network.html#general-network-and-wallet-configuration)
	- [安全的网络与钱包配置](http://dev.bitshares.works/en/master/development/apps/network.html#secure-network-and-wallet-configuration)

## 用例
- [交易所、网桥与网关](http://dev.bitshares.works/en/master/development/use_cases/uc_exchange_integration.html)
	- [交易所手把手指导](http://dev.bitshares.works/en/master/development/use_cases/uc_exchange_integration.html#step-by-step-instructions-for-exchanges)
	- [查询区块链获取数据](http://dev.bitshares.works/en/master/development/use_cases/uc_exchange_integration.html#query-blockchain-for-required-data)
- [商户](http://dev.bitshares.works/en/master/development/use_cases/uc_merchant.html)
	- [钱包登录协议](http://dev.bitshares.works/en/master/development/use_cases/uc_merchant.html#wallet-login-protocol)
	- [钱包商户协议](http://dev.bitshares.works/en/master/development/use_cases/uc_merchant.html#wallet-merchant-protocol)
- [交易商]() 
	- [公共API](http://dev.bitshares.works/en/master/development/use_cases/uc_traders.html#public-api)
	- [私有API](http://dev.bitshares.works/en/master/development/use_cases/uc_traders.html#private-api)

- [支持的功能](http://dev.bitshares.works/en/master/development/use_cases/supporting_features.html)
	- [用户发行资产](http://dev.bitshares.works/en/master/development/use_cases/supporting_features.html#user-issued-assets)
	- [白名单和黑名单](http://dev.bitshares.works/en/master/development/use_cases/supporting_features.html#whitelists-and-blacklists)
	- [层次化的企业账户](http://dev.bitshares.works/en/master/development/use_cases/supporting_features.html#hierarchical-corporate-accounts)
- [使用白名单和黑名单](http://dev.bitshares.works/en/master/development/use_cases/uc_using_whitelist.html)
	- [用户白名单](http://dev.bitshares.works/en/master/development/use_cases/uc_using_whitelist.html#user-whitelists)
	- [资产市场白名单](http://dev.bitshares.works/en/master/development/use_cases/uc_using_whitelist.html#asset-market-whitelists)
	- [资产用户白名单](http://dev.bitshares.works/en/master/development/use_cases/uc_using_whitelist.html#asset-user-whitelists)