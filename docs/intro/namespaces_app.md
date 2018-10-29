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

















































