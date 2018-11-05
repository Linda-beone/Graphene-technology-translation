开发工作更新: 比特股witness程序监控器
==============================================

**原文链接**：
[https://steemit.com/bitshares/@clockwork/devwork-update-bitshares-witness-monitor](https://steemit.com/bitshares/@clockwork/devwork-update-bitshares-witness-monitor)

**译者**：[https://github.com/zoowii](zoowii)

**校对者**：
   
-------

大家好,
我已经决定在steem上发布一些更新，讲述下我做的一些开发工作。
我刚通过telegram集成的方式完成了一个比特股的witness程序的监控脚本的第一个版本。
你可以通过以下链接找到这个程序：
[https://github.com/clockworkgr/bitshares-witness-monitor](https://github.com/clockworkgr/bitshares-witness-monitor)


首先，我想先感谢下@roelandp，我从他的一些工作([https://github.com/roelandp/Bitshares-Witness-Monitor](https://github.com/roelandp/Bitshares-Witness-Monitor))中得到了做这个项目的灵感。

我的监控脚本当前版本仅检查witness节点的进程存活状态，但可能会在将来的版本中添加喂价和种子节点的检查功能。

我打算通过使用telegram的可用命令来允许控制监控程序的几乎所有参数，所以理论上你几乎不需要重新启动监控脚本。你可以暂停它，更新备份keys，恢复运行，修改使用的API节点，以及更新所有监控参数。

例如，如果您想要非常“严密”的监控，你可以将检查间隔时间设置为3秒（差不多是每产一个块检查一次），将漏块阈值设置为切换产块周期前允许1个丢失块，以及设置非常大的重置时间窗口。 因此可以确保及时只错过一个块，你的签名用私钥会被更新为备份的witness节点私钥。

一个“宽松”的监控策略是在切换之前快速地连续寻找超过3个漏块。 因此需要设置漏块阈值为3，设置重置时间窗口为5分钟（这样每5分钟漏块计数器会被重置），并且每隔30秒检查一次。

当你需要在你的witness节点服务器上做一些操作的时候，你还可以根据需要强制切换到备用witness程序。

如果有任何witness节点使用了这种方式，请花点时间来提供反馈，或者提交新特性，或者你可以通过github issue来提交BUG。

下文（基本上是代码仓库中的README文件）更好地解释了这些功能：

首先，复制这个代码仓库（或下载代码仓库的zip压缩包）。

    git clone https://github.com/clockworkgr/bitshares-witness-monitor
    cd bitshares-witness-monitor
    npm install

然后用你喜欢的编辑器打开`config-sample.json`文件，根据你自己的设置编辑这个文件：


    {
        "witness_id": "1.6.XXX",
        "api_node": "wss://<你选择的API节点地址>",
        "private_key": "5kTSOMEPRIVATEKEY111111111111",
        "missed_block_threshold": 3,
        "checking_interval": 10,
        "backup_key": "BTSXXXXXXXXXXXXXXXXXX",
        "reset_period": 300
        "debug_level": 3,
        "telegram_token": "<telegram的access_token>",
        "telegram_password": "<telegram的access password>",
        "retries_threshold": 3
    }

然后把编辑后的内容保存为`config.json`文件.

(译者注：下文是`config.json`中参数的说明)

* private_key 

你的witness节点中用来给更新witness操作签名的账户的管理者私钥。

* missed_block_threshold 

在脚本切换签名私钥之前，重置周期时间窗口需要的最少的漏块数量阈值。 建议设置为2或更大，因为1可能会触发维护间隔时的更新。（参阅：[https://github.com/bitshares/bitshares-core/issues/504](https://github.com/bitshares/bitshares-core/issues/504) )

* checking_interval 

脚本检查新的漏块的时间间隔。

* backup_key 

切换节点时用于备用witness节点的公用签名私钥。

* reset_period 

漏块计数器重置周期的秒数.

* debug_level 

日志级别，可选值为：

    0: Minimum - 显示日志和错误
    1: Info - 级别0 + 基本日志
    2: Verbose - 级别1 + 详细日志
    3: Transient - 级别2 + 临时信息

但是这个参数目前没有使用。

* telegram_token 

你的提醒机器人的telegram的access token。你可以通过以下链接得到一个access token: [https://telegram.me/BotFather])(https://telegram.me/BotFather)

* telegram_password 

你从telegram得到的选择的access password。

* retries_threshold 
连接到API节点的连接最多的失败次数，达到后提醒机器人就通过telegram发出提醒。

# 运行

简单使用:

在一个screen（或者类似的工具）会话中执行：

    node index.js

为了更稳定，我推荐你使用`forever`这个工具：

    sudo npm install forever -g

然后用以下命令运行（而不是`node index.js`):

    forever index.js

注意：当`forever`工具重启这个进程的时候，它将从你提供的默认的 `config.json`文件来启动，而不保留在shell会话中使用下面提到的电报命令做出的修改。

# Telegram 命令

打开你的telegram机器人的一个会话窗口，你可以使用以下这些命令:

* /pass <你配置的telegram密码>

这个命令必须先执行的，用来做身份验证。否则下面的命令都无法正常工作。

*  /changepass <新密码>

这个命令会更新你的telegram的access password，并且执行后需要你重新通过`/pass`命令来再次登录。

* /stats

这个命令会返回当前配置，以及监控会话的统计信息。

* /switch

这个命令会用你当前配置的备用私钥替换你当前的签名私钥，并立刻生效。

* /new_key <BTS_public_signing_key>

这个命令会设置一个新的备用私钥并替换到当前配置中。

* /new_node wss://<api_node_url>

这个命令会设置一个新的用来连接的API节点地址。

* /threshold X

这个命令会设置更新签名前的漏块阈值为X个漏块。

* /interval Y

这个命令设置脚本的检查间隔时间为Y秒。

* /window Z

这个命令设置漏块计数器的周期为Z秒。

* /retries N

这个命令会设置通过telegram提醒你前连接API节点的最大失败次数为N。

* /reset


这个命令会在当前时间周期中重置漏块计数器。

* /pause

这个命令会暂停监控。

* /resume

这个命令会恢复监控。
