  **原文链接**：[http://dev.bitshares.works/en/master/bts_guide/tutorials/uia-update-manual.html#uia-update-manual](http://dev.bitshares.works/en/master/bts_guide/tutorials/uia-update-manual.html#uia-update-manual)
 
 **译者**：[https://github.com/b-t-s-1](b-t-s-1)
 
 **校对者**： 

*** 

更新/更改现有的UIA
==================================

创建后，发行人可以修改UIA。 为此，已创建单独的调用`update_asset`。

什么可以和不可以改变
--------------------------------

除了

* 符号
* 精度，

每个参数，选项或设置都可以更新。

 <table style="width: 750px;"><tbody>
    <tr>
        <td bgcolor="LightBlue">注意</td>
    </tr>
    <tr>
        <td bgcolor="MintCream">删除权限后，无法再次重新启用该权限！</td>
    </tr>
</table>

## Python 示例

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
        flags       = {"charge_market_fee" : True,
                       "white_list" : True,
                       "override_authority" : True,
                       "transfer_restricted" : True,
                       "disable_force_settle" : True,
                       "global_settle" : False,
                       "disable_confidential" : True,
                       "witness_fed_asset" : False,
                       "committee_fed_asset" : True,
                       }
        permissions_int = 0
        for p in permissions :
            if permissions[p]:
                permissions_int += perm[p]
        flags_int = 0
        for p in permissions :
            if flags[p]:
                flags_int += perm[p]
        options = {"max_supply" : 100000000000,
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
                   "description" : "My fancy description 2"
                   }

        tx = graphene.rpc.update_asset("SYMBOL", None, options, True)
        print(json.dumps(tx, indent=4))
		