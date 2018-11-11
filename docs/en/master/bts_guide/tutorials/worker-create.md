  **原文链接**：[http://dev.bitshares.works/en/master/bts_guide/tutorials/worker-create.html#id1](http://dev.bitshares.works/en/master/bts_guide/tutorials/worker-create.html#id1)
 
 **译者**：[https://github.com/b-t-s-1](b-t-s-1)
 
 **校对者**： 
  
***

# How to Create a Worker [¶](#how-to-create-a-worker "Permalink to this headline")

Table of Contents

* [How to create a Worker](#id1)
  - [By using the UI](#by-using-the-ui)
  - [By using the command](#by-using-the-command)
* [How to see Proposals on the Chain](#how-to-see-proposals-on-the-chain)
  - [By using the UI](#id2)
  - [By using the command](#id3)
* [How to Vote for a Worker](#how-to-vote-for-a-worker)
  - [By using the UI](#id4)
  - [By using the command](#id5)
* [How Workers Get Paid](#how-workers-get-paid)
* [Technical Details](#technical-details)

***

## [How to create a Worker](#id6) [¶](#id1 "Permalink to this headline")

### [By using the UI](#id7) [¶](#by-using-the-ui "Permalink to this headline")

You can create a Worker Proposal by using BitShares UI form. Go to _Voting_ page. There are three tabs: "Witnesses", "Committee", and "Workers". In the "Worker" tab, you will find _CREATE A NEW WORKER_ button.

**a worker proposal form**

### [By using the command](#id8) [¶](#by-using-the-command "Permalink to this headline")

Workers are currently created with the cli_wallet with the following command syntax:

```
>>> create_worker owner_account work_begin_date work_end_date daily_pay name url worker_settings broadcast
```

| parameter | example value |
| --- | --- |
| owner_account | "worker-name" |
| work_begin_date | "2015-10-28T00:00:00" |
| work_end_date | "2015-10-29T00:00:00" |
| daily_pay | 100000 |
| name | "Description" |
| url | "[http://URL](http://URL) " |
| worker_settings | {"type" : "vesting", "pay_vesting_period_days" : 1} |
| broadcast | false |

**Example**

An <cite>owner_account</cite> is creating a one day worker starting Oct 28 and will get paid 1 BTS/day (vesting in 1 day, 1 BTS = 100,000 'satoshi') to make an android app. The first command won't broadcast, this will just check:

```
>>> create_worker "worker-name" "2015-10-28T00:00:00" "2015-10-29T00:00:00" 100000 "Description" "http://URL" {"type" : "vesting", "pay_vesting_period_days" : 1} false
```

The URL should point to something describing your proposal. We recommend to answer the following questions:

* What will you do with the funds?
* By when will you be done?
* For how much?

The variable <cite>type</cite> can be

* `<span class="pre">refund</span>` to return your pay back to the pool to be used for future projects,
* `<span class="pre">vesting</span>` to pay that you pay yourself, or
* `<span class="pre">burn</span>` to destroys your pay thus reducing share supply, equivalent to share buy-back of a company stock

The variable `<span class="pre">pay_vesting_period_days</span>` is the integer number of days you set for vesting. Some people don't want workers to withdraw and sell large sums of BTS immediately, as it puts sell pressure on BTS. Also, if you require vesting, you have "skin in the game" and thus an incentive to improve BTS value. Pay is pay per day (not hour or maintenance period) and is in units of 1/100,000 BTS (the precision of BTS)

To **actually** generate a worker proposal, replace the last <cite>false</cite> by <cite>true</cite>.

***

## [How to see Proposals on the Chain](#id9) [¶](#how-to-see-proposals-on-the-chain "Permalink to this headline")

### [By using the UI](#id10) [¶](#id2 "Permalink to this headline")

You can check the Worker Proposals by BitShares UI form. Go to _Voting_ page. There are three tabs: "Witnesses", "Committee", and "Workers". In the "Worker" tab, you will find all Worker Proposals list.

### [By using the command](#id11) [¶](#id3 "Permalink to this headline")

You also can inspect all the objects 1.4.*:

```
>>> get_object 1.14.4
get_object 1.14.4
[{
    "id": "1.14.4",
    "worker_account": "1.2.22517",
    "work_begin_date": "2015-10-21T11:00:00",
    "work_end_date": "2015-11-21T11:00:00",
    "daily_pay": 1000000000,
    "worker": [
      1,{
        "balance": "1.13.235"
      }
    ],
    "vote_for": "2:73",
    "vote_against": "2:74",
    "total_votes_for": "14632377015617",
    "total_votes_against": 0,
    "name": "bitasset-fund-pool",
    "url": "https://bitsharestalk.org/index.php/topic,19317.0.html"
  }
]
```

***

## [How to Vote for a Worker](#id12) [¶](#how-to-vote-for-a-worker "Permalink to this headline")

### [By using the UI](#id13) [¶](#id4 "Permalink to this headline")

Voting is important. You have the UI form for voting. It's very easy to use. Go to _Voting_ page. There are three tabs: "Witnesses", "Committee", and "Workers". In the "Worker" tab, check a Toggle Vote check box and _SAVE_. You might be asked to login. Confirm the transaction.

```
.. image:: ../../../../_static/imgs/ui-voting-worker-2.png
```

>

| alt: | BitShares |
| width: | 600px |
| align: | center |

### [By using the command](#id14) [¶](#id5 "Permalink to this headline")

You can vote using the CLI:

```
>>> update_worker_votes your-account {"vote_for":["proposal-id"]} true
```

for example:

```
>>> update_worker_votes "awesomebitsharer" {"vote_for":["1.4.0"]} true
```

you can also vote against or abstain (remove your vote for or against):

```
>>> update_worker_votes your-account {"vote_against":["proposal-id"]} true
>>> update_worker_votes your-account {"vote_abstain":["proposal-id"]} true
```

***

## [How Workers Get Paid](#id15) [¶](#how-workers-get-paid "Permalink to this headline")

Every hour the worker budget is processed and workers are paid in full order of the number of votes for minus the number of votes against. The last worker to get paid will be paid with whatever is left, so may receive partial payment. The daily budget can be estimated by inspecting the most recent budget object 2.13.* for example:

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

So the daily budget is:

```
worker_budget*24 = 1340913100 * 24 = 32181914400 (or 321,8191.44 BTS)
```

There is currently a maximum daily worker pay of 500k BTS, and this can be found using the <cite>get_global_properties</cite> command in the cli_wallet.

***

## [Technical Details](#id16) [¶](#technical-details "Permalink to this headline")

Every second,:

```
[ 17/(2^32) * reserve fund ]
```

is allocated for witnesses and workers. The reserve fund is maximum number of BTS available less those currently in circulation ([source]([https://github.com/cryptonomex/graphene/blob/f85dec1c23f6bf9259ad9f15311b2e4aac4f9d44/libraries/chain/include/graphene/chain/config.hpp](https://github.com/cryptonomex/graphene/blob/f85dec1c23f6bf9259ad9f15311b2e4aac4f9d44/libraries/chain/include/graphene/chain/config.hpp)))

Every hour the total available reserve fund is calculated by finding how many BTS are available to be distributed and how many BTS will be returned to the reserve fund (i.e., "burnt") during the next maintenance interval.

First find how many BTS have not been distributed:

```
>>> from_initial_reserve = max_supply - current supply of BTS
```

The max_supply can be obtained by:

```
>>> get_object 1.3.0
```

and the current_supply is given in:

```
>>> get_object 2.3.0
```

Modify it by adding the accumulated fees and witness budget remaining (i.e., 1.5 BTS per block is budgeted, so budget remaining is 1.5 BTS * (number of blocks left in maintenance period+blocks missed by witnesses)) in this maintenance cycle (they will be added to the "reserve fund" permanently at maintenance):

```
updated reserve fund = from_initial_reserve + from_accumulated_fees + from_unused_witness_budget
```

variables all from: <cite>get_object 2.13.*</cite> (choose the most recent one, for example)

Next calculate how much is available to be spent on workers and witnesses is:

```
total_budget = (updated reserve fund)*(time_since_last_budget)*17/(2^32)
```

rounded up to the nearest integer

Ok, now to find how much workers will get in this budget period (1 hour), you find the smaller of the available pay AFTER subtracting witness budget from the <cite>total_budget</cite> OR the <cite>worker_budget_per_day/24</cite> from <cite>get_global_properties</cite>:

```
worker_budget=min( total_budget - witness_budget , worker_budget_per_day / 24 )
```

That is how much per hour allocated for all workers. NOW you rank each worker and pay them one hours worth of pay in order or # votes.

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
