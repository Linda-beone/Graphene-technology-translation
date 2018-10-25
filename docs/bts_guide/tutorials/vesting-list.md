
查询授权资产
===========================================

**原文链接**：
[http://dev.bitshares.works/en/master/bts_guide/tutorials/vesting-list.html](http://dev.bitshares.works/en/master/bts_guide/tutorials/vesting-list.html)

**译者**：[https://github.com/zoowii](zoowii)

**校对者**：

-------------------------

一个账户的授权资产余额可以通过以下命令查看：

    >>> get_vesting_balances <账户名>

返回结果形如：


    [{
        "id": "1.13.241",
        "owner": "1.2.282",
        "balance": {
          "amount": 4158699804,
          "asset_id": "1.3.0"
        },
        "policy": [
          1,{
            "vesting_seconds": 7776000,
            "start_claim": "1970-01-01T00:00:00",
            "coin_seconds_earned": "16408550952570000",
            "coin_seconds_earned_last_update": "2016-02-08T09:00:00"
          }
        ],
        "allowed_withdraw": {
          "amount": 2114844530,
          "asset_id": "1.3.0"
        },
        "allowed_withdraw_time": "2016-02-08T11:26:12"
      }
    ]

以上结果中`balance`部分提供了总的授权资产余额（资产金额和资产编号），而`allowed_withdraw`部分表示目前可提现的授权资产余额。 这个对象还可以看出授权资产按币龄计算的估计结果（按线性授权的规律）。
