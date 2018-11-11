  **原文链接**：[http://dev.bitshares.works/en/master/bts_guide/migration/howto-importing-wallet.html#howto-importing-wallet](http://dev.bitshares.works/en/master/bts_guide/migration/howto-importing-wallet.html#howto-importing-wallet)
 
 **译者**：[https://github.com/b-t-s-1](b-t-s-1)
 
 **校对者**： 
  
***  

[BItShares Developers Portal](../../index.html) master

Introduction :

* [Introduction & Architectures](../../intro/architectures.html)
* [Development Environments](../../intro/environments.html)
* [System Components Elements](../../components/components.html)

Development

* [Getting Started](../../development/installation.html)
* [BitShares Accounts](../accounts/bts_account.html)
* [Integration Guide](../../development/integration.html)
* [Blockchain Interaction](../../development/interaction.html)
* [Testnets](../../development/testnets.html)
* [Support and Optimizations](../../supports_dev/supports.html)
* [BSIPs](../../development/bsip.html)

Guides

* [Developer Guide (Tutorials)](../tutorials/index.html)
* [Frequently Asked Questions (FAQ)](../index_faq.html)
* [BitShares Developers' Contributions](../../references/tech_articles.html)
* [Articles](../../references/tech_articles.html#articles)
* [BitShares Repositories](../../references/githubrepo.html)

API

* [1. API Guide](../../api/api_about.html)
* [2. API Access and Restrictions](../../api/restrictions.html)
* [3. Available Calls and Often used API](../../api/calls.html)
* [4. Blockchain API(s)](../../api/blockchain_api.html)
* [5. Wallet API - Calls](../../api/wallet_api.html)
* [6. Objects and IDs](../../api/objects_ids.html)
* [7. Graphene - Namespaces](../../api/namespaces.html)

References

* [BitShares Community](../../references/info_comunities.html)
* [BitShares Blockchain](../../references/info_comunities.html#bitshares-blockchain)
* [BitShares Community Events](../../references/info_bts_events.html)
* [Glossary](../../knowledge_base/glossary.html)
* [Knowledge Base](../../knowledge_base/index_kb.html)

[BItShares Developers Portal](../../index.html)

* [Docs](../../index.html) >>
* Importing Your Wallet
* [Edit on GitHub](https://github.com/bitshares/dev.bitshares.works/blob/master/docs/bts_guide/migration/howto-importing-wallet.rst)

***

# Importing Your Wallet [¶](#importing-your-wallet "Permalink to this headline")

Contents

* [Web Wallet](#web-wallet)
* [CLI wallet](#cli-wallet)
  - [Claiming Balances](#claiming-balances)
  - [Manually claim balances](#manually-claim-balances)

## [Web Wallet](#id1) [¶](#web-wallet "Permalink to this headline")

The web wallet of BitShares 2.0 has a **Wallet management Console.** that will
help you import your funds. It can be access via <cite>BitShares 2.0: Settings -> Wallets</cite>

In order to import your existing accounts and claim all your funds you need to
choose `<span class="pre">Import</span> <span class="pre">Keys</span>`.

Note

If loading the file files with `<span class="pre">invalid</span> <span class="pre">format</span>` please ensure that
you have followed the steps described [Exporting Your Wallet](howto-exporting-wallet.html) and make
sure to click `<span class="pre">Import</span> <span class="pre">Keys</span>` and **not** `<span class="pre">Restore</span> <span class="pre">Backup</span>`.

Here you can provide the wallet backup file produced from BitShares 0.9.3c and
the pass phrase. Depending on the size of your import file, this step may take
some time to auto-complete. Please be patient.

The wallet will list all of your accounts including the number of private keys
stored in the account names accordingly. The more often you have used your
account, the higher this number should be. Confirm by pressing `<span class="pre">Import</span>`.

The wallet management console will now give an overview over unclaimed balances.

If you click on `<span class="pre">Balance</span> <span class="pre">Claim</span>` you will be brought to this screen.

You are asked to define where to put your individual balances if you have
multiple accounts.

After confirming all required steps, your accounts and the balances should
appear accordingly.

Note

After importing your accounts and balances, we recommend to make a
new backup of your wallet that will then contain access to your newly
imported accounts and corresponding balances.

## [CLI wallet](#id2) [¶](#cli-wallet "Permalink to this headline")

The wallet backup file can be imported by
<pre><span>&gt;&gt;&gt; </span><span>import_accounts</span> <span>&lt;</span><span>path</span> <span>to</span> <span>exported</span> <span>json</span><span>&gt;</span> <span>&lt;</span><span>password</span> <span>of</span> <span>wallet</span> <span>you</span> <span>exported</span> <span>from</span><span>&gt;</span>
</pre>

Note that this doesn't automatically claim the balances.

### [Claiming Balances](#id3) [¶](#claiming-balances "Permalink to this headline")

For each account `<span class="pre"><my_account_name></span>` in your wallet (run `<span class="pre">list_my_accounts</span>` to see them)::
<pre><span>&gt;&gt;&gt; </span><span>import_account_keys</span> <span>/</span><span>path</span><span>/</span><span>to</span><span>/</span><span>keys</span><span>.</span><span>json</span> <span>&lt;</span><span>my_password</span><span>&gt;</span> <span>&lt;</span><span>my_account_name</span><span>&gt;</span> <span>&lt;</span><span>my_account_name</span><span>&gt;</span>
</pre>

Note

In the release tag, this will create a full backup of the wallet after every key it imports.
If you have thousands of keys, this is quite slow and also takes up a lot of disk space.
Monitor your free disk space during the import and, if necessary,
periodically erase the backups to avoid filling your disk. The latest code
only saves your wallet after all keys have been imported.

The command above will only import your keys into the wallet and will **not**
claim your funds. In order to claim the funds you need to execute::
<pre><span>&gt;&gt;&gt; </span><span>import_balance</span> <span>&lt;</span><span>my_account_name</span><span>&gt;</span> <span>[</span><span>&quot;*&quot;</span><span>]</span> <span>true</span>
</pre>

Note

If you would like to preview this claiming transaction, you can
replace the `<span class="pre">true</span>` with a `<span class="pre">false</span>`. That way, the transaction will not be
broadcast.

To verify the results, you can run::
<pre><span>&gt;&gt;&gt; </span><span>list_account_balances</span> <span>&lt;</span><span>my_account_name</span><span>&gt;</span>
</pre>

### [Manually claim balances](#id4) [¶](#manually-claim-balances "Permalink to this headline")

Balances can be imported one by one. The proper syntax to do so is:
<pre><span>&gt;&gt;&gt; </span><span>import_balance</span> <span>&lt;</span><span>account</span> <span>name</span><span>&gt;</span> <span>&lt;</span><span>private</span> <span>key</span><span>&gt;</span> <span>true</span>
</pre>

But I always import my accounts and then use the GUI to import my balances cause
it's way easier.

***

© Copyright 2018 BitShares Blockchain Foundation.

          Revision `479cd19d`.

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


