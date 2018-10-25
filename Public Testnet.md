 ===========================================

  **原文链接**：<http://dev.bitshares.works/en/master/development/testnets.html>

 **译者**：https://github.com/towel223 （小办龙的蜗牛）

 **校对者**：

 ===========================================

# 公共测试网 #
本节介绍了如何在测试网环境中部署和定制石墨烯区块链并且展示让节点（见证人节点）生产区块的步骤。
## 目录 ##

- 如何设置开放的公共测试网

	1.安装与设置
	
	2.创世配置
	
	3.初始化区块链
	
	4.连接命令行钱包
		
		获得资助的权利
		管理账户
	
	5.建立理事会

	6.Web钱包

----------
# 如何设置开放式公共测试网 #

## 1.安装与配置 ##

1.1比特股内核测试网分支

    git clone https://github.com/bitshares/bitshares-core.git bitshares-core-testnet
    cd bitshares-core-testnet/
    git checkout testnet

1.2配置

区块链参数

这个区块链参数可以在libraries/chain/include/graphene/chain/config.hpp文件中修改:

    vim libraries/chain/include/graphene/chain/config.hpp

默认种子节点列表

我们可以在libraries/app/aplication.cpp中添加一个默认种子节点，见证人应该尝试连接上他。并且将添加稍后要安装的机器的地址和端口

    testnet.bitshares.eu:11010

在相应的git提交中可以看到所有的更改设置参数

1.3初始化编译

    cmake .
     make

我们首先需要编译石墨烯工具包，以便于让他以适当的格式生成一个简易的创世文件。

# 2.创世配置 #
2.1创建一个创世文件

我们将创建一个包含创世区块名为my-genesis.json的创世文件

    mkdir -p genesis
    programs/witness_node/witness_node --create-genesis-json genesis/my-genesis.json
    vim genesis/my-genesis.json

my-genesis.json是网络的初始化状态

2.2编辑一个创世文件

如果你想要定制网络的初始化状态，编辑my-genesis.json。这样允许你管理的内容如下：

- 在刚开始时的账户的名字和公钥
- 资产的初始化供应（包括核心资产）
- 链上参数的初始值
- 初始见证人的账户和密钥（或实际情况中的任何账户）

链的标识是一个创世状态的哈希值。所有交易签名都仅有唯一一个链上标识。所以编辑创世文件将改变你的链上标识，并且使你无法与现有的所有链同步数据（除非他们有人也用和你一样的创世文件）

## 复制最终的创世文件 ##
我们现在复制我们的创世模版文件覆盖到石墨烯根目录下:

    $ cp genesis/my-genesis.json genesis.json
    $ vim genesis.json
    $ git add genesis.json
    $ git commit -m "Added genesis.json"

这个初始时间戳需要复制到genesis.json文件的initial_timestamp参数中。选好一个离你要产生创世区块最接近的未来的时间。（比如10分钟以后）

## 把创世写成二进制编码 ##
为了用二进制编码执行你的新创世区块，我们需要重新编译它，并提供给cmake参数使创世区块能够正确识别。

    $ make clean
    $ find . -name "CMakeCache.txt" | xargs rm -f
    $ find . -name "CMakeFiles" | xargs rm -Rf
    $ cmake -DGRAPHENE_EGENESIS_JSON="$(pwd)/genesis.json" .

你可以增加GRAPHENE_EGENESIS_JSON为默认参数：

    set(GRAPHENE_EGENESIS_JSON "${CMAKE_CURRENT_SOURCE_DIR}/genesis.json" )

到CMakeLists.txt文件。这样你就可以不用老师提供参数了。

# 3.初始化区块链 #
3.1初始化创世区块

--enable-stale-production标志可以告诉witness_node在链上产生的是零区块或是很旧的区块。我们在命令行上指定--enable-stale-production的参数，因为我们不经常用到它（虽然这个总是在配置文件中被指定）

    programs/witness_node/witness_node --genesis-json genesis.json \
      								  --enable-stale-production \
      								  --data-dir data/testnet


链的ID::

    Started witness node on a chain with 0 blocks.
    Chain ID is cf307110d029cb882d126bf0488dc4864772f68d9888d86b458d16e6c36aa74b

*注释*

*如果其他见证人产生区块并且见证人的参与度足够高，随后运行链接的一个已存在的见证人节点超过点对点网络，或从一个已有的数据目录获得区块链状态，就不需要--enable-stale-production标志。*

