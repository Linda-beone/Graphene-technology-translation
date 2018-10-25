
如何把图形界面钱包的账户导入命令行钱包
===========================================

**原文链接**：
[http://dev.bitshares.works/en/master/bts_guide/tutorials/cli_import_guiwallet_account.html](http://dev.bitshares.works/en/master/bts_guide/tutorials/cli_import_guiwallet_account.html)

**译者**：[https://github.com/zoowii](zoowii)

**校对者**：

---------

命令行和图形界面钱包是两个不同的程序。它们有不同的备份方式。你可以手动把私钥从图形界面钱包程序导入命令行钱包程序，从而拥有一个命令行钱包。

你的钱包私钥是非常非常重要的。通过把你的私钥导入一个新的命令行钱包，你可以在命令行钱包中控制你的账户资产。

# 1. 在你的图形界面钱包中找到你的私钥

  1. 登录到你的图形界面钱包
  2. 访问 [设置] – [权限]。 可以看到 *活动*, *所有者*, *备注* 标签页。
  3. 在上步每个标签页中，点击你的公钥（或私钥图标）。一个私钥查看对话框会被打开。
  4. 在这个对话框界面中，点击[显示]按钮，如果要求登录请登录到钱包。然后保存下你的每个私钥和公钥，用于之后的使用。
  
  
> 主要提示： 把你的私钥备份到一个安全的地方。

# 2. 通过指向到一个活跃钱包节点来启动一个命令行钱包程序

**例子:**

    ./programs/cli_wallet/cli_wallet --server-rpc-endpoint ws://localhost:8090
    或
    ./programs/cli_wallet//cli-wallet -s wss://bitshares.openledger.info/ws

你可以看到一个输入提示框:

    new>>>
    
# 3. 设置密码并解锁

为你的命令行钱包设置一个密码，并解锁钱包

> 提示： 这个密码不需要和图形界面钱包的密码一样。通过本章节的操作，你会创建一个有新密码的*新钱包*。


    new >>> set_password mypass123
    set_password mypass123
    null
    
    locked >>> unlock mypass123
    unlock mypass123
    null
    unlocked >>>

	
# 4. 导入私钥

把你之前从图形界面钱包保存下来的私钥导入到你的新命令行钱包中。

    import_key 你的账户名 你保存下的私钥

完成以上步骤后，你就完成了导入账户到命令行钱包的操作了。不需要声明账户余额。你的账户余额可以在钱包中查看到。

> 提示： 如果你使用这个钱包来转账，你需要导入这个账户的活动权限私钥。然而如果你有不同的活动权限私钥和备注权限私钥，并且转账的时候使用了备注，你也需要把备注私钥导入这个命令行钱包。

# 5. 检查账户
-------------------------------

使用 `list_my_accounts` 命令可以查看你导入的账户列表。

检查账户余额：

    unlocked >>> list_account_balances your-account-name
    list_account_balances your-account-name
    31016.69330 BTS
    0 CNY
    0 USD

    unlocked >>>


# 常用命令

|  命令   |  说明   |
| --- | --- |
|``set_password`` |  给当前钱包设置新密码。只有`new`或`unlocked`状态的钱包才可以执行这个命令。    |
|``unlock`` |  解锁当前钱包  |
|``gethelp`` |   (比如： gethelp "list_accounts")  返回某个命令的详细帮助文档  |
| ``info``   | 查看当前同步状态  |
|``about`` | 返回客户端版本，石墨烯框架，fc哭的git版本好，boost版本号，openssl版本号等信息  |
|``import_key`` | `import_key <账户名> "<wif私钥>"` 导入私钥到一个已经存在的账户中。这个私钥必须和这个账户的所有者权限或者活动权限匹配。  |
|``list_my_accounts``|  列出这个钱包中所有账户列表。这个命令返回这个钱包有私钥的所有账户信息的列表。 |
|``list_account_balances`` | 列出某个账户的所有余额。每个账户中每种资产类型有一个余额。这个命令返回账户中余额不为0的所有资产和余额信息。   |
|``get_account`` |  返回指定账户的信息。 |
|``import_balance`` |  `import_balance <name> ["*"] true` 这个命令会钩子一个把指定WIF私钥控制的所有余额转移到指定账户名的交易  |
|``suggest_brain_key`` |  `创建一个推荐的用来创建账户的安全的助记词。 `  |
| `create_account_with_brain_key`| 指定一组助记词来创建账户 |
|``dump_private_keys`` | 导出这个钱包管理的所有私钥。这些私钥会用WIF格式显示。你可以把这些私钥通过`import_key()`命令导入到其他钱包     |
|``upgrade_account`` |  `upgrade_account faucet true`  升级账户到会员账户。这个命令会让这个账户得到终身会员身份。 |
|``register_account``|    `register_account <账户名> <所有者权限公钥> <活动权限公钥> <注册人账户>  <推荐人账户> <推荐人奖励比例> <是否广播>` 在链上注册一个第三方用户的账户   |
|``transfer``|    `transfer <来源账户> <接收账户> <金额> <资产> <备注> <是否广播>`  把一定金额的资产从一个账户转账到另外一个账户。  |
|``transfer2``|    `transfer2 <来源账户> <接收账户> <金额> <资产> <备注> <是否广播>`  这个命令和`transfer`类似，但是它总是会广播交易并返回签名后交易的交易ID。  |
|``get_account_history`` |  比如：`get_account_history "name" "5"` 返回指定账户的最近操作历史。  |
|``get_private_key``|     `get_private_key <公钥>` 获取指定公钥对应的私钥。这个公钥必须在这个钱包中管理。  |


