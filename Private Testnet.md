 ===========================================

  **原文链接**：<http://dev.bitshares.works/en/master/development/testnets.html>

 **译者**：https://github.com/towel223 （小办龙的蜗牛）

 **校对者**：

 ===========================================

# 私有测试网 #
为了处理速度的原因，一些开发者们可能想部署他们自己能够支配的本地石墨烯区块链。本节将说明如何准备私有测试网的网络环境并且如何让节点（见证人节点）生产区块的步骤.
## 目录 ##

如何设置私有测试网

1.必备条件

2.创建测试网文件夹

3.创建私有测试网的创世文件

4.自定义创世文件

默认创世文件

5.嵌入创世文件（可选）

6.建立一个数据目录

7.设置见证人配置

8.启动区块生产

9.获取链的ID

10.创建一个新钱包

11.取得创世资金

12.创建另外的账户

两个账户之间转账

13.创建理事会成员



## 如何设置私有测试网 ##

1.必备条件

如果你没有安装比特股内核并构建它。请查看安装向导。并且确保你已经编译witness_node和cli_wallet成功。

2.创建一个测试网目录

在任何你喜欢的未知，创建一个新的目录（比如【Testnet-Home】。并把witness_node和cli_wallet复制到那。【Testnet-Home】目录将包含所有与测试网有关的文件和目录。
打开一个命令提示符窗口并切换当前目录为【Testnet-Home】。

3.为私有测试网创建一个创世文件

genesis.json用来初始化网络状态。我们在相同的目录为私有网络创建一个新的genesis.json文件名字叫my-genesis.json。：


    ./witness_node --create-genesis-json=my-genesis.json



上面命令将在Testnet-Home目录创建my-genesis.json。一旦完成这个任务，见证人节点将自行终止。

*注释*

*witness_node将启动一个witness_node_data_dir在默认数据目录（通过config.ini配置）。如果你想使用不同的目录名和数据目录，你可以在命令行中用--data-dir，设置你的数据文件夹路径。另外，witness_node_data_dir文件夹将被创建，并且使用默认的config.ini文件启动见证人节点。*



4.自定义创世文件

如果你想自定义网络的初始状态，编辑my-genesis.json。你可以管理以下内容：
在创世时已存在的账户，他们名字和公钥
资产和它们的供应（包括核心资产）
链的初始化参数
初始见证人的账户和签名密钥（实际上能看到任何账户）
默认创世设置
石墨烯代码有一个包含所有见证人、理事会成员、资金、单一账户的缺省创世区块：

5KQwrPbwdL6PhXujxW37FSSQZ1JiwsST4cqQzDeyXtP79zkvFD3



5.嵌入创世文件（可选）

一旦你建好了genesis.json,你就可以像如下设置cmake的变量：


    cmake -DGRAPHENE_EGENESIS_JSON="$(pwd)/genesis/my-genesis.json"



然后重新构建。注意，有时为了让GRAPHENE_EGENESIS_JSON发挥作用，必须清理架构和缓存中的变量：

    make clean
    find . -name "CMakeCache.txt" | xargs rm -f
    find . -name "CMakeFiles" | xargs rm -Rf
    cmake -DGRAPHENE_EGENESIS_JSON="$(pwd)/genesis/my-genesis.json" .



删除缓存将重置所有cmake变量，所以如果你用像build-ubuntu这样的指令它会告诉你设置其他cmake变量，你将需要增加变量到cmake命令行中。
复制genesis.json中所有内容信息的嵌入witness_node二进制文件中，另外复制链的ID到cli_wallet二进制文件中。嵌入操作时可以简化后面的指令如下：
在见证人节点命令行上或见证人节点配置文件中，你不需要指定的创世文件。
当在cli_wallet命令行开启新钱包时，你不需要指定链的ID。
嵌入式创世文件是一个为了让用户预编译过程中更容易的重要设计，在一些小的变动中，也可选择复杂编译的方法产生二进制文件。



6.创建一个数据目录
我们在我们的见证人节点创建一个新的数据目录。：

    ./witness_node --data-dir data/my-blockprod --genesis-json my-genesis.json --seed-nodes "[]"   # or
    
    ./witness_node --data-dir=data/my-blockprod --genesis-json=my-genesis.json --seed-nodes "[]"



*注释：*

*data/my-blockprod目录不存在的话，将被见证人节点创建。
seed-nodes = []创建一个空的种子节点列表避免连接默认的硬编码种子节点。*

已知的问题：在输入参数和值时丢失“=”。这个问题导致了一个漏洞，应该增加了1.60，如果你编译实际增加了1.58，“=”被忽略了。

下面的信息意思是初始化完成。通过小的创世例子，它几乎瞬间就完成了，除非你添加大量的余额。用ctrl-c关闭见证人节点。

    3501235ms th_a main.cpp:165 main] Started witness node on a chain with 0 blocks.
    3501235ms th_a main.cpp:166 main] Chain ID is cf307110d029cb882d126bf0488dc4864772f68d9888d86b458d16e6c36aa74b



因此，你将获得两条信息：