3.2设置生产区块

创建一个非常简单的配置文件data/testnet/config.ini：

    $ mkdir -p data/testnet
    $ vim data/testnet/config.ini

我们把所有见证人的标识和钥匙放入配置文件，以便于我们开始生产区块。

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
    # 为每个见证人节点添加公钥和私钥:
    private-key = ["GPH6MRyAjQq8ud7hVNYcfnVPJqcVpscN5So8BhtHuGYqET5GDW5CV","5KQwrPbwdL6PhXujxW37FSSQZ1JiwsST4cqQzDeyXtP79zkvFD3"]
    private-key = [<pubkey>,<privkey>]
    private-key = [<pubkey>,<privkey>]
    private-key = [<pubkey>,<privkey>]
    private-key = [<pubkey>,<privkey>]
    private-key = [<pubkey>,<privkey>]
    private-key = [<pubkey>,<privkey>]
    private-key = [<pubkey>,<privkey>]
    private-key = [<pubkey>,<privkey>]
    private-key = [<pubkey>,<privkey>]
    private-key = [<pubkey>,<privkey>]
    

由列表中列出witness-id的witness_node来生产区块，并且用指定的私钥对区块签名。通常每个见证人都是不同的节点，但为了启动测试网，我们为了启动让所有见证人在一个节点上签署区块。

*注释*

*设置rpc-endpoint = 0.0.0.0:11011将打开远程调用端口11011连接命令行钱包或网络钱包。通过p2p-endpoint = 0.0.0.0:11011，该节点可以在互联网上访问，所以可以作为种子节点。*

3.3嵌入创世区块（可选）

现在我们已经建立了区块链，并使用了正确的区块，我们可以把它加入到二进制文件中。因为我们已经这个文件放在根目录下，并且通过genesis.json的默认编译工具链自动获取。我们需要重新编译包含创世区块如下：

    make clean
    find . -name "CMakeCache.txt" | xargs rm -f
    find . -name "CMakeFiles" | xargs rm -Rf
    cmake -DCMAKE_BUILD_TYPE=Release .

删除缓存重启所有cmake变量，你或许会想要设置其他cmake变量。那么就必需把那些变量加入上面的cmake行中。

将创世区块中的所有内容嵌入到witness_node的二进制文件中，另外复制链的ID到cli_wallet二进制文件中。嵌入操作时可以简化后面的指令如下：

- 在见证人节点命令行上或见证人节点配置文件中，你不需要指定的创世文件。
- 当在cli_wallet命令行开启新钱包时，你不需要指定链的ID。

# 4.连接命令行钱包 #
我们将演示如何连接一个命令行钱包到新的区块链，并在新区块链上完成我们第一笔交易。

为了创建一个钱包，你必须指定已预先设定好的配置服务。通过见证人节点进入管理设置。

## 创建一个钱包-公共测试网: ##
    programs/cli_wallet/cli_wallet --wallet-file my-wallet.json -s ws://127.0.0.1:11011 -H 127.0.0.1:8090 -r 127.0.0.1:8099

>注释：“-H”这个参数是必要的，便于我们可以通过RPC-HTTP-JSON用命令行钱包
后面的“-R”将打开一个给水龙头打开一个端口。

如果你看到set_password命令的提示，这意味着你的CLI钱包已经成功连上测试网的见证人节点了。

- set_password

这个密码是用来在钱包中加密私钥的。（比如supersecet是一个密码）：

    >>> set_password supersecret



- 解锁钱包

- 

    >>> unlock supersecret


