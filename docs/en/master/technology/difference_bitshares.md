 **原文链接**：[http://how.bitshares.works/en/master/technology/difference_bitshares.html](http://how.bitshares.works/en/master/technology/difference_bitshares.html)
 
 **译者**：[https://github.com/b-t-s-1](b-t-s-1)
 
 **校对者**： 
  
***

# BitShares有什么不同

在这里，从交换的角度来看，我们简要概述了BitShares 2.0与基于satoshi的区块链（如比特币，莱特币等）的不同之处。

目录

* 代币
* 注册身份
* 不需要地址
* 备注
* 确保资金安全
* 全节点和客户端
* 对象IDs

## 代币
与所有基于satoshi的客户端相比，BitShares 2.0提供了各种区块链代币。不仅有BTS（核心代币），还有许多其他的代币。因此，作为交易所，你需要通过其ID（1.3.0（BTS），1.3.1（USD），......）或符号来区分不同的资产。

## 注册身份

BitShares 2.0中的所有参与者都必须具有注册的唯一名称。这与邮件地址类似，用于处理接收。当交易时，你只需要告诉你的客户你的BitShares帐户名称，他们就可以向你发送资金。

## 不需要地址

在BitShares 2.0中，我们将权限与身份分开。因此，当交易时，你不需要再次处理地址。事实上，你实际上不可能使用地址，因为它们只定义了可以控制资金（或帐户名称）的权限。这应该会大大简化集成，因为你不需要存储数千个地址及其相应的私钥。

## 备注

为了区分客户，我们使用类似于BitShares 1的所谓的备注，它们是加密的。与BitShares 1.0相反，我们现在有一个单独的备忘录密钥，它只能解码你的备忘录而无法花费资金。因此，为了监控交易所的存款，你不再需要将私钥暴露给互联网是上连接的机器。相反，你只需解码备忘录并将资金留在原来的位置。


## 资金保护

资金可以通过_hierarchical cooperation accounts_来保护。在现实中，它们是（阈值）多重签名帐户，如果多个签名有效，资金才能从中支出。与大多数其他加密货币相反，你可以在区块链上提出交易，并且不需要其他通信方式来将你的批准添加到某些交易中。你可以在账户类型中找到更多详情

* account-membership
* securing-funds

## 全节点和客户端

我们从头开始重写核心组件，并将核心P2P和区块链组件与钱包分开。因此，你可以运行没有钱包的完整节点，并将你的钱包连接到任何公共（或非公共）全节点（执行witness_node）。可以安全地建立通信，但私钥永远不会离开钱包。

## 对象IDs

由于BitShares 2.0为用户提供了许多不同的功能，因此我们决定使用_object ids_来解决这些问题。


<table>
    
    <tr>
        <td colspan="2">对象 ID | 翻译为</td>    
    </tr>
   <tr>
        <td>1.3.1</td> 
        <td>USD资产</td> 
   </tr>
   <tr>
        <td>1.3.0</td> 
        <td>BTS资产</td> 
   </tr>
   <tr>
        <td>1.2.&lt;id></td> 
        <td>用户ID&lt;id></td> 
   </tr>
   <tr>
        <td>1.6.&lt;id></td> 
        <td>区块签名者&lt;id&gt;</td> 
   </tr>
   <tr>
        <td> 1.11.&lt;id></td> 
        <td>操作id&lt;id></td> 
   </tr>
</table>

