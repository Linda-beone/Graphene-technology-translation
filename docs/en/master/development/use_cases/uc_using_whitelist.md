  **原文链接**：[http：//dev.bitshares.works/en/master/development/use_cases/uc_using_whitelist.html#](http：//dev.bitshares.works/en/master/development/use_cases/uc_using_whitelist.html#)
 
 **译者**：[https://github.com/b-t-s-1](b-t-s-1)
 
 **校对者**： 

***  
  
# 使用白名单和黑名单

目录

* 用户白名单
* 资产市场白名单
* 资产用户白名单
  - 定义资产的列表权限
  - 将`alice`添加到白名单

***

## 用户白名单

任何实时会员都可以使用黑白名单来对其他帐户发表意见。 他们**不会**阻止任何人与您的帐户进行互动但作为*list authority*的基础。

**示例***

`白`名单用户可以添加到`提供`的帐户白名单中:

```
>>> whitelist_account provider white white_listed true
```

相比之下，`黑`名单用户可以添加到黑名单:

```
>>> whitelist_account provider black black_listed true
```

两者都可以从列表中删除:

```
>>> whitelist_account provider black no_listing true
>>> whitelist_account provider white no_listing true
```

**定义**

帐户的白名单和黑名单使用以下API调用：

<dl>
<dt>
signed_transaction <code>graphene::<a>wallet</a>::<a>wallet_api</a><code>::</code></code><code>whitelist_account</code><span>(</span>string <em>authorizing_account</em>, string <em>account_to_list</em>, account_whitelist_operation::account_listing <em>new_listing_status</em>, bool <em>broadcast</em> = false<span>)</span><br></dt>  

<dd><p>白名单和黑名单帐户，主要用于在白名单资产中进行交易。</p> 
<p>帐户可以以白名单或将其列入黑名单的形式自由指定有关其他帐户的意见。 此信息仅用于链验证，以确定帐户是否有权以强制执行白名单的资产类型进行交易，但第三方也可以将此信息用于其他用途，只要它不与使用白名单资产冲突。</p> 
<p>强制执行白名单的资产指定用于维护其白名单的帐户列表，以及用于维护其黑名单的帐户列表。 为了使给定帐户A在白名单资产S中持有和交易，A必须由S的白名单权限中的至少一个列入白名单，并且不被S的黑名单权限列入黑名单。 如果A收到S的余额，并且稍后从允许其持有S的白名单中删除，或者添加到任何黑名单S中，则A的余额将被冻结，直到A的授权恢复。</p>
<p><dl>
<dt><strong>返回</strong></dt>
<dd>已签名的交易更改白名单状态</dd>
<dt><strong>参数</strong></dt>
<dd><ul>
<li><code><span>authorizing_account</span></code>: 正在进行白名单的帐户</li>
<li><code><span>account_to_list</span></code>: 正在列入白名单的帐户</li>
<li><code><span>new_listing_status</span></code>: 新的白名单状态</li>
<li><code><span>broadcast</span></code>: true表示在网络上广播交易</li>
</ul>
</dd>
</dl>
</p>
</dd></dl>

它需要一个*new_listing_status*

<dl>
<dt>
<em>enum </em><code>graphene::<a>chain</a>::<a>account_whitelist_operation</a><code>::</code></code><code>account_listing</code><a>;</a><br></dt>
<dd><p><em>值:</em></p>
<dl>
<dt>
<code>no_listing</code> = 0x0<a>;</a><br></dt>
<dd><p>对此帐户没有任何意见。</p>
</dd></dl>

<dl>
<dt>
<code>white_listed</code> = 0x1<a>;</a><br></dt>
<dd><p>此帐户已列入白名单，但未列入黑名单。</p>
</dd></dl>

<dl>
<dt>
<code>black_listed</code> = 0x2<a>;</a><br></dt>
<dd><p>This account is blacklisted, but not whitelisted. </p>
</dd></dl>

<dl>
<dt>
<code>white_and_black_listed</code> = white_listed | black_listed<a>;</a><br></dt>
<dd><p>This account is both whitelisted and blacklisted. </p>
</dd></dl>

</dd></dl>

***

## [Asset Market Whitelists](#id3) [¶](#asset-market-whitelists "Permalink to this headline")

An issuer of an user-issued-asset may want to restrict trading partners for his
assets for legal reasons. For instance, a gateway for US dollar may not be
allowed to let his customers trade USD against CNY because additional licenses
would be required. Hence, in BitShares 2.0 we let issuers chose to restrict
trading partners with white- and black-lists.

**Example**

A gateway with IOU `<span class="pre">G.USD</span>` that wants to prevent his customers from trading
`<span class="pre">G.USD</span>` against `<span class="pre">bitCNY</span>` can do so by adding `<span class="pre">bitCNY</span>` to the blacklist of
`<span class="pre">G.USD</span>` by issuing::

```
>>> update_asset G.USD "" "{blacklist_markets:[CNY]}" true
```

Alternatively, if an issuer may want to only open the market `<span class="pre">G.USD</span> <span class="pre">:</span> <span class="pre">bitUSD</span>`
with his asset, he can do so as well with::

```
>>> update_asset G.USD "" "{whitelist_markets:[USD]}" true
```

Note

The third argument for `<span class="pre">update_asset</span>` replaces the existing
settings. Make sure to have all desired settings present.

**Definition**

Asset Market white-lists work with the following API call:

<dl>
<dt>
signed_transaction <code>graphene::<a>wallet</a>::<a>wallet_api</a><code>::</code></code><code>update_asset</code><span>(</span>string <em>symbol</em>, optional&lt;string&gt; <em>new_issuer</em>, asset_options <em>new_options</em>, bool <em>broadcast</em> = false<span>)</span><br></dt>
<dd><p>Update the core options on an asset. There are a number of options which all assets in the network use. These options are enumerated in the asset_object::asset_options struct. This command is used to update these options for an existing asset.</p>
<p><dl>
<dt><strong>Note</strong></dt>
<dd>This operation cannot be used to update BitAsset-specific options. For these options, <code><a><span><span>update_bitasset()</span></span></a></code> instead.</dd>
<dt><strong>Return</strong></dt>
<dd>the signed transaction updating the asset </dd>
<dt><strong>Parameters</strong></dt>
<dd><ul>
<li><code><span>symbol</span></code>: the name or id of the asset to update </li>
<li><code><span>new_issuer</span></code>: if changing the asset&#x2019;s issuer, the name or id of the new issuer. null if you wish to remain the issuer of the asset </li>
<li><code><span>new_options</span></code>: the new asset_options object, which will entirely replace the existing options. </li>
<li><code><span>broadcast</span></code>: true to broadcast the transaction on the network </li>
</ul>
</dd>
</dl>
</p>
</dd></dl>
<dl>
<dt>
<em>struct </em><code>asset_options</code><br></dt>
<dd><p>The <a><span>asset_options</span></a> struct contains options available on all assets in the network. </p>
<p><dl>
<dt><strong>Note</strong></dt>
<dd>Changes to this struct will break protocol compatibility </dd>
</dl>
</p>
<div>
<p>Public Functions</p>
<dl>
<dt>
void <code>validate</code><span>(</span><span>)</span> <em>const</em><br></dt>
<dd><p>Perform internal consistency checks. <dl>
<dt><strong>Exceptions</strong></dt>
<dd><ul>
<li><code><span>fc::exception</span></code>: if any check fails </li>
</ul>
</dd>
</dl>
</p>
</dd></dl>

</div>
<div>
<p>Public Members</p>
<dl>
<dt>
<a>share_type</a> <code>max_supply</code> = GRAPHENE_MAX_SHARE_SUPPLY<br></dt>
<dd><p>The maximum supply of this asset which may exist at any given time. This can be as large as GRAPHENE_MAX_SHARE_SUPPLY </p>
</dd></dl>

<dl>
<dt>
uint16_t <code>market_fee_percent</code> = 0<br></dt>
<dd><p>When this asset is traded on the markets, this percentage of the total traded will be exacted and paid to the issuer. This is a fixed point value, representing hundredths of a percent, i.e. a value of 100 in this field means a 1% fee is charged on market trades of this asset. </p>
</dd></dl>

<dl>
<dt>
<a>share_type</a> <code>max_market_fee</code> = GRAPHENE_MAX_SHARE_SUPPLY<br></dt>
<dd><p>Market fees calculated as <a><span>market_fee_percent</span></a> of the traded volume are capped to this value. </p>
</dd></dl>

<dl>
<dt>
uint16_t <code>issuer_permissions</code> = UIA_ASSET_ISSUER_PERMISSION_MASK<br></dt>
<dd><p>The flags which the issuer has permission to update. See <a><span>asset_issuer_permission_flags</span></a>. </p>
</dd></dl>

<dl>
<dt>
uint16_t <code>flags</code> = 0<br></dt>
<dd><p>The currently active flags on this permission. See <a><span>asset_issuer_permission_flags</span></a>. </p>
</dd></dl>

<dl>
<dt>
<a>price</a> <code>core_exchange_rate</code> = <a><span>price</span></a>(<a><span>asset</span></a>(), <a><span>asset</span></a>(0, <a><span>asset_id_type</span></a>(1)))<br></dt>
<dd><p>When a non-core asset is used to pay a fee, the blockchain must convert that asset to core asset in order to accept the fee. If this asset&#x2019;s fee pool is funded, the chain will automatically deposite fees in this asset to its accumulated fees, and withdraw from the fee pool the same amount as converted at the core exchange rate. </p>
</dd></dl>

<dl>
<dt>
flat_set&lt;<a>account_id_type</a>&gt; <code>whitelist_authorities</code><br></dt>
<dd><p>A set of accounts which maintain whitelists to consult for this asset. If whitelist_authorities is non-empty, then only accounts in whitelist_authorities are allowed to hold, use, or transfer the asset. </p>
</dd></dl>

<dl>
<dt>
flat_set&lt;<a>account_id_type</a>&gt; <code>blacklist_authorities</code><br></dt>
<dd><p>A set of accounts which maintain blacklists to consult for this asset. If flags &amp; white_list is set, an account may only send, receive, trade, etc. in this asset if none of these accounts appears in its <a><span>account_object::blacklisting_accounts</span></a> field. If the account is blacklisted, it may not transact in this asset even if it is also whitelisted. </p>
</dd></dl>

<dl>
<dt>
flat_set&lt;<a>asset_id_type</a>&gt; <code>whitelist_markets</code><br></dt>
<dd><p>defines the assets that this asset may be traded against in the market </p>
</dd></dl>

<dl>
<dt>
flat_set&lt;<a>asset_id_type</a>&gt; <code>blacklist_markets</code><br></dt>
<dd><p>defines the assets that this asset may not be traded against in the market, must not overlap whitelist </p>
</dd></dl>

<dl>
<dt>
string <code>description</code><br></dt>
<dd><p>data that describes the meaning/purpose of this asset, fee will be charged proportional to size of description. </p>
</dd></dl>

</div>
</dd></dl>
<dl>
<dt>
<em>enum </em><code>graphene::<a>chain</a><code>::</code></code><code>asset_issuer_permission_flags</code><br></dt>
<dd><p><em>Values:</em></p>
<dl>
<dt>
<code>charge_market_fee</code> = 0x01<br></dt>
<dd><p>an issuer-specified percentage of all market trades in this asset is paid to the issuer </p>
</dd></dl>

<dl>
<dt>
<code>white_list</code> = 0x02<br></dt>
<dd><p>accounts must be whitelisted in order to hold this asset </p>
</dd></dl>

<dl>
<dt>
<code>override_authority</code> = 0x04<br></dt>
<dd><p>issuer may transfer asset back to himself </p>
</dd></dl>

<dl>
<dt>
<code>transfer_restricted</code> = 0x08<br></dt>
<dd><p>require the issuer to be one party to every transfer </p>
</dd></dl>

<dl>
<dt>
<code>disable_force_settle</code> = 0x10<br></dt>
<dd><p>disable force settling </p>
</dd></dl>

<dl>
<dt>
<code>global_settle</code> = 0x20<br></dt>
<dd><p>allow the bitasset issuer to force a global settling  this may be set in permissions, but not flags </p>
</dd></dl>

<dl>
<dt>
<code>disable_confidential</code> = 0x40<br></dt>
<dd><p>allow the asset to be used with confidential transactions </p>
</dd></dl>

<dl>
<dt>
<code>witness_fed_asset</code> = 0x80<br></dt>
<dd><p>allow the asset to be fed by witnesses </p>
</dd></dl>

<dl>
<dt>
<code>committee_fed_asset</code> = 0x100<br></dt>
<dd><p>allow the asset to be fed by the committee </p>
</dd></dl>

</dd></dl>

***

## [Asset User Whitelists](#id4) [¶](#asset-user-whitelists "Permalink to this headline")

Asset User white- and black-lists serve the need for companies to restrict
service to a subset of accounts. For instance, a fiat gateway may require
to follow KYC/AML regulations and can hence only deal with those
customers that have been verified accordingly. If the issuer of an user-issued
asset desires, he may set a restriction so that only users on the white-list
(and/or **not** on the blacklist) are allowed to hold his token.

Instead of putting all verified accounts into the respective asset's white-list
directly, BitShares 2.0 allows to define one or several white-list
_authorities_. In practice, the white- and black-lists of these accounts are
combined and serve as white- and black-lists for the asset.

This allows for easy out-sourcing of KYC/AML verification to 3rd-party
providers.

Note

By removing a user from the whitelist, funds can effectively be
frozen.

**Example**

Let's assume user `<span class="pre">alice</span>` wants to own a gateways IOUs called `<span class="pre">G.USD</span>` which are
restricted by a whitelists. Before being able to own `<span class="pre">G.USD</span>`, `<span class="pre">alice</span>` needs
to be white-listed by one of the authorities of `<span class="pre">G.USD</span>`.

### [Defining an asset's list authorities](#id5) [¶](#defining-an-asset-s-list-authorities "Permalink to this headline")

We now define the authorities (i.e. accounts) that define the white- and
blacklist of the asset `<span class="pre">G.USD</span>`. We add `<span class="pre">g-issuer</span>` and `<span class="pre">kycprovider</span>` to
the white- and black-list::

```
>>> update_asset G.USD "" "{blacklist_authorities:[g-issuer, kycprovider], whitelist_authorities:[g-issuer, kycprovider], flags:white_list}" true
```

Note

The third argument for `<span class="pre">update_asset</span>` replaces the existing
settings. Make sure to have all desired settings present.

### [Adding `<span class="pre">alice</span>` to a whitelist](#id6) [¶](#adding-alice-to-a-whitelist "Permalink to this headline")

Let's assume the only authority is the issuer `<span class="pre">g-issuer</span>` himself for
simplicity. The issuer now needs to add `<span class="pre">alice</span>` to `<span class="pre">g-issuer</span>`'s account
whitelist::

```
>>> whitelist_account g-issuer alice white_listed true
```

**Definition**

White- and Black-listing of assets works with the following API call:

<dl>
<dt>
signed_transaction <code>graphene::<a>wallet</a>::<a>wallet_api</a><code>::</code></code><code>update_asset</code><span>(</span>string <em>symbol</em>, optional&lt;string&gt; <em>new_issuer</em>, asset_options <em>new_options</em>, bool <em>broadcast</em> = false<span>)</span><br></dt>
<dd><p>Update the core options on an asset. There are a number of options which all assets in the network use. These options are enumerated in the asset_object::asset_options struct. This command is used to update these options for an existing asset.</p>
<p><dl>
<dt><strong>Note</strong></dt>
<dd>This operation cannot be used to update BitAsset-specific options. For these options, <code><a><span><span>update_bitasset()</span></span></a></code> instead.</dd>
<dt><strong>Return</strong></dt>
<dd>the signed transaction updating the asset </dd>
<dt><strong>Parameters</strong></dt>
<dd><ul>
<li><code><span>symbol</span></code>: the name or id of the asset to update </li>
<li><code><span>new_issuer</span></code>: if changing the asset&#x2019;s issuer, the name or id of the new issuer. null if you wish to remain the issuer of the asset </li>
<li><code><span>new_options</span></code>: the new asset_options object, which will entirely replace the existing options. </li>
<li><code><span>broadcast</span></code>: true to broadcast the transaction on the network </li>
</ul>
</dd>
</dl>
</p>
</dd></dl>
<dl>
<dt>
<em>struct </em><code>asset_options</code><br></dt>
<dd><p>The <a><span>asset_options</span></a> struct contains options available on all assets in the network. </p>
<p><dl>
<dt><strong>Note</strong></dt>
<dd>Changes to this struct will break protocol compatibility </dd>
</dl>
</p>
<div>
<p>Public Functions</p>
<dl>
<dt>
void <code>validate</code><span>(</span><span>)</span> <em>const</em><br></dt>
<dd><p>Perform internal consistency checks. <dl>
<dt><strong>Exceptions</strong></dt>
<dd><ul>
<li><code><span>fc::exception</span></code>: if any check fails </li>
</ul>
</dd>
</dl>
</p>
</dd></dl>

</div>
<div>
<p>Public Members</p>
<dl>
<dt>
<a>share_type</a> <code>max_supply</code> = GRAPHENE_MAX_SHARE_SUPPLY<br></dt>
<dd><p>The maximum supply of this asset which may exist at any given time. This can be as large as GRAPHENE_MAX_SHARE_SUPPLY </p>
</dd></dl>

<dl>
<dt>
uint16_t <code>market_fee_percent</code> = 0<br></dt>
<dd><p>When this asset is traded on the markets, this percentage of the total traded will be exacted and paid to the issuer. This is a fixed point value, representing hundredths of a percent, i.e. a value of 100 in this field means a 1% fee is charged on market trades of this asset. </p>
</dd></dl>

<dl>
<dt>
<a>share_type</a> <code>max_market_fee</code> = GRAPHENE_MAX_SHARE_SUPPLY<br></dt>
<dd><p>Market fees calculated as <a><span>market_fee_percent</span></a> of the traded volume are capped to this value. </p>
</dd></dl>

<dl>
<dt>
uint16_t <code>issuer_permissions</code> = UIA_ASSET_ISSUER_PERMISSION_MASK<br></dt>
<dd><p>The flags which the issuer has permission to update. See <a><span>asset_issuer_permission_flags</span></a>. </p>
</dd></dl>

<dl>
<dt>
uint16_t <code>flags</code> = 0<br></dt>
<dd><p>The currently active flags on this permission. See <a><span>asset_issuer_permission_flags</span></a>. </p>
</dd></dl>

<dl>
<dt>
<a>price</a> <code>core_exchange_rate</code> = <a><span>price</span></a>(<a><span>asset</span></a>(), <a><span>asset</span></a>(0, <a><span>asset_id_type</span></a>(1)))<br></dt>
<dd><p>When a non-core asset is used to pay a fee, the blockchain must convert that asset to core asset in order to accept the fee. If this asset&#x2019;s fee pool is funded, the chain will automatically deposite fees in this asset to its accumulated fees, and withdraw from the fee pool the same amount as converted at the core exchange rate. </p>
</dd></dl>

<dl>
<dt>
flat_set&lt;<a>account_id_type</a>&gt; <code>whitelist_authorities</code><br></dt>
<dd><p>A set of accounts which maintain whitelists to consult for this asset. If whitelist_authorities is non-empty, then only accounts in whitelist_authorities are allowed to hold, use, or transfer the asset. </p>
</dd></dl>

<dl>
<dt>
flat_set&lt;<a>account_id_type</a>&gt; <code>blacklist_authorities</code><br></dt>
<dd><p>A set of accounts which maintain blacklists to consult for this asset. If flags &amp; white_list is set, an account may only send, receive, trade, etc. in this asset if none of these accounts appears in its <a><span>account_object::blacklisting_accounts</span></a> field. If the account is blacklisted, it may not transact in this asset even if it is also whitelisted. </p>
</dd></dl>

<dl>
<dt>
flat_set&lt;<a>asset_id_type</a>&gt; <code>whitelist_markets</code><br></dt>
<dd><p>defines the assets that this asset may be traded against in the market </p>
</dd></dl>

<dl>
<dt>
flat_set&lt;<a>asset_id_type</a>&gt; <code>blacklist_markets</code><br></dt>
<dd><p>defines the assets that this asset may not be traded against in the market, must not overlap whitelist </p>
</dd></dl>

<dl>
<dt>
string <code>description</code><br></dt>
<dd><p>data that describes the meaning/purpose of this asset, fee will be charged proportional to size of description. </p>
</dd></dl>

</div>
</dd></dl>
<dl>
<dt>
<em>enum </em><code>graphene::<a>chain</a><code>::</code></code><code>asset_issuer_permission_flags</code><br></dt>
<dd><p><em>Values:</em></p>
<dl>
<dt>
<code>charge_market_fee</code> = 0x01<br></dt>
<dd><p>an issuer-specified percentage of all market trades in this asset is paid to the issuer </p>
</dd></dl>

<dl>
<dt>
<code>white_list</code> = 0x02<br></dt>
<dd><p>accounts must be whitelisted in order to hold this asset </p>
</dd></dl>

<dl>
<dt>
<code>override_authority</code> = 0x04<br></dt>
<dd><p>issuer may transfer asset back to himself </p>
</dd></dl>

<dl>
<dt>
<code>transfer_restricted</code> = 0x08<br></dt>
<dd><p>require the issuer to be one party to every transfer </p>
</dd></dl>

<dl>
<dt>
<code>disable_force_settle</code> = 0x10<br></dt>
<dd><p>disable force settling </p>
</dd></dl>

<dl>
<dt>
<code>global_settle</code> = 0x20<br></dt>
<dd><p>allow the bitasset issuer to force a global settling  this may be set in permissions, but not flags </p>
</dd></dl>

<dl>
<dt>
<code>disable_confidential</code> = 0x40<br></dt>
<dd><p>allow the asset to be used with confidential transactions </p>
</dd></dl>

<dl>
<dt>
<code>witness_fed_asset</code> = 0x80<br></dt>
<dd><p>allow the asset to be fed by witnesses </p>
</dd></dl>

<dl>
<dt>
<code>committee_fed_asset</code> = 0x100<br></dt>
<dd><p>allow the asset to be fed by the committee </p>
</dd></dl>

</dd></dl>[Next](../interaction.html "Blockchain Interaction") [Previous](supporting_features.html "Supporting Features")

***

© Copyright 2018 BitShares Blockchain Foundation.

          Revision `71213459`.

Built with [Sphinx](http://sphinx-doc.org/) using a [theme](https://github.com/rtfd/sphinx_rtd_theme) provided by [Read the Docs](https://readthedocs.org) .

      Read the Docs
      v: master

<dl>
      <dt>Versions</dt>

        <dd><a>latest</a></dd>

        <dd><a>master</a></dd>

    </dl>
<dl>
      <dt>Downloads</dt>

        <dd><a>pdf</a></dd>

        <dd><a>htmlzip</a></dd>

        <dd><a>epub</a></dd>

    </dl>
<dl>
      <dt>On Read the Docs</dt>
        <dd>
          <a>Project Home</a>
        </dd>
        <dd>
          <a>Builds</a>
        </dd>
    </dl>

***

Free document hosting provided by [Read the Docs](http://www.readthedocs.org).