在石墨烯中，余额包含在账户里。输入一个在石墨烯创世时存在的账户.(例如<name>是一个账户名，私钥是<wifkey>）

- import_key:

-

    >>> import_key <name> "<wifkey>"

资金是存贮在创世链的余额对象中。这些资金可以被提取得，而且不需要费用。


- import_balance:

-

    >>> import_balance <name> ["*"] true

# 管理账户 #
- upgrade_account

获得终生会员资格：成为终生会员资格需要去创建一个账户。你可以升级这个账户。：

>>> upgrade_account faucet true


- register_account:

-

    register_account <name> <owner-public_key> <active-public_key> <registrar_account> <referrer_account> <referrer_percent> <broadcast>


这个命令允许你仅使用公钥注册账户。

例如：

    >>> register_account alpha GPH4zSJHx7D84T1j6HQ7keXWdtabBBWJxvfJw72XmEyqmgdoo1njF GPH4zSJHx7D84T1j6HQ7keXWdtabBBWJxvfJw72XmEyqmgdoo1njF faucet faucet 0 true

测试从faucet发送CORE到alpha的转账

## 打开一个用户名为"alpha"的新钱包 ##
    >>> import_key alpha 5HuCDiMeESd86xrRvTbexLjkVg2BEoKrb7BAA5RLgXizkgV3shs
    
    >>> upgrade_account alpha true
    
    >>> create_witness alpha "http://www.alpha" true
    

## 获取私钥 ##
get_private_key命令可以帮助我们获得为区块署名的私钥：

    >>> get_private_key GPH6viEhYCQr8xKP3Vj8wfHh6WfZeJK7H9uhLPDYWLGCRSj5kHQZM

# 5.建立理事会 #
当然，我们的网络需要一个理事会。我们需要初始化建立新的账户并且是他们成为理事会成员。

5.1创建成员账号


- create_account_with_brain_key:

.

    create_account_with_brain_key com0 com0 faucet faucet true
    create_account_with_brain_key com1 com1 faucet faucet true
    create_account_with_brain_key com2 com2 faucet faucet true
    create_account_with_brain_key com3 com3 faucet faucet true
    create_account_with_brain_key com4 com4 faucet faucet true
    create_account_with_brain_key com5 com5 faucet faucet true
    create_account_with_brain_key com6 com6 faucet faucet true

5.2升级成员账号

只有终生会员才能成为理事会成员，因此我们为账户转入资金并升级成终生会员。:

    transfer faucet com0 100000 CORE "some cash" true
    transfer faucet com1 100000 CORE "some cash" true
    transfer faucet com2 100000 CORE "some cash" true
    transfer faucet com3 100000 CORE "some cash" true
    transfer faucet com4 100000 CORE "some cash" true
    transfer faucet com5 100000 CORE "some cash" true
    transfer faucet com6 100000 CORE "some cash" true
    upgrade_account com0 true
    upgrade_account com1 true
    upgrade_account com2 true
    upgrade_account com3 true
    upgrade_account com4 true
    upgrade_account com5 true
    upgrade_account com6 true

5.3注册为理事会成员

我们可以通过create_commitee_member申请成为理事会成员

    create_committee_member
    create_committee_member com0 "" true
    create_committee_member com1 "" true
    create_committee_member com2 "" true
    create_committee_member com3 "" true
    create_committee_member com4 "" true
    create_committee_member com5 "" true
    create_committee_member com6 "" true

5.4通过水龙头账户投票

    vote_for_committee_member
    vote_for_committee_member faucet com0 true true
    vote_for_committee_member faucet com1 true true
    vote_for_committee_member faucet com2 true true
    vote_for_committee_member faucet com3 true true
    vote_for_committee_member faucet com4 true true
    vote_for_committee_member faucet com5 true true
    vote_for_committee_member faucet com6 true true

6.Web钱包

在我们为其他人提供区块链网络的入口后，我们需要在Nginx上安装Web钱包

    sudo apt-get install git nodejs-legacy npm
    sudo npm install -g webpack coffee-script
    

6.1安装相关包

    sudo apt-get install git nodejs-legacy npm
    sudo npm install -g webpack coffee-script

6.2获取Web钱包

然后，我们从区块链上下载的bitshare-ui库并安装在相应节点上：
    
    git clone https://github.com/bitshares/bitshares-ui
    cd bitshares-ui/
    
    for I in dl web; do cd $I; npm install; cd ..; done

6.3配置

获取我们运行的链的chain_id：

    $ curl --data '{"jsonrpc": "2.0", "method": "get_chain_properties", "params": [], "id": 1}' http://127.0.0.1:11011/rpc && echo

链的id是用于告诉Web钱包连入了哪个区块链网络并且如何连接的。为此需要修改dl/src/chain/config.comffee文件并加入我们的区块链


    Test:
    core_asset: "TEST"
    address_prefix: "TEST"
    chain_id: "<chain-id>"

另外，我们需要告诉我们的Web钱包连接到那个见证人节点。这个工作可以在dl/src/stores/SettingsStore.js文件中完成。

    connection: "ws://<host>/ws",
    faucet_address: "https://<host>",
    
    # also edit the "default" settings

6.4编译

编译Web钱包

这一步将产生在文件夹中生产静态文件。

    cd web
    npm run build