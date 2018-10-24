# debug\_node : what-if test tool

## 介绍

debug_node是一个工具，允许开发人员使用区块链中的状态运行许多有趣的“假设”测试。就像“如果我为下一次硬分叉时间的到来产生足够的区块来会发生什么？”或者“如果这个帐户（我没有私钥）进行此交易会怎么样？”

## 建立

确保您已构建正确的目标:

    $ make get_dev_key debug_node cli_wallet witness_node

使用 ``get_dev_key`` 生成密钥对:

    $ programs/genesis_util/get_dev_key "" nathan
    [{"private_key":"5KQwrPbwdL6PhXujxW37FSSQZ1JiwsST4cqQzDeyXtP79zkvFD3","public_key":"BTS6MRyAjQq8ud7hVNYcfnVPJqcVpscN5So8BhtHuGYqET5GDW5CV","address":"BTSFAbAx7yuxt725qSZvfwWqkdCwp9ZnUama"}]

在`block_db`目录中获取区块链的副本: $
programs/witness\_node/witness\_node --data-dir data/mydatadir \# ...
wait for chain to sync ^C $ cp -Rp
data/mydatadir/blockchain/database/block\_num\_to\_block ./block\_db

使用以下命令设置一个新的datadir :ref: ``config.ini`` settings:

    # setup API endpoint
    rpc-endpoint = 127.0.0.1:8090
    # setting this to empty effectively disables the p2p network
    seed-nodes = []
    # set apiaccess.json so we can set up
    api-access = "data/debug_datadir/api-access.json"

  - 例如 `configuration file parameters <bts-config-ini-eg>`

然后设置``data/debug_datadir/api-access.json``以允许访问调试API，如下所示:

``` sourceCode json
{
   "permission_map" :
   [
      [
         "bytemaster",
         {
            "password_hash_b64" : "9e9GF7ooXVb9k4BoSfNIPTelXeGOZ5DrgOYMj94elaY=",
            "password_salt_b64" : "INDdM6iCi/8=",
            "allowed_apis" : ["database_api", "network_broadcast_api", "history_api", "network_node_api", "debug_api"]
         }
      ],
      [
         "*",
         {
            "password_hash_b64" : "*",
            "password_salt_b64" : "*",
            "allowed_apis" : ["database_api", "network_broadcast_api", "history_api"]
         }
      ]
   ]
}
```

有关``api-access.json``格式的更多详细信息，请参阅.

设置完成后，对新准备的datadir运行``debug_node`` :

    programs/debug_node/debug_node --data-dir data/debug_datadir

运行`cli_wallet`连接到`debug_node`端口，使用用户名和密码访问新的`debug_api`（以及另外一个钱包文件）:

    programs/cli_wallet/cli_wallet -s 127.0.0.1:8090 -w debug.wallet -u bytemaster -p supersecret

# 用法示例

从datadir加载一些块:

    dbg_push_blocks block_db 20000

注意，当推入大量块时，有时`cli_wallet`挂起，你必须按``Ctrl + C``并重新启动它（让`debug_node`继续运行）。

使用我们自己的私钥生成（假）块:

    dbg_generate_blocks 5KQwrPbwdL6PhXujxW37FSSQZ1JiwsST4cqQzDeyXtP79zkvFD3 1000

更新`angel`帐户由我们自己的私钥控制并生成（假）转移:

    dbg_update_object {"_action":"update", "id":"1.2.1090", "active":{"weight_threshold":1,"key_auths":[["BTS6MRyAjQq8ud7hVNYcfnVPJqcVpscN5So8BhtHuGYqET5GDW5CV",1]]}}
    import_key angel 5KQwrPbwdL6PhXujxW37FSSQZ1JiwsST4cqQzDeyXtP79zkvFD3
    transfer angel init0 999999 BTS "" true

## 如何工作

这些命令通过从主链创建diff(s)来实现，这些diff(s)应用于指定块高度的本地链。它可以让你轻松查看从真实链中分叉的调试链中的“假设”场景，例如： “如果我们用截止到今天的所有区块数据生成更多的块，一直到将来的硬分叉时间到来，链条是否会停留？我可以在硬分叉后在钱包中进行X，Y和Z交易吗？”任何连接到此节点的人都会看到相同的调试链，因此你可以使用`cli_wallet`进行更改，并查看它们是否存在于其他`cli_wallet`实例（或GUI钱包或API脚本）中。

## 局限性

主要限制是:

  - 没有diffs的导出格式，因此你不能真正[1]将多个debug_node相互连接。
  - 一旦你的链上产生了伪造的块或交易，你就不能真正[1]将块或交易从主网络传输到你的链。

[1]它理论上应该是可能的，但它是非一般的，完全未经测试。


