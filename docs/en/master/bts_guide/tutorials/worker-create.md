  **原文链接**：[http://dev.bitshares.works/en/master/bts_guide/tutorials/worker-create.html#id1](http://dev.bitshares.works/en/master/bts_guide/tutorials/worker-create.html#id1)
 
 **译者**：[https://github.com/b-t-s-1](b-t-s-1)
 
 **校对者**： 
  
***

# 如何创建工人

目录

* 如何创建一个工人
  - 使用UI
  - 使用命令
* 如何查看链上的提案
  - 使用UI
  - 使用命令
* 如何为工人投票
  - 使用UI
  - 使用命令
* 工人如何获得报酬
* 技术细节

***

## 如何创建一个工人

### 使用UI

你可以使用BitShares UI形式创建工人提案。 转到投票页面。 有三个标签：“见证人”，“委员会”和“工人”。 在“工人”选项卡中，你将找到*创建一个新的工人*的按钮。
  
![](http://dev.bitshares.works/en/master/_static/imgs/ui-worker-create3.png)

**工人提案表**  
  
![](http://dev.bitshares.works/en/master/_static/imgs/ui-worker-create.png)

### 使用命令

当前用cli_wallet使用以下命令语法创建工作程序：

```
>>> create_worker owner_account work_begin_date work_end_date daily_pay name url worker_settings broadcast
```  

<table>
        <tr>
            <td>参数</td>
            <td>示例值</td>
        </tr>
        <tr>
            <td>所有者帐户</td>
            <td>“工人名字”</td>
        </tr>
        <tr>
            <td>工作开始日期</td>
            <td>“2015-10-28T00:00:00”</td>
        </tr>
        <tr>
            <td>工作结束日期</td>
            <td>“2015-10-29T00:00:00”</td>
        </tr>
       <tr>
            <td>每日工资</td>
            <td>100000</td>
        </tr>
       <tr>
            <td>名字</th>
            <td>“描述”</td>
        </tr>
       <tr>
            <td>url</th>
            <td>“http://URL”</td>
        </tr>
       <tr>
            <td>工人设置</th>
            <td>{“类型” : “归属”, “支付归属期” : 1}</td>
        </tr>
       <tr>
            <td>广播</td>
            <td>失败</td>
        </tr>
    </table>


**示例**

*所有者账户*正在创建一个从10月28日开始的一天工作人员，并将获得1BTS/天（1天归属，1 BTS = 100,000'Satoshi'）以制作Android应用程序。 第一个命令不会广播，这只会检查：

```
>>> create_worker "worker-name" "2015-10-28T00:00:00" "2015-10-29T00:00:00" 100000 "Description" "http://URL" {"type" : "vesting", "pay_vesting_period_days" : 1} false
```

该网址应指向描述你的提案的内容。 我们建议您回答以下问题：

* 你会用这笔资金做什么？
* 你什么时候完成？
* 多少钱？

变量*type*可以是

* `退款`将你的回报退回到池中以用于未来的项目，	
* `归属`支付你自己的费用，或者
* `销毁`来销毁你的工资，从而减少股票供应，相当于回购公司股票

变量`pay_vesting_period_days`是你为归属设置的整数天数。 有些人不希望工人立即提现和出售大量的BTS，因为它给BTS带来了卖压。 此外，如果你需要归属，你就有“皮肤在游戏中”，从而激励提高BTS价值。 支付是每天支付（不是小时或维护期），单位为1/100,000 BTS（BTS的精度）

要**实际**生成一个工人提案，将最后的*false*替换为*true*。

***

## 如何查看链上的提案

### 通过使用UI

你可以使用BitShares UI形式创建工人提案。 转到投票页面。 有三个标签：“见证人”，“委员会”和“工人”。 在“工人”选项卡中，你将找到所有工人提案列表。
  
![](http://dev.bitshares.works/en/master/_static/imgs/ui-voting-worker.png)

### 通过使用该命令

你还可以检查所有对象1.4.*:

```
>>> get_object 1.14.4
get_object 1.14.4
[{
    "id": "1.14.4",
    "worker_account": "1.2.22517",
    "work_begin_date": "2015-10-21T11:00:00",
    "work_end_date": "2015-11-21T11:00:00",
    "daily_pay": 1000000000,
    "worker": [
      1,{
        "balance": "1.13.235"
      }
    ],
    "vote_for": "2:73",
    "vote_against": "2:74",
    "total_votes_for": "14632377015617",
    "total_votes_against": 0,
    "name": "bitasset-fund-pool",
    "url": "https://bitsharestalk.org/index.php/topic,19317.0.html"
  }
]
```

***

## 如何为工人投票

### 通过使用UI

投票很重要。 你可以用UI形式投票。 它非常易于使用。 转到投票页面。 有三个标签：“见证人”，“委员会”和“工人”。 在“工人”选项卡中，选中“切换投票”复选框并保存。 你可能会被要求登录。 确认交易。

```
.. image:: ../../../../_static/imgs/ui-voting-worker-2.png
```

>

| alt: | BitShares |
| width: | 600px |
| align: | center |

### 通过使用命令

你可以使用CLI投票：

```
>>> update_worker_votes your-account {"vote_for":["proposal-id"]} true
```

示例:

```
>>> update_worker_votes "awesomebitsharer" {"vote_for":["1.4.0"]} true
```

你也可以投票反对或弃权（删除你的赞成票或反对票）：

```
>>> update_worker_votes your-account {"vote_against":["proposal-id"]} true
>>> update_worker_votes your-account {"vote_abstain":["proposal-id"]} true
```

***

## 工人如何获得报酬

每小时处理工人预算，工人按照赞成票数减去反对票数的完整顺序支付。 获得报酬的最后一名工人将获得剩余的费用，因此可能会收到部分付款。 可以通过检查最新的预算对象2.13来估算每日预算。*例如：

```
>>> get_object 2.13.361
get_object 2.13.361
[{
    "id": "2.13.361",
    "time": "2015-10-28T15:00:00",
    "record": {
      "time_since_last_budget": 3600,
      "from_initial_reserve": "106736452914941",
      "from_accumulated_fees": 15824269,
      "from_unused_witness_budget": 2250000,
      "requested_witness_budget": 180000000,
      "total_budget": 1520913100,
      "witness_budget": 180000000,
      "worker_budget": 1340913100,
      "leftover_worker_funds": 0,
      "supply_delta": 1502838831
    }
  }
]
```

所以每日预算是：

```
worker_budget*24 = 1340913100 * 24 = 32181914400 (or 321,8191.44 BTS)
```

目前最大每日工人工资为500k BTS，可以使用cli_wallet中的`get_global_properties`命令找到。

***

## 技术细节
每秒,:

```
[ 17/(2^32) * 储备基金 ]
```

分配给见证人和工人。 储备基金是可用的最大BTS数量减去目前流通的BTS数量[来源]
([https://github.com/cryptonomex/graphene/blob/f85dec1c23f6bf9259ad9f15311b2e4aac4f9d44/libraries/chain/include/graphene/chain/config.hpp](https://github.com/cryptonomex/graphene/blob/f85dec1c23f6bf9259ad9f15311b2e4aac4f9d44/libraries/chain/include/graphene/chain/config.hpp)))

每小时总可用储备金是通过查找可分配的BTS数量以及在下一个维护间隔期间将多少BTS返回储备金（即“销毁”）来计算的。

首先找出尚未分发的BTS数量：

```
>>> from_initial_reserve = max_supply - current supply of BTS
```

max_supply可以通过以下方式获得：

```
>>> get_object 1.3.0
```

current_supply按照如下给出：

```
>>> get_object 2.3.0
```

通过添加剩余的累计费用和见证人预算来修改它（即，每个区块的预算为1.5 BTS，因此在此维护周期中，预算剩余为1.5 BTS（维护期间剩余的区块数+见证人丢的区块）），（在维护时永久加入“储备基金”）：

```
updated reserve fund = from_initial_reserve + from_accumulated_fees + from_unused_witness_budget
```

变量全部来自：*get_object 2.13* (例如，选择最新的变量）

Next calculate how much is available to be spent on workers and witnesses is:

```
total_budget = (updated reserve fund)*(time_since_last_budget)*17/(2^32)
```

四舍五入到最接近的整数

好的，现在要找出工人在这个预算期间（1小时）可以获得多少工资，你会发现从 `total_budget`或`get_global_properties`中的`worker_budget_per_day/24`中扣除见证人预算后的可用工资中的较小者：

 you find the smaller of the available pay AFTER subtracting witness budget from the <cite>total_budget</cite> OR the <cite>worker_budget_per_day/24</cite> from <cite>get_global_properties</cite>:

```
worker_budget=min( total_budget - witness_budget , worker_budget_per_day / 24 )
```

这就是为每个工人分配的每小时多少钱。 现在你对每个工人进行排名并按顺序支付他们一小时的工资或者投票。


