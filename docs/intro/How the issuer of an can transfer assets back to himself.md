原文链接：https://steemit.com/bitshares/@xeroc/how-the-issuer-of-an-iouuia-can-transfer-assets-back-to-himself

译者：bt666

# IOU/UIA的发行人如何将资产转回自己名下 #

与中心化交易所类似，管理员可以完全控制您的余额，比特股上的用户发行资产的发行方对所发行的资产也有控制权。 通过这种方式，他们可以在技术上撤销意外的转账操作（可能会失去信誉）。 本文介绍如何从技术角度来做到这一点。

**特殊许可**

资产的发行方对其资产拥有特殊权限。 更确切地说：发行方可以获得特殊权限。 这些特殊权限是

- 要求将资产持有者列入白名单（适用于高级用户）
- 将股权转给发行人或其他地方
- 要求发行人批准所有转账（这需要使用提案，适用于高级用户）
- 禁用私密交易，以便所有余额和金额始终是公开的

**许可与标志**

如上所述，发行人可以获得特殊权限。 这可以通过设置一个所谓的**标志**来启用特殊权限来实现。 只要特殊标志未启用，区块链就不允许使用这些额外的权限。 这公开广播发行人将使用这个标志。

由于发行人可以随时设置标志，人们怎么能相信发行人不会滥用权力，或者在攻击后担心攻击者获得了对资产的操作权限？ 好吧，这可以通过权限来完成，发行人可以使用权限来反向选择一个或多个额外权限！ 一旦发行人取消了从任何账户转账的许可，他将再也无法获得此特权。

**撤销转账**

如上所述（我想在此强调这一点），发行人只能撤销他创建的资产的转账！

**启用标志**

首先，我们需要在资产设置中启用特殊权限，发行方可以将资产转回自己（技术上称为覆盖转账）。 确保启用该标志而不是禁用权限（除非您真的知道自己在做什么，否则最好不要触摸权限！）
启用该标志后，请确保正确更新您的资产，以便区块链了解更改。

**构建撤销转账操作**

要使用覆盖转账功能，有一个称为oeverride-transfer-operation的特殊操作（多么明显）。 它采用以下形式：

    [
      38,{
    "issuer": "1.2.0",
    "from": "1.2.0",
    "to": "1.2.0",
    "amount": {
      "amount": 0,
      "asset_id": "1.3.0"
    },
    [...]
      }
    ]

它看起来就像常规的转账操作，不同之处在于操作ID为39，另外附加了发行方字段。 如果上述参数设置正确，并且交易由发行人签名，则交易有效并将被添加到区块链中（在检查双倍开销，权限等之后）。

交易历史记录将显示一个新条目，类似于：

![](https://steemitimages.com/p/HuuaCwcKuiEnEHW9tPu6pRxQm9j8Qmw4mASETwisxcVHcG4sYi9zgeCQVBFEZPb3RwU?format=match&mode=fit)

**Python脚本**

为了简化此功能的使用，我编写了一个[简单的脚本](https://github.com/xeroc/python-graphenelib/blob/develop/examples/transfer_back_to_issuer.py)，只需要进行一些设置：

    issuer = "xeroc"
    from_account = "maker"
    to_account = "xeroc"
    asset = "LIVE"
    amount = 100.0
    wifs = ["<active-wif-key-of-issuer>"]
    witness_url = "ws://testnet.bitshares.eu/ws"

如果你不知道如何处理witness_url，你不应该阅读这篇文章

在使用此脚本之前，请确保安装最新版本的python-grapehenlibs。 如果提示说缺少方法，请尝试从github **开发**分支安装！