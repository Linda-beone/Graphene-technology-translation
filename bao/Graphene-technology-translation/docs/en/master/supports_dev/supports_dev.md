**原文链接**：[http://dev.bitshares.works/en/master/supports_dev/supports.html](http://dev.bitshares.works/en/master/supports_dev/supports.html)
 **译者**：[https://github.com/b-t-s-1](b-t-s-1)
**校对者**：
---
# 支持和优化

本节旨在收集并提供比特股开发的有用信息。我们希望在持续的添加其他工具，库和支持信息。


 **目录**
* ##### Python-比特股
* ##### API支持 & 参考
* ##### 库
* ##### 软件开发工具包
* ##### 工具
* ##### 插件
* ##### 程序
***

## Python - 比特股

  - [比特股的Python库:GitHub](https://github.com/bitshares/python-bitshares#python-library-for-bitshares)
      - 比特股的全功能客户端库- 完全用python编写。
  - [Python-比特股 0.1 文档](http://docs.pybitshares.com/)-<http://pybitshares.com/>
  - `(更多...) <lib-python>`

## API支持和参考
  - 比特股区块浏览器和封装器
      - 对于主网和测试网：ES 封装查询比特股数据。 *试试看！*
      - 检查比特股区块链的健康状况。

  - [比特股浏览器 REST API - 安装指导](https://github.com/oxarbitrage/bitshares-explorer-api#bitshares-explorer-rest-api)
 
      - 逐步完成为生产环境启动并运行自己的比特股浏览器 API所需要的一切。


## 库

  - [比特股-fc: Doxygen文档](http://open-explorer.io/doxygen/fc/)
  - [比特股-fc: GitHub](https://github.com/bitshares/bitshares-fc#fc)
  
      - FC代表快速编译c++库，并提供一组用于开发异步库的实用程序库。

## 软件开发工具包

|       |
| ----- |
|       |
| 工具 |


  - [Docker容器](https://github.com/bitshares/bitshares-core/blob/master/README-docker.md)
  
      - 内置Dockerfile以支持docker容器。本自述文件用作文档。

  - [bitsharesjs](https://github.com/bitshares/bitsharesjs#bitsharesjs-bitsharesjs)

    - 用于比特股加密和序列化的JavaScript工具
  
  - [bitsharesjs-ws](https://github.com/bitshares/bitsharesjs-ws#bitshares-websocket-interface-bitsharesjs-ws)
      - 比特股的Javascript websocket接口
  - `what-if-test`
      - debug_node是一个工具，允许开发人员使用区块链中的状态运行许多有趣的“假设”测试。
  - `monitor-account-python`
      - (如何设置)
  - `monitor-balance-nodejs`
      - 此nodejs脚本监视基于石墨烯的网络中帐户的余额历史记录

## 插件

  - [讨论: "创建插件脚本"(\#1302)](https://github.com/bitshares/bitshares-core/pull/1302)
  - `elastic-search-plugin`
      - 如何将帐户历史数据存储到elasticsearch数据库中。
  - `memory-nodes`
      - 使用witness_node可执行选项帮助显著减少RAM使用。
  - [插件模板 - (创建一个demo API hello)](https://github.com/bitshares/bitshares-core/blob/hello_plugin/libraries/plugins/hello/README.md)
  
# 程序

  - `websocket-script-support`
      - Ptython - Ptython - websocket-client与内核 API交互