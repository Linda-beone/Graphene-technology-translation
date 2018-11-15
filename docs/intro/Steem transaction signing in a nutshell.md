原文链接：https://steemit.com/steem/@xeroc/steem-transaction-signing-in-a-nutshell

译者：bt666

# Steem在nutshell中的交易签名 #

本文适用于尝试使用他们喜欢的语言为Steem（或比特股）区块链实现交易签名的开发人员。 它简要介绍了什么是交易，它们是如何构建的，最重要的是它们如何签名以便包含在一个区块中。

我还在写这篇文章是因为我花了将近2年的时间才完全实现了python-steem和python-graphene。 如果不是这两年，我们今天就无法使用piston了。

此外，我希望本教程可以让人们快速了解正在发生的事情，以便他们能更深入地了解。

**什么是交易签名**

通常，交易签名从用户想要做某事开始。 这种意图会影响他的帐户，因此需要通过有效（在我们的例子中为加密）签名来授权该帐户。 与纸质签名不同，这个数字签名需要与实际意图（交易）相关联，否则可以将其复制到其他交易并多次使用。 出于这个原因，每个交易都是独立签名的，签名仅对该特定的交易有效。

本教程将介绍如何在给定特定意图/交易的情况下派生签名。

**开始**

在我们的例子中，我们从一个简单的意图开始，那就是：

