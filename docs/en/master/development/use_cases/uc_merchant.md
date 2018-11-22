 **原文链接**：[http://dev.bitshares.works/en/master/development/use_cases/uc_merchant.html#](http://dev.bitshares.works/en/master/development/use_cases/uc_merchant.html#)
 
 **译者**：[https://github.com/b-t-s-1](b-t-s-1)
 
 **校对者**： 
  
***

# 商家

**商家**使用石墨烯的货币计价资产网络（例如比特股）。与传统的支付解决方案类似让他们的客户使用比特美元，比特欧元或任何其他*稳定*付款区块链资产。

##### 目录
* ##### 钱包登陆协议 
  * ##### 准备
    * ##### 步骤 1 - 商家登陆按钮
    * ##### 步骤 2 - 压缩你的JSON表示
    * ##### 步骤 3 - 转换为Base58
    * ##### 步骤 4: 钱包确认
    * ##### 步骤 4: 服务验证鉴权
* ##### 钱包商家协议 
  * ##### 隐私问题
    * ##### 步骤 1 - 通过JSON定义你的凭据
    * ##### 步骤 2 - 压缩你的JSON表示
    * ##### 步骤 3 - 转换为Base58
    * ##### 步骤 4 - 传递给钱包
    * ##### 步骤 5: 从钱包接收调用 
    * ##### 步骤 6: 付款完成
    * #####  Python 脚本示例
我们在此说明了作为商家安全运营所需的步骤。商家通过区块链从客户那里获取资金并交付产品。 因此，商家应监控区块链并收到入交易通知。得益于（加密）交易备忘录，可以很容易区分发送给每个交易商的不同的客户。

对于交易，我们建议你阅读`what-is-different`和`Often used API Calls`。


-----

协议/API

## 钱包登陆协议

登录协议背后的想法是允许另一方验证你是特定帐户的所有者。传统方式登录是将密码发送到服务器执行，但此方法是受制于网络钓鱼攻击。石墨烯采用加密挑战/响应验证用户是否控制特定帐户而不是密码。

在这个例子中，我们假设

>   - <https://merchant.org>  将要登陆的服务
>   - <https://wallet.org>  帮助用户登录的钱包提供商

商家为用户提供登录按钮（在应用程序页面上）。
### 准备

#### 步骤 1 - 商家登陆按钮

登陆按钮链接到
[https://wallet.org/login\#${args}](https://wallet.org/login#$%7Bargs%7D)

${args} : A JSON object containing following information and serialized
as described below (step 2 & step 3)

    {
       "onetimekey" : "${SERVER_PUBLIC_KEY}",
       "account"  : "${OPT_ACCOUNT_NAME}",
       "callback" : "https://merchant.org/login_callback"
    }

商家服务器需要保存用户会话中的`${SERVER_PRIVATE_KEY}`和与之相关联的`${SERVER_PUBLIC_KEY}`以便验证登录。

#### 步骤 2 - 压缩你的JSON表示
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

使用[LZMA-JS]库进行压缩将JSON转换为二进制数组。 这将是最紧凑的形式数据。 运行压缩后，示例JSON从579字节减少到281字节。

#### 步骤 3 - 转换为Base58

使用[bs58]库编码base58中的压缩数据。 Base58可以友好，高效的使用URL。转换为base58后，字符串385个字符可以很容易地在URL中传递，并且可以轻松支持更大的凭据。
  
#### 步骤 4 - 钱包确认

当用户加载 “https://wallet.org/login#${args}” 时，他们将通过选择他们想要登陆的账户来确认登陆请求。如果在'${args}`中指定了“account”，那么它将默认为该帐户。

在确定帐户后，必须有足够的密钥来授权帐户以下列的方式参与登录过程。

>   - 钱包生成一个`WALLET_ONETIMEKEY`并派生一个`共享秘密'

使用`https：// merchant.org`提供的`SERVER_PUBLIC_KEY``${ARGS}`。此共享密钥是一个是“随机”的512位数据，只有钱包才知道。然后钱包收集有足够字符的共享密钥来授权账户。在简单情况下这将是一个单一的签名，但在更复杂的情况下验证需要多个因素是多因素。

收集完所有签名后，钱包会将用户重定向到`https://merchant.org/login_callback?a=${result}`其中`result`是编码的JSON对象，包含以下信息：

    {
       "account": "Graphene Account Name",
       "server_key": "${SERVER_PUBLIC_KEY}",
       "account_key": "${WALLET_ONETIMEKEY}",
       "signatures" : [ "SIG1", "SIG2", .. ]
    }

#### 步骤 5 - 服务验证鉴权

从钱包收到`result`后，<https://merchant.org>将会在用户的会话数据中查找`{SERVER_PRIVATE_KEY}`然后将它与`{WALLET_ONETIMEKEY}`组合起来生成钱包使用的*共享密钥*。

一旦恢复了此共享密钥，就可以使用它来恢复与提供的签名对应的公钥。

最后一步是验证提供签名的公钥足以授权给定帐户当前的石墨烯区块链的状态。这可以通过见证人API调用来实现：

    verify_account_authority( account_name_or_id, [public_keys...] )

如果提供的密钥有足够的权限来授权帐户，`verify_account_authority`调用将返回`true`否则它将返回`false`

-----

## 钱包商家协议

**如何维护钱包提供商的用户和商家隐私，钱包提供商永远不应该直接访问凭据数据。**
 
在这个例子中，我们假设

>   - <https://merchant.org> 是托管服务器的服务
>   - <https://wallet.org> 是托管服务器的钱包提供者

# 隐私问题

该协议的目标是保证用户和商家的隐私，钱包提供者永远不应该直接访问凭据数据。


将数据从“https:// merchant.org”安全传递到javascript钱包https://wallet.org`，数据必须在`＃`之后传递。假设钱包提供商没有提供页面旨在破坏你的隐私，只有你的web浏览器才会访问凭据数据。 

#### 步骤 1: 通过 JSON 定义你的凭据 

此凭据提供钱包显示所需的所有数据凭据给用户。

    {
       "to" : "merchant_account_name",
       "to_label" : "Merchant Name",
       "memo" : "Invoice #1234",
       "currency": "BTS",
       "line_items" : [
            { "label" : "Something to Buy", "quantity": 1, "price" : "1000.00 SYMBOL" },
            { "label" : "10 things to Buy", "quantity": 10, "price" : "1000.00 SYMBOL" },
            { "label" : "User Specified Price", "quantity": 1, "price" : "CUSTOM SYMBOL" },
            { "label" : "User Asset and Price", "quantity": 1, "price" : "CUSTOM" }
        ],
        "note" : "Something the merchant wants to say to the user",
        "callback" : "https://merchant.org/complete"
    }

这个数据本身就是579个字符，在URL编码后是916个字符，自身具有2000个字符的限制，此方法不能压缩为我们想要的那样。

#### 步骤 2: 压缩你的JSON 表示

使用[LZMA-JS]（https://github.com/nmrugg/LZMA-JS/）库进行压缩将JSON转换为二进制数组。 这将是最紧凑的数据形式。

（例如）运行压缩后，示例JSON压缩从579字节降低到281字节。减少到281 579字节的字节数。

#### 步骤 3: 转换为Base58

使用[bs58]库编码base58中的压缩数据。 Base58可以友好，高效的使用URL。

（例如）转换为base58后，字符串385个字符可以很容易地在URL中传递，并且可以轻松支持更大的凭据。

#### 步骤 4: 传递给钱包

一旦知道了Base58数据，就可以将其传递给钱包以下网址:

    https://wallet.org/#/invoice/BASE58BLOB

#### 步骤 5: 从钱包接收调用

钱包签署交易后，已包含在“块12345”的交易作为“交易4”广播并获得来自<https://wallet.org>的确认，钱包将指引用户`https：//merchant.org/complete块= 12345＆TRX=4`

然后，商家从`https：//wallet.org/api？block = 12345＆trx =4`请求该交易，包含在区块链中的该交易做出响应。商家会从交易中解密备忘录并使用备忘录内容进行付款凭据确认。

#### 步骤 6: 付款完成

此时，用户已成功进行了付款和商家已经验证已收到付款，不需要维护一个全节点。

#### Python 脚本示例

``` sourceCode python
import json
import lzma
from graphenebase.base58 import base58encode, base58decode
from binascii import hexlify, unhexlify

invoice = {
    "to": "bitshareseurope",
    "to_label": "BitShares Europre",
    "currency": "EUR",
    "memo": "Invoice #1234",
    "line_items": [
        {"label": "Something to Buy", "quantity": 1, "price": "10.00"},
        {"label": "10 things to Buy", "quantity": 10, "price": "1.00"}
    ],
    "note": "Payment for reading awesome documentation",
    "callback": "https://bitshares.eu/complete"
}

compressed = lzma.compress(bytes(json.dumps(invoice), 'utf-8'), format=lzma.FORMAT_ALONE)
b58 = base58encode(hexlify(compressed).decode('utf-8'))
url = "https://bitshares.openledger.info/#/invoice/%s" % b58

print(url)



 