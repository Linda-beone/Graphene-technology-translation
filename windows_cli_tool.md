.. _cli-tool:

===========================================

 **原文链接**：<http://dev.bitshares.works/en/master/development/installation/windows_cli_tool.html>

**译者**：https://github.com/ArcticCat1220 （北极猫）

**校对者**：

Windows (x64)命令行钱包
=======================

::: {.contents}
目录
:::

------------------------------------------------------------------------

在本节中，我们将会向您展示如何安装Windows命令行钱包工具，并以多种方式从区块链中获取数据。

如何安装BitShares Core
----------------------

### Windows (x64)命令行钱包

1.  下载BitShares-Core-\*-x64-cli-tools.zip文件，链接为https://github.com/bitshares/bitshares-core/releases
2.  解压该文件至您的工作站。 (例如d:BitShares)
3.  从Mozilla中下载\**cacer.pem*\*文件，链接为https://curl.haxx.se/docs/caextract.html
4.  保存文件至您解压命令行工具的相同文件夹下。 (例如d:BitShares)
5.  打开Windows命令行(cmd.exe)
6.  进入您的安装目录(例如d:BitShares)
7.  设置环境变量，键入: [set
    SSL\_CERT\_FILE=d:/BitShares/cacert.pem]{.title-ref}
8.  启动钱包：\`cli\_wallet -s <wss://%7BANY> VALID AUTHORITY}/ws\`。
    例如， [cli\_wallet -s
    wss://bitshares.openledger.info/ws]{.title-ref}

------------------------------------------------------------------------

启动命令行钱包（通过Windows命令行）
-----------------------------------

**注意：** 我们使用\*the public API node of
OpenLedger\*并通过安全的websocket建立连接。

    >cli_wallet -s wss://bitshares.openledger.info/ws

如果您成功打开了命令行钱包，您将会看到相似的信息并收到`new >>>`的提示。请设置密码并解锁命令行钱包:

    Logging RPC to file: logs\rpc\rpc.log
    2029115ms th_a       main.cpp:136                  main                 ] key_to_wif( committee_private_key ): 5KCBDTcyDqzsqehcb52tW5nU6pXife6V2rX9Yf7c3saYSzbDZ5W
    2029119ms th_a       main.cpp:140                  main                 ] nathan_pub_key: BTS6MRyAjQq8ud7hVNYcfnVPJqcVpscN5So8BhtHuGYqET5GDW5CV
    2029121ms th_a       main.cpp:141                  main                 ] key_to_wif( nathan_private_key ): 5KQwrPbwdL6PhXujxW37FSSQZ1JiwsST4cqQzDeyXtP79zkvFD3
    Starting a new wallet with chain ID 4018d7844c78f6a6c41c6a552b898022310fc5dec06da467ee7905a8dad512c8 (from egenesis)
    2029128ms th_a       main.cpp:188                  main                 ] wdata.ws_server:   wss://bitshares.openledger.info/ws
    2029807ms th_a       main.cpp:193                  main                 ] wdata.ws_user:  wdata.ws_password:
    Please use the set_password method to initialize a new wallet before continuing
    new >>> 

-   `set_password`设置密码

例如我们用\`superaecret123\`作为密码:

    new >>> set_password supersecret123
    set_password supersecret123
    null
    locked >>>

-   `unlock` - 解锁命令行钱包:

        locked >>> unlock "supersecret123"
        unlock "supersecret123"
        null
        unlocked >>>

在此之后，您将可以使用命令行钱包（钱包API）里的任何可操作指令，或者手动构建您自己的交易了。
\-\-\-\-\--

**问题. 如何以纯净模式关闭命令行钱包？**

-   虽然在Windows中，关闭整个窗口将产生令人厌烦的异常，但您可以通过\**ctrl-c或者ctrl-d*\*来停止该进程。

------------------------------------------------------------------------

示例
----

一些可尝试的指令

-   

    \[General\](\#general)

    :   -   about
        -   help
        -   gethelp

-   

    \[Wallet Calls\](\#wallet-calls)

    :   -   is\_new
        -   is\_locked

-   

    \[Account Calls\](\#account-calls)

    :   -   list\_account\_balances
        -   get\_account
        -   get\_account\_id

-   

    \[Asset Calls\](\#asset-calls)

    :   -   list\_asset
        -   get\_asset

### 综述

-   `about`

<!-- -->

    unlocked >>> about
    about
    {
      "client_version": "2.0.180328",
      "graphene_revision": "92eb45cbd3e61c163561b0e6cf7fc99c633e4fcf",
      "graphene_revision_age": "14 days ago",
      "fc_revision": "4441e1480778d8e98c4d0322b0e42b19752640b5",
      "fc_revision_age": "16 days ago",
      "compile_date": "compiled on Mar 28 2018 at 20:47:02",
      "boost_version": "1.57",
      "openssl_version": "OpenSSL 1.0.2l  25 May 2017",
      "build": "win32 64-bit"
    }

-   `about` - \_[error]() (\_\*You don\'t need to add prentacises
    \'()\'.\_)

<!-- -->

    unlocked >>> about ()
    about ()
    4 parse_error_exception: Parse Error
    Unexpected char '40' in ""
        {"c":40,"s":""}
        th_a  json_relaxed.hpp:675 fc::json_relaxed::variant_from_stream

        {"str":"about () "}
        th_a  json.cpp:474 fc::json::variants_from_string
    unlocked >>>

-   `help`

<!-- -->

    unlocked >>> help
    help
        variant_object about()
        void add_operation_to_builder_transaction(transaction_handle_type transaction_handle, const operation & op)
        signed_transaction approve_proposal(const string & fee_paying_account, const string & proposal_id, const approval_delta & delta, bool broadcast)
        transaction_handle_type begin_builder_transaction()
        ...
        ...
        signed_transaction whitelist_account(string authorizing_account, string account_to_list, account_whitelist_operation::account_listing new_listing_status, bool broadcast)
        signed_transaction withdraw_vesting(string witness_name, string amount, string asset_symbol, bool broadcast)

    unlocked >>>

-   `gethelp` - \"list\_accounts\"

<!-- -->

    unlocked >>> gethelp "list_accounts"

    Lists all accounts registered in the blockchain. This returns a list of all account names and their account ids, sorted by account name.
    Use the 'lowerbound' and limit parameters to page through the list. To retrieve all accounts, start by setting 'lowerbound' to the empty string '""', and then each iteration, pass the last account name returned as the 'lowerbound' for the next 'list_accounts()' call.

    Parameters:
        lowerbound: the name of the first account to return. If the named account does not exist, the list will start at the account that comes after 'lowerbound' (type: const string &) limit: the maximum number of accounts to return (max: 1000) (type: uint32_t)

    Returns
        a list of accounts mapping account names to account ids

    unlocked >>>

-   `gethelp` - \"save\_wallet\_file\"

<!-- -->

    unlocked >>> gethelp "save_wallet_file"
    gethelp "save_wallet_file"

    Saves the current wallet to the given filename.

    Parameters:
        wallet_filename: the filename of the new wallet JSON file to create or overwrite. If 'wallet_filename' is empty, save to the current filename. (type: string)

    unlocked >>>

### 钱包指令

-   `is_new`

<!-- -->

    unlocked >>> is_new
    is_new
    false
    unlocked >>>

-   `is_locked`

<!-- -->

    unlocked >>> is_locked
    is_locked
    false
    unlocked >>>

### 账户指令

\_ `list_account_balances` \_(e.g. an existing user account
\"cedar036)\_

    unlocked >>> list_account_balances "cedar036"
    list_account_balances "cedar036"
    75.22668 BTS
    3 OPEN.STEEM

    unlocked >>>

-   `get_acount`

<!-- -->

    unlocked >>> get_account "cedar036"
    get_account "cedar036"
    {
      "id": "1.2.539269",
      "membership_expiration_date": "1970-01-01T00:00:00",
      "registrar": "1.2.96393",
      "referrer": "1.2.96393",
      "lifetime_referrer": "1.2.96393",
      "network_fee_percentage": 2000,
      "lifetime_referrer_fee_percentage": 3000,
      "referrer_rewards_percentage": 0,
      "name": "cedar036",
      "owner": {
        "weight_threshold": 1,
        "account_auths": [],
        "key_auths": [[
            "BTS7pRNvabZG2uU5SS9AyUWmsCdKijC1tHjCtiTDH7AeRkdH4qoVk",
            1
          ]
        ],
        "address_auths": []
      },
      "active": {
        "weight_threshold": 1,
        "account_auths": [],
        "key_auths": [[
            "BTS8jpGqVjRYytbguEZd4ao9CqdEUiMjkwfo9pgUE8RofW7higmQx",
            1
          ]
        ],
        "address_auths": []
      },
      "options": {
        "memo_key": "BTS8jpGqVjRYytbguEZd4ao9CqdEUiMjkwfo9pgUE8RofW7higmQx",
        "voting_account": "1.2.96393",
        "num_witness": 0,
        "num_committee": 0,
        "votes": [],
        "extensions": []
      },
      "statistics": "2.6.539269",
      "whitelisting_accounts": [],
      "blacklisting_accounts": [],
      "whitelisted_accounts": [],
      "blacklisted_accounts": [],
      "owner_special_authority": [
        0,{}
      ],
      "active_special_authority": [
        0,{}
      ],
      "top_n_control_flags": 0
    }
    unlocked >>>

-   `get_account_id`

<!-- -->

    unlocked >>> get_account_id "cedar036"
    get_account_id "cedar036"
    "1.2.539269"
    unlocked >>>

-   `get_account_history`

<!-- -->

    unlocked >>> get_account_history "cedar036" "4"
    get_account_history "cedar036" "4"
    2018-01-12T23:48:57 Transfer 40.46468 BTS from blocktrades to cedar036   (Fee: 0.01662 BTS)    
    2018-01-12T22:31:00 Transfer 3 OPEN.STEEM from tsugimoto0105 to cedar036   (Fee: 0.01662 BTS)
    2017-12-25T18:56:03 Transfer 0.00100 BTS from cedar036 to sharebits17 -- could not decrypt memo   (Fee: 0.23700 BTS)
    2017-12-25T17:44:15 Transfer 35 BTS from bitshares-users to cedar036   (Fee: 0.21851 BTS)

    unlocked >>>

-   `get_account_history` - \_error (\*bad parameter format)\_

<!-- -->

    unlocked >>> get_account_history "cedar036, 7"
    get_account_history "cedar036, 7"
    10 assert_exception: Assert Exception
    a0 != e: too few arguments passed to method
        {}
        th_a  api_connection.hpp:169 fc::generic_api::call_generic
    unlocked >>>

### 资产指令

-   list\_assets:

        unlocked >>> list_assets "BTS" "2"
        list_assets "BTS" "2"
        [{
            "id": "1.3.0",
            "symbol": "BTS",
            "precision": 5,
            "issuer": "1.2.3",
            "options": {
              "max_supply": "360057050210207",
              "market_fee_percent": 0,
              "max_market_fee": "1000000000000000",
              "issuer_permissions": 0,
              "flags": 0,
              "core_exchange_rate": {
                "base": {
                  "amount": 1,
                  "asset_id": "1.3.0"
                },
                "quote": {
                  "amount": 1,
                  "asset_id": "1.3.0"
                }
              },
              "whitelist_authorities": [],
              "blacklist_authorities": [],
              "whitelist_markets": [],
              "blacklist_markets": [],
              "description": "",
              "extensions": []
            },
            "dynamic_asset_data_id": "2.3.0"
          },{
            "id": "1.3.368",
            "symbol": "BTS.SHARE",
            "precision": 4,
            "issuer": "1.2.31073",
            "options": {
              "max_supply": "100000000000000",
              "market_fee_percent": 0,
              "max_market_fee": "1000000000000000",
              "issuer_permissions": 79,
              "flags": 128,
              "core_exchange_rate": {
                "base": {
                  "amount": 0,
                  "asset_id": "1.3.0"
                },
                "quote": {
                  "amount": 0,
                  "asset_id": "1.3.0"
                }
              },
              "whitelist_authorities": [],
              "blacklist_authorities": [],
              "whitelist_markets": [],
              "blacklist_markets": [],
              "description": "share of btsbots",
              "extensions": []
            },
            "dynamic_asset_data_id": "2.3.368"
          }
        ]
        unlocked >>>

\>
注意：如需列出所有资产，您需要将下界赋值为空字符串并作为列表的开头部分，且循环操作是必须的。

-   `get_asset`:

        unlocked >>> get_asset "BTS"
        get_asset "BTS"
        {
          "id": "1.3.0",
          "symbol": "BTS",
          "precision": 5,
          "issuer": "1.2.3",
          "options": {
            "max_supply": "360057050210207",
            "market_fee_percent": 0,
            "max_market_fee": "1000000000000000",
            "issuer_permissions": 0,
            "flags": 0,
            "core_exchange_rate": {
              "base": {
                "amount": 1,
                "asset_id": "1.3.0"
              },
              "quote": {
                "amount": 1,
                "asset_id": "1.3.0"
              }
            },
            "whitelist_authorities": [],
            "blacklist_authorities": [],
            "whitelist_markets": [],
            "blacklist_markets": [],
            "description": "",
            "extensions": []
          },
          "dynamic_asset_data_id": "2.3.0"
        }
        unlocked >>>

-   `get_asset` - \_error (\*need to use capital letters)\_

<!-- -->

    unlocked >>> get_asset "bts"
    get_asset "bts"
    10 assert_exception: Assert Exception
    a:
        {}
        th_a  wallet.cpp:3076 graphene::wallet::wallet_api::get_asset
    unlocked >>>

| 

| 
