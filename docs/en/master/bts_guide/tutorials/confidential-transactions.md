 **原文链接**：[http://dev.bitshares.works/en/master/bts_guide/tutorials/confidential-transactions.html#confidential-transactions-guide](http://dev.bitshares.works/en/master/bts_guide/tutorials/confidential-transactions.html#confidential-transactions-guide)
 
 **译者**：[https://github.com/b-t-s-1](b-t-s-1)
 
 **校对者**： 
  
***

# 机密转移

**如何使用CLI钱包执行机密交易比特股**

本教程介绍如何使用CLI钱包在比特股中执行机密交易。机密交易是隐藏发送的金额和参与交易的各方。机密交易也称为盲交易。隐私是重要的，任何帐户都不应该被使用两次，而且加上尽全力来备份和保护钱包并记录使用的密钥。单纯使用区块链分析，任何第三方都无法识别到资金是如何转移的。

重要的是要意识到目前Steath的功能实现需要仔细注意细节，如果不按照这其中的步骤以及不考虑潜在损失，将会带来丢失账户余额的更高风险。但是，按照本指南，您可以放心将你的余额存储在区块链中,除了你对其他人完全隐藏。记住这些重要的警告，让我们开始。

本教程我们将说明完成每个步骤所需的CLI命令。你必须熟悉witness\_node和cli\_wallet软件以遵循本指南。有关更多信息如何构建和使用软件请参阅在github上或在比特股维基上的自述文件。 我将使用简单的密码和帐户名简化演示。你应该更改为更安全的值以获得更高的安全性。在以下示例中，另请注意CLI钱包回显输入的命令，该命令未在引用的登陆会话中显示。

#### Table of Contents

##### 步骤 1: 创建一个盲账户 
##### 步骤 2: 将余额从标准公共帐户转移到盲帐户 
##### 步骤 3: 接收从另一个帐户转移的资产余额           
* ##### 列出盲账及其余额 
##### 步骤 4: 在机密帐户之间转移资产
##### 步骤 5: 将资产从机密帐户转回公共帐户

----- 

# 步骤 1: 创建一个盲账户

盲帐户未在区块链上注册，如命名的比特股的帐户。相反，盲账户只不过是一个标记的公钥。分配给密钥的标签仅为你的*钱包*所知。因此，在完成余额转移后，为盲账户创建一个新钱包并备份是至关重要的。

创建盲账户的第一步是创建一个新的钱包和为它设置一个很难破解的高质量密码。然后，使用此钱包，我们将创建一个带标签的帐户并用“brainkey”保护它。“brainkey”实际上是你账户使用的私钥。所有比特股加密系统都是基于公钥/私钥对，公钥可以分享给其他人，私钥只有你知道。

对于机密帐户，“brainkey”仅存储在钱包中，所以，如果钱包文件丢失或被破坏，你没有记录
在纸上或其他地方，将无法恢复你的机密帐户余额。即使你在钱包之外的地方记录了你的“brainkey”，我不相信任何现有的恢复方法可以导入公钥/brainkey密钥对到你的钱包访问你的余额。至少在将来的某个时候是可能的，而不像你丢了钱包而且没有记录“brainkey”这种不可能情况。

    >>> create_blind_account alice "alice-brain-key which is a series of words that are usually very long"                                                                   
    1483572ms th_a       wallet.cpp:743                save_wallet_file     ] saving wallet to file /home/admin/BitShares2/blindAliceWallet
    BTS5Qmr9H9SM39EHmVgXtsVjUGn2xBUtqbF6MdQ6RpnxUWNak7mse
    true

create _blind_account命令的结果是打印alice账户的公钥以及命令是否成功（真实）。请注意，当你启动cli_wallet时，如果加-w参数，CLI接口在命令行上将更新你指定的钱包文件。CLI钱包没有退出或离开命令，所以我们用control-C终止会话（如图所示）^C），它将我们返回到操作系统shell。

# 步骤 2: 将余额从标准公共帐户转移到盲帐户

现在我们已经创建了一个新的名为alice的未注册的盲账户，我们可以从任何来源，公共或非公共的转进资产，我们将描述从一个盲账户瞬间转移余额到其他账户的步骤，从公众视角，这对于完全隐藏余额是至关重要的，但我们将说明从一个公开的注册账户转移到我们创建的盲账户过程。

首先，我们需要一个拥有资产的帐户钱包。可以是任何有资产的账户，所以我们将用peter的公共账户将资金转移到alice帐户。我们还需要拥有为alice帐户打印的公钥。由于alice帐户未注册，我们需要一种方法在我们进行交易之前作为参考。所以在这个CLI会话中我们会还告诉你如何创建一个标签，然后我们将使用标签将资产转移到alice盲帐户中。

    >>> list_account_balances "peters-public-registered-account"                                                        
    5000 BTS
    
    >>> set_key_label "BTS5Qmr9H9SM39EHmVgXtsVjUGn2xBUtqbF6MdQ6RpnxUWNak7mse" "Alice-is-Blind"
    true
    
    >>> transfer_to_blind "peters-public-registered-account" BTS [[Alice-is-Blind,100]] true
    3369305ms th_a       wallet.cpp:3794               transfer_to_blind    ] to_amounts: [["Alice-is-Blind","100"]]
    peters-public-registered-account sent 100 BTS to 1 blinded balance fee: 40 BTS
    100 BTS to  Alice-is-Blind
              receipt: 2B2vTjJ19hgqzGp8qdc8MEWmsgEUGECNJcoQTYNQqMU8bRofmbQYemXs56FoUc4Z5PdVM65nsySZgwJMq9Z SkpWQFhEqLGuZi1N3jQm8yBwaLD2DQzwY5AEW1rSK9HWJbfqNLtx8U4kc3o9xKtJoED2SgHW6jDQ7igBTcVhuUiKSwFu3DFa6LTeS5 Wm5khjgy1LrR5uhmp
    
    >>> list_account_balances "peters-public-registered-account"                                                       
    list_account_balances verbaltech2
    4860 BTS

