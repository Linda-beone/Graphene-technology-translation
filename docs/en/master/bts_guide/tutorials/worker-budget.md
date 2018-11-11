  **原文链接**：[http://dev.bitshares.works/en/master/bts_guide/tutorials/worker-budget.html#worker-budget](http://dev.bitshares.works/en/master/bts_guide/tutorials/worker-budget.html#worker-budget)
 
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
* Claim Worker Pay
* [Edit on GitHub](https://github.com/bitshares/dev.bitshares.works/blob/master/docs/bts_guide/tutorials/worker-budget.rst)

***

# Claim Worker Pay [¶](#claim-worker-pay "Permalink to this headline")

**How to find how much per hour allocated for all worker**

Every second, <cite>[ 17/(2^32) * reserve fund ]</cite> is allocated for witnesses and
workers where reserve fund is how many BTS are currently not distributed (see
the [source code](https://github.com/cryptonomex/graphene/blob/f85dec1c23f6bf9259ad9f15311b2e4aac4f9d44/libraries/chain/include/graphene/chain/config.hpp)).

Every hour the total available reserve fund is calculated by finding how many
BTS are available to be distributed and how many BTS will be returned to the
reserve fund (i.e., "burnt") during the next maintenance interval.

First find how many BTS have not been distributed:

```
# get max_supply from
get_object 1.3.0
# get current_supply from
get_object 2.3.0

==> from_initial_reserve = max_supply - current supply of BTS
```

Then modify it by adding the accumulated fees and witness budget remaining
(i.e., 1.5 BTS per block is budgeted, so budget remaining is

```
1.5 BTS * (number of blocks left in maintenance period+blocks missed by witnesses))
```

in this maintenance cycle (they will be added to the "reserve fund" permanently
at maintenance)

```
get_object 2.13.*

==> updated reserve fund = from_initial_reserve +  from_accumulated_fees + from_unused_witness_budget
```

For example:

```
>>> get_object 2.13.361
get_object 2.13.361
[{
    "id": "2.13.361",
    "time": "2015-10-28T15:00:00",
    "record": {
      "time_since_last_budget": 3600,
      "from_initial_reserve": "106736452914941",
      "from_accumulated_fees": 15824269,
      "from_unused_witness_budget": 2250000,
      "requested_witness_budget": 180000000,
      "total_budget": 1520913100,
      "witness_budget": 180000000,
      "worker_budget": 1340913100,
      "leftover_worker_funds": 0,
      "supply_delta": 1502838831
    }
  }
]
```

Then calculate how much is available to be spent on workers and witnesses is:

```
total_budget = (updated reserve fund)*(time_since_last_budget)*17/(2^32)  #rounded up to the nearest integer
```

Ok, now to find how much workers will get in this budget period (1 hour), you
find the smaller of the available pay AFTER subtracting witness budget from the
total_budget OR the worker_budget_per_day/24 from "get_global_properties"

```
worker_budget=min(total_budget-witness_budget,worker_budget_per_day/24)
```

That is how much per hour allocated for all workers. NOW you rank each worker
and pay them one hours worth of pay in order or # votes.

## References: [¶](#references "Permalink to this headline")

* [The Blockchain Worker System](https://bitshares.org/doxygen/group__workers.html) ([*](#id1)open a doxygen documentation)

***

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
