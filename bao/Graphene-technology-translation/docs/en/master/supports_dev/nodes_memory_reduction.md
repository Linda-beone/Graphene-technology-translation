**原文链接**：[http://dev.bitshares.works/en/master/supports_dev/nodes_memory_reduction.html#memory-nodes](http://dev.bitshares.works/en/master/supports_dev/nodes_memory_reduction.html#memory-nodes)
 **译者**：[https://github.com/b-t-s-1](b-t-s-1)
**校对者**：


# 减少节点内存


**目录**

#### Four New Options to Reduce RAM
  * ##### programs/witness_node/witness_node –帮助
* ##### Options for plugin accout_history
* ##### –plugins
* ##### –track-account
* ##### –max-ops-per-account
* ##### –partial-operations
#### Combinations
* ##### Special Notes

-----

## 减少RAM的四种新选择

比特股区块链很大，石墨烯技术将链中重放的所有数据存储到RAM中。目前（2017-09-04）一个完整的节点需要40GB的RAM。大多数情况下，由于机器需要可用的RAM，所以加载所有内容的完整节点不需要而且昂贵。使用``witness_node``可执行选项可以显着减少RAM的使用。

以下是可用于减少RAM的4个新选项：

### programs/witness\_node/witness\_node --帮助

  - \---plugins arg：Space-separated list of plugins to activate

### 插件accout_history的选项

  - \--track-account arg：用于跟踪历史记录的帐户ID（可以指定多次） 

  - \--partial-operations arg：仅保留内存中与帐户历史记录跟踪相关的操作

  - \--max-ops-per-account arg：每个帐户的最大操作数将保留在内存中

-----

### \---插件

允许只运行所需的插件。

默认情况下，如果命令行启动时没有插件参数，则节点将以加载的默认插件开始，这些插件是：

  - witness
  - account\_history
  - market\_history

如果您只是在验证块之后，你可以仅使用见证插件启动节点，如下所示：

    programs/witness_node/witness_node --data-dir data/my-blockprod 
                                       --rpc-endpoint "127.0.0.1:8090" 
                                       --plugins "witness"

### \--track-account

仅允许跟踪所选帐户的历史记录。

假设你有一个应用程序钱包或只对1个帐户或几个帐户的历史记录感兴趣，则无需在网络其余部分的大量帐户历史记录中消耗大量内存。你仍然可以像往常一样进行转帐和一切操作，只是帐户历史记录无法使用。

为了仅跟踪一个帐户的历史记录，您可以将节点作为:

    programs/witness_node/witness_node --data-dir data/my-blockprod 
                                       --rpc-endpoint "127.0.0.1:8090" 
                                       --track-account "\"1.2.282\""

追踪多个帐户:

    programs/witness_node/witness_node --data-dir data/my-blockprod 
                                       --rpc-endpoint "127.0.0.1:8090" 
                                       --track-account "\"1.2.282\"" "\"1.2.24484\"" "\"1.2.2058\""

### \--max-ops-per-account

节点将为网络中的每个帐户存储多个操作。
我们发现大部分区块链大小实际上是系统中自动化僵尸账户所做的数百万订单的历史记录。 去中心化交易所流动性非常需要做这些账户的市场，但对大多数节点来说价值不大。

此参数允许从所有账户可以删除最早的操作历史记录。余额和其他不会受影响，请记住这只是删除历史记录。

通过将每个帐户的操作数量限制为1000，区块链大小的减少超乎想象，并允许你在减少内存的机器中运行节点，可以使用此选项的组合以4-5 gigs运行。

通过以以下命令，将节点在区块链中保存的每个帐户的操作数减少到100：

    programs/witness_node/witness_node --data-dir data/my-blockprod 
                                       --rpc-endpoint "127.0.0.1:8090" 
                                       --max-ops-per-account 100

# \--partial-operations

删除操作历史对象。

比特股内核将操作存储在两个不同的对象中，即2.9.X和1.11.X.它们并不完全相同但是如果你用`--track-account`或`--max-ops-per-account`删除操作，你也可以添加这个选项来减少内存使用量：

    programs/witness_node/witness_node --data-dir data/my-blockprod 
                                       --rpc-endpoint "127.0.0.1:8090" 
                                       --max-ops-per-account 100 
                                       --partial-operations true

-----

## 组合

有意义的组合都是有效的，可以用来满足你的需求。

我个人启动我的节点，每个帐户1000个操作和部分操作:

    programs/witness_node/witness_node --data-dir data/my-blockprod 
                                       --rpc-endpoint "127.0.0.1:8090" 
                                       --max-ops-per-account 1000 
                                       --partial-operations true

这将允许我运行少于5 gigs 的节点（4.820492G）:

    ffffffffff600000      4K r-x--   [ anon ]
     total          4820492K
    root@alfredo:~# pmap 28685

# 特别说明

  - 一个新的选项可能是``untrack-account``。我们可以识别出biggers并运行一个带有bots帐户历史记录的节点。

-----

贡献者: @oxarbitrage