以上两个步骤将比特股资产从一个名为"peters-public-registered-account"的公共，注册账户转移到一个名为alice使用"Alice-is-Blind"标签的唯一的未注册账户。重点强调，请注意这些标签不会从一个CLI会话延续到下一个，所以每次你从一个源账户像如这里的"peters-public-registered-account"转移资产到一个盲账户，你需要设置一个盲账户的标签。

**添加联系人**

目前没有工具通过轻钱包或者OpenLedger网页钱包将资产转移到一个盲账户。它们只支持WIF（钱包导入格式），因此不会接受你的盲帐户的“brainkey”作为有效私钥。通过定义联系人，将来你可能会避免当你从一个公共账户转移到一个盲账户的时候每次都设置标签。但是，请记住每一个你在公共帐户和机密帐户之间建立的关联可能会更容易地追踪你的步骤，所以，为了方便起见，请三思而行。他们可能会绕过你采取的隐藏你余额的措施。如果你直接在公共账户和保密账户之间转移资产，并将其留在机密帐户，情况也是如此。总的来说，完全隐藏你的余额所在，你需要转移到至少两个不同的保密账户。我们会稍后将详细介绍这一点。下一步，我们看一下如何接收发送的资产到alice的盲账户。

# 步骤 3: 接收从另一个帐户转移的资产余额

将资产从一个帐户转移到一个机密帐户涉及到至少两步，第一步发送资产，第二步接收资产到机密账户。我们介绍了这个过程需要在步骤2中发送资产，现在让我们看看它需要什么完成转移并验证我们的余额是否正确。

    >>> receive_blind_transfer "2B2vTjJ19hgqzGp8qdc8MEWmsgEUGECNJcoQTYNQqMU8bRofmbQYemXs56FoUc4Z5PdVM65nsySZgwJMq9ZSkpWQFhEqLGuZi1N3jQm8yBwaLD2DQzwY5AEW1rSK9HWJbfqNLtx8U4kc3o9xKtJoED2SgHW6jDQ7igBTcVhuUiKSwFu3DFa6LTeS5Wm5khjgy1LrR5uhmp "peter" "from Peter"
    100 BTS  peter  =>  alice   "from Peter"

使用从在步骤2中的命令返回的余额收据数值，我们可以接收（即导入）到alice盲账户中。请注意，必须标记余额的来源，这是长余额收据密钥的参数。它意味着代表资产所在的来源账户转移，但它不一定。三个参数中的最后一个是备注文本字段，它是任意文本值。注意三个参数是必需的。在下一节中，我们将描述如何列出机密帐户及其余额，以便我们验证我们的转移是正确和完整的。

## 列出盲账及其余额

