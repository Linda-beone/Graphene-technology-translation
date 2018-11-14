  **原文链接**：[http://dev.bitshares.works/en/master/bts_guide/migration/howto-importing-wallet.html#howto-importing-wallet](http://dev.bitshares.works/en/master/bts_guide/migration/howto-importing-wallet.html#howto-importing-wallet)
 
 **译者**：[https://github.com/b-t-s-1](b-t-s-1)
 
 **校对者**： 
  
***  

# 导入你的钱包

目录

* 网络钱包
* CLI钱包
  - 申请余额
  - 手动申请余额

## 网络钱包

BitShares 2.0的网络钱包有一个**钱包管理控制台**。 这将有助于你导入资金。 它可以通过BitShares 2.0访问：设置 - >钱包  
  
![](http://dev.bitshares.works/en/master/_images/wallet-management-console.png)

要导入现有帐户并申领所有资金，你需要选择`导入密钥`。

 <table style="width: 750px;"><tbody>
    <tr>
        <td bgcolor="LightBlue">注意</td>
    </tr>
    <tr>
        <td bgcolor="MintCream"> 如果使用无效格式加载文件文件，请确保你已按照[导出钱包](http://dev.bitshares.works/en/master/bts_guide/migration/howto-exporting-wallet.html)中所述的步骤操作，并确保单击导入密钥而不是还原备份。</td>
    </tr>
</table>
  
  
![](http://dev.bitshares.works/en/master/_images/wallet-management-console-import-keys.png)

在这里，你可以提供从BitShares 0.9.3c和密码短语生成的钱包备份文件。 根据导入文件的大小，此步骤可能需要一些时间才能自动完成。 请耐心等待。

钱包将列出你的所有帐户，包括相应存储在帐户名称中的私钥数量。 你使用帐户的次数越多，此数字就越高。 按`导入`确认。  
  
![](http://dev.bitshares.works/en/master/_images/wallet-management-console-imported-keys.png)

钱包管理控制台现在将概述无人认领的余额。  
  
![](http://dev.bitshares.works/en/master/_images/wallet-management-console-claim-balances.png)

如果你点击`余额申领`，你将进入此屏幕。  
  
![](http://dev.bitshares.works/en/master/_images/wallet-management-console-claiming-balances.png)

如果你有多个帐户，系统会要求你定义个人余额的存放位置。

确认所有必需步骤后，你的帐户和余额将相应显示。

<table style="width: 750px;"><tbody>
    <tr>
        <td bgcolor="LightBlue">注意</td>
    </tr>
    <tr>
        <td bgcolor="MintCream"> 导入帐户和余额后，我们建议对钱包做一次新的备份，钱包含账户的访问权限和相应的余额。</td>
    </tr>
</table>

## CLI钱包

钱包备份文件可以导入  

`>>> import_accounts <path to exported json> <password of wallet you exported from>`

请注意，这不会自动声明余额。

### 申请余额

对于钱包中的每个帐户`<my_account_name>;`（运行`list_my_accounts`以查看它们）:  
  
`>>> import_accounts_keys /path/to/keys.json <my_password> <my_account_name> <my_account_name>`

<table style="width: 750px;"><tbody>
    <tr>
        <td bgcolor="LightBlue">注意</td>
    </tr>
    <tr>
        <td bgcolor="MintCream">在发布标记中，这将在导入的每个密钥后创建钱包的完整备份。 如果你有成千上万的密钥，这很慢，也占用了大量的磁盘空间。 在导入期间监视可用磁盘空间，并在必要时定期擦除备份以避免填满磁盘。 最新代码仅在导入所有密钥后保存你的钱包。</td>
    </tr>
</table>

上面的命令只会将你的密钥导入钱包，并且**不会**要求您的资金。 要申请您需要执行的资金:

`>>> import_balance <my_account_name> ["*"] true;`

<table style="width: 750px;"><tbody>
    <tr>
        <td bgcolor="LightBlue">注意</td>
    </tr>
    <tr>
        <td bgcolor="MintCream">如果要预览此声明交易，可以使用false替换true。 这样，交易就不会被广播。</td>
    </tr>
</table>

要验证结果，你可以运行:
<pre><span>&gt;&gt;&gt; </span><span>list_account_balances</span> <span>&lt;</span><span>my_account_name</span><span>&gt;</span>
</pre>

### 手动申请余额

余额可以逐个导入。 这样做的正确语法是：
<pre><span>&gt;&gt;&gt; </span><span>import_balance</span> <span>&lt;</span><span>account</span> <span>name</span><span>&gt;</span> <span>&lt;</span><span>private</span> <span>key</span><span>&gt;</span> <span>true</span>
</pre>

但我总是导入我的帐户，然后使用GUI导入我的余额，因为它更容易。