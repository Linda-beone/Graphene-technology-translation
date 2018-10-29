原文链接：http://dev.bitshares.works/en/master/bts_guide/tutorials/exchange_single_node.html
译者 ：bt666

# 比特股交易所对接指南 （单节点版）
本文档的目的是协助第三方交易所上线比特股交易。

本节描述的方案是单节点版本的方案。与另一文档描述的双节点方案相比，单节点的方案可以节约内存、磁盘以及同步的时间。

[TOC]

## 基本概念
### 共识
比特股使用DPOS共识机制给持有比特股的人投票从而生产区块。标准的产块间隔周期是3秒。

### 账户

1. 在比特股中，资金存储在账户里，而不像比特币那样存储在地址里。对于交易所而言，需要为用户创建一个账户用于比特股的充值。

	- 可以使用网页钱包或轻钱包注册新账号

    <table style="width: 750px;"><tbody>
    <tr>
        <td bgcolor="LightBlue">注意</td>
    </tr>
    <tr>
        <td bgcolor="MintCream"> 对于交易所而言，已注册的账户请使用钱包模式（本地钱包），而不要使用账户模式（云钱包），这是因为交易所需要使用一些高级功能，这些功能在账户模式下会出现问题。 <br>

	 -[BitShares-UI 版本](https://github.com/bitshares/bitshares-ui/releases)
	- [BitShares UI 钱包：https://wallet.bitshares.org/](https://wallet.bitshares.org/)
	- [创建账户指南](http://how.bitshares.works/en/latest/user_guide/create_account.html)	 
	
    </td>
    </tr>
    </table>
 
	- 不是所有的钱包都可以免费注册的，一般来说，普通的或者数字账户可以免费注册，如my-exchange或myexchange2017
	- 在轻钱包页面，账号下方会显示一个数字。这个数字是比特股系统中内置的账号ID，接下来将会用到。

    <table style="width: 750px;"><tbody>
    <tr>
        <td bgcolor="LightBlue">提示</td>
    </tr>
    <tr>
        <td bgcolor="MintCream"> 可以在区块浏览器中输入账户名来获取账户ID</td>
    </tr>
    </table>

	- 你也可以在自己搭建的节点同步完成后，使用命令行来获取钱包的ID。可以在**查看用于提现的账户名**一节查看所需的命令。

    <table style="width: 750px;"><tbody>
    <tr>
        <td bgcolor="LightBlue">注意</td>
    </tr>
    <tr>
        <td bgcolor="MintCream">为了保障资金安全，交易所可以使用一个账户用于充值，另一个账户用于提现</td>
    </tr>
    </table>

2. 用户充值就是将其他账户的资金转账到交易所开放的账户里。
	- 账户名是付款地址
	- 每笔汇款都可以带有备注，交易所使用备注来区分不同用户的充值
	- 特定的备注与交易所的用户关系有关，交易所必须设置它
	- 备注是加密了的，只有汇款方或收款方的备注密钥才能加以解密

3. 用户提现就是将资金从交易所账户中转到用户的账户。目标账户名由用户提供。

	- 由于某些用户需要将资金直接从一个交易所转到另一个交易所，且另一个交易所是基于备注的，因此提现最好是使用备注。

4. 使用网页钱包注册的账户是基本会员账户。该账户可以升级到终身会员（LTM）账户，升级后，接下来的交易手续费可以节省80%。
	- 终身会员可以创建新账户
	- 当前的费率标准可以在钱包查看。在界面中点击切换到[资源管理器] - [费用表]选项卡。
	
5. 每个账户默认都有三种密钥，可以在侧边菜单栏的权限页面查看。这三种分别是：活动的、账户所有者的以及备注密钥。
	- 其中活动权限密钥用于转账等日常操作；账户所有者权限密钥用于修改密钥；备注密钥用于加密和解密备注。
	- 默认地，活动权限密钥与备注密钥相同，但是可以改成不同的
	- 上面这三种密钥都是可以被修改的。其中账户所有者的（账户权限）具有最高的权限，可以修改所有的密钥；使用账户所有者（活动权限）密钥，不能修改所有者（账户权限）密钥，但是可以修改其他两种密钥。
	
### 资产

1. 比特股系统中有两种不同的资产，其中核心资产是BTS。比特股系统中其他资产的交易方式与BTS的类似。
2. 每个账户可以同时拥有多种资产。

### 区块链结构
	
   - 每个区块都有一个ID，其中包含了区块内容的哈希值
   - 每个区块都包含前一个区块的ID，存储在previous字段里，从而形成一条链
   - 每个区块都包含多笔交易信息，存储在transaction字段，	且按顺序存储。当使用API获取区块信息时，会返回transaction_ids字段，该字段是交易IDs的列表。它是一系列交易的哈希值（没有签名的）。
   - 每个交易都包含多个操作，存储在operations字段，且按顺序存储。
   - 每种操作也有一个ID，它是一个全局的数字，在程序执行时内部产生，而且并不是哈希值
   

	阅读：[区块组件信息](http://dev.bitshares.works/en/master/components/lib_block.html#lib-block)

## 软硬件的基本要求

- 独立的服务器或VPS
- 8G的内存（越大越好）
- 50G的硬盘

安装64位的Ubuntu 16.04 LTS（在32位的Ubuntu上无法运行），或64位的Ubuntu 14.04 LTS，或者Windows服务器。

查看[系统要求](http://dev.bitshares.works/en/master/development/apps/node.html#system-requirements-node)

## 程序准备

要启动比特股系统，需要运行一下这些程序：正常节点 witness_node，以及命令行钱包 cli_wallet。

### 架构描述

 - witness_node使用P2P协议连接比特股网络，接收网络中的最新区块，将本地签名了的交易广播到网络
 - witness_node提供了API，其他程序可以通过websocket和HTTP RPC服务调用这些API（以下称为节点APIs）
 - Cli_wallet通过websocket服务连接witness_node
 - Cli_wallet管理钱包文件，文件中包含加密过的用户私钥。一个钱包文件可以包含多个私钥。
 - 你可以同时运行多个Cli_wallet线程，并连接witness_node来同时管理多个钱包文件
 - Cli_wallet提供了交易签名功能，在witness_node签名后广播该签名
 - Cli_wallet提供了可供其他程序通过HTTP服务调用的API（以下称为钱包APIs）


    <table style="width: 750px;"><tbody>
    <tr>
        <td bgcolor="LightBlue">注意</td>
    </tr>
    <tr>
        <td bgcolor="MintCream">建议交易所使用一个账户用于充值，另一个账户用于提现</td>
    </tr>
    </table>

### Windows

编译后的Windows可执行程序可以在Github上下载，链接 [https://github.com/bitshares/bitshares-core/releases/latest。](https://github.com/bitshares/bitshares-core/releases/latest)。

该文件名是BitShares-Core-2.0.xxxxxx-x64-cli-tools.zip，可解压。文件包括三个可执行程序和两个动态链接程序。查看安装指南：[CLI-Wallet Windows(x64)](http://dev.bitshares.works/en/master/development/installation/windows_cli_tool.html#cli-tool)

### Linux 

如果使用 Linux 系统，需要自行编译几个上述程序。推荐使用 Ubuntu 16.04 LTS ，编译步骤如下：

    sudo apt-get update
    sudo apt-get install autoconf cmake git libboost-all-dev libssl-dev doxygen g++ libcurl4-openssl-dev

    git clone https://github.com/bitshares/bitshares-core.git
    cd bitshares-core
    git checkout <LATEST_RELEASE_TAG>
    git submodule update --init --recursive
    mkdir build
    cd build
    cmake -DCMAKE_BUILD_TYPE=Release ..
    make witness_node cli_wallet

阅读 [安装指南](http://dev.bitshares.works/en/master/development/installation.html#installation-guide)


<table style="width: 750px;"><tbody>
	<tr>
	<td bgcolor="LightBlue">注意</td>
	</tr>
	<tr>
	<td bgcolor="MintCream">在上述步骤中，将<LATEST_RELEASE_TAG>替换为最新的版本号</td>
	</tr>
</table>

编译完成后，产生的两个可执行程序是：

- build/programs/witness_node/witness_node
- build/programs/cli_wallet/cli_wallet

可以将上面两个程序拷贝到其他目录下或者其他服务器上执行。默认情况下，程序是在当前目录下。


<table style="width: 750px;"><tbody>
	<tr>
	<td bgcolor="LightBlue">注意</td>
	</tr>
	<tr>
	<td bgcolor="MintCream">当拷贝到其他服务器上执行时，当服务器操作系统或者其他软硬件欢迎不同的话，程序可能无法运行。</td>
	</tr>
</table>

如果你使用的是Ubuntu 14.04 LTS (64位)，在执行上述步骤前你需要编译和安装Boost库。

请注意当前只支持1.57.0到1.65.0版本的Boost库。

    sudo apt-get install cmake make libbz2-dev libdb++-dev libdb-dev libssl-dev openssl libreadline-dev autoconf libtool git autotools-dev build-essential g++ libbz2-dev libicu-dev python-dev doxygen
    
    wget -c 'http://sourceforge.net/projects/boost/files/boost/1.57.0/boost_1_57_0.tar.bz2/download' -O boost_1_57_0.tar.bz2
    tar xjf boost_1_57_0.tar.bz2
    cd boost_1_57_0
    ./bootstrap.sh
    sudo ./b2 install

也可以使用其他Linux发行版进行编译，这超出了本文所讨论的内容范围。

## 环境准备

为保证系统的正常运行，你需要确认下服务器系统时间是否正确。不准备的时间会带来一些问题，比如区块同步失败，转账失败等。

Ubuntu系统上建议使用以下命令安装NTP服务器：

    Sudo timedatectl set-ntp false
    Sudo apt-get install ntp

根据部署环境的不同，可能需要你修改默认的NTP服务器地址。

如果是Windows系统，设置系统同步时间。

## 同步数据

由于同时运行多个程序的必要性，Ubuntu系统中建议在screen或tmux上启动程序。

下面的一些描述主要是针对Ubuntu系统的，因此使用带有./的命令。对于Windows系统，通过命令行界面切换到程序的目录后，不需要./来执行程序。

### witness_node

可以使用./witness_node --help来查看该命令的参数

#### 初始操作

    ./witness_node -d witness_node_data_dir

按下Ctrl+C结束该命令

该命令会在当前目录下创建一个数据目录，witness_node_data_dir，其中包含区块链目录，用来存储数据以及配置文件 ( [config.ini](http://dev.bitshares.works/en/master/development/apps/node_config_example.html#bts-config-ini-eg) )。

对于交易所而言，建议对config.ini做部分修改

1. 可以关闭p2p的日志来减少磁盘存储的压力。找到filename=logs/p2p/p2p.log这一行，在行首加上#来注释掉这一行，或者将level=info below [logger.p2p]日志级别设置为error。
2. 同时修改下面这个代码块，从来保存控制台日志

    [logger.default]
    level=info
    appenders=stderr

改为
    
    [log.file_appender.default]
    filename=logs/default/default.log
    
    [logger.default]
    level=info
    appenders=stderr,default

之后控制台日志就可以24小时不间断地保存在witness_node_data_dir/logs/default/目录下。

3. 下面的参数可以减小运行所需的内存大小。原理是不保存BTS内置交易引擎的历史交易记录索引，因为这部分数据通常都用不到。

    history-per-size = 0

如果是 2.0.171105a 及以后版本，则需要设置这个参数：

    plugins = witness account_history

阅读更多：[减少节点内存使用]{http://dev.bitshares.works/en/master/supports_dev/nodes_memory_reduction.html#memory-nodes}

注意：

* config.ini 里默认 plugins 前有个“#”符号，需要删除；

* 默认的 plugins 配置是 “witness account_history market_history”，这里实际是去掉“market_history”；

* 如果 config.ini 里没找到该配置项，比如从老版本升级上来时不会更新已有配置文件，

* 可以在 config.ini 最前面添加一行（不要加在文件最后面），

* 也可以另外找个空目录生成一个 config.ini 文件再拷过来修改。

4.  以下参数表示每个账号保留多少条历史记录供查询，默认值是 1000。

对交易所来说，如果充值、提现记录较多，可考虑设置成一个较大的值，比如

    max-ops-per-account = 1000
 
改为

    max-ops-per-account = 1000000

则会保留一百万条数据。更早的数据会从内存中被删除而无法快速查询到（但仍然记录在链上）。

5. 以下两个参数会大量减少运行需要的内存，原理是不保存与交易所账户无关的历史数据索引。

    track-account = "1.2.12345"
    partial-operations = true


请将 12345 替换成你的账户数字 ID 。数字前的 “1.2.” 表示类型是账户。

如果需要监控多个账户，则使用多个 track-account 配置，如：
    
    track-account = "1.2.12345"
    track-account = "1.2.12346"
    partial-operations = true

目前存在一个 BUG ，配置多个 track-account 会导致上面的日志修改不生效。

绕过这个问题的方法，是不动 config.ini ，而是启动 witness_node 的时候，在命令行后面添加 –track-account 参数。比如：

    ./witness_node --track-account "\"1.2.12345\"" --track-account "\"1.2.12346\""

注意：

* 参数首尾双引号需要保留，所以使用 \ 进行转义。 Linux 下可以使用双引号外加一层单引号的方式，则不需要转义。

* 如果需要增加、修改、删除追踪账号，修改后，需要重建索引才能生效。方法是按 Ctrl + C 结束程序，然后加 –replay-blockchain 参数重新启动。

使用ctrl+c命令终止程序并添加--replay-blockchain参数重新运行程序，比如：
    
    ./witness_node -d witness_node_data_dir --track-account "\"1.2.12345\"" --track-account "\"1.2.12346\"" --replay-blockchain


阅读更多：[减少节点内存使用]{http://dev.bitshares.works/en/master/supports_dev/nodes_memory_reduction.html#memory-nodes}

#### 重新执行

再次启动 witness_node ，开始同步数据。根据网络条件、服务器硬件条件不同，初次同步可能需要花几个小时到几天时间。

    ./witness_node -d witness_node_data_dir --rpc-endpoint 127.0.0.1:8090
    --track-account "\"1.2.12345\""
    --track-account "\"1.2.12346\""
    --partial-operations true
    --max-ops-per-account 1000000
    --replay-blockchain

上述命令中，使用 –rpc-endpoint 开启节点 API 服务，这样就可以使用 cli_wallet 和其他程序连接使用。

<table style="width: 750px;"><tbody>
	<tr>
	<td bgcolor="LightBlue">注意</td>
	</tr>
	<tr>
	<td bgcolor="MintCream">以后再需要重新启动 witness_node 时，一般不要加 –replay-blockchain 参数，否则启动会很慢。</td>
	</tr>
</table>

### 运行一个 cli_wallet 用于提现操作

    ./cli_wallet -w wallet_for_withdrawal.json -s ws://127.0.0.1:8090 -H 127.0.0.1:8091

上述命令使用 -w 参数指定钱包文件， -s 参数连接到 witness_node ， -H 参数开启钱包 API 服务，监听端口 8091。

注意：

- 可以使用./cli_wallet --help查看命令参数
- cli_wallet 与 witness_node 间通信的数据不包含私密数据，一般不需加密，也不需要对节点 RPC 端口作刻意保护（加一层保护也未尝不可）。

- 但 cli_wallet 与充提程序间的通信是明文，可能需要包含密码，如果部署为多机架构，需要注意加密，可采用 SSH 隧道的方式。

- 此外， cli_wallet 处于解锁状态时，通过 RPC 端口可以转移钱包内账户资金，需要注意防止非授权访问，强烈不建议钱包 RPC 直接开放公网访问。

- 为 cli_wallet 的 RPC 配置证书或者密码的做法，本人没有研究过，故此不作描述。

执行成功会显示：

    new >>>

首先需要为钱包文件设置一个密码

    new >>> set_password my_password_1234

执行成功会显示：

    locked >>>

然后解锁钱包

    locked >>> unlock my_password_1234

解锁成功会显示：

    unlocked >>>

使用 info 命令可以查看当前同步情况

    unlocked >>> info
    info
    {
      "head_block_num": 17249870,
      "head_block_id": "0107364e2bf1c4ed1331ece4ad7824271e563fbb",
      "head_block_age": "23 seconds old",
      "next_maintenance_time": "31 minutes in the future",
      "chain_id": "4018d7844c78f6a6c41c6a552b898022310fc5dec06da467ee7905a8dad512c8",
      "participation": "96.87500000000000000",
      ...
    }

### 运行另一个 cli_wallet 用于处理充值

    ./cli_wallet -w wallet_for_deposit.json -s ws://127.0.0.1:8090 -H 127.0.0.1:8093

这个 cli_wallet 也开启钱包 API 服务，监听端口 8093

请参考前面的章节设置密码及解锁。

# 账户设置

考虑到安全性，可以使用两个账号分别处理充值和提现，这里假设 deposit-account 用于充值，withdrawal-account 用于提现。

### 修改充值账户的备注密钥

在上述任何一个 cli_wallet 中执行 suggest_brain_key ，会得到一对密钥，示例如下：

    unlocked >>> suggest_brain_key
    suggest_brain_key
    {
      "brain_priv_key": ".....",
      "wif_priv_key": "5JxyJx2KyDmAx5kpkMthWEpqGjzpwtGtEJigSMz5XE1AtrQaZXu",
      "pub_key": "BTS69uKRvM8dAPn8En4SCi2nMTHKXt1rWrohFbwaPwv2rAbT3XFzf"
    }

在轻钱包里，账户权限页面，将备注密钥修改为上述结果中的 pub_key 。

注意：

1. 修改过后请注意备份轻钱包，否则轻钱包里可能无法解密修改前的备注。

2. 修改过后，如果仍需要使用轻钱包进行带备注的转账、或者读取新的转入/转出转账备注，
	- 则需要将上述 wif_priv_key 导入到轻钱包
	- 导入后可以做个新的备份。

3. 这个方法也可以用来修改账户的活跃权限密钥和账户权限密钥，有需要时可以使用。

### 将充值账户的备注密钥导入到连接到负责充值的 cli_wallet

如果钱包已锁定，需要先用 unlock 命令解锁。

这里需用到上述 suggest_brain_key 结果中的 wif_priv_key ：

    unlocked >>> import_key deposit-account 5JxyJx2KyDmAx5kpkMthWEpqGjzpwtGtEJigSMz5XE1AtrQaZXu

导入时 cli_wallet 会自动生成一个或者两个备份文件，可删除。

这时可按 Ctrl + D 退出钱包，对钱包文件 wallet_for_deposit.json 进行备份，然后重新启动 cli_wallet 。

退出时会报个错，不用管它。

如果编译时没有引入 readline 库，则需要用 Ctrl + C 退出

由于没有导入活跃权限密钥，负责处理充值的 cli_wallet 无法动用充值账户的资金，只能查看历史记录。

### 从轻钱包中取得提现账户的活跃权限密钥

参考：[用户指南-权限](http://dev.bitshares.works/en/master/bts_guide/accounts/bts_permissions.html#acc-permission)

### 将提现账户的活跃权限密钥导入到负责提现的 cli_wallet

    unlocked >>> import_key withdrawal-account 5xxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx

同样可以对钱包文件作个备份

<table style="width: 750px;"><tbody>
	<tr>
	<td bgcolor="LightBlue">注意</td>
	</tr>
	<tr>
	<td bgcolor="MintCream">检查一下提现账户的活跃权限密钥和备注密钥是否一样，如果不一样，则需要将备注密钥也导入，否则无法处理带备注的提现。</td>
	</tr>
</table>

导入命令仍然是：

    Unlocked >>> import_key withdrawal-account 5xxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx

## 钱包命令

cli_wallet 里，

* 使用 help 命令可以列出命令清单及参数

* 如果编译时有 doxygen ，使用 gethelp 命令可以获取具体命令的参数说明及示例，如

    unlocked >>> gethelp get_account

## 钱包API

钱包开启了 HTTP RPC  方式的 API 服务时，可以通过 HTTP RPC  方式调用**所有的**钱包命令，效果与在钱包里输入命令相同。

示例：
    
    curl -d '{"jsonrpc": "2.0", "method": "get_block", "params": [1], "id": 1}' http://127.0.0.1:8093/rpc

即：method 传入命令名，params 数组传入参数清单。

返回：

    {"id":1,
       "result":{
      "previous":"0000000000000000000000000000000000000000",
      "timestamp":"2015-10-13T14:12:24",
      "witness":"1.6.8",
      "transaction_merkle_root": "0000000000000000000000000000000000000000",
      "extensions": [],
      "witness_signature":"1f53542bb60f1f7a653bac70d6b1613e73b9adc952031e30e591e601dd60d493ba5c9a832e155ff0c40ea1dd53512e9f93bf65a8191497ea67d701bc2502f93af7",
      "transactions": [],
      "block_id": "00000001b656820f72f6b28cda811778632d4998",
      "signing_key": "BTS6ZQEFsPmG6jWspNDdZHkehNmPpG7gkSHkphmRZQWaJ2LrcaVSi",
      "transaction_ids": []
      }
    }


如果执行成功，结果会有 result ，否则会有 error 。

注意：

- HTTP RPC 请求 URI 为 /rpc 。

- 在钱包里输入命令，返回结果是经过美化的；使用 HTTP RPC 请求时，返回的是 json 格式的原始数据。就原始数据而言，需要注意几点：
- 金额是 {“amount”:467116432,”asset_id”:”1.3.0″} 格式，
	- 其中* asset_id 可以通过 get_asset 命令查到具体名称， BTS 的 asset_id 是 1.3.0 ，其他资产有其他 id。

	- amount 是数量去掉小数点之后的值，比如 BTS 是 5 位小数，上面例子中实际是 4671.16432 BTS。

- 账户是 1.2.xxxxx 的格式，可以通过 get_account 获取账户信息。

- 操作类型（op）是数值格式，比如 0 表示转账操作。

##  处理充值

### 获取当前的“无法回退区块”块号

与比特币等采用确认数来从概率上降低交易回退的可能性有所不同， BTS 里可采用**无法回退区块**块号来判断交易是否可能回退。

**无法回退**区块及更早区块里面的交易，可以保证不会发生回退。

在 cli_wallet 里使用命令 get_dynamic_global_properties 来获取无法回退的块号。如：

    
    get_dynamic_global_properties
    {
      "id": "2.1.0",
      "head_block_number": 21955727,
      ...
      "last_irreversible_block_num": 21955709
    }

其中， head_block_number 为最新区块号， last_irreversible_block_num 为无法回退区块号。

### 查询充值账户历史

使用 get_relative_account_history 命令来查询充值账户历史，检测是否有新的充值。如：

比如：
    
    unlocked >>> get_relative_account_history deposit-account 1 100 100
    
    unlocked >>> get_relative_account_history deposit-account 101 100 200
    
    curl -d '{"jsonrpc": "2.0", "method": "get_relative_account_history", "params": ["deposit-account",1,100,100], "id": 1}' http://127.0.0.1: 8093/rpc

四个参数分别为：账户名，最小编号，最大返回数量，最大编号。编号从 1 开始。

注意：某个版本的 cli_wallet 最大返回数量超过 100 时有个bug，导致结果不准，使用时请避免 limit 超过 100 。

返回结果是一个数组，按时间倒序排序，即最新的记录排在最前面。

* 如果没有新充值，则数组长度为 0 。

* 如果有新的记录，其中第N条数据为 result[N]，格式可能为：

{
"memo":"",
"description":"Transfer 1 BTS from a to b -- Unlock wallet to see memo. (Fee: 0.22941 BTS)",
"op":{
"id":"1.11.1234567",
"op":[
 0,
 {
"fee":{
 "amount": 22941,
 "asset_id":"1.3.0"
},
"from":"1.2.12345",
"to":"1.2.45678",
"amount":{
 "amount": 100000,
 "asset_id":"1.3.0"
},
"memo":{
 "from":"BTS7NLcZJzqq3mvKfcqoN52ainajDckyMp5SYRgzicfbHD6u587ib",
 "to":"BTS7SakKqZ8HamkTr7FdPn9qYxYmtSh2QzFNn49CiFAkdFAvQVMg6",
 "nonce": "5333758904325274680",
 "message": "0b809fa8169453422343434366514a153981ea"
},
"extensions":[
]
 }
],
"result":[
 0,
 {
 }
],
"block_num":1234567,
"trx_in_block":7,
"op_in_trx":0,
"virtual_op":1234
}
}

可见，结果中并没有显式包含每条记录的编号，需要程序自行推算、记录。一般将该数组顺序颠倒，然后逐一处理比较合适。

首先要判断该交易所在区块是否已经无法回退。

- 取 result[N][“op”][“block_num”] 与 last_irreversible_block_num 作比较，如果可以不可回退则继续处理，可以回退则先跳过不处理。


<table style="width: 750px;"><tbody>
	<tr>
	<td bgcolor="LightBlue">注意</td>
	</tr>
	<tr>
	<td bgcolor="MintCream">交易没有进块时，仍然可能在 get_relative_account_history 中出现，并且所在块号会一直改变，难以判断状态。</td>
	</tr>
</table>

所以请使用 last_irreversible_block_num 来判断。

- result[N][“op”][“op”] 是数组格式，取数组里第一个元素 result[N][“op”][“op”][0] ，如果是 0 ，则表示转账；

- 则可以取第二个元素中 “to” 字段，即 result[N][“op”][“op”][1][“to”] ，判断它是否与 deposit-account 的 ID 相同，来判断是否转入；

- 如果是，则取第二个字段中 “amount” 字段里 “asset_id” 字段 result[N][“op”][“op”][1][“amount”][“asset_id”] 判断是否正确资产类型， 
- 然后取 “amount” 里面的 “amount” ，即 result[N][“op”][“op”][1][“amount”][“amount”] ，加上小数点位数，得出充值金额；

- 取最外层的 “memo” 字段，即 result[N][“memo”] ，得出用户在交易所的 ID ，进行入账。

- result[N][“op”][“id”] 是这笔转账的唯一 ID ，可以记录备查。

- 同时，推荐将结果中 block_num, trx_in_block, op_in_trx 几个数据也记录下来，含义分别是 块号、块中第几个交易、交易中第几个操作。

另外，由于他方转账时，可能只记录交易ID （哈希值），或者交易签名，而不记录操作ID或者块号，为了方便检查问题，建议在充值检测时，记录操作对应的交易 ID 和交易签名，方法如下：

根据上述 block_num ，调用 get_block 命令获取块内容，如
    
    unlocked >>> get_block 16000000
    
    curl -d ‘{“jsonrpc”: “2.0”, “method”: “get_block”, “params”: [160000], “id”: 1}’ http://127.0.0.1:8093/rpc
    
设结果中块内容为 result ，根据上述 trx_in_block ，

- 取 result[“transaction_ids”][trx_in_block] ，即为对应的交易 ID；

- 取 result[“transactions”][trx_in_block][“signatures”] ，即为交易签名，是个数组，因为多重签名账户转账可能包含多个签名。

注意： 

1. 钱包必须先解锁才能解密备注。
2. 如果检测到有充值的备注不正确，或者资产类型不正确，注意不要简单退回，因为可能是从其他交易所转来的，退回后对方处理起来也会很麻烦。

3. 一个块里很可能有多笔充值，结果的 block_num 相同，甚至可能 trx_in_block 和 op_in_trx 也相同，但 virtual_op 不同，需注意处理。

 - 可以肯定 blocknum + trx_in_block + op_in_trx + vitrual_op 的组合是唯一的。

 -	这里还要注意个问题， virtual_op 的数据目前有个 BUG，如果不是每次重启参数都一样并且都 replay ，重新查历史数据，会发现这个值会不一致

4. 由于存在“提议”功能，可以延期执行，用 get_block 然后用 trx_in_block 定位时可能取不到对应交易，或者取到的交易与充值操作不对应。

	- 延期执行功能目前很少有人用，但理论上存在，请注意错误处理。

## 处理提现

### 网络状态检查

为了安全起见，只有当 witness_node 网络正常时，才处理提现。

在负责提现的 cli_wallet 中使用 info 命令检查网络状态

    unlocked >>> info
    info
    {
      "head_block_num": 17249870,
      "head_block_id": "0107364e2bf1c4ed1331ece4ad7824271e563fbb",
      "head_block_age": "23 seconds old",
      "next_maintenance_time": "31 minutes in the future",
      "chain_id": "4018d7844c78f6a6c41c6a552b898022310fc5dec06da467ee7905a8dad512c8",
      "participation": "96.87500000000000000",
      ...
    }


需检查的字段有：

* head_block_age 最好是在 1 分钟以内。

* participation 最好在 80 以上，表示 witness_node 所连网络有 80% 的出块节点正常工作。

另外，网络正常时， last_irreversible_block_num 与 head_block_num 之间的差值不会太大（一般 30 以内）;这个可以作为参考。

### 提现账户余额检查

使用 list_account_balances 命令检查提现账户余额是否足够（注意资产类型、并且算上手续费）

    unlocked >>> list_account_balances withdrawal-account

注意：

1. 注意资产类型。

2. 注意加上手续费。因为备注是按长度收费，所以带备注时手续费会比不带备注时高一些。

### 账户名有效性检查

使用 get_account_id 命令可以检查客户输入的提现目的账号是否有效

    locked >>> get_account_id test-123
    get_account_id test-123
    "1.2.96698"
    
    locked >>> get_account_id test-124
    get_account_id test-124
    10 assert_exception: Assert Exception
    rec && rec->name == account_name_or_id:
    {}
    th_a  wallet.cpp:597 get_account

### 发送提现

使用 transfer2 命令发送提现交易。比如：

    unlocked >>> transfer2 withdrawal-account to-account 100 BTS "some memo"

参数分别是：源账户名，目的账户名，金额，币种，备注该命令会签名并广播交易，然后返回一个数组，第一个元素是交易 id ，第二个元素是详细交易内容。

注意：

1. 如果币种是 BTS ，金额小数位数最多 5 位。如果是其他资产，通过 get_asset 命令可以查看资产的小数位数，”precision”字段。
2. 也可以使用 transfer 命令，但是这样不会直接返回交易 ID ，而是需要调用其他 API 来计算出来，所以不推荐。
3. 备注通常用 UTF-8 编码。
4. 建议记录相关数据备查，比如交易 id 、 json 格式的详细交易内容等。

### 提现结果检查

使用 get_relative_account_history 命令获取 withdrawal-account 的提现历史，参照充值处理章节，如果发现有新的记录。

交易所在块号早于last_irreversible_block_num ，则表示交易已经进块并且不可回退。

注意：

交易没有进块时，仍然可能在 get_relative_account_history 中出现，并且所在块号会一直改变，难以判断状态。所以请使用 last_irreversible_block_num 来判断。

根据该条记录的 block_num 字段，使用 get_block 命令查询详情：

    unlocked >>> get_block 12345

返回结果中， transaction_ids 字段数据里应该包含前面的交易 id 。

建议记录上述 get_relative_account_history 结果中的 id (1.11.x), block_num, trx_in_block 备查。

###  关于失败重发

在有些情况下可能交易发送后，没有及时被打包进入区块。

与比特币不同， BTS 的交易里面有个超时时间字段。

使用 cli_wallet 签名广播交易时，该字段值默认是本机系统时间加 2 分钟。

本机交易特别多的时候，超时时间会加长。

如果在网络时间到达该超时时间之后，交易仍然没有被打包进块，则该交易会被所有网络节点丢弃，不再有可能被打包。

因此，如果出现交易广播了但没有在账户历史中出现，先检查本机系统时间是否滞后。

* 如果 last_irreversible_block_num 对应的块时间已经过了交易的超时时间，那么重发是安全的。

* 如果交易已经在历史中出现，则检查交易所在块号是否已经固定，而不是一直随着最新块号更新。

* 如果交易所在块号一直更新，表示交易还没被打包进块，需继续耐心等待被打包或超时。

* 如果块号已固定，随着时间推移，网络正常时， last_irreversible_block_num 很快就会超过该块号。

* 如果 head_block_num 一直增长，而 last_irreversible_block_num 不增长，
	* 则很可能 witness_node 进入了一个较短分叉链，或者网络出现问题，导致交易无法完全确认。
	*  这种情况下，请检查 witness_node 是否有新版本需要升级，或联系开发团队。

* 如果重发仍然无法被打包，则可能遇到网络异常或者拥堵，这种情况比较少见，请联系开发团队。

## 其他

* cli_wallet 有个参数 –daemon ，使用此参数启动后，会在后台运行

* 需要关闭 witness_node 时，按一次 Ctrl C，然后等待程序自己退出。

	* 正常退出后，重新启动时，不需重建索引，启动会比较快。

	* 正常退出后，可以对数据目录 witness_node_data_dir 打包备份，需要时可直接恢复使用。

	* 如果异常退出，则重新启动时，很可能需要重建索引，启动比较慢。

* 如果 witness_node 出现异常，一般先尝试重启，如果不行则可尝试带 –replay-blockchain 参数重启，即手工触发重建索引。

	* 如果没有解决，则使用备份恢复。

	* 如果没有备份，则重新同步，可能耗时较长。

* 多重签名： BTS 原生支持账户级多重签名，并且有提案-批准的机制，可以在线发起多签请求，然后确认完成多签交易，具体参考相关教程。

* 硬件钱包： 暂无支持

* 冷存储： 可以实现，步骤有些复杂，示例：

	* 在离线机器，启动 witness_node 及 cli_wallet ，使用 suggest_brain_key 命令离线生成密钥对。

	* 然后用轻钱包将账户密钥修改为上述密钥，则账户进入冷存状态。
	* 需要动用冷存账户时：

		* 可以用暂时变热的方式，即将私钥导入轻钱包使用，用完后再换成新密钥。

		* 纯冷模式也可实现，但当前 cli_wallet 支持不好，有需要的请单独联系。


## 相关信息

* 图文教程 [http://jc.btsabc.org/]( http://jc.btsabc.org/)

* 自建节点教程  [http://btsabc.org/article-477-1.html]( http://btsabc.org/article-477-1.html)

* 获取账户私钥[ http://btsabc.org/article-761-1.html](http://btsabc.org/article-761-1.html)

* 英文对接文档[ http://docs.bitshares.org/integration/exchanges/step-by-step.html](http://docs.bitshares.org/integration/exchanges/step-by-step.html)

* 英文 API 文档 [http://docs.bitshares.org/api/index.html](http://docs.bitshares.org/api/index.html)
* 贡献者：@abit


