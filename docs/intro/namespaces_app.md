原文： http://dev.bitshares.works/en/master/api/namespaces/app.html

译者： bt666

## Graphene::App

namespace app

未命名组
    
    template <typename T>
    T dejsonify(const string &s, uint32_t max_depth)

使用JSON字符串的vectors向量集作为boost::program_options参数

类型定义


    typedef boost::multiprecision::uint256_t u256


方法
    
    void load_configuration_options(const fc::path &data_dir, const bpo::options_description &cfg_options, bpo::variables_map &options)
    
    void load_configuration_options(const fc::path &data_dir, const boost::program_options::options_description &cfg_options, boost::program_options::variables_map &options)

    u256to256(const fc::uint128 &t)
    
    fc::uint128 to_capped128(constu256 &t)
    
    string uint128_amount_to_string(const fc::uint128 &amount, const uint8_t precision)
    
    
    string price_to_string(const price &_price, const uint8_t base_precision, const uint8_t quote_precision)
    
    string price_diff_percent_string(const price &old_price, const price &new_price)
    
    class abstract_plugin
    
    #include <plugin.hpp>
    
     graphene::app::plugin的子类

公共方法

    virtual void plugin_initialize(const boost::program_options::variables_map &options) = 0

执行初始启动的例程，注册插件索引、回调等。

所有插件必须提供一个initial()方法，该方法会在应用程序启动后不久被调用。该方法应该包含设置代码，如初始化变量，向数据库中添加索引，注册数据库的回调方法，添加APIs等，以及在选项映射集中应用选项。

该方法在数据库打开前调用，因此任何需要链状态的例程都不能被该方法调用。这些例程应该在startup()中执行。

    参数

- options: 通过配置文件或命令行传给应用程序的选项

    virtual void plugin_startup() = 0

开始执行常规例程。

所有插件必须提供一个startup()方法，该方法会在应用程序启动完成后调用。方法中应该包含调度任务或获取链状态的代码。

    virtual void plugin_shutdown() = 0

干净地关闭插件

调用该方法来完全关闭插件(根据监听到的SIGTNT或者SIGTERM信号)

    virtual void plugin_set_app(application *a) = 0

使用插件注册应用的实例。

由框架调用来设置应用。
    
    virtual void plugin_set_program_options(boost::program_options::options_description &command_line_options, boost::program_options::options_description &config_file_options) = 0


填写插件支持的命令行参数。

该方法使用插件支持的命令行和配置文件选项来设置参数。如果插件不需要这些选项，可以简单地提供这个方法的空实现。

    参数

- command_line_options：插件支持在命令行上使用的所有选项
- config_file_options：插件支持存储在配置文件中的所有选项


application 类

 #include <application.hpp>

公共成员

    boost::signals2::signal<void()> syncing_finished

同步完成后发出的信号量(is_finished_syncing返回true)。

    class block_api 类

 #include <api.hpp>

api 块

 database_api 类

 #include <database_api.hpp>

database_api类为链数据库实现了RPC API。

该API公开了访问数据库的对象，它们的查询状态可被区块链验证节点追踪到。API是只读的。所有对数据库的操作都必须通过事务执行。使用network_broadcast_api广播事务。

公共方法

    fc::variants get_objects(const vector<object_id_type> &ids)const

由给定的IDs获取相应的对象。

如果给定的IDs不能匹配到任何对象，则返回null空对象。

返回值

按照ids中的查询顺序返回对象。

参数

- ids：被查询对象的IDs
    
    void set_subscribe_callback(std::function<void(const variant&)> cb, bool notify_remove_create, )

注册一个回调句柄，可以用来订阅对象数据库的变化。

参数

- cb: 所注册的回调句柄
- nofity_remove_create： 是否订阅通用对象的创建和删除事件。当设置为true时，API服务器会将最近所有新创建的对象和所有新删除对象的ID通知给客户端。默认情况下，API服务器不允许订阅通用事件，可以在服务器启动时更改该配置。

    void set_pending_transaction_callback(std::function<void(const variant &signed_transaction_object)> cb)

注册一个回调句柄，当事务推送到数据库时，可以接收到通知。

注意： 区块中的事务在执行前后可以多次推送到数据库或者从数据库中弹出。每次推送完，客户端会接到通知。

参数

- cb: 所注册的回调句柄


    void set_block_applied_callback(std::function<void(const variant &block_id)> cb)

注册一个回调句柄，当区块推送到数据库时，可以接收到通知。

- cb: 所注册的回调句柄

    void cancel_all_subscriptions()

停止接收任何通知。