一个名为data/my-blockprod的目录被创建完成并且有一个config.ini文件在里面。

链的ID现在已经知道，并在后面的信息中显示出来。



7.设置见证人配置

打开[Testnet-Home]/data/my-blockprod/config.ini文件并如下设置，不必要时可以注释他们：

    rpc-endpoint = 127.0.0.1:8090
    genesis-json = my-genesis.json
    enable-stale-production = true
    
    private-key = ["GPH6MRyAjQq8ud7hVNYcfnVPJqcVpscN5So8BhtHuGYqET5GDW5CV","5KQwrPbwdL6PhXujxW37FSSQZ1JiwsST4cqQzDeyXtP79zkvFD3"]
    
    witness-id = "1.6.1"
    witness-id = "1.6.2"
    witness-id = "1.6.3"
    witness-id = "1.6.4"
    witness-id = "1.6.5"
    witness-id = "1.6.6"
    witness-id = "1.6.7"
    witness-id = "1.6.8"
    witness-id = "1.6.9"
    witness-id = "1.6.10"
    witness-id = "1.6.11"



在上文列表中列出witness-id的witness_node来生产区块，并且用指定的私钥对区块签名。通常每个见证人都是不同的节点，但为了启动测试网，我们为了启动让所有见证人在一个节点上签署区块。



8.开始区块生产

现在再次运行见证人节点：

    ./witness_node --data-dir data/my-blockprod --enable-stale-production --seed-nodes "[]"



*警告*

*如果你想用不同的目录名和不同的数据目录，你必须用--data-dir在启动命令行中设置你的数据目录文件夹路径。另外，这个witness_node_data_dir文件夹将会被创建，并且用一个默认config.ini文件启动witness_node！*

注释：
由于这是一个测试网，我们不需要在命令行上指定genesis.json。现在我们可以在config.ini中指定它。
--enable-stale-production标志用来告诉witness_node在链上生产的是零区块或者旧区块。我们并不经常需要在命令行上指定--enable-stale-production的参数。（虽然它经常在配置文件中被指定）
添加空的--seed-nodes参数是为了避免连接默认种子节点产生硬编码
随后，运行连接到一个在点对点网络上已存在的见证人节点，或从数据目录中得到区块链状态，不一定需要--enable-stale-production标志。



9.获得链的ID

（参看#6.当我们创建一个数据目录，我们就能获得链的ID。）

这个链的ID（例如区块链 id）是一个创世区块链上的哈希值。所有的交易签名都仅存在于唯一链的ID。所以编辑创世文件将改变你链的ID，并且导致无法同步所有已存在的链。（除非他们中的一个也用了和你相同的创世文件）

为了达到测试的目标，--dbg-init-key选项将帮你通过替换j见证人的块生产密钥来快速创建针对任何创世文件的新链。

每个钱包都被特定关联到一个唯一链，通过他们的链ID指定。这是为了保护用户在真实链上使用测试网的钱包。

这个链的ID在见证人启动时被打印输出。它可以通过调用get_chain_properties的API查询见证人节点获取。：


    curl --data '{"jsonrpc": "2.0", "method": "get_chain_properties", "params": [], "id": 1}' http://127.0.0.1:11011/rpc && echo


这个curl命令将返回一个包含chain_id的JSON对象。




10.创建一个新钱包

我们现在准备将一个新钱包连入你的私有测试网见证人节点。你必须指定一个链的ID和服务器。保证你的见证人节点稳定运行并且在另外一个命令提示符窗口中运行命令（一个空白的用户名和一个密码才行）

    ./cli_wallet --wallet-file my-wallet.json
       --chain-id cf307110d029cb882d126bf0488dc4864772f68d9888d86b458d16e6c36aa74b
       --server-rpc-endpoint ws://127.0.0.1:11011 -u '' -p ''
    



*注释：*

*确认更换上文链的ID（例如区块链id）cf307110d0...36aa74b成你通过你的witness_node获得的自己的链的ID。这个chain-id传递到CLI-wallet中匹配的id生成的，并且被见证人节点使用。
--server-rpc-endpoint其中witness_node的端口号是通过--rpc-endpoint定义的（打开）。*

如果你看到set_password命令的提示，这意味着你的CLI钱包已经成功连上测试网的见证人节点了。

你需要为你的钱包创建一个新的密码并保存好。这个密码是用来在钱包中加密私钥的。例如，我们用“supersecret”作为密码：

    >>> set_password supersecret



11.获取创世资金
在石墨烯中，余额包含在账户里。在你的钱包里，输入一个在石墨烯创世时存在的账户，你需要在这之前知道它的用户名和私钥。我们通过用import_key命令，输入一个账户名为“nathan”的账户（一个常规的测试账户）:

    unlock supersecret
    import_key nathan "5KQwrPbwdL6PhXujxW37FSSQZ1JiwsST4cqQzDeyXtP79zkvFD3"



注意
nathan是在创世文件中定义的账户名。如果你是在创建之后，编辑你的my-genesise.json文件。你也可以放一个不同的名字在哪。而且把私钥5KQwrPbwdL...P79zkvFD3定义在config.ini文件。