支持[@ xeroc](https://steemit.com/@xeroc) / piston的博客文章

**操作**

在Steem（以及其他基于石墨烯的区块链）中的意图称为操作。 在我们的例子中，操作具有以下形式：


    ['vote',
       {'author': 'xeroc',
    'permlink': 'piston',
    'voter': 'xeroc',
    'weight': 10000}]

我们可以清楚地知道

- 操作的类型（投票）
- 文章作者和permlink（xeroc，piston）
- 选民（也是xeroc，因为我为自己的文章投票）
- 票权（10000对应100％）

**交易**

在下一步中，我们将此（以及可能其他的）操作封装到交易中。 这个步骤的目的是连续执行附加的多个操作，附加所需签名、到期时间以及添加TaPOS参数（参见下文）。 在我们的例子中（一票操作），它采用以下形式：

    tx = {'ref_block_num': 36029,
      'ref_block_prefix': 1164960351,
      'expiration': '2016-08-08T12:24:17',
      'operations': [['vote',
      {'author': 'xeroc',
       'permlink': 'piston',
       'voter': 'xeroc',
       'weight': 10000}]],
      'extensions': [],
      'signatures': [],
      }

我们注意到，操作现在是交易的一部分（作为操作数组的一部分），现在有了用于签名和到期时间的字段。 到期时，如果交易还没打包进区块，则意味着该交易过期了。 通常到期时间是30秒之后。

让我们稍微讨论一下ref_block_ *参数：ref_block_num的最后两个字节表示区块号，通过引用区块号来指示过去的某个特定的区块。 另一方面，ref_block_prefix是从该特定引用的区块的块id获得的。 它是块id的一个无符号整数（4字节），但不是从第一个位置开始，而是具有4个字节的偏移量。 以下代码是获取当前头块并计算这两个参数的相应python代码：

    dynBCParams = noderpc.get_dynamic_global_properties()
    ref_block_num = dynBCParams["head_block_number"] & 0xFFFF
    ref_block_prefix = struct.unpack_from("<I", unhexlify(dynBCParams["head_block_id"]), 4)[0]

这两个参数的目的是防止在分叉时发生重放攻击。 一旦两个链发生了分叉，这两个参数可以用来识别两个不同的区块。 在一条链上使用另一条链上签名的交易会使的该签名无效。

**序列化**

在开始签名之前，我们首先需要定义实际签名的内容。 由于JSON对象没有特定的顺序且对大小没有要求，我们首先执行所谓的序列化。 从技术上讲，如果序列化一个（JSON）对象，最终会得到一个简单的二进制向量（读取字符串），它包含完全相同的信息，但对于内容放置的位置和方式有非常严格的结构。 它本质上只是内容的不同表示形式，可以让签名更容易且唯一，匹配与JSON键的排序方式无关。

在Steem中签名交易的好处是API节点以JSON格式接收交易，以便我们可以在纯文本中读取它的内容。 也就是说，有效的签名交易的形式是：

    {'expiration': '2016-08-09T10:06:15',
     'extensions': [],
     'operations': [['vote',
     {'author': 'piston',
      'permlink': 'xeroc',
      'voter': 'xeroc',
      'weight': 10000}]],
     'ref_block_num': 61828,
     'ref_block_prefix': 3564338418,
     'signatures': ['201b35c7f94d2ae56d940863a8db37edff78e3b8f4935b6c6fc131a04b92d0f9596c368128ab298c446143915e35996a9644314fff88b6a6438946403ec7249a24']}

签名是交易二进制、序列化的表示！

首先从给我们的操作编号开始，而不是让他们投票，评论，转账，...

    # 操作类型
    operations = {}
    operations["vote"] = 0
    # ....

这些数字在[steemd源代码](https://github.com/steemit/steem/blob/master/libraries/chain/include/steemit/chain/protocol/operations.hpp#L10)或[python-steem](https://github.com/xeroc/python-steemlib/blob/master/steembase/operations.py#L6)中定义。

现在让我们开始按照[交易的定义](https://github.com/steemit/steem/blob/master/libraries/chain/include/steemit/chain/protocol/transaction.hpp#L10)序列化前两项：

    buf = b""
    
    # ref_block_num
    buf += struct.pack("<H", tx["ref_block_num"])
    
    # ref_block_num
    buf += struct.pack("<I", tx["ref_block_prefix"])

在本例中，<表示我们以小端形式存储整数。 H是长度为2的无符号整数（uint16），而I是长度为4的无符号整数（uint32）。
我们将它们简单地附加到我们新创建的序列化缓冲区b中。

我们的序列化元素列表中的下一个是到期时间。 现在的问题在于，实际时间在上面的JSON对象和用于签名的序列化表单中表示方式不同。 这就是为什么我们将它进行转换的原因，因为以后可以使用无符号整数（uint32）表示时间戳。

    # 到期
    timeformat = '%Y-%m-%dT%H:%M:%S%Z'
    buf += struct.pack("<I", timegm(time.strptime((tx["expiration"] + "UTC"), timeformat)))

接下来，我们一次添加一个操作。 为了序列化以了解会有多少操作，我们首先添加上实际操作数，格式为varint编码的整数：

    buf += bytes(varint(len(tx["operations"])))

现在遍历所有的操作（在示例中只进行了投票操作），第一个添加到序列化缓冲区的是id，用于标识我们的操作。 我们已经收集了以上所有这些操作，operations["vote"]=0。 此id也是varint编码的。

    for op in tx["operations"]:
    
    # op[0] == "vote"
    buf += varint(operations[op[0]])

以上所有内容基本上与操作本身无关。 现在我们需要区分它们是如何编码/序列化的，为了简单起见，我只关注投票操作。
它按顺序包含（作为操作数据）以下元素：

1. 选民，
1. 作者，
1. permlink，和
1. 权重。

前三个都表示为长度前缀字符串。 这意味着：

1. 将字符串的长度编码为varint，并添加到缓冲区
1. 将字符串本身添加到缓冲区

我们可以为选民，作者和permlink执行这些操作。 权重是长度为2的短（带符号）整数。我们需要区分赞成票和否决票，而不是无符号整数。 另请注意，100％表示为+10000，-100％表示-10000，我们只使用整数而不使用浮点数（后端根本不知道浮点数！）

 if op[0] == "vote":
        opdata = op[1]
        buf += (varint(len(opdata["voter"])) +
                bytes(opdata["voter"], "utf-8"))
        buf += (varint(len(opdata["author"])) +
                bytes(opdata["author"], "utf-8"))
        buf += (varint(len(opdata["permlink"])) +
                bytes(opdata["permlink"], "utf-8"))
        buf += struct.pack("<h", int(opdata["weight"]))

**序列化结果**

将操作序列化非常重要。 我们来看看二进制形式：

首先我们序列化TaPOS参数ref_block_num（36029）

    bd8c..............................................................

和ref_block_prefix（1164960351）并获取

    ....5fe26f45......................................................

然后我们添加到期时间2016-08-08T12:24:17

    ............f179a857..............................................

之后，我们需要追加操作次数（01）

    ....................01............................................

并完成我们的操作：

操作ID（0）

    ......................00..........................................

选民（xeroc，长度5）

    ........................057865726f63..............................

作者（xeroc，长度5）

    ....................................057865726f63..................

permlink（piston，长度6）

    ................................................06706973746f6e....

票权（+1000）

    ..............................................................1027

我们的序列化交易的最终结果如下所示：

    bd8c5fe26f45f179a8570100057865726f63057865726f6306706973746f6e1027

**ECC 签名**

现在问题来了。 序列化交易的实际签名。 这意味着我们需要：

- 序列化缓冲区
- 链id（用于区分石墨烯链的不同实例并防止重放攻击）
- 用于签名的私钥（这些密钥是否足以实际授权交易由开发人员来保证）

在STEEM中，链id是256位长的0序列（唯一的）。 我们使用这个二进制形式的链id，并附加上序列化缓冲区从而获取消息。 我们不想签名实际消息，而是签名消息的SHA256哈希值。 消息的这个哈希值称为摘要。

    # 签名
    chainid = "0" * int(256 / 4)
    message = unhexlify(chainid) + buf
    digest = hashlib.sha256(message).digest()

现在我们获取了所有私钥并用所有私钥来签名交易。 每个私钥都将产生一个签名，这些签名需要添加到原始交易的签名密钥中。 在我们的例子中，我们只使用一个了私钥，以WIF表示。 我们从WIF获取实际的二进制私钥（为了简单起见，我使用了来自steembase.account的PrivateKey类）

    wifs = ["5JLw5dgQAx6rhZEgNN5C2ds1V47RweGshynFSWFbaMohsYsBvE8"]
    sigs = []
    for wif in wifs:
    p = bytes(PrivateKey(wif))  # binary representation of private key

幸运的是，我们不需要手动完成所有签名，可以使用ecdsa python包。 我们通过正确加载私钥来获取SigningKey：

 `sk = ecdsa.SigningKey.from_string(p, curve=ecdsa.SECP256k1)`

现在，我们会实现一个至关重要的循环，因为后端只接受规范签名，我们无法知道将要生成的签名是否是规范的。 也就是说我们将为ECDSA签名推导出确定性的k参数，而生成此参数将在哈希前将循环计数器添加到摘要中。 这将导致每轮都有一个新的确定性k，因为产生一个规范或非规范签名。

       cnt = 0
    i = 0
    while 1 :
    cnt += 1
    # 确定性 k
    #
    k = ecdsa.rfc6979.generate_k(
    sk.curve.generator.order(),
    sk.privkey.secret_multiplier,
    hashlib.sha256,
    hashlib.sha256(digest + bytes([cnt])).digest())

通过使用适当的ECDSA签名调用生成签名：

      # Sign message
        #
        sigder = sk.sign_digest(
            digest,
            sigencode=ecdsa.util.sigencode_der,
            k=k)

现在我们在r和s值表示签名并验证它的规范性。 如果签名规范，就可以跳出循环并继续执行：

     # 转换签名格式
        #
        r, s = ecdsa.util.sigdecode_der(sigder, sk.curve.generator.order())
        signature = ecdsa.util.sigencode_string(r, s, sk.curve.generator.order())

        # 确保签名是规范的!
        #
        lenR = sigder[3]
        lenS = sigder[5 + lenR]
        if lenR is 32 and lenS is 32 :
            # ........

一旦我们确保签名是规范的，我们就得出了所谓的恢复参数。 它简化了签名的验证，因为它将签名链接到单个唯一的公钥。 如果没有此参数，后端需要测试多个公钥而不是一个公钥。 因此我们推导出这个参数，添加4和27以保持与其他协议的兼容性，现在已经获得了签名。

     # 派生出恢复参数
            #
            i = recoverPubkeyParameter(digest, signature, sk.get_verifying_key())
            i += 4   # compressed
            i += 27  # compact
            break

派生出有效的规范签名后，我们将其格式化为十六进制表示形式，并将其添加到交易签名中。 请注意，我们不仅添加签名，还添加了恢复参数。 这种签名称为**紧凑**签名。

    tx["signatures"].append(
        hexlify(
            struct.pack("<B", i) +
            signature
        ).decode("ascii")
    )

完成。

一旦你派生出新rx(包括签名)，你就可以使用python-steem来验证你的交易和签名，方法如下：

    from steembase import transactions
    tx2 = transactions.Signed_Transaction(**tx)
    tx2.deriveDigest("STEEM")
    pubkeys = [PrivateKey(p).pubkey for p in wifs]
    tx2.verify(pubkeys, "STEEM")

如果它没有抛出异常，你就做对了！

本例发的完整源代码可以在[github](https://gist.github.com/9bda11add796b603d83eb4b41d38532b)上找到。

快乐编码！
