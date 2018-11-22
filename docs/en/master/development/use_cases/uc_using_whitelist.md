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
<dd><p>此帐户已列入黑名单，但未列入白名单。</p>
</dd></dl>

<dl>
<dt>
<code>white_and_black_listed</code> = white_listed | black_listed<a>;</a><br></dt>
<dd><p>此帐户已列入白名单并列入黑名单。</p>
</dd></dl>

</dd></dl>

***

## 资产市场白名单

由于法律原因，用户发行资产的发行方可能希望限制贸易伙伴的资产。 例如，可能不允许美元网关让他的客户以人民币兑换美元，因为需要额外的许可证。 因此，在BitShares 2.0中，我们让发行人选择使用白名单和黑名单来限制贸易伙伴。

**示例**

IOU `G.USD`的网关想要防止他的客户针对`bitCNY`交易`G.USD`，可以通过发送将`bitCNY`添加到`G.USD`的黑名单中来实现这一点。

```
>>> update_asset G.USD "" "{blacklist_markets:[CNY]}" true
```

或者，如果发行人可能只想用他的资产开立市场`G.USD：bitUSD`，他也可以这样做:

```
>>> update_asset G.USD "" "{whitelist_markets:[USD]}" true
```

 <table style="width: 750px;"><tbody>
    <tr>
        <td bgcolor="LightBlue">注意</td>
    </tr>
    <tr>
        <td bgcolor="MintCream">update_asset的第三个参数替换现有设置。 确保存在所有所需的设置。</td>
    </tr>
</table>

**定义**

资产市场白名单使用以下API调用：

<dl>
<dt>
signed_transaction <code>graphene::<a>wallet</a>::<a>wallet_api</a><code>::</code></code><code>update_asset</code><span>(</span>string <em>symbol</em>, optional&lt;string&gt; <em>new_issuer</em>, asset_options <em>new_options</em>, bool <em>broadcast</em> = false<span>)</span><br></dt>  
<dd><p>更新资产的核心选项。 网络中的所有资产都使用多种选项。 这些选项在asset_object :: asset_options结构中枚举。 此命令用于更新现有资产的这些选项。</p>
<p><dl>
<dt><strong>注意</strong></dt>
<dd>此操作不能用于更新BitAsset特定的选项。 对于这些选项，请改为update_bitasset（）。</dd>
<dt><strong>返回</strong></dt>
<dd>已签名的交易更新资产</dd>
<dt><strong>参数</strong></dt>
<dd><ul>
<li><code><span>symbol</span></code>:要更新的资产的名称或ID</li>
<li><code><span>new_issuer</span></code>:如果更改资产的发行人，新发行人的名称或身份。 如果您希望保留资产的发行人，则为null </li>
<li><code><span>new_options</span></code>:新的asset_options对象，它将完全取代现有的选项。</li>
<li><code><span>broadcast</span></code>:如果在网络上广播交易，则为true</li>
</ul>
</dd>
</dl>
</p>
</dd></dl>
<dl>
<dt>
<em>struct </em><code>asset_options</code><br></dt>
<dd><p>The <a><span>asset_options</span></a> asset_options结构包含网络中所有资产的可用选项。</p>
<p><dl>
<dt><strong>注意</strong></dt>
<dd>对此结构的更改将破坏协议兼容性</dd>
</dl>
</p>
<div>
<p>公共职能</p>
<dl>
<dt>
void <code>validate</code><span>(</span><span>)</span> <em>const</em><br></dt>
<dd><p>执行内部一致性检查。<dl>
<dt><strong>异常</strong></dt>
<dd><ul>
<li><code><span>fc::exception</span></code>:如果任何检查失败</li>
</ul>
</dd>
</dl>
</p>
</dd></dl>

</div>
<div>
<p>公众成员</p>
<dl>
<dt>
<a>share_type</a> <code>max_supply</code> = GRAPHENE_MAX_SHARE_SUPPLY<br></dt>
<dd><p>在任何给定时间可能存在的此资产的最大供应量。 这可以与GRAPHENE_MAX_SHARE_SUPPLY一样大</p>
</dd></dl>

<dl>
<dt>
uint16_t <code>market_fee_percent</code> = 0<br></dt>
<dd><p>当该资产在市场上交易时，该交易总额的这一百分比将被提取并支付给发行人。 这是一个固定点值，代表百分之一百，即该字段中的值100意味着对该资产的市场交易收取1％的费用。 </p>
</dd></dl>

<dl>
<dt>
<a>share_type</a> <code>max_market_fee</code> = GRAPHENE_MAX_SHARE_SUPPLY<br></dt>
<dd><p>按交易量的market_fee_percen计算的市场费用上限为此值。 </p>
</dd></dl>

