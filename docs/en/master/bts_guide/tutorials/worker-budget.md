  **原文链接**：[http://dev.bitshares.works/en/master/bts_guide/tutorials/worker-budget.html#worker-budget](http://dev.bitshares.works/en/master/bts_guide/tutorials/worker-budget.html#worker-budget)
 
 **译者**：[https://github.com/b-t-s-1](b-t-s-1)
 
 **校对者**： 
  
***  

# 声明工人报酬

**如何找到为所有工人分配每小时多少钱**

每秒，[17 /（2 ^ 32）*储备金]分配给见证人和工人，其中储备金是目前多少BTS没有分配的([见源代码](https://github.com/cryptonomex/graphene/blob/f85dec1c23f6bf9259ad9f15311b2e4aac4f9d44/libraries/chain/include/graphene/chain/config.hpp))。

每小时总可用储备金通过查找多少BTS可以用于分发，并且在下一个维护间隔期间，将返回多少BTS到储备金（即“销毁”）。

首先找出尚未分发的BTS数量：

```
# get max_supply from
get_object 1.3.0
# get current_supply from
get_object 2.3.0

==> from_initial_reserve = max_supply - current supply of BTS
```

然后通过添加累计费用和剩余的见证人预算来修改它（即每个区块预算1.5 BTS，因此剩余预算为

```
1.5 BTS * (维护期间剩余的区块数+见证人丢的区块))
```

在这个维护周期中（它们将永久地添加到“储备金”中）


```
get_object 2.13.*

==> updated reserve fund = from_initial_reserve +  from_accumulated_fees + from_unused_witness_budget
```

示例:

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

然后计算可用于工人和见证人的金额是：

```
total_budget = (updated reserve fund)*(time_since_last_budget)*17/(2^32)  #rounded up to the nearest integer
```

好的，现在要找出工人在这个预算期间（1小时）可以获得多少工资，你会发现从 `total_budget`或`get_global_properties`中的`worker_budget_per_day/24`中扣除见证人预算后的可用工资中的较小者：  
 
```
worker_budget=min(total_budget-witness_budget,worker_budget_per_day/24)
```

这就是为每个工人分配的每小时多少钱。 现在你对每个工人进行排名并按顺序支付他们一小时的工资或者投票。

## 参考文献：

* [区块链工人系统](https://bitshares.org/doxygen/group__workers.html) ([*](#id1)打开doxygen文档)

 