对于你已创建机密帐户的任何钱包，你可以使用“get_my_blind_accounts”CLI命令列出存在的帐户，并使用从中返回的帐户来获取他们的余额：

    >>> get_my_blind_accounts                                                                  
    [[
    "alice",
    "BTS5Qmr9H9SM39EHmVgXtsVjUGn2xBUtqbF6MdQ6RpnxUWNak7mse"
    ]]
    
    >>> get_blind_balances "alice"                                                                
    100 BTS

回顾一下，你已学会如何：

> 1.  创建一个新的CLI钱包并为其添加一个盲帐户
> 2.  创建一个盲账户标签
> 3.  从一个公共账户到一个盲账户发送资产
> 4.  从一个其他账户到一个盲账户接收或者导入资产
> 5.  列出cli钱包包含的盲账户
> 6.  列出盲账户的资产余额

这些是基本步骤是针对一个简单的单一资产从一个公共账户到一个盲账户的单向转移，接下来我们将研究如何通过使用第二个盲帐户来掩盖我们的操作以隐藏我们的余额，最后我们将看到如何从一个盲帐户转回公共帐户，以结束我们利用CLI钱包使用机密帐户保护你的资产的研究。

第一部分是一个如何使用比特股CLI钱包将资产从一个注册的公共账户转移到一个机密（即盲）账户的基本演示。它解释了所涉及的步骤和目前使用机密特性的限制。第2部分我们将展示如何将资产从一个机密帐户转移到另外一个，并通过描述如何将资产从一个机密账户转回一个注册的公共账户得出我们的观察结论。

第一部分提到真正隐藏帐户余额和消除对资产如何到达的公共跟踪，在公共路径上从公共来源到最终机密地址应使用至少两个机密账户。这是因为从公共帐户转移的目标地址是可见的。公众可能无法查询机密资产账户，但将资产置于如此明显的隐藏的地方并不明智。

但是，如果这些资产转移到另一个机密帐户他们的下落无法进行追踪单纯区块链通过分析。因为机密地址之间的转移不能被追踪，甚至认为资产仍然存在于第一个机密地址（公共注册账号之外的目的地）。额外的机密层对于机密转账来说，可以提供对于更高的安全性使得资产不会被发现，对于那些具有更高要求的人来说。不言而喻，对于大额转移，将资产分配到多个机密账户是重要的安全策略。最后，请注意保密帐户中的资产不计入投票。因此你应该考虑如何使用机密帐户会影响你的参与以及影响比特股生态系统的政策和建议。

# 步骤 4: 在机密帐户之间转移资产

让我们从创建第二个钱包和机密帐户开始，用作我们假设的最终目的地，我们将此帐户称为bobby。我们已经在第1部分中展示了如何执行此操作，但你可能希望在继续之前回顾一下这些步骤。

    >>> create_blind_account bobby "bobby-brain-key which is a series of words that are usually very long"                                                                   
    1434971ms th_a       wallet.cpp:743                save_wallet_file     ] saving wallet to file /home/admin/BitShares2/blindBobWallet
    BTS6V829H9SM39EHmVgXtsVjUGn2xBUtqbF6MdQ6RpnxUWNakaV26
    true

我们需要使用alice帐户重新启动CLI钱包，钱包有100个BTS余额。我们将创建一个标Bob的机密账户（bobby）的标签，并将一些比特股资产从alice转移到bobby。注意该过程与以前一样，我们需要为bobby（目的地）设置标签帐户来做转账。


    >>> set_key_label "BTS6V829H9SM39EHmVgXtsVjUGn2xBUtqbF6MdQ6RpnxUWNakaV26" "bobby"
    true
    
    >>> blind_transfer alice bobby 80 BTS true
    318318ms th_a       wallet.cpp:743                save_wallet_file     ] saving wallet to file /home/admin/BitShares2/blindAliceWallet
    blind_transfer_operation temp-account fee: 15 BTS
    5 BTS to  alice
              receipt: iiMe3q3X4DqW1AqCXfkYEcuRsRATxMwSvJpaUuCbMTcxRUUGeBPPwYU1SRRs4tEQGPNmP$Js4jTJkDGEHzUm33o6h14wa1XNsmedLJCKnwmyGeqFB4vPRk9ZxnaizbMNu8bHr62xQaTc73ALxAZEPRdkNLyqMk$oDEFja3vCPgcyDYCQmkVnNiAQaKeMG83KrW11QZMHQZfzZ8ofTSTEy8qruLAa27vrjAM6q2ckbD8ZTNMWnkSWniq$4fay3Tbcd2zsy9EgxuxN
    
    80 BTS to  bobby
              receipt: iiMe3q3X4DqW1AqCXfkYEcuRsRATxMwSvJpaUuCbMTcxRUUsn1qUtjfqLYUaNycrpKHfmUG1PR9mxd2nVKB15RYSryyjSn54ADzNBaFzxTY1s699iJWWHw2itiagfcKtvwizhN9Ru8nfnzgx8c5vi7RCLNB2PgrcTxSjYUJW1sfMicFyLRgYrCHFyNd1VhBeWpsLMwagcTGkUTf4rNDyXTrRqqLf2Nhy6P3ohk3J5WbshYyHxuLJGY2E7B5nPpFuf4Bnf9paD6jW

