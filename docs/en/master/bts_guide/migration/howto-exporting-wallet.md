  **原文链接**：[http://dev.bitshares.works/en/master/bts_guide/migration/howto-exporting-wallet.html#howto-exporting-wallet](http://dev.bitshares.works/en/master/bts_guide/migration/howto-exporting-wallet.html#howto-exporting-wallet)
 
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
* Exporting Your Wallet
* [Edit on GitHub](https://github.com/bitshares/dev.bitshares.works/blob/master/docs/bts_guide/migration/howto-exporting-wallet.rst)

***

# Exporting Your Wallet [¶](#exporting-your-wallet "Permalink to this headline")

In this tutorial, we will export you wallet including all keys to access your
accounts and funds, into a single JSON-formated file. Note that your private
keys will be encrypted and you will be required to provide the corresponding
pass phrase when importing your funds into BitShares 2.0.

Contents

* [BitShares 1.0 Full Client](#bitshares-1-0-full-client)
  - [Synchronize your Wallet](#synchronize-your-wallet)
  - [Export via the main menu](#export-via-the-main-menu)
  - [Export via the console](#export-via-the-console)

## [BitShares 1.0 Full Client](#id1) [¶](#bitshares-1-0-full-client "Permalink to this headline")

Since the snapshot has taken place already, all you need to do now to get
access to your funds in BitShares 2.0 is described in the following.

Firstly, you need to upgrade your BitShares client to version 0.9.3c. To do the
upgrade you need to:

* download the installation file from the [bitshares webpage](https://github.com/bitshares/bitshares-0.x/releases)
* uninstall your previous version of the BitShares client
* install the new version

### [Synchronize your Wallet](#id2) [¶](#synchronize-your-wallet "Permalink to this headline")

Note

If you see all your funds in your wallet, you can safely skip
this paragraph.

We now need to sync with the blockchain. This is only necessary with if
you think that since the last time you did the syncing there have been
some new transactions involving any of your accounts.

[Legacy BitShares 0.9.3 Blockchain Download](legacy-blockchain.html)

You can see the syncing progress from the status bar or from the `<span class="pre">info</span>`
command in the console (`<span class="pre">account</span> <span class="pre">list->advanced</span> <span class="pre">settings->console</span>`).
After having _synced_ the blockchain, your wallet will automatically attempt to
rescan the blockchain for new transcations. Depending on the amount of accounts
in your wallet, this steps should only take very few minutes.

Since BitShares 0.9.3c, we have a Graphene compatible Export Keys function that
can be accessed in two ways:

* by accessing it in the main menu
* by issuing a command in the console.

### [Export via the main menu](#id3) [¶](#export-via-the-main-menu "Permalink to this headline")

Just select `<span class="pre">File</span> <span class="pre">Menu</span> <span class="pre">-></span> <span class="pre">Export</span> <span class="pre">Wallet</span>`  and you'll be asked to select a
file location where the keys will be exported.

Note

Due to a known bug, if you are on Windows the only option that will
work for you is the console command - the file exported using the menu will not
be compatible with BTS 2.0. This refers to Windows only.

### [Export via the console](#id4) [¶](#export-via-the-console "Permalink to this headline")

* navigate to the console: Account List -> Advanced Settings -> Console
* type: wallet_export_keys [full path to the file]/[file name].jsone.g. on Windows: `<span class="pre">wallet_export_keys</span> <span class="pre">C:\Users\[your</span> <span class="pre">user</span> <span class="pre">name]\Desktop\keys.json</span>`e.g. on Mac: `<span class="pre">wallet_export_keys</span> <span class="pre">/Users/[your</span> <span class="pre">user</span> <span class="pre">name]/Desktop/keys.json</span>`e.g. on Linux: `<span class="pre">wallet_export_keys</span> <span class="pre">/home/[your</span> <span class="pre">user</span> <span class="pre">name]/Desktop/keys.json</span>`
* Please replace `<span class="pre">[your</span> <span class="pre">user</span> <span class="pre">name]</span>` with your Windows account name.
* and hit Enter

Note

The exported wallet file will be encrypted with your pass phrase!
Make sure to remember it when trying to use that file again!

Note

If you are on Windows and your file path tries to access the C drive
directly (e.g. C:keys.json) you might need to run the BitShares client as an
administrator. So the least complicated option will be to aim for the desktop
as in the example above.

# wallet.bitshares.org [¶](#wallet-bitshares-org "Permalink to this headline")

The keys of the [web wallet](http://wallet.bitshares.org) can be exported simply by downloading a backup
wallet. It can be obtained from the web wallet's preferences:
(<cite>Account List->Advanced Settings->Wallet</cite>).

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

