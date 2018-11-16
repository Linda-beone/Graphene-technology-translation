  **原文链接**：[http://dev.bitshares.works/en/master/bts_guide/tutorials/uia-create-gui.html#creating-new-uia-gui](http://dev.bitshares.works/en/master/bts_guide/tutorials/uia-create-gui.html#creating-new-uia-gui)
 
 **译者**：[https://github.com/b-t-s-1](b-t-s-1)
 
 **校对者**：   

***

# 创建新的UIA（GUI）

目录

* 创建资产功能
  - 主要设置
  - 描述
  - 权限（可选）
  - 标志（可选）
  - 资产创建费
* 查看现有资产

***

## 创建资产功能

转到**[设置]** - **[创建资产]**

1. 点击[创建资产]   
 
<center>![](http://dev.bitshares.works/en/master/_static/imgs/uia-ui-1.png)</center>  

### 主要设置    

![](http://dev.bitshares.works/en/master/_static/imgs/uia-ui-3-primary.png)

为了允许在本地资产中支付交易费用，需要使用核心汇率，客户可以从资产的费用池中隐含地将UIA交易到BTS。

这还要求费用池得到资助（例如由发行人）。由于BitShares中的所有价格都在内部表示为分数（即a/b），我们需要定义引用（UIA）和基数（BTS）之间的比率，即价格的分子和分母= a/b

| Symbol: | The Symbol defined here will be reserved in the system for your assets. One the asset is created, the symbol cannot be changed again. |
| Maximum Supply: | This is also a permanent setting and denotes the maximum amount of shares that can ever exist at the same time.       Please consult the Networks fees in the explorer! |
| Number of Decimal Points: |
|  | This is used to denoted the number of decimal places. A 0 will result in an asset that cannot be separated below integer amounts (e.g. 1, 2, ..) |
| SmartCoin: | – |
| Quote Asset Amount: |
|  | Core exchange rate |
| Base Asset Amount: |
|  | Core exchange rate |

 <table style="width: 750px;"><tbody>
    <tr>
        <td bgcolor="LightBlue">注意</td>
    </tr>
    <tr>
        <td bgcolor="MintCream">少于5个字符的符号是非常昂贵的。 请参阅资源管理器中的网络费用！</td>
    </tr>
</table>

### 描述    

![](http://dev.bitshares.works/en/master/_static/imgs/uia-ui-4-description.png)

| Description: | description can be used to let everyone know the purpose of the asset, or an internet address for further information. |
| Short Name: | Short Name is also a permanent setting and denotes the maximum amount. |
| Preferred Market Pairing: |
|  | – |
| Asset Name: | – |

### 权限（可选）    

![](http://dev.bitshares.works/en/master/_static/imgs/uia-ui-5-permissions.png)

即使大多数UIA的默认设置都没有问题，我们也可以选择退出一些可用的功能。 （默认情况下，或启用权限）。

 <table style="width: 750px;"><tbody>
    <tr>
        <td bgcolor="LightBlue">注意</td>
    </tr>
    <tr>
        <td bgcolor="MintCream">将权限设置为false后，无法重新激活权限！</td>
    </tr>
</table>

我们可以选择退出：

* 使能市场费用
* 要求持有人列入白名单
* 发行人可以将资产转回给自己
* 发行人必须批准所有交易
* 禁用机密交易

 <table style="width: 750px;"><tbody>
    <tr>
        <td bgcolor="LightBlue">注意</td>
    </tr>
    <tr>
        <td bgcolor="MintCream">将权限设置为false后，无法重新激活权限！</td>
    </tr>
</table>

设置这些权限并不意味着启用了这些功能。 为此，我们还需要启用相应的标志。 （见下文）

### 标志（可选）

这些标志用于实际启用特定功能，例如市场费用或机密转账。

如果我们设置了市场费用的权限，我们可以在这里启用市场费用并设置百分比和最大值。

我们这里还可以使用户的要求列入白名单，启用机密转账，并赋予发行人从客户账户中提取资产的权力。  
  
![](http://dev.bitshares.works/en/master/_static/imgs/uia-ui-6-flag.png)

### 资产创建费

资产创建费用取决于您的符号长度。 3字符符号是最短的并且相当昂贵，而具有5个或更多字符的符号明显更便宜。

50％的资产创建费用于预先填充资产费用池。 剩余的50％，其中20％进入网络，80％进入推荐计划。 这意味着，如果您是终身会员，您可以在归属期（目前为90天）后收回40％的资产创建费。

***

## 查看现有资产

转到**[浏览]** - **[资产]** - **[用户问题资产]**  
  
![](http://dev.bitshares.works/en/master/_static/imgs/uia-ui-7.png)