实际打印输出比上面显示的多，但是重要结果已经显示出来。从这里你可以看到我们首先创建了一个bobby账户的标签，以及blind_transfer指令，费用是15BTS，其中发送了80个BTS（100 在第一部分中100BTS被转移到alice帐户）到bobby的机密账户并提供了两个余额收据：第一个为5个BTS返回到alice帐户作为找零（剩余的资金），第二个是发送给bobby账户的80BTS的收据，这个收据是为了包含在blindBobWallet的bobby账号接收到转移的资金。

正如你所看到的，在CLI钱包中使用机密转账是相当繁琐的“手动”过程。但请注意，你不需要这样做“receive_blind_transfer”将5BTS找零导入到alice帐户，至少不用管。另外需要注意的是在外界看来，可以看到alice发送的少于100BTS金额到两个新的输出，其中一个是返回更改，这使得跟踪正在发生的事情变得更加困难，因为每个输出的数量是不可见的。

# 步骤 5: 将资产从机密帐户转回公共帐户

在我们往返过程的最后一步中，我们将从bobby机密账户转回一些BTS到原来一开始命名的peter的公共账户。完成这一步没有什么新的要求，但有几点提一下。首先，请记住源地址转进公共帐户可能是可见的，所以在公共帐户和你的机密资产存放地，考虑使用一个或多个中间机密帐户添加隔离层。其次，虽然你要发送到注册的可能认为不需要标签访问的公共帐户，但事实并非如此。

必须将标签分配给公共目标地址才能返回盲（机密）账户的资产。帐户公钥值随时可以通过帐户的权限页面使用。在“活动权限”头部使用账户/密钥显示如下：

    >>> receive_blind_transfer "iiMe3q3X4DqW1AqCXfkYEcuRsRATxMwSvJpaUuCbMTcxRUUsn1qUtjfqLYUaNycrpKHfmUG1PR9mxd2nVKB15RYSryyjSn54ADzNBaFzxTY1s699iJWWHw2itiagfcKtvwizhN9Ru8nfnzgx8c5vi7RCLNB2PgrcTxSjYUJW1sfMicFyLRgYrCHFyNd1VhBeWpsLMwagcTGkUTf4rNDyXTrRqqLf2Nhy6P3ohk3J5WbshYyHxuLJGY2E7B5nPpFuf4Bnf9paD6jW "alice" "from Alice"
    100 BTS  alice  =>  bobby   "from Alice"
    
    set_key_label BTS9oxUqKFD8gfGoXb6AwDBEoBt8W47g4Mtz8SW8inUeHemR9nOi9 "peter"
    true
    
    >>> blind_transfer bobby peter 50 BTS true                                                      
    2263915ms th_a       wallet.cpp:743                save_wallet_file     ] saving wallet to file /home/admin/BitShares2/blindBobWallet
    blind_transfer_operation temp-account fee: 15 BTS
    15 BTS to  bobby
              receipt: boqRZqyKaZW6bExrystPwFdXvzUBJSjGeaqy482NxBJ6S9Un4zima1mzysTrUipBiBpm4CrLTvCJZfqDaAaqEpmxWAWAKhi2GmnuT7nLU6n18GWjLxUnpskyywA8qCBw9VTAvaxtrNpFRtxx16NzJiZEYk6zfndvLJ2txvjq9cTT16QRXdqPQ75GJxuTAWKNdvzYm3NyK3w3K3462AbutEF9TyNGEfHidvAff49Q3yBATFs1g5NkGAMsmx4ffgwnFeMPBqi58cSZ
    
    50 BTS to  peter
              receipt: boqRZqyKaZW6bExrystPwFdXvzUBJSjGeaqy482NxBJ6S9VPCqArXCypszWZnpCeG7jfS3oUnbtmn5bmmVH5HCXJg9QxCmn4pocbJ8ipRHfzgeq1mLMewQNn6HGrkb5WbosSntj3o4LcSEMpw2etsR2GjnBxcdxN879rBwxm6inhbpsoYn1nGwS4H