<dl>
<dt>
uint16_t <code>issuer_permissions</code> = UIA_ASSET_ISSUER_PERMISSION_MASK<br></dt>
<dd><p>发行人有权更新的标志。 请参阅asset_issuer_permission_flags。</p>
</dd></dl>

<dl>
<dt>
uint16_t <code>flags</code> = 0<br></dt>
<dd><p>此权限上当前有效的标志。 请参阅asset_issuer_permission_flags。</p>
</dd></dl>

<dl>
<dt>
<a>price</a> <code>core_exchange_rate</code> = <a><span>price</span></a>(<a><span>asset</span></a>(), <a><span>asset</span></a>(0, <a><span>asset_id_type</span></a>(1)))<br></dt>
<dd><p>当非核心资产用于支付费用时，区块链必须将该资产转换为核心资产才能接受该费用。 如果该资产的费用池被资助，该链将自动将该资产的费用存入其累计费用，并从费用池中提现与核心汇率相同的金额。</p>
</dd></dl>

<dl>
<dt>
flat_set&lt;<a>account_id_type</a>&gt; <code>whitelist_authorities</code><br></dt>
<dd><p>一组帐户，用于维护白名单以查询此资产。 如果whitelist_authorities非空，则只允许whitelist_authorities中的帐户保留，使用或转移资产。</p>
</dd></dl>

<dl>
<dt>
flat_set&lt;<a>account_id_type</a>&gt; <code>blacklist_authorities</code><br></dt>
<dd><p>一组帐户，用于维护黑名单以查询此资产。 如果设置了flags＆white_list，则帐户只能在此资产中发送，接收，交易等，如果这些帐户中没有一个出现在其account_object :: blacklisting_accounts字段中。 如果该帐户被列入黑名单，即使该帐户也列入白名单，它也不会在此资产中进行交易。 </p>
</dd></dl>

<dl>
<dt>
flat_set&lt;<a>asset_id_type</a>&gt; <code>whitelist_markets</code><br></dt>
<dd><p>定义此资产可在市场上交易的资产</p>
</dd></dl>

<dl>
<dt>
flat_set&lt;<a>asset_id_type</a>&gt; <code>blacklist_markets</code><br></dt>
<dd><p>定义此资产可能无法在市场上交易的资产，不得与白名单重叠 </p>
</dd></dl>

<dl>
<dt>
string <code>description</code><br></dt>
<dd><p>描述此资产的含义/目的的数据，费用将按描述的大小成比例收取。 </p>
</dd></dl>

</div>
</dd></dl>
<dl>
<dt>
<em>enum </em><code>graphene::<a>chain</a><code>::</code></code><code>asset_issuer_permission_flags</code><br></dt>
<dd><p><em>值:</em></p>
<dl>
<dt>
<code>charge_market_fee</code> = 0x01<br></dt>
<dd><p>发行人指定的资产中所有市场交易的百分比支付给发行人</p>
</dd></dl>

<dl>
<dt>
<code>white_list</code> = 0x02<br></dt>
<dd><p>帐户必须列入白名单才能持有此资产</p>
</dd></dl>

<dl>
<dt>
<code>override_authority</code> = 0x04<br></dt>
<dd><p>发行人可以将资产转回给自己</p>
</dd></dl>

<dl>
<dt>
<code>transfer_restricted</code> = 0x08<br></dt>
<dd><p>要求发行人是每次转移的一方</p>
</dd></dl>

<dl>
<dt>
<code>disable_force_settle</code> = 0x10<br></dt>
<dd><p>禁止强清</p>
</dd></dl>

<dl>
<dt>
<code>global_settle</code> = 0x20<br></dt>
<dd><p>允许bitasset发行者强制全球设置，这可以在权限中设置，但不能设置标志</p>
</dd></dl>

<dl>
<dt>
<code>disable_confidential</code> = 0x40<br></dt>
<dd><p>允许资产与机密交易一起使用</p>
</dd></dl>

<dl>
<dt>
<code>witness_fed_asset</code> = 0x80<br></dt>
<dd><p>允许资产由见证人提供</p>
</dd></dl>

<dl>
<dt>
<code>committee_fed_asset</code> = 0x100<br></dt>
<dd><p>允许资产由委员会提供 </p>
</dd></dl>

</dd></dl>

***

## 资产用户白名单

资产用户白名单和黑名单满足了公司将服务限制为一部分帐户服务的需求。 例如，法定网关可能需要遵守KYC/AML法规，因此只能处理已经相应验证的客户。 如果用户发行资产的发行者需要，他可以设置限制，以便只允许白名单上（和/或黑名单上的用户）持有他的代币。

BitShares 2.0不是直接将所有已验证的帐户放入相应资产的白名单，而是允许定义一个或多个白名单权限。 在实践中，这些帐户的白名单和黑名单被合并，并作为资产的白名单和黑名单。

