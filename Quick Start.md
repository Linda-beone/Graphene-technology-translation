 ===========================================

  **原文链接**：<http://dev.bitshares.works/en/master/development/testnets.html>

 **译者**：https://github.com/towel223 （小办龙的蜗牛）

 **校对者**：

 ===========================================

#快速启动指南 - 公共测试网#
比特股开放式公共测试网部署完成后，他会为任何使用者提供完整的功能，并且在开发者之间分享。所有的开发者都有机会构建一个教程。
## 目录 ##

>在公共测试网上使用用户界面钱包
>在公共测试网上使用命令行
>>其他参考
# 在公共测试网上使用用户界面钱包 #
>浏览一个Web钱包 - 公共测试网：（http://testnet.bitshares.eu/)

>如果你需要创建一个账户，设置测试网水龙头地址。
>>水龙头:(https://faucet.testnet.bitshares.eu/)
>>进入设置菜单-水龙头，并且设置水龙头地址

>创建一个账户
>>云钱包账户：用【CREATE ACCOUNT】按钮
>>本地钱包账户：用【advance form】按钮

>接收测试网核心资产TEST。
>>在你免费注册的账户中将有1万TEST。

    注释
    水龙头的作用：水龙头地址被用于为新注册用户付费。

# 在公共测试网使用命令行 #
在这一节，我们演示一个在Ubuntu16.04LTS(64 bit)环境下安装配置的例子。

查看更多选项：转到安装指南

1.安装必要的软件

    sudo apt-get update
    sudo apt-get install autoconf cmake git libboost-all-dev libssl-dev g++ libcurl4-openssl-dev

2.构建比特股内核-选择测试网分支

为了运行一个全节点，需要获取分支测试网源码并编译：

    git clone https://github.com/bitshares/bitshares-core.git bitshares-core-testnet
    cd bitshares-core-testnet
    git checkout testnet
    cmake

3.编译完成后，通过如下命令运行节点:

    $ programs/witness_node/witness_node --genesis-json=genesis.json


*警告:
在这个网络中，任何事情都是会发生的。可以预见的是，任何代币的外部价格在这个区块链上都毫无疑问的是零。在任何时候这个区块链都可以发生重置。*

# 其他参考 #

>测试网-Python脚本

>种子节点：testnet.bitshares.eu:11010

>区块链的标识：39f5e2ede1f8bc1a3a54a7914414e3779e33193f1f5693510e73cb7a87617447

>测试网络的创世区块