现在我们有私钥导入到了钱包，但仍然没有资金在里面。资金储存在创世余额对象中。可以用import_balance命令获取这些资金，并且不需要支付手续费：


    import_balance nathan ["5KQwrPbwdL6PhXujxW37FSSQZ1JiwsST4cqQzDeyXtP79zkvFD3"] true



在最后，我们有一个账户导入到了钱包并且这个账户充满了从创世资金池取得的BTS。你可以用下面的命令，看到这个账户的信息和余额：

    get_account nathan
    list_account_balances nathan



我们现在创建另外一个账户（名字是alpha）以便于我们可以在nathan和alpha之间来回转账。

通常要通过一个已存在的账户来创建一个新的账户-我们需要资金付注册费用。然而，注册账户并成为终生成员账户是又必备条件的。所以在我们创建其他账户之前，需要先升级nathan这个账户到终生成员账户。

    upgrade_account nathan true
    get_account nathan



在下一步的响应的信息中membership_expiration_date你将看到如1969-12-31T23:59:59。如果你看到的时间是1970-01-01T00:00:00，那么一定是出现了差错，并且nathan没有成功升级。
现在，我们可以用nathan注册一个账户。但是首先需要获得一个公钥作为新账户的公钥。我们用suggest_brain_key命令实现。
并且响应的信息如下：

    suggest_brain_key
    {
    "brain_priv_key": "MYAL SOEVER UNSHARP PHYSIC JOURNEY SHEUGH BEDLAM WLOKA FOOLERY GUAYABA DENTILE RADIATE TIEPIN ARMS FOGYISH COQUET",
    "wif_priv_key": "5JDh3XmHK8CDaQSxQZHh5PUV3zwzG68uVcrTfmg9yQ9idNisYnE",
    "pub_key": "BTS78CuY47Vds2nfw2t88ckjTaggPkw16tLhcmg4ReVx1WPr1zRL5"
    }


现在我们可以注册一个账户。register_account命令可以让你仅用公钥注册一个账户。

    register_account alpha BTS78CuY47Vds2nfw2t88ckjTaggPkw16tLhcmg4ReVx1WPr1zRL5 BTS78CuY47Vds2nfw2t88ckjTaggPkw16tLhcmg4ReVx1WPr1zRL5 nathan nathan 0 true



>用一个刚刚用suggest_brain_key创建的公钥pub_key



在账户之间转账



“here is some cash”这段文本是专门用在这笔转账上的备注。如果你不需要那，可以什么都不填。

现在我们可以打开用户名为alpha的一个新钱包：

    import_key alpha 5JDh3XmHK8CDaQSxQZHh5PUV3zwzG68uVcrTfmg9yQ9idNisYnE
    upgrade_account alpha true
    create_witness alpha "http://www.alpha" true



>所用私钥wif_priv_key是刚刚suggest_brain_key创建的。

get_private_key命令可以让我们获得与公钥相对应的私钥。这个私钥必须是已经在钱包里的：


    get_private_key BTS78CuY47Vds2nfw2t88ckjTaggPkw16tLhcmg4ReVx1WPr1zRL5



>你可以尝试用此命令确认suggest_brain_key输出的密钥对。你将获得同样一对要是



13.创建理事会成员

    create_account_with_brain_key com0 com0 nathan nathan true
    create_account_with_brain_key com1 com1 nathan nathan true
    create_account_with_brain_key com2 com2 nathan nathan true
    create_account_with_brain_key com3 com3 nathan nathan true
    create_account_with_brain_key com4 com4 nathan nathan true
    create_account_with_brain_key com5 com5 nathan nathan true
    create_account_with_brain_key com6 com6 nathan nathan true
    transfer nathan com0 100000 CORE "some cash" true
    transfer nathan com1 100000 CORE "some cash" true
    transfer nathan com2 100000 CORE "some cash" true
    transfer nathan com3 100000 CORE "some cash" true
    transfer nathan com4 100000 CORE "some cash" true
    transfer nathan com5 100000 CORE "some cash" true
    transfer nathan com6 100000 CORE "some cash" true
    upgrade_account com0 true
    upgrade_account com1 true
    upgrade_account com2 true
    upgrade_account com3 true
    upgrade_account com4 true
    upgrade_account com5 true
    upgrade_account com6 true
    create_committee_member com0 "http://www.com0" true
    create_committee_member com1 "http://www.com1" true
    create_committee_member com2 "http://www.com2" true
    create_committee_member com3 "http://www.com3" true
    create_committee_member com4 "http://www.com4" true
    create_committee_member com5 "http://www.com5" true
    create_committee_member com6 "http://www.com6" true
    vote_for_committee_member nathan com0 true true
    vote_for_committee_member nathan com1 true true
    vote_for_committee_member nathan com2 true true
    vote_for_committee_member nathan com3 true true
    vote_for_committee_member nathan com4 true true
    vote_for_committee_member nathan com5 true true
    vote_for_committee_member nathan com6 true true
    
    propose_parameter_change com0 {"block_interval" : 6} true