这样可以轻松地将KYC/AML验证外包给第三方提供商。

 <table style="width: 750px;"><tbody>
    <tr>
        <td bgcolor="LightBlue">注意</td>
    </tr>
    <tr>
        <td bgcolor="MintCream">通过从白名单中删除用户，可以有效地冻结资金。</td>
    </tr>
</table>

**示例**

让我们假设用户`alice`想要拥有一个名为`G.USD`的网关IOU，它受到白名单的限制。 在能够拥有`G.USD`之前，`alice`需要由`G.USD`的一个授权者列入白名单。

### 定义资产的列表权限

我们现在定义用于定义资产`G.USD`的白名单和黑名单的权限（即帐户）。 我们将`g-issuer`和`kycprovider`添加到白名单和黑名单:

```
>>> update_asset G.USD "" "{blacklist_authorities:[g-issuer, kycprovider], whitelist_authorities:[g-issuer, kycprovider], flags:white_list}" true
```

 <table style="width: 750px;"><tbody>
    <tr>
        <td bgcolor="LightBlue">注意</td>
    </tr>
    <tr>
        <td bgcolor="MintCream">update_asset的第三个参数替换现有设置。 确保所需的设置。</td>
    </tr>
</table>

### 将`alice`添加到白名单

让我们假设唯一的权威是发行人`g-issuer`本人为了简单起见。 发行人现在需要添加`alice`到`g-issuer`的帐户白名单:

```
>>> whitelist_account g-issuer alice white_listed true
```

**定义**

资产的白名单和黑名单使用以下API调用：

<dl>
<dt>
signed_transaction <code>graphene::<a>wallet</a>::<a>wallet_api</a><code>::</code></code><code>update_asset</code><span>(</span>string <em>symbol</em>, optional&lt;string&gt; <em>new_issuer</em>, asset_options <em>new_options</em>, bool <em>broadcast</em> = false<span>)</span><br></dt>
<dd><p>更新资产的核心选项。 网络中的所有资产都使用多种选项。 这些选项在asset_object :: asset_options结构中枚举。 此命令用于更新现有资产的这些选项。</p>
<p><dl>
<dt><strong>注意</strong></dt>
<dd>此操作不能用于更新BitAsset特定的选项。 对于这些选项，请改为update_bitasset（）。</dd>
<dt><strong>返回</strong></dt>
<dd>已签名的交易更新资产</dd>
<dt><strong>参数</strong></dt>
<dd><ul>
<li><code><span>symbol</span></code>: 要更新的资产的名称或ID </li>
<li><code><span>new_issuer</span></code>: 如果更改资产的发行人，新发行人的名称或身份。 如果您希望保留资产的发行人，则为null</li>
<li><code><span>new_options</span></code>: 新的asset_options对象，它将完全取代现有的选项。</li>
<li><code><span>broadcast</span></code>: 如果在网络上广播交易，则为true</li>
</ul>
</dd>
</dl>
</p>
</dd></dl>
<dl>
<dt>
<em>struct </em><code>asset_options</code><br></dt>
<dd><p>The <a><span>asset_options</span></a>asset_options结构包含网络中所有资产的可用选项。</p>
<p><dl>
<dt><strong>注意</strong></dt>
<dd>对此结构的更改将破坏协议兼容性</dd>
</dl>
</p>
<div>
<p>公共职能</p>
<dl>
<dt>
void <code>validate</code><span>(</span><span>)</span> <em>const</em><br></dt>
<dd><p>执行内部一致性检查。<dl>
<dt><strong>异常</strong></dt>
<dd><ul>
<li><code><span>fc::exception</span></code>: 如果任何检查失败</li>
</ul>
</dd>
</dl>
</p>
</dd></dl>

</div>
<div>
<p>公共职能</p>
<dl>
<dt>
void <code>validate</code><span>(</span><span>)</span> <em>const</em><br></dt>
<dd><p>执行内部一致性检查。<dl>
<dt><strong>异常</strong></dt>
<dd><ul>
<li><code><span>fc::exception</span></code>:如果任何检查失败</li>
</ul>
</dd>
</dl>
</p>
</dd></dl>

</div>
<div>
<p>公众成员</p>
<dl>
<dt>
<a>share_type</a> <code>max_supply</code> = GRAPHENE_MAX_SHARE_SUPPLY<br></dt>
<dd><p>在任何给定时间可能存在的此资产的最大供应量。 这可以与GRAPHENE_MAX_SHARE_SUPPLY一样大</p>
</dd></dl>