取消之前订阅的所有市场和对象。


    optional<block_header> get_block_header(uint32_t block_num)const

检索区块头部信息

返回值

要检索的区块的头部信息，如果不存在匹配的对象，则返回null

参数

block_num: 应该返回的区块的高度
    
    map<uint32_t, optional<block_header>> get_block_header_batch(const vector<uint32_t> block_nums)const

通过区块数检索多个区块

返回值

要检索的区块的头部信息构成的数组，如果不存在匹配的对象，则返回null

参数

应该返回的区块的高度构成的向量集


    optional<signed_block> get_block(uint32_t block_num)const

检索一个完全的，带签名的节点

返回值

被检索到的区块，如果不存在匹配的对象，则返回null

参数

block_num: 应该返回的区块的高度

    processed_transaction get_transaction(uint32_t block_num, uint32_t trx_in_block)const

用来获取单个事务

    optional<signed_transaction> get_recent_transaction_by_id(const transaction_id_type &id)const

如果事务还没过期，则此方法将返回给定ID的事务，否则将返回NULL（如果未知）。 未知的事务并不意味着它没有包含在区块链中。


    chain_property_object get_chain_properties()const

检索与链关联的chain_property_object。


    global_property_object get_global_properties()const

检索当前的global_property_object。

    fc::variant_object get_config()const

检索编译时常量。


    chain_id_type get_chain_id()const

获取链ID


    dynamic_global_property_object get_dynamic_global_properties()const

检索当前的dynamic_global_property_object。

    检索当前的dynamic_global_property_object。

确定公钥的文本表示（Base-58格式）当前是否与区块链上的任何已注册（即非隐身）账户相关联

返回值

公钥是否已知

参数

	- public_key: 公钥

    account_id_type get_account_id_from_string(const std::string &name_or_id)const

从名称或ID获取账户对象。

返回值

账户ID

参数

account_name_or_id: 账户的ID或者名称

    vector<optional<account_object>> get_accounts(const vector<std::string> &account_names_or_ids)const

使用ID获取账户列表。

