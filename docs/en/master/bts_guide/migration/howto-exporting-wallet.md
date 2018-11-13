  **原文链接**：[http://dev.bitshares.works/en/master/bts_guide/migration/howto-exporting-wallet.html#howto-exporting-wallet](http://dev.bitshares.works/en/master/bts_guide/migration/howto-exporting-wallet.html#howto-exporting-wallet)
 
 **译者**：[https://github.com/b-t-s-1](b-t-s-1)
 
 **校对者**： 
  
***  

# 导出你的钱包 

在本教程中，我们会将你的钱包（包括访问你的帐户和资金的所有密钥）导出到一个JSON格式的文件中。 请注意，你的私钥将被加密，并且在将资金导入BitShares 2.0时，你需要提供相应的密码。

目录

* BitShares 1.0全客户端
  - 同步你的钱包
  - 通过主菜单导出
  - 通过控制台导出

## BitShares 1.0全客户端

由于快照已经发生，所以你现在需要做的就是在BitShares 2.0中访问你的资金，如下所述。

首先，你需要将BitShares客户端升级到0.9.3c版。 要进行升级，你需要：

* 从bitshares网页下载安装文件 [bitshares webpage](https://github.com/bitshares/bitshares-0.x/releases)
* 卸载以前版本的BitShares客户端
* 安装新版本

### 同步你的钱包

 <table style="width: 750px;"><tbody>
    <tr>
        <td bgcolor="LightBlue">注意</td>
    </tr>
    <tr>
        <td bgcolor="MintCream">如果你在钱包中看到所有资金，则可以安全地跳过此段落。</td>
    </tr>
</table>  

我们现在需要与区块链同步。 只有当你认为自上次执行同步以来，有一些涉及你的帐户的新交易时，才需要这样做。

[旧版BitShares 0.9.3区块链下载](http://dev.bitshares.works/en/master/bts_guide/migration/legacy-blockchain.html)

你可以从状态栏或控制台中的`info`命令（`帐户列表 - >高级设置 - >控制台`）查看同步进度。 同步区块链后，你的钱包为新的交易将自动尝试重新扫描区块链。 根据钱包中的帐户数量，此步骤只需几分钟。

从BitShares 0.9.3c开始，我们有一个兼容石墨烯的导出密钥功能，可以通过两种方式访问：

* 通过在主菜单中访问它
* 通过在控制台中发送命令

### 通过主菜单导出

只需选择`文件菜单 - >导出电子钱包`，系统就会要求你选择要导出密钥的文件位置。

 <table style="width: 750px;"><tbody>
    <tr>
        <td bgcolor="LightBlue">注意</td>
    </tr>
    <tr>
        <td bgcolor="MintCream">由于已知错误，如果你在Windows上，唯一适合你的选项是控制台命令 - 使用菜单导出的文件将与BTS 2.0不兼容。 这仅指Windows。</td>
    </tr>
</table> 


### 通过控制台导出

* 进入到控制台：帐户列表 - >高级设置 - >控制台
* 类型：例如 在Windows上: `wallet_export_keys[文件的完整路径]/[文件名] .json`  
  	例如在Mac上：`wallet_export_keys /Users/[你的用户名]/Desktop/keys.json`  
    例如在Linux上：`wallet_export_keys /home/[你的用户名]/Desktop/keys.json`
* 请用您的Windows帐户名替换`[你的用户名]`。
* 并按Enter键

 <table style="width: 750px;"><tbody>
    <tr>
        <td bgcolor="LightBlue">注意</td>
    </tr>
    <tr>
        <td bgcolor="MintCream">导出的钱包文件将使用你的密码加密！尝试再次使用该文件时务必记住它！</td>
    </tr>
</table> 

 <table style="width: 750px;"><tbody>
    <tr>
        <td bgcolor="LightBlue">注意</td>
    </tr>
    <tr>
        <td bgcolor="MintCream">如果你在Windows上，并且文件路径尝试直接访问C盘（例如C：keys.json）你可能需要使用管理员来运行BitShares客户端。 因此，最不复杂的选择是放在桌面如上例所示。</td>
    </tr>
</table> 
  
![](http://dev.bitshares.works/en/master/_images/export-wallet-console.png)  

# wallet.bitshares.org

只需下载备份钱包即可导出网络钱包的密钥。 它可以从网络钱包的首选项中获得:(*帐户列表 - >高级设置 - >钱包*）。