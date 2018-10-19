Account Registration (developer)
===================================

**原文链接**：
[http://dev.bitshares.works/en/master/bts_guide/accounts/account-create.html](http://dev.bitshares.works/en/master/bts_guide/accounts/account-create.html)

**译者**：[https://github.com/zoowii](zoowii)

**校对者**：
   
-------

在石墨烯区块链上注册一个账户的过程包含了两个步骤：

* 随机产生一个脑钱包私钥并从脑钱包密钥中推导得出公私钥对（一个公钥和对应的一个私钥）
* 在区块链上创建并注册（译者注：创建和注册账户是2个步骤，创建账户不上链，注册账户上链）上一步的公私钥对关联的账户

   
# 1. 前提条件

* 连接到witness_node(全节点或API节点)(译者注：witness_node是石墨烯的不包含钱包功能的全节点程序)
* 启动cli_wallet程序的命令行窗口，接下来要使用这个命令行窗口

   
# 2. 脑钱包私钥，私钥和公钥的推导

我们可以在`cli_wallet`窗口中使用`suggest_brain_key`命令来产生一组新的单词。结果看起来像下面这样：


    >>> suggest_brain_key
    {
      "brain_priv_key": "FILINGS THEREOF ENSILE JAW OVERBID RETINAL PILULAR RYPE CHITTY RAFFERY HANDGUN ERANIST UNPILE TWISTER BABYDOM CIBOL",
      "wif_priv_key": "5JVrt2921aikA7QP5ZCtR2sJh4wbEnsHsK6qo67Shnk9ArKMzNT",
      "pub_key": "BTS7D8jpQ2UwaQxqKyGpuhFQ9LBugCNxDhE7UN2jqgjVQgzG7zo9n"
    }

以上这些值的结构关系如下：

    HASH(brain_priv_key) -> wif_priv_key
    HASH(HASH(brain_priv_key)) -> pub_key

因此，如果你保存好了脑钱包私钥，你就可以从中恢复出你需要的公私钥，从而可以访问你在区块链上的账户和资产。

> 注意：虽然 `suggest_brain_key`命令的结果中私钥只有一个（这个私钥用于账户的 **所有者权限**)，
> 大多数石墨烯钱包的实现还会产生一个额外的私钥，用于账户的 **活动权限**!

# 3. 创建并注册账户

如果想不让其他人介入，而是自己创建注册一个账户，需要在你的另外一个账户中有资产。然后你就可以通过`create_account_with_brain_key`命令
来创建账户（译者注：需要先完成上面几个步骤）:

    >>> create_account_with_brain_key <脑钱包私钥> <账户名> <注册人账户名> <推荐人账户名> <是否广播>

例如：

    >>> create_account_with_brain_key "FILINGS THEREOF ENSILE JAW OVERBID RETINAL PILULAR RYPE CHITTY RAFFERY HANDGUN ERANIST UNPILE TWISTER BABYDOM CIBOL" mywallet myfunds anonymous 100 true

# 4. 注册一个账户

如果你想为其他人注册一个账户,你需要知道被注册账户的公钥。理论上来说，石墨烯区块链的每个账户有3组密钥，分别是 **拥有者**，**活动**, **备注**密钥。然而简单起见，本文中我们只使用其中一个公钥。


为了在链上注册一个账户，我们需要另外一个用来给这个注册行为支付注册手续费的账户。这个支付注册手续费的账户称作 `注册人账户`.
注册时另外提供了一个称作`推荐人账户`的账户，推荐人账户能得到这个新账户的推荐奖励中的`推荐人奖励比例`。 任何已注册账户都可以作为推荐人。
因此在这个例子中可以说`匿名`用户推荐了我们。用法如下：

    >>> register_account 账户名, 所有者权限公钥, 活动权限公钥, 注册人账户, 推荐人账户, 推挤人奖励比例, 是否广播

在以下这个例子中，我们使用以上推导得到的公钥创建了一个名为`mywallet`的新用户，支付注册手续费的账户是`myfunds`账户:

    >>> register_account mywallet BTS7D8jpQ2UwaQxqKyGpuhFQ9LBugCNxDhE7UN2jqgjVQgzG7zo9n BTS7D8jpQ2UwaQxqKyGpuhFQ9LBugCNxDhE7UN2jqgjVQgzG7zo9n myfunds anonymous 100 true

> 注意: 为了注册一个账户，注册人（本例中是`myfunds`）需要是一个 **终身会员**账户! 

   

## `register_account`

| | |
|  -------------   | ------------------------------------------------ |
|   说明:   |  用来在石墨烯区块链上创建一个第三方的账户. 这个命令是用来为他人注册账户的（注册人没拥有这个账户的私钥）。当你作为一个注册人时，其他用户需要产生他们自己的私钥并把龚玥传给你（注册人）。注册人使用这个命令来代表其他用户注册账户。  |

####  **参数** 

*[账户名]* 新账户的名称，需要在链上唯一。短账户名需要更高的注册手续费；具体规则还会变动，但是通常一个有超过8个字母和至少一个数字的账户名称注册手续费更便宜（比如: mywallet)(译者注: mywallet是`贵`账户名，mywallet1是更便宜的账户名)  

*[所有者权限公钥]*  新账户的所有者权限的公钥 (比如：`suggest_brain_key`命令产生的"pub_key"部分的值)  

*[活动权限公钥]* 新账户的活动权限的公钥 (比如：`suggest_brain_key`命令产生的"pub_key"部分的值)

*[注册人账户]* 支付注册这个账户的注册手续费的账户名称  (比如： myfunds)          

*[推荐人账户]* 新账户的推荐人的账户名，这个推荐人账户可以得到这个新用户交易手续费的一部分 (参数例子： anonymous) 

*[推荐人奖励比例]* 这个新用户的交易手续费排除链上拿走部分剩下的一定比例 (0 - 100) 会发送给推荐人 (参数例子： 100) 

*[是否广播]*  true表示把这个交易广播到区块链网络 (参数例子： true)   