此函数的语义与[get_objects](http://dev.bitshares.works/en/master/api/namespaces/app.html#classgraphene_1_1app_1_1database__api_1a5a8b2fb34a9ef56da9825e15200720d1)相同

返回值

与给定ID对应的账户

参数

	- account_names_or_ids: 要检索的账户的ID或名称

    vector<limit_order_object> get_account_limit_orders(const string &account_name_or_id, const string &base, const string &quote, uint32_t limit = 101, optional<limit_order_id_type> ostart_id = optional<limit_order_id_type>(), optional<price> ostart_price = optional<price>())

获取与指定帐户和指定市场相关的所有订单，返回的结果按价格降序排序。

返回值

account_names_or_ids对应的账户的订单列表

注意

1. 如果account_name_or_id没有绑定到账户，则返回空列表
2. start_id和start_price可以为空，如果是这样，api将返回订单的“第一页”; 如果指定了start_id，则其价格将优先用于页面查询，否则将使用start_price; 如果start_id指定的订单被意外取消，可以协同使用start_id和start_price，在这种情况下，所返回的订单的价格可能低于或等于start_price，但订单的ID大于start_id

参数

- account_name_or_id: 要检索的账户的名称或ID
- base: 基础资产
- quote: 引用资产
- limit: 每个查询可以获取的项的限制，不大于101
- start_id: 启动订单的ID，获取价格低于或等于该订单，但订单ID大于此订单的订单
- start_price: 获取价格低于或等于该价格的订单

    std::map<string, full_account> get_full_accounts(const vector<string> &names_or_ids, bool subscribe)

获取与指定帐户相关的所有对象并订阅更新。

该方法获取给定帐户的所有相关对象，并订阅对给定帐户的更新。 如果names_or_ids中的任何字符串都没有绑定到帐户，则该输入将被忽略。 所有其他的账户将被检索和订阅。

返回值

从names_or_ids到相应帐户的字符串构成的Map集合

参数

- callback: 更新的函数调用
- names_or_ids: 每项必须是要检索的帐户的名称或ID

    vector<account_id_type> get_account_references(const std::string account_id_or_name)const
    
返回值

所有涉及到账户所有者权限或活跃权限中的私钥或账户ID的账户
    
    vector<optional<account_object>> lookup_account_names(const vector<string> &account_names)const

按名称获取帐户列表。

此函数的语义与[get_objects](http://dev.bitshares.works/en/master/api/namespaces/app.html#classgraphene_1_1app_1_1database__api_1a5a8b2fb34a9ef56da9825e15200720d1)相同

返回值

账户名映射到对IDs的Map集合

参数

- lower_bound_name: 返回的第一个名字的下界
- limit: 返回结果的元素个数不得超过1000。

    vector<asset> get_account_balances(const std::string &account_name_or_id, const flat_set<asset_id_type> &assets)const

获取账户中各种资产的余额

返回值

账户资产的余额

参数

- account_name_or_id: 要获取余额的帐户的ID或名称
- assets: 要获取余额的资产的ID; 如果为空，则获取帐户中所有资产的余额


    vector<balance_object> get_balance_objects(const vector<address> &addrs)const

返回值

与集合中的地址相关的无人认领的余额对象

    uint64_t get_account_count()const

获取区块链上已注册的帐户总数。

    vector<optional<asset_object>> get_assets(const vector<asset_id_type> &asset_ids)const
    
根据ID获取资产列表。

此函数的语义与[get_objects](http://dev.bitshares.works/en/master/api/namespaces/app.html#classgraphene_1_1app_1_1database__api_1a5a8b2fb34a9ef56da9825e15200720d1)同

返回值

给定ID对应的资产

参数

- asset_ids: 要检索的资产的ID集合

    vector<asset_object> list_assets(const string &lower_bound_symbol, uint32_t limit)const
    
根据符号名称按字母顺序获取资产。

返回值

查到的资产的集合

参数

- lower_bound_symbol:要检索的符号名称的下限
- limit: 要获取的资产个数的最大值（不得超过101）

    vector<optional<asset_object>> lookup_asset_symbols(const vector<string> &symbols_or_ids)const

根据符号获取资产列表。

此函数的语义与[get_objects](http://dev.bitshares.works/en/master/api/namespaces/app.html#classgraphene_1_1app_1_1database__api_1a5a8b2fb34a9ef56da9825e15200720d1)相同

返回值

给定符号或ID对应的资产

参数

要检索的资产的符号或字符串化的ID

    uint64_t get_asset_count()const

获取资产个数

返回值

资产的个数

    vector<limit_order_object> get_limit_orders(asset_id_type a, asset_id_type b, uint32_t limit)const

获取特定市场的限价单。

返回值

限价单集合，从最低价到最高价排好序

参数

- a: 正在出售的资产的ID
- b: 正在购买的资产的ID
- limit: 要检索的最大订单数

    vector<call_order_object> get_call_orders(asset_id_type a, uint32_t limit)const
    
给定资产的平仓单

返回 

平仓单集合，按照最早平仓到最近平仓的顺序排序

参数

- a: 被平仓的资产的ID
- limit: 要检索的最大订单数

    vector<force_settlement_object> get_settle_orders(asset_id_type a, uint32_t limit)const

获取给定资产的强制清算订单。

返回值

被清算的订单集合，按照最早清算到最近清算的顺序排序

参数

- a: 资产ID
- limit: 要检索的最大订单数
- start: 返回结果的偏移量

    vector<call_order_object> get_margin_positions(const std::string account_id_or_name)const

返回值

给定帐户ID或名称的所有未结算的保证金头寸。

    void subscribe_to_market(std::function<void(const variant&)> callback, asset_id_type a, asset_id_type b, )

当两个资产之间的市场活跃订单发生变化时，请求通知。

回调将传递<pair <operation，operation_result >>的集合变量。 集合包含改变市场的有序操作及其结果。

参数

- callback: 市场发生变化是调用的回调函数
- a: 第一个资产ID
- b: 第二个资产ID

    void unsubscribe_from_market(asset_id_type a, asset_id_type b)

取消订阅特定市场的更新。

参数

- a: 第一个资产ID
- b: 第二个资产ID

    market_ticker get_ticker(const string &base, const string &quote)const

返回市场assetA:assetB的报价

返回值

过去24小时内的市场报价

参数

- a: 第一个资产的字符串名称
- b: 第二个资产的字符串名称

    market_volume get_24_volume(const string &base, const string &quote)const

返回市场assetA:assetB的24小时成交量

回值

过去24小时内的市场成交量

参数

- a: 第一个资产的字符串名称
- b: 第二个资产的字符串名称

    order_book get_order_book(const string &base, const string &quote, unsigned limit = 50)const
    
返回市场base:quote的订单信息

返回值

市场的订单信息

参数

- base: 第一个资产的字符串名称
- quote: 第二个资产的字符串名称
- depth: 订单的深度。每个问价和竞价的最大深度，上限是50。优先考虑最适中的

    vector<market_volume> get_top_markets(uint32_t limit)const

返回按base_volume值递减排序的24小时市场交易量集合。 注意:这个API是实验性的，在下一个版本中可能会有变化。

返回值

递减排序的成交量集合

参数

- limit: 最大结果数

    vector<market_trade> get_trade_history(const string &base, const string &quote, fc::time_point_sec start, fc::time_point_sec stop, unsigned limit = 100)const

返回市场assetA:assetB的最近交易，按时间排序，最新的交易排在最前面。 范围是[结束，开始]注意：必须是UTC时区，暂不支持其他时区。

返回值

近期市场交易

参数

- a: 第一个资产的字符串名
- b: 第二个资产的字符串名
- stop: UNIX时间戳格式的结束时间，即要检索的最早交易的时间戳
- limit: 要检索的交易数，上限是100
- start: UNIX时间戳格式的开始时间，即要检索的最新交易的时间戳

    vector<market_trade> get_trade_history_by_sequence(const string &base, const string &quote, int64_t start, fc::time_point_sec stop, unsigned limit = 100)const

返回市场assetA：assetB的交易，按时间排序，最新交易排最前。 范围是[结束，开始]注意：必须是UTC时区，暂不支持其他时区。

返回值

市场交易

参数

- a: 第一个资产的字符串名
- b: 第二个资产的字符串名
- stop: UNIX时间戳格式的结束时间，即要检索的最早交易的时间戳
- limit: 要检索的交易数，上限是100
- start: 开始检索的交易偏移量整数值，要检索的最新交易

    vector<optional<witness_object>> get_witnesses(const vector<witness_id_type> &witness_ids)const

根据ID获取见证人列表。

此函数的语义与[get_objects](http://dev.bitshares.works/en/master/api/namespaces/app.html#classgraphene_1_1app_1_1database__api_1a5a8b2fb34a9ef56da9825e15200720d1)相同

返回值

给定IDs对应的见证人集合

参数

- 要检索的见证人IDs集合

    fc::optional<witness_object> get_witness_by_account(const std::string account_id_or_name)const

获取给定账户拥有的见证人

返回值

见证人对象，如果给定账户没有见证人，则返回null

参数

- account_id_or_name: 要检索见证人的帐户的ID

    map<string, witness_id_type> lookup_witness_accounts(const string &lower_bound_name, uint32_t limit)const

已注册见证人的名称或IDs

返回值

见证人名称与对应IDs的映射Map集合

参数

- lower_bound_name: 要返回的第一个名字的下限
- limit:返回结果的最大元素个数，不得超过1000

uint64_t get_witness_count()const

获取在区块链上已注册的见证人总数

    vector<optional<committee_member_object>> get_committee_members(const vector<committee_member_id_type> &committee_member_ids)const

根据ID获取理事会成员列表。

此函数的语义与[get_objects](http://dev.bitshares.works/en/master/api/namespaces/app.html#classgraphene_1_1app_1_1database__api_1a5a8b2fb34a9ef56da9825e15200720d1)相同

返回值

给定IDs对应的理事会成员

参数

- committee_member_ids：要检索的理事会成员的IDs

    fc::optional<committee_member_object> get_committee_member_by_account(const std::string account_id_or_name)const

获取给定账户拥有的理事会成员

返回值

参数

理事会成员对象，如果给定账户没有理事会成员，则返回null

参数

- account_id_or_name: 要检索理事会成员的帐户的ID

    map<string, committee_member_id_type> lookup_committee_member_accounts(const string &lower_bound_name, uint32_t limit)const

获取已注册的理事会成员的名称和IDs

返回值

理事会成员名称到对应IDs的映射Map集合

参数

- lower_bound_name: 要返回的第一个名字的下限
- limit:返回结果的最大元素个数，不得超过1000

    uint64_t get_committee_count()const

获取在区块链上已注册理事会的总数

    vector<worker_object> get_all_workers()const

返回值 

获取所有工人对象

    vector<optional<worker_object>> get_workers_by_account(const std::string account_id_or_name)const

获取给定账户拥有的工人

返回值

工人对象，如果给定账户没有工人，则返回null

参数

- account_id_or_name: 要检索工人的帐户的ID

    uint64_t get_worker_count()const

获取在区块链上已注册工人的总数

    vector<variant> lookup_vote_ids(const vector<vote_id_type> &votes)const

给定一组投票，返回他们投票的对象。

结果将是理事会成员对象，见证人对象和工人对象的混合体。

结果将与投票的顺序相同。如果未找到的任何投票IDs，则返回Null。


    std::string get_transaction_hex(const signed_transaction &trx)const

获取事务的序列化二进制形式的hexdump。

    std::string get_transaction_hex_without_sig(const signed_transaction &trx)const

获取剔除签名的事务的序列化二进制形式的hexdump。

    set<public_key_type> get_required_signatures(const signed_transaction &trx, const flat_set<public_key_type> &available_keys)const

这个API将接受一个部分签名的事务和一组公钥，所有者可以对这些公钥进行签名并返回应该向事务添加签名的最小公钥子集。


    set<public_key_type> get_potential_signatures(const signed_transaction &trx)const


此方法将返回所有可能为给定事务签名的公钥集。在调用[get_required_signatures](http://dev.bitshares.works/en/master/api/namespaces/app.html#classgraphene_1_1app_1_1database__api_1a9ae2eb6a83c27a7b4eec2b00ee8ba371)获取最小子集之前，钱包可以使用调用该方法将其公钥集过滤成相关的子集。

    bool verify_authority(const signed_transaction &trx)const

返回值

如果trx具有所有必需的签名，则返回true，否则抛出异常

    bool verify_account_authority(const string &account_name_or_id, const flat_set<public_key_type> &signers)const

返回值

如果签名者有足够的权限授权帐户，则返回true

    processed_transaction validate_transaction(const signed_transaction &trx)const
    
针对当前状态验证事务而不在网络上广播它

    vector<fc::variant> get_required_fees(const vector<operation> &ops, asset_id_type id)const

对于每个操作，计算指定资产类型中的所需费用。 如果资产类型没有有效的core_exchange_rate

    vector<proposal_object> get_proposed_transactions(const std::string account_id_or_name)const

返回值

与指定帐户ID相关的提议交易集。

    vector<blinded_balance_object> get_blinded_balances(const flat_set<commitment_type> &commitments)const

返回值

委托ID的盲余额对象集

    vector<withdraw_permission_object> get_withdraw_permissions_by_giver(const std::string account_id_or_name, withdraw_permission_id_type start, uint32_t limit)const

为付款人获取未过期的提现权限对象（例如：定期客户）

返回值

账户提现权限对象

参数	

- account: 从中获取对象的帐户ID或名称
- start: 在结果中跳过此ID之前，提现权限对象（1.12.X）。 用于分页。
- limit: 要检索的最大对象数

    vector<withdraw_permission_object> get_withdraw_permissions_by_recipient(const std::string account_id_or_name, withdraw_permission_id_type start, uint32_t limit)const

获取收款人的未过期提现权限对象（例如：服务提供商）	

返回值

账户提现权限对象

参数

- account: 从中获取对象的帐户ID或名称
- start: 在结果中跳过此ID之前，提现权限对象（1.12.X）。 用于分页。
- limit: 要检索的最大对象数

    class database_api_impl : public std::enable_shared_from_this<database_api_impl>

公共方法

    vector<vector<account_id_type>> get_key_references(vector<public_key_type> key)const

返回值

所有在其所有者或主动权限中引用密钥或帐户ID的帐户。

    vector<limit_order_object> get_limit_orders(asset_id_type a, asset_id_type b, uint32_t limit)const
	
返回值

给定两种资产的账本双方的限价单，用于限制每一方的订单数 

    vector<proposal_object> get_proposed_transactions(const std::string account_id_or_name)const

TODO：添加加速处理过程的二级索引

    void on_objects_new(const vector<object_id_type> &ids, const flat_set<account_id_type> &impacted_accounts)

每当区块用于报告已变化的对象时调用

    void on_applied_block()

注意：此方法无法生成，因为它是在应用区块的过程中调用的。

    struct get_required_fees_helper

用于实现具有潜在嵌套提议的get_required_fees（）的相互递归函数的容器方法

    class history_api

*#include <api.hpp>*

[history_api](http://dev.bitshares.works/en/master/api/namespaces/app.html#classgraphene_1_1app_1_1history__api)类为帐户历史记录实现RPC API。

此API包含访问帐户历史记录的方法

公共方法

    vector<operation_history_object> get_account_history(const std::string account_id_or_name, operation_history_id_type stop = operation_history_id_type(), unsigned limit = 100, operation_history_id_type start = operation_history_id_type())const

获取给定账户相关的操作

返回值

帐户执行的操作列表，按最新时间到最晚排序。

参数

- account_id_or_name: 应查询其历史记录的帐户ID或名称
- stop: 要检索的最早操作的ID
- limit: 要检索的最大操作数（不得超过100）
- start: 要检索的最新操作的ID

    history_operation_detail get_account_history_by_operations(const std::string account_id_or_name, vector<uint16_t> operation_types, uint32_t start, unsigned limit)

按操作类型获取与指定帐户筛选相关的操作。

返回值

history_operation_detail

参数

- account_id_or_name: 应查询其历史记录的帐户ID或名称
- operation_types: 我们想要在帐户中获取操作的操作的ID（0 =转账，1 =限价单创建，...）
- start: 开始循环抛出历史记录的序列号
- limit: 要返回的最大条目数（从起始编号）

    vector<operation_history_object> get_account_history_operations(const std::string account_id_or_name, int operation_id, operation_history_id_type start = operation_history_id_type(), operation_history_id_type stop = operation_history_id_type(), unsigned limit = 100)const

仅获取与指定帐户相关的被询问操作。

返回值

按帐户执行的操作列表，从最近到最旧排序。

参数

- account_id_or_name: 应查询其历史记录的帐户ID或名称
- operation_id: 我们想要在帐户中获取操作的操作的ID（0 =转账，1 =限价单创建，...）
- stop: 要检索的最旧操作的ID
- limit: 要检索的最大操作数（不得超过100）
- start: 要检索的最新操作的ID

    vector<operation_history_object> get_relative_account_history(const std::string account_id_or_name, uint32_t stop = 0, unsigned limit = 100, uint32_t start = 0)const

获取与特定于该帐户的事件编号引用的指定帐户相关的操作。 可以在帐户统计信息中找到帐户的当前操作数（或使用0表示开始）。

返回值

按帐户执行的操作列表，从最近到最旧排序。

参数

- account_id_or_name: 应查询其历史记录的帐户ID或名称
- stop: 要检索的最旧操作的ID。默认值是0，同时会查询操作的“限制”数
- limit: 要检索的最大操作数（不得超过100）
- start: 要检索的最新操作的ID。默认值是0，它将从最近的操作开始查询

    struct limit_order_group

*#include <api.hpp>*

限价单组的摘要数据

公共成员

    price min_price

组中可能的最低价

    price max_price

组中可能的最高价

    share_type total_for_sale

待售资产总额，资产ID为min_price.base.asset_id

class login_api

*#include <api.hpp>*
[
login_api](http://dev.bitshares.works/en/master/api/restrictions.html#classgraphene_1_1app_1_1login__api)类实现RPC API的底层。

必须从此API请求所有其他API。

公共方法

    bool login(const string &user, const string &password)

授权RPC服务器

返回值

登录成功返回true；否则，返回false

注意

必须在请求其他API之前调用此方法。 在客户端进行成功身份验证之前，可能无法访问其他API。

参数

- user: 登录的用户名
- password: 登录的密码

    fc::api<block_api> block()const

检索网络区块的API

    fc::api<network_broadcast_api> network_broadcast()const

检索网络广播的API

    fc::api<database_api> database()const

检索数据库的API

    fc::api<history_api> history()const

检索历史的API

    fc::api<network_node_api> network_node()const

检索网络节点的API

    fc::api<crypto_api> crypto()const

检索加密的API


    fc::api<asset_api> asset()const

检索资产的API

    fc::api<orders_api> orders()const

检索订单的API

    fc::api<graphene::debug_witness::debug_api> debug()const

检索代码漏洞的API(如果可用的话)

    void enable_api(const string &api_name)

被调用以启用API。不是反射的

    class network_broadcast_api : public std::enable_shared_from_this<network_broadcast_api>

*#include <api.hpp>*

[network_broadcast_api](http://dev.bitshares.works/en/master/api/namespaces/app.html#classgraphene_1_1app_1_1network__broadcast__api)类允许广播事务。

公共方法

    void broadcast_transaction(const signed_transaction &trx)

广播交易到网络

在广播之前，将在本地数据库中检查交易的有效性。 如果它无法在本地应用，则会抛出错误并且不会广播该交易

参数

- trx: 要广播的交易

    void broadcast_transaction_with_callback(confirmation_callback cb, const signed_transaction &trx)

此版本的广播交易注册了一个回调方法，当交易包含在块中时将调用该方法。 回调方法包括块中的交易id，块号和交易号。

    fc::variant broadcast_transaction_synchronous(const signed_transaction &trx)

此版本的广播交易注册了一个回调方法，当交易包含在块中时将调用该方法。 回调方法包括块中的交易id，块号和交易号。

    void on_applied_block(const signed_block &b)

不是反射的，因此API客户端无法访问。

注册此函数以在接收到块时从链数据库接收applied_block信号。 然后，它会向已请求在块中包含特定txid时通知的客户端发送回调。

    class network_node_api

*#include <api.hpp>*

[network_node_api](http://dev.bitshares.works/en/master/api/namespaces/app.html#classgraphene_1_1app_1_1network__node__api)类允许维护p2p连接。

公共方法

    fc::variant_object get_info()const

返回通用的网络信息，例如P2P端口

    void add_node(const fc::ip::endpoint &ep)

add_node连接到一个新的节点

参数

- ep: 要连接的节点的IP/端口

    std::vector<net::peer_status> get_connected_peers()const

获取与节点的所有当前连接的状态。

    fc::variant_object get_advanced_node_parameters()const

获取高级节点参数，例如所需和最大连接数。

    void set_advanced_node_parameters(const fc::variant_object &params)

设置高级节点参数，例如所需和最大连接数。

参数

- params: 包含要设置的参数的名称/值对的JSON对象

    std::vector<net::potential_peer_record> get_potential_peers()const

返回潜在节点的列表

    class orders_api

*#include <api.hpp>*

[orders_api](http://dev.bitshares.works/en/master/api/namespaces/app.html#classgraphene_1_1app_1_1orders__api)类公开了对使用分组订单插件处理的数据的访问。

公共方法

    flat_set<uint16_t> get_tracked_groups()const

获取服务器配置的跟踪组。

返回值

表示已配置组的数字列表，其中1表示价格为0.01％差异

    vector<limit_order_group> get_grouped_limit_orders(asset_id_type base_asset_id, asset_id_type quote_asset_id, uint16_t group, optional<price> start, uint32_t limit)const

在给定市场中获取分组限价单。

返回值

分组限价单，从最佳报价到最差报价

参数

- base_asset_id: 正在出售的资产的ID
- quote_asset_id: 正在购买的资产的ID
- group: 每个订单组中的最高价格差异必须是配置值之一
- start: 可选价格，表示要检索的第一个订单组
- limit: 要检索的最大订单组数量（不得超过101）

    class plugin : public graphene::[app](http://dev.bitshares.works/en/master/api/namespaces/app.html#_CPPv3N8graphene3appE)::[abstract_plugin](http://dev.bitshares.works/en/master/api/namespaces/app.html#_CPPv3N8graphene3app15abstract_pluginE)

*#include <api.hpp>*

提供[abstract_plugin](http://dev.bitshares.works/en/master/api/namespaces/app.html#classgraphene_1_1app_1_1abstract__plugin)函数的基本默认实现。

公共方法

    void plugin_initialize(const boost::program_options::variables_map &options)

执行早期启动例程并注册插件索引，回调等。

插件必须提供一个方法initialize（），它将在应用程序启动时尽早调用。 此方法应包含早期设置代码，例如初始化变量，向数据库添加索引，从数据库注册回调方法，添加API等，以及在选项映射中应用任何选项

在数据库打开之前调用此方法，因此任何需要链状态的例程都不能通过此方法调用。 这些例程应该在startup（）中执行。

参数：

- options: 通过配置文件或命令行传递给应用程序的选项

    void plugin_startup()

开始正常运行时操作。

插件必须提供一个方法startup（），它将在应用程序启动结束时调用。 此方法应包含调度任何任务或需要链状态的代码。

    void plugin_shutdown()

干净地关闭插件。

调用它来请求干净关闭（例如由于SIGINT或SIGTERM）。

    void plugin_set_app([application](http://dev.bitshares.works/en/master/api/namespaces/app.html#_CPPv3N8graphene3app11applicationE) *a)

使用插件注册应用程序实例。

这由框架调用以设置应用程序。

    void plugin_set_program_options(boost::program_options::options_description &command_line_options, boost::program_options::options_description &config_file_options)

填写插件使用的命令行参数。

此方法使用插件支持的任何命令行和配置文件选项填充其参数。 如果插件不需要这些选项，它可能只是提供此方法的空实现。

参数

- command_line_options: 此插件支持命令行的所有选项
- config_file_options: 此插件支持存储在配置文件中的所有选项

命名空间 detail

方法

graphene::[chain](http://dev.bitshares.works/en/master/api/namespaces/chain.html#_CPPv3N8graphene5chainE)::[genesis_state_type](http://dev.bitshares.works/en/master/api/namespaces/chain.html#_CPPv3N8graphene5chain18genesis_state_typeE)create_example_genesis()

    class application_impl : public node_delegate

*#include <api.hpp>*

公共方法

    bool has_item(const net::item_id &id)

如果受托人具有该项目，则网络无需获取它

    bool handle_block(const graphene::net::block_message &blk_msg, bool sync_mode, std::vector<fc::uint160_t> &contained_transaction_message_ids)

允许应用程序在向对等方广播之前验证项目。

返回值

如果此消息导致区块链切换分叉，则返回true;否则返回false

参数

- sync_mode: 如果消息是通过同步过程获取的，则为true，在正常操作期间为false

异常

- exception:如果错误验证该项目，否则该项目可以安全地进行广播。

    std::vector<item_hash_t> get_block_ids(const std::vector<graphene::net::item_hash_t> &blockchain_synopsis, uint32_t &remaining_item_count, uint32_t limit)

假设所有数据元素都以某种方式排序，此方法应该返回限制在我们识别的概要中的最后一个ID之后发生的ID。

返回时，remaining_item_count将设置为结果中返回的最后一项之后区块链中的项目数，如果结果包含区块链中的最后一项，则为0

    message get_item(const graphene::net::item_id &id)

给定所请求数据的哈希值，获取正文。

    std::vector<item_hash_t> get_blockchain_synopsis(const graphene::net::item_hash_t &reference_point, uint32_t number_of_blocks_after_reference_point)

返回用于同步的区块链的概要。这包括块状哈希列表，其间隔以指数方式朝向生成块增加。当同步到对等体时，对等体使用这些数据来确定我们是否与它们在同一个分支上，如果不是，它们需要向我们发送什么阻止它们才能让我们使用它们。

在过度简化的案例中，这是我们目前首选区块链的直接概要;当我们第一次连接到同伴时，这就是我们要发送的内容。它看起来像这样：如果区块链为空，它将返回空列表。如果区块链有一个区块，它将返回一个仅包含该区块的列表。如果它包含多个块：列表中的第一个元素将是我们无法撤消的最高编号块的哈希值第二个元素将是区块链的可撤销段中的中间点处的项目的哈希值第三个是通过块链的可撤销段的~3 / 4，第四个将在~7 / 8 ...＆c。列表中的最后一项将是我们首选链上最新块的哈希值，因此如果区块链有26个块标记为a-z，则概要为：anuxz的想法是通过发送一个小（<30）数字对于块ID，我们可以总结一个巨大的区块链。块ID在链的末端附近更密集，因为我们在第一次连接时更可能几乎同步，并且叉可能很短。如果我们在示例中同步的对等体是在块'v'开始的fork上，那么它们将回复我们的概要，其中包含从块'u'开始的所有块的列表，最后一块它们知道我们有共同点。

在实际代码中，有几个复杂的问题。

首先，作为优化，我们通常不会发送整个区块链的概要，我们只发送我们有撤销数据的区块链的概要。如果他们的fork没有在我们的撤消历史记录中构建，我们将无法切换，因此没有理由获取块。

其次，当一个同行回复我们的初步概要并给我们一个他们认为我们缺失的块的列表时，他们只会一次发送几千块的块。在我们得到那些块ID之后，我们需要通过发送另一个概要来请求更多的块（我们不能只说“发送给我

下一个2000 ids“因为他们可能已经切换了自己的叉子而他们没有追踪他们发送给我们的东西”。为了提高性能，我们希望首先获得相当长的块ID列表，然后开始下载块。对等体不处理与初始请求有任何不同的后续块id请求;它将我们发送的概要视为我们的区块链，并将其响应完全基于此。因此，为了获得我们想要的响应（在他们发送给我们的最后一个块之后的下一个块id，或者，如果失败，它们发送的最后一个块id的最短分叉），我们需要构建一个概要，就好像我们的区块链由以下部分组成：



1. 我们的块链中的块直到fork点（如果有一个fork）或head块（如果没有fork）

1. 我们已经从他们的叉子推出的块（如果有一个叉子）

1. 他们先前发送给我们的块ID在p2p代码中处理了段3，它只是告诉我们它具有的块数（在number_of_blocks_after_reference_point中），因此我们可以在它们的概要中留出空间。 我们负责从我们的活动区块链和fork数据库构建段1和段2的概要。 reference_point参数是该对等体中已成功推送到区块链的最后一个块，因此它告诉我们对等体是在fork还是在主链上。

    void sync_status(uint32_t item_type, uint32_t item_count)

在调用handle_message成功后调用此方法。

参数

- item_type: 我们正在同步的项目类型将与传递给sync_from（）调用的项目相同
- item_count: 尚未发送到handle_item（）的节点已知的项目数。 在item_count更多调用handle_item（）之后，该节点将处于同步状态

    void connection_count_changed(uint32_t c)

每当连接的对等体数量发生变化时进行呼叫。

    fc::time_point_sec get_block_time(const graphene::net::item_hash_t &block_id)

返回块生成的时间（如果block_id = 0，则返回生成时间）。 如果我们不知道块，返回time_point_sec :: min（）

命名空间 impl

方法

    template <typename T>
    T dejsonify(const string &s)