<dl>
<dt>
uint16_t <code>market_fee_percent</code> = 0<br></dt>
<dd><p>当该资产在市场上交易时，该交易总额的这一百分比将被提取并支付给发行人。 这是一个固定点值，代表百分之一百，即该字段中的值100意味着对该资产的市场交易收取1％的费用。 </p>
</dd></dl>

<dl>
<dt>
<a>share_type</a> <code>max_market_fee</code> = GRAPHENE_MAX_SHARE_SUPPLY<br></dt>
<dd><p>按交易量的market_fee_percen计算的市场费用上限为此值。 </p>
</dd></dl>

<dl>
<dt>
uint16_t <code>issuer_permissions</code> = UIA_ASSET_ISSUER_PERMISSION_MASK<br></dt>
<dd><p>发行人有权更新的标志。 请参阅asset_issuer_permission_flags。</p>
</dd></dl>

<dl>
<dt>
uint16_t <code>flags</code> = 0<br></dt>
<dd><p>此权限上当前有效的标志。 请参阅asset_issuer_permission_flags。</p>
</dd></dl>

<dl>
<dt>
<a>price</a> <code>core_exchange_rate</code> = <a><span>price</span></a>(<a><span>asset</span></a>(), <a><span>asset</span></a>(0, <a><span>asset_id_type</span></a>(1)))<br></dt>
<dd><p>当非核心资产用于支付费用时，区块链必须将该资产转换为核心资产才能接受该费用。 如果该资产的费用池被资助，该链将自动将该资产的费用存入其累计费用，并从费用池中提现与核心汇率相同的金额。</p>
</dd></dl>

<dl>
<dt>
flat_set&lt;<a>account_id_type</a>&gt; <code>whitelist_authorities</code><br></dt>
<dd><p>一组帐户，用于维护白名单以查询此资产。 如果whitelist_authorities非空，则只允许whitelist_authorities中的帐户保留，使用或转移资产。</p>
</dd></dl>

<dl>
<dt>
flat_set&lt;<a>account_id_type</a>&gt; <code>blacklist_authorities</code><br></dt>
<dd><p>一组帐户，用于维护黑名单以查询此资产。 如果设置了flags＆white_list，则帐户只能在此资产中发送，接收，交易等，如果这些帐户中没有一个出现在其account_object :: blacklisting_accounts字段中。 如果该帐户被列入黑名单，即使该帐户也列入白名单，它也不会在此资产中进行交易。 </p>
</dd></dl>

<dl>
<dt>
flat_set&lt;<a>asset_id_type</a>&gt; <code>whitelist_markets</code><br></dt>
<dd><p>定义此资产可在市场上交易的资产</p>
</dd></dl>

<dl>
<dt>
flat_set&lt;<a>asset_id_type</a>&gt; <code>blacklist_markets</code><br></dt>
<dd><p>定义此资产可能无法在市场上交易的资产，不得与白名单重叠 </p>
</dd></dl>

<dl>
<dt>
string <code>description</code><br></dt>
<dd><p>描述此资产的含义/目的的数据，费用将按描述的大小成比例收取。 </p>
</dd></dl>

</div>
</dd></dl>
<dl>
<dt>
<em>enum </em><code>graphene::<a>chain</a><code>::</code></code><code>asset_issuer_permission_flags</code><br></dt>
<dd><p><em>值:</em></p>
<dl>
<dt>
<code>charge_market_fee</code> = 0x01<br></dt>
<dd><p>发行人指定的资产中所有市场交易的百分比支付给发行人</p>
</dd></dl>

<dl>
<dt>
<code>white_list</code> = 0x02<br></dt>
<dd><p>帐户必须列入白名单才能持有此资产</p>
</dd></dl>

<dl>
<dt>
<code>override_authority</code> = 0x04<br></dt>
<dd><p>发行人可以将资产转回给自己</p>
</dd></dl>

<dl>
<dt>
<code>transfer_restricted</code> = 0x08<br></dt>
<dd><p>要求发行人是每次转移的一方</p>
</dd></dl>

<dl>
<dt>
<code>disable_force_settle</code> = 0x10<br></dt>
<dd><p>禁止强清</p>
</dd></dl>

<dl>
<dt>
<code>global_settle</code> = 0x20<br></dt>
<dd><p>允许bitasset发行者强制全球设置，这可以在权限中设置，但不能设置标志</p>
</dd></dl>

<dl>
<dt>
<code>disable_confidential</code> = 0x40<br></dt>
<dd><p>允许资产与机密交易一起使用</p>
</dd></dl>

<dl>
<dt>
<code>witness_fed_asset</code> = 0x80<br></dt>
<dd><p>允许资产由见证人提供</p>
</dd></dl>

<dl>
<dt>
<code>committee_fed_asset</code> = 0x100<br></dt>
<dd><p>允许资产由委员会提供 </p>
</dd></dl>