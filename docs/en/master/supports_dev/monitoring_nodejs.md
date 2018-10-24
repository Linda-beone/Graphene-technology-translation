**原文链接**：[http://dev.bitshares.works/en/master/supports_dev/monitoring_nodejs.html#monitor-balance-nodejs](http://dev.bitshares.works/en/master/supports_dev/monitoring_nodejs.html#monitor-balance-nodejs)
 **译者**：[https://github.com/b-t-s-1](b-t-s-1)
**校对者**：

# 监控余额历史 - NodeJs

此nodejs脚本监视基于石墨烯的网络（如比特股 2.0）中帐户的余额历史记录。

## 安装

用于监视流入交易的NodeJS库可以从下面下载:

    git clone https://github.com/xeroc/node-graphene-monitor
    cd node-graphene-monitor
    npm install

## 配置

要使用此库，我们可以指示任何全节点为任何帐户的所有流入交易发送通知。让我们更详细地讨论以下示例脚本:

我们首先准备变量并导入所有必需的模块

定义accountID和memo_wif_key。accountID可以从GUI钱包获得，或者通过以下命令获得:

    >>> get_account <accountname>

如果脚本异常存在，您可以通过将last_op设置为异常退出之前捕获的最后一个操作ID来继续操作。

> 当前实现的最大历史记录大小为100个交易。如果您错过了当前实施的100多个交易，则需要手动修复。

这些设置在config.js中

- rpc连接设置
- 备忘录解码wif格式的私钥
- 要监控的帐户ID
- 在非正常关闭的情况下，你在备份中成功记录的最后一个操作

>注意:最后一个操作最多显示100个最近的交易。如果由于停机而错过了更多。需要人工干预。

此外，在石墨烯中，通常使用不同的备忘录密钥来加密备忘录。那样，公开备忘录私钥只会暴露交易备忘录（针对该密钥）而不会损失任何资金。因此，将备忘录私钥存储在第三方服务和脚本中是安全的。备忘录公钥可以从帐户设置或通过命令行:

    >>> get_account <myaccount>

在cli钱包里。相应的私钥可以从以下命令获取:

    >>> dump_private_keys

请注意，后一个命令以明文wif显示所有私钥。

## 运行

监控脚本可以通过以下命令执行:

    npm start