o3SqoCF43MRDK3ouYrFBcAK2TTPXfnnvAU3r1UvhNHpxuNaS1cexbd88Nn6BTxSifKdJ8ysFft98e88Cbek

> \>\>\> get\_blind\_balances bab1 get\_blind\_balances bab1 15 BTS

此CLI会话的解释基本上与原来步骤4的相同.虽然帐户信息与命令使用不同，但是在转移过程中的他们的作用是相同的。

最后一个示例演示了如何在多个机密账户之间拆分余额。这非常有用，因为它不仅可以节交易费用，同时还隐藏了最终的金额。这一点见下面语法说明：

    >>> list_account_balances "peters-public-registered-account"                                                        
    4860 BTS
    
    >>> set_key_label "BTS5Qmr9H9SM39EHmVgXtsVjUGn2xBUtqbF6MdQ6RpnxUWNak7mse" "Alice-is-Blind"
    true
    
    >>> set_key_label "BTS6V829H9SM39EHmVgXtsVjUGn2xBUtqbF6MdQ6RpnxUWNakaV26" "bobby"
    true
    
    >>> transfer_to_blind peter BTS [[alice,800],[alice,2000],[bobby,2000] true
    peter sent 4800 BTS to 3 blinded balances fee: 40 BTS
    800 BTS to  alice
      receipt: 2Dif6AK9AqYGDLDLYcpcwBmzA36dZRmuXXJR8tTQsXg32nBGs6AetDT2E4u4GSVbMKEiTi54sqYu1Bc23cPvzSAyPGEJTLkVpihaot4e1FUDnNPz41uFfu2G6rug1hcRf2Qp5kkRm4ucsAi4Fzb2M3MSfw4r56ucztRisk9JJjLdqFjUPuiAiTdM99JdfKZy8WTkKF2npd
    
    2000 BTS to  alice
      receipt: 28HrgG1nzkGEDNnL1eZmNvN9JmTVQp7X88nf7rfayjM7sACY8yA7FjV1cW5QXHi1sqv1ywCqfnGiNBqDQWMwpcGB1KdRwDcJPaTMZ5gZpw7Vw4BhdnVeZHY88GV5n8j3uGmZuGBEq18zgHDCFiLJ6WAYvs5PiFvjaNjwQmvBXaC6CqAJWJKXeKCCgmoVJ3CQCw2ErocfVH
    
    2000 BTS to  bobby
      receipt: 82NxBJ6S9Un4zima1mzyboqRZqyKaZW6bExrystPwFdXvzUBJSjGeaqy4sTrUipBiBpm4CrLTvCJZfqDaAaqEpmxWAWAKhi2GmnuT7nLU6n18GWjLxUnpskyywA8qCBw9VTAvaxtrk6zfndvLJ2txvjq9cTT16QRXdqPQ75GJNpFRtxx16NzJiZEY49Q3yBATFs1g5NkGAMsmx4ffgwnFeMPBqi58cSZxuTAWKNdvzYm3NyK3w3K3462AbutEF9TyNGEfHidvAff
    
    20 BTS to  peter
      receipt: cwBmzA36dZRmuXXJR8tTQs2Dif6AK9AqYGetDT2E4u4GSVbMKEiTi54saot4e1FUDnNPz41uFDLDLYcpXg32nBGs6Afu2G6rpguiAiTdM99JdfKZy8WTkKF2npd1hcRf2Qp5kkRm4ucsAi4Fzb2M3MSfw4r56ucztRisk9JJjLdqFjUPqYu1Bc23cPvzSAyPGEJTLkVpih

在这种情况下，大家唯一看到的是“peter”账号发送了4800BTS到四个不同的地方。注意，虽然800和2000 BTS被发送到alice的机密账户，但是他们没有显示在区块链上。

**结论**：外界不知道每个输出有多少，只知道加加起来总共4800 BTS。