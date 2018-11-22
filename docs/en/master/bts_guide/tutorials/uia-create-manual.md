  **原文链接**：[http://dev.bitshares.works/en/master/bts_guide/tutorials/uia-create-manual.html#uia-create-manual](http://dev.bitshares.works/en/master/bts_guide/tutorials/uia-create-manual.html#uia-create-manual)
 
 **译者**：[https://github.com/b-t-s-1](b-t-s-1)
 
 **校对者**： 

***    

手动创建UIA
===========================

创建资产
----------------------



    >>> create_asset <issuer> <symbol> <precision> <options> {} false

 <table style="width: 750px;"><tbody>
    <tr>
        <td bgcolor="LightBlue">注意</td>
    </tr>
    <tr>
        <td bgcolor="MintCream">最后的假允许检查和验证构造的事务并且不广播它。 空{}可用于构造../user/mpa，并且是另一个教程的主题。</td>
    </tr>
</table>

# 参数


`精度`可以是从0开始的任何正整数。作为选项，我们传递一个可以包含这些设置的**JSON*对象：



```  
   {
      "max_supply" : 10000,    # Integer in satoshi! (100 for precision 1 and max 10)
      "market_fee_percent" : 0.3,
      "max_market_fee" : 1000, # in satoshi
      "issuer_permissions" : <permissions>,
      "flags" : <flags>,
      "core_exchange_rate" : {
          "base": {
            "amount": 21,           # denominator
            "asset_id": "1.3.0"     # BTS
          },
          "quote": {
            "amount": 76399,        # numerator
            "asset_id": "1.3.1"     # !THIS! asset
          }
      },
      "whitelist_authorities" : [],
      "blacklist_authorities" : [],
      "whitelist_markets" : [],
      "blacklist_markets" : [],
      "description" : "My fancy description"
   }
```  
标志构造为包含这些标志/权限的JSON对象

```
   {
      "charge_market_fee" : true,
      "white_list" : true,
      "override_authority" : true,
      "transfer_restricted" : true,
      "disable_force_settle" : true,
      "global_settle" : true,
      "disable_confidential" : true,
      "witness_fed_asset" : true,
      "committee_fed_asset" : true
   }
```  

权限和标志被建模为二进制标志的总和（参见下面的示例）

- 资产用户白名单

在../../integration/asset-whitelist中更详细地描述了白名单。

# 发行股票

创建资产后，在发行人发行股票之前不会存在任何股票：

    issue_asset <account> <amount> <symbol> <memo> True
  

# Python 示例
  

    from grapheneapi import GrapheneClient
    import json

    perm = {}
    perm["charge_market_fee"] = 0x01
    perm["white_list"] = 0x02
    perm["override_authority"] = 0x04
    perm["transfer_restricted"] = 0x08
    perm["disable_force_settle"] = 0x10
    perm["global_settle"] = 0x20
    perm["disable_confidential"] = 0x40
    perm["witness_fed_asset"] = 0x80
    perm["committee_fed_asset"] = 0x100


    class Config():
        wallet_host           = "localhost"
        wallet_port           = 8092
        wallet_user           = ""
        wallet_password       = ""

    if __name__ == '__main__':
        graphene = GrapheneClient(Config)

        permissions = {"charge_market_fee" : True,
                       "white_list" : True,
                       "override_authority" : True,
                       "transfer_restricted" : True,
                       "disable_force_settle" : True,
                       "global_settle" : True,
                       "disable_confidential" : True,
                       "witness_fed_asset" : True,
                       "committee_fed_asset" : True,
                       }
        flags       = {"charge_market_fee" : False,
                       "white_list" : False,
                       "override_authority" : False,
                       "transfer_restricted" : False,
                       "disable_force_settle" : False,
                       "global_settle" : False,
                       "disable_confidential" : False,
                       "witness_fed_asset" : False,
                       "committee_fed_asset" : False,
                       }
        permissions_int = 0
        for p in permissions :
            if permissions[p]:
                permissions_int += perm[p]
        flags_int = 0
        for p in permissions :
            if flags[p]:
                flags_int += perm[p]
        options = {"max_supply" : 10000,
                   "market_fee_percent" : 0,
                   "max_market_fee" : 0,
                   "issuer_permissions" : permissions_int,
                   "flags" : flags_int,
                   "core_exchange_rate" : {
                       "base": {
                           "amount": 10,
                           "asset_id": "1.3.0"},
                       "quote": {
                           "amount": 10,
                           "asset_id": "1.3.1"}},
                   "whitelist_authorities" : [],
                   "blacklist_authorities" : [],
                   "whitelist_markets" : [],
                   "blacklist_markets" : [],
                   "description" : "My fancy description"
                   }

        tx = graphene.rpc.create_asset("nathan", "SYMBOL", 3, options, {}, True)
        print(json.dumps(tx, indent=4))

