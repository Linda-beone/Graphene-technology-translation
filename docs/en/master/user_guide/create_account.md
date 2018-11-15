  **原文链接**：[http://how.bitshares.works/en/master/user_guide/create_account.html](http://how.bitshares.works/en/master/user_guide/create_account.html)
 
 **译者**：[https://github.com/b-t-s-1](b-t-s-1)
 
 **校对者**： 
  
***  


# 2. 创建一个BitShares钱包

目录

* 创建一个BitShares钱包
  - 术语
  - 轻钱包或网络钱包？
  - 创建一个帐户
     + 步骤
  - 登录
     + 云钱包登录方式
  - 高级：创建一个帐户
     + 步骤
  - BitShares钱包功能
     + 快速查看钱包选项
         * 仪表板
         * 侧面菜单 - 下拉条目

***

## 2.1. 术语

在本节中，我们要描述术语并指导您创建和注册您的BitShares帐户。

**钱包**

钱包与区块链交互以处理账户和资金功能。用户注册以创建单个钱包。单个钱包可以携带许多帐户。拥有终身会员资格（LTM）的用户可以并行注册多个帐户;所有这些都存储在一个钱包里。此外，用户可以创建多个钱包来正确管理其帐户。

**帐户**

在BitShares中，您可以创建自己的**唯一帐户名**，因此，您可以轻松记住。并且您将使用帐户名称与其他BitShares帐户用户（BTS持有者）进行通信（例如发送资金），像如电子邮件地址一样。使用帐户名称的好处是，人们可以通过使用可读且令人难忘的单词而不是加密地址来识别您。

每个用户至少有一个帐户可用于与区块链交互。该帐户可以被视为具有个人余额，交易历史等的单个银行帐户。由于这些帐户在区块链上注册并向公众开放，我们建议选择假名以实现一些隐私。

**密钥**

密钥是指用于保护对您的帐户和资金的访问的加密技术。防止他人访问这些密钥是非常重要的。

BitShares具有_owner，active和memo keys_。每个密钥都有_公钥和私钥_。重要的是要知道所有者权限对整个帐户具有管理权限。 Active 权限被视为“在线”权限，允许访问资金和一些帐户设置。

***

## 2.2. 轻钱包或网络钱包？

在我们创建钱包之前，让我们检查一下你可以拥有什么类型的BitShares钱包。快速查看下面的图表。  
  
![](http://how.bitshares.works/en/master/_images/BitShares-wallet-flow.png)

你有没有找到你想要的钱包类型？

* **如果要安装Light Wallet（BitShares UI），请下载[BitShares UI Releases](https://github.com/bitshares/bitshares-ui/releases)文件并将其安装到您的计算机上。**  
  * *这并不意味着您将拥有本地钱包。*

* 如果您想使用**网络钱包**，请访问此链接([https://wallet.bitshares.org](https://wallet.bitshares.org))。

***

## 2.3. 创建一个帐户

在本节中，您将创建一个**云钱包**。

我们使用术语云钱包，但技术上没有任何内容存储在云端。 我们称之为云钱包，因为**您可以随时使用任何网络浏览器的凭据（用户名和密码）**来访问您的帐户。

**欢迎BitShares - 创建帐户和登录表单**  
  
![](http://how.bitshares.works/en/master/_images/welcome-bitshares1.png)

### 2.3.1. 步骤

* 1.点击[**创建帐户**]
* 2.输入[**帐户名称**]。 您可以创建唯一的BitShares帐户名称。
* 3.设置密码。 复制并使用**生成的密码**
* 4.输入或粘贴密码进行确认。
* 5.选中复选框。 **确保在检查之前阅读！**
* 6.点击[**创建帐户**]  
  
![](http://how.bitshares.works/en/master/_images/create-account1.png)

在您提交之前，如果您保存了正确的密码，请再次检查您的密码。

**只有你可以再次打开你的钱包。 没有人能帮忙。**不要丢失！

* 7.单击[**显示我的密码**]并仔细检查您是否有正确的密码。
* 8.点击[**好，带我去控制面板**]  
  
![](http://how.bitshares.works/en/master/_images/create-account4.png)

* 如果未打开，请单击顶部菜单[**Dashbord**]。  
  
![](http://how.bitshares.works/en/master/_images/dashboard.png)

现在，你有一个BitShares **云钱包**。 在您为帐户注入资金之前，请登录以确保您的密码是否正确。

***

## 2.4. 登录

单击右上角的**Locked Key**图标打开登录表单。  
  
![](http://how.bitshares.works/en/master/_images/topmenu-lock.png)

### 2.4.1. 云钱包登录表单

如果您按照上述步骤创建了BitShares帐户，则您将云钱包作为默认钱包。

在登录表单上，您可以看到从哪个钱包登录表单。 （即登录：帐户名称（云钱包））  
  
![](http://how.bitshares.works/en/master/_images/login-cloud-wallet.png)

如果您成功登录，则会找到**未锁定密钥**。  
  
![](http://how.bitshares.works/en/master/_images/topmenu-unlock2.png)

***

## 2.5.  高级：创建一个帐户

在本节中，您将创建一个**本地钱包**。

如果您有云钱包，则可以从任何浏览器访问您的钱包。 但是，在本地电子钱包中，您只能通过**用于注册和创建帐户的同一台计算机和网络浏览器**访问您的资金。

本地钱包要求您创建**备份文件**以管理您的帐户和资金。 备份文件可用于移动  
  
![](http://how.bitshares.works/en/master/_images/local-login1.png)

### 2.5.1. 步骤

* 1.点击[**高级表格**]
* 2.输入[**帐户名称**]。 您可以创建唯一的BitShares帐户名称。
* 3.设置密码。 创建自己的强密码。
* 4.输入或粘贴密码进行确认。
* 5.点击[**创建帐户**]

> **如果这是第一个账户，那么水龙头将为您支付注册费！**  
  
![](http://how.bitshares.works/en/master/_images/local-login2a.png)

> **您的网络浏览器是您的钱包：**请阅读以下信息。  
  
![](http://how.bitshares.works/en/master/_images/local-login2b.png)

* 点击[**现在创建备份**]  
  
![](http://how.bitshares.works/en/master/_images/local-login3.png)

**创建帐户备份并保存在安全的地方非常重要**。

* 单击[**DOWNLOAD**]以保存备份（.bin）文件。  
  
![](http://how.bitshares.works/en/master/_images/local-login4.png)

**祝贺你，你准备好了！**

***

## 2.6. BitShares钱包功能

### 2.6.1. 快速查看钱包选项  
  
![](http://how.bitshares.works/en/master/_images/functions1.png)  
  
<table>
        <tr>
            <td></td>
            <td>条目名称</td>
            <td>解释</td>
        </tr>
        <tr>
            <td>1</td>
            <td>仪表板</td>
            <td>钱包组合，未结订单，保证金头寸和活动信息</td>
        </tr>
        <tr>
            <td>2</td>
            <td>交易所</td>
            <td>BitShares交易所，交易信息</td>
        </tr>
        <tr>
            <td>3</td>
            <td>浏览器</td>
            <td>BitShares Live区块链，资产，账户，见证人成员，委员会成员，市场和费率表</td>
        </tr>
       <tr>
            <td>4</td>
            <td>发送</td>
            <td>打开发送表单。 您可以将资金发送给其他BitShares帐户持有人</td>
        </tr>
       <tr>
            <td>5</th>
            <td>BitShares帐户名称</td>
            <td>数据在仪表板页面上显示的帐户名称</td>
        </tr>
       <tr>
            <td>6</th>
            <td>密钥图标</td>
            <td>点击，打开登录表单。 锁定/解锁密钥图标显示您当前是否已登录帐户</td>
        </tr>
       <tr>
            <td>7</th>
            <td>侧面菜单图标</td>
            <td>侧面菜单图标在下拉列表中打开钱包其他菜单</td>
        </tr>
       <tr>
            <td>8</td>
            <td>资产总额</td>
            <td>目前显示在仪表板中的总资产</td>
        </tr>
       <tr>
            <td>9</td>
            <td>BitShares钱包版本</td>
            <td>BitShares UI钱包的发行版</td>
        </tr>
       <tr>
            <td>10</td>
            <td>延时</td>
            <td>网络连接延迟</td>
        </tr>
       <tr>
            <td>11</td>
            <td>服务器节点名称</td>
            <td>要连接的服务器节点名称</td>
        </tr>
    </table>

#### 2.6.1.1. 仪表板  
  
![](http://how.bitshares.works/en/master/_images/dashboard3.png)

**仪表板选项卡**  

   <table>
        <tr>
            <td>标签名称</td>
            <td>解释</td>
        </tr>
        <tr>
            <td>投资组合</td>
            <td>您的资产清单。 如果您不需要观看，可以过滤资产并隐藏一些资产。</td>
        </tr>
        <tr>
            <td>未结订单</td>
            <td></td>
        </tr>
        <tr>
            <td>保证金头寸</td>
            <td></td>
        </tr>
       <tr>
            <td>活动</th>
            <td>显示您的所有交易。 （即，下面显示了一种可供选择的交易类型。）</td>
        </tr>
    </table>




**活动 - 过滤器**  
  
![](http://how.bitshares.works/en/master/_images/dashboard-activity2.png)

#### 2.6.1.2. 侧面菜单 - 下拉条目  
  
<table>
        <tr>
            <td>选项</td>
            <td></td>
        </tr>
        <tr>
            <td>登陆</td>
            <td>点击，打开登录表单。</td>
        </tr>
        <tr>
            <td rowspan="2">创建账户</td>
            <td>拥有终身会员资格（LTM）的用户可以并行注册多个帐户</td>
        </tr>
        <tr>
            <td>所有这些都存储在一个钱包里</td>
        </tr>
       <tr>
            <td>发送（传统）</td>
            <td>转账详情（原始页面）。 顶部菜单上的发送是新表单。</td>
        </tr>
       <tr>
            <td>充值</th>
            <td>从其他方存入资金（原始存款页面）</td>
        </tr>
       <tr>
            <td>充值（beta）</th>
            <td>选择您要存放的资产，并为您提供发送地址，网关，identicon和注释。</td>
        </tr>
       <tr>
            <td>提现</th>
            <td>（原始提款页面）</td>
        </tr>
       <tr>
            <td>提现（beta）</td>
            <td>搜索资产以提现</td>
        </tr>
       <tr>
            <td rowspan="18">设置</td>
            <td>您可以管理钱包外观和其他设置。</td>
        </tr>
       <tr>
            <td>设置 - 云钱包登录模式：</td>
        </tr>
       <tr>
            <td rowspan="6">总体<br/> 
            账号<br/>
            还原/导入<br/>
            节点<br/>
            水龙头<br/>
            重新设置 </td>
        </tr>
       <tr>
        </tr>       
        <tr>
        </tr>
        <tr>
        </tr>
        <tr>
        </tr>
        <tr>
        </tr>
        <tr>
        </tr>
        <tr>
            <td>设置 - 本地钱包登录模式：</td>
        </tr>        
        <tr>
            <td rowspan="8">总体<br/> 
            本地钱包<br/>
            帐号<br/>
            密码<br/>
            备份<br/>
            还原/导入<br/>
            访问<br/>
            水龙头</td>
        </tr> 
        <tr>
        </tr>
        <tr>
        </tr>
        <tr>
        </tr>
        <tr>       
        </tr>
        <tr>
        </tr>
        <tr>
        </tr>
        <tr>
        </tr>
        <tr>
            <td>新闻</td>
            <td>BitShares区块链基金会和其他新闻</td>
        </tr>       
         <tr>
            <td>帮助</td>
            <td>打开“帮助”页面</td>
        </tr>
        <tr>
            <td rowspan="4">投票
            </td>
        </tr>
        <tr>
            <td>您可以投票给j见证人，委员会或工人。 或者您可以设置代理以进行投票。</td>
        </tr>
        <tr>
            <td>投票很重要：在Bitshares以同样的方式对你在的社区很重要。 您的投票权重与您拥有的BTS数量直接相关。</td>
        </tr>
        <tr>
            <td>如果您没有积极参与社区活动，我们鼓励您选择代表您兴趣的代理人。</td>
        </tr>
        <tr>
            <td>资产</td>
            <td>发行资产</td>
        </tr>
        <tr>
            <td>签名消息</td>
            <td></td>
        </tr>
        <tr>
            <td>会员统计</td>
            <td>基本会员是默认会员。 您可以在此处升级到终身会员资格。</td>
        </tr>        
        <tr>
            <td rowspan="3">归属余额</td>
        </tr>
        <tr>
            <td>归属余额包含通过推荐计划或工人工资获得的任何费用，</td>
        </tr>
        <tr>
            <td>例如。 他们有一个很长的归属期，并在该归属期内不断解锁，直到所有余额都可用</td>
        </tr>
        <tr>
            <td>白名单</td>
            <td>您可以设置白名单和/或黑名单。 此外，您还可以查看“白名单”和“黑名单”。</td>
        </tr>
        <tr>
            <td rowspan="4">权限</td>
        </tr>
        <tr>
            <td>您可以查看/续订每个帐户的（Active，Owner和Memo）公钥和私钥信息。</td>
        </tr>
        <tr>
            <td>Active权限：它被认为是“在线”许可。 控制访问资金和一些帐户设置。</td>
        </tr>
        <tr>
            <td>Owner权限：此权限对帐户具有管理权限。此外，如果要更改云钱包t密码，请使用“云钱包”标签页。</td>
        </tr>
        <tr>
     <td colspan="2">（账户）</td>    
        </tr>
    </table>
