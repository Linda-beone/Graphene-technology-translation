  **原文链接**：[http://dev.bitshares.works/en/master/bts_guide/tutorials/howto-importing-wallet-remarks.html#migration-remarks](http://dev.bitshares.works/en/master/bts_guide/tutorials/howto-importing-wallet-remarks.html#migration-remarks)
 
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

* [Developer Guide (Tutorials)](index.html)
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
* Migration Remarks
* [Edit on GitHub](https://github.com/bitshares/dev.bitshares.works/blob/master/docs/bts_guide/tutorials/howto-importing-wallet-remarks.rst)

***

# Migration Remarks [¶](#migration-remarks "Permalink to this headline")

## Remarks on Private Keys [¶](#remarks-on-private-keys "Permalink to this headline")

Depending on the users (trade) activity and investors behavior please also note
the following:

* **TITAN Transactions**:If there is a chance that you might have received a TITAN transaction(default transaction format) since you last opened your wallet, you arerequired to completely synchronize with the BitShares 1.0 blockchain to catchthat transaction and generate the corresponding private key. After that, youcan use the graphene-compatible wallet export function.
* **Market transactions**:Each market order is associated a key that is derived when placing yourorder and is hence part of your wallet. Please note that for the transitionto BitShares 2.0, all open orders (except short orders) will be closed andthe funds returned to their owners.
* **Cold Storage Funds**:You cold storage funds can be claimed by simply importing your cold privatekey into BitShares 2.0. This will result in a transaction that claims yourfunds and puts them into your account.
* **PTS/AGS donators**:You can import your corresponding private keys into BitShares 2.0 directly toclaim your funds. Note that, since the web wallet cannot parse your <cite>wallet.dat</cite> file, you may be required to manually dump your private keys.with a 3rd party tool. If you feel not comfortable with this, we recommendyou import your <cite>wallet.dat</cite> file into the BitShares 1.0 client and export agraphene-compatible wallet file that can be imported in the BitShares 2.0 webwallet.

## Technical Explanation [¶](#technical-explanation "Permalink to this headline")

The BitShares 2.0 wallet architecture is vastly different than BitShares 1.0.
In BitShares 1.0, each account consists of dozens or even thousands of keys,
each of which is controlling a small portion of your balance and for TITAN
users, none of the balances associated with your account use the same key as
your account.  Under BitShares 2.0, all of these "balances" become unique
accounts rather than a single logical account. The BitShares 2.0 wallet has
an "import" interface that allows you to specify a set of private keys and
the name of an account that you would like to receive all of the funds
associated with those keys. Then it generates a transaction that spends the
full balances from all accounts associated with those keys to your new
unified BitShares 2.0 account. The BitShares 0.9.x wallet provides a utility
to dump all private keys associated with a given account to make the
migration process easy.

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
