  **原文链接**：[http://how.bitshares.works/en/master/user_guide/bitshares-client.html](http://how.bitshares.works/en/master/user_guide/bitshares-client.html)
 
 **译者**：[https://github.com/b-t-s-1](b-t-s-1)
 
 **校对者**： 
  
***  
  
[BitShares Documentation](../index.html) master

Technology

* [Graphene](../technology/graphene.html)
* [What is BitShares?](../technology/what_bitshares.html)
* [The History of BitShares](../technology/history_bitshares.html)
* [Delegated Proof of Stake (DPOS)](../technology/dpos.html)
* [What is different BitShares](../technology/difference_bitshares.html)
* [Technology - BitShares.org](../technology/bitshares_features.html)

BTS Holders

* [1. Things you should know](../bts_holders/things_to_know.html)
* [2. Assets Tokens](../bts_holders/tokens.html)
* [3. Blockchain Governance](../bts_holders/bts_governance.html)
* [4. Community Memberships](../bts_holders/community_members.html)
* [5. Decentralized Exchange (DEX)](../bts_holders/dex.html)
* [6. Investor Guide](../bts_holders/investor_guide.html)
* [7. Migrating from BitShares 1.0 to BitShares 2.0](../bts_holders/migrating_bitshares20.html)

User Guide

* [1. BitShares Accounts](accounts/bts_account.html)
* [2. Create a BitShares Wallet](create_account.html)
* [3. BitShares Client and Login Mode](#)
  - [3.1. BitShares Client](#bitshares-client)
    + [3.1.1. Light Client](#light-client)
    + [3.1.2. Web Client](#web-client)
  - [3.2. Login Forms](#login-forms)
    + [3.2.1. Cloud Wallet Login](#cloud-wallet-login)
    + [3.2.2. Local Wallet Login](#local-wallet-login)
  - [3.3. Summary](#summary)
    + [3.3.1. Cloud Wallet](#cloud-wallet)
    + [3.3.2. Local Wallet](#local-wallet)
    + [3.3.3. Settings - LOGIN MODE](#settings-login-mode)
  - [3.4. Frequently asked Questions](#frequently-asked-questions)
* [4. Backups and Restore your Wallet](backup_local_wallet.html)
* [5. Fund (Send) & Transactions](fund_account.html)
* [6. Bridge and Gateway](bridge_gateway.html)
* [7. Exchange and Explore](dex_exchange_ui.html)

Resources

* [BitShares Community](../resources/info_comunities.html)
* [BitShares Blockchain](../resources/info_comunities.html#bitshares-blockchain)
* [Resources External](../resources/info_external.html)

[BitShares Documentation](../index.html)

* [Docs](../index.html) >>
* 3. BitShares Client and Login Mode
* [Edit on GitHub](https://github.com/bitshares/how.bitshares.works/blob/master/home/docs/checkouts/readthedocs.org/user_builds/howbitsharesworks/checkouts/master/docs/conf.py/user_guide/bitshares-client.rst)

***

# [3. BitShares Client and Login Mode](#id1) [¶](#bitshares-client-and-login-mode "Permalink to this headline")

Table of Contents

* [BitShares Client and Login Mode](#bitshares-client-and-login-mode)
  - [BitShares Client](#bitshares-client)
    + [Light Client](#light-client)
    + [Web Client](#web-client)
  - [Login Forms](#login-forms)
    + [Cloud Wallet Login](#cloud-wallet-login)
    + [Local Wallet Login](#local-wallet-login)
  - [Summary](#summary)
    + [Cloud Wallet](#cloud-wallet)
    + [Local Wallet](#local-wallet)
    + [Settings - LOGIN MODE](#settings-login-mode)
  - [Frequently asked Questions](#frequently-asked-questions)

***

## [3.1. BitShares Client](#id2) [¶](#bitshares-client "Permalink to this headline")

You have sole control of your accounts and funds and they are created on your computer (within the light-client or the browser web-client).

* If you created a **Clout wallet**, you will be able to access to your wallet by using your credentials (username and password) from any browser or computers. You do not need to worry about a backup files. (_You do not have the functionality._)
* If you created a **Local wallet**, you will be able to access your account **only** on the computer that you have used to register and create your account. If you have created a backup file, you can import it and restore your wallet somewhere else.

### [3.1.1. Light Client](#id3) [¶](#light-client "Permalink to this headline")

Download client files and install BitShares Wallet to your computer.

**> Note: This download does not mean that you will have a Local wallet.**

* [Download the Official Light Client](http://bitshares.org/download/)
* [BitShares-UI – Latest Release](https://github.com/bitshares/bitshares-ui/releases)

**BitShares-UI**:
This is a web application and runs in a browser. A connection is established to a trusted node in the network that serves as a gateway to the rest of the ecosystem.

### [3.1.2. Web Client](#id4) [¶](#web-client "Permalink to this headline")

Access the network and open the wallet in the browsers via one of our partners.

* wallet.bitshares.org [https://wallet.bitshares.org](https://wallet.bitshares.org)
* (more...)

***

## [3.2. Login Forms](#id5) [¶](#login-forms "Permalink to this headline")

### [3.2.1. Cloud Wallet Login](#id6) [¶](#cloud-wallet-login "Permalink to this headline")

The cloud wallet only allows for a single account to be accessed at a time. This wallet is generally the correct choice for a new user.

**Login Form**

You login with your account name and your password. The login form shows which type of wallet you login to. (i.e., Login with: **Account name (cloud wallet)**)

### [3.2.2. Local Wallet Login](#id7) [¶](#local-wallet-login "Permalink to this headline")

A Big difference between a Cloud wallet and a Local wallet is the local wallet creates a database within your browser. This means that access to your funds is tied to the browser only. If you attempt to access your local wallet from any other computer, or any other browser, it will fail unless you actively import your backup file from the original browser backup file.

**Login Form**

You select your Local wallet backup file name and type your password to login a Local wallet. The **Key file(.bin)** should be your wallet backup file name. The backup file contains all your keys information.

***

## [3.3. Summary](#id8) [¶](#summary "Permalink to this headline")

The difference between a Cloud wallet and a Local Wallet.

### [3.3.1. Cloud Wallet](#id9) [¶](#cloud-wallet "Permalink to this headline")

* BitShare UI wallet will create a **Cloud wallet** as a default wallet. (i.e., [CREATE ACCOUNT])
* The Cloud wallet allows you to login from any web browser at any time to gain access to your account by using your credentials (username and password).
* The Cloud wallet only allows for a single account to be accessed at a time.
* If you have a Cloud wallet, you don't need to worry about a backup. (_You don't have the functionality in the Cloud wallet_).
* **You can switch the INTERFACE by using the [Settings] - [General] - [Login Mode], however your account won't switch, only the *interface* switches.**
* **Even you import Private keys (was in the Cloud wallet) to the Local wallet, you do not have a brain key to associate with the Private keys you imported. Therefor, a brainkey restore won't find those Private keys. (In this case, no meaning to do a brainkey backup and restore.)**
* **The Cloud wallet has no brainkey.** The password is basically the equivalent of the brainkey, but it's only used for that one account.

### [3.3.2. Local Wallet](#id10) [¶](#local-wallet "Permalink to this headline")

* **If you know you want to have a Local wallet, use an [advanced form] link on the Welcome to BitShares form and create a backup file. This is the only way to create a Local wallet.**
* The Local wallet creates a correct pair of keys (a brainkey and private keys) and save the information to your browser.
* The Local wallet creates a database with in your browser. This means that you can only access your funds from the same computer and web browser that you have used to register and create your account.  If you attempt to access your local wallet from any other computer, or any other browser, it will fail unless you actively import your backup file from the original browser backup file.
* You have to create a backup files to manage the Cloud wallet account.
* The Cloud wallet has Backup options. Go to [Settings] - [Backup] to find.- **Create local wallet backup** : create a Binary File (.bin) backup.- **Create brainkey backup** : give you long random phrases. You need to write down and keep it in a safe place.
* The backup files can be used to move your local wallet to different computers or different browsers. In order to restore your local wallet you will need the backup file and your password! Therefor, it's extremely important you create a backup and keep a safe place.

### [3.3.3. Settings - LOGIN MODE](#id11) [¶](#settings-login-mode "Permalink to this headline")

**Users often misunderstand about this feature.**

This setting feature allows you to select the LOGIN MODE. You can just switch the _interface_. You are **not** switching your account from one to another.

Go to [Settings] - [General] - **LOGIN MODE** to find the feature.

> **This feature only switch the *interface*! Not your account self.**

***

## [3.4. Frequently asked Questions](#id12) [¶](#frequently-asked-questions "Permalink to this headline")

* **Can I switch (by changing the Wallet Mode or importing private keys) my Cloud wallet to a Local wallet?**
  - No. Your account won't switch, only the _interface_ switches.
* **I have a Cloud wallet. Can I have a Local wallet?**
  - Yes. But you will have to create new account for the Local wallet.
* **How can I move my funds from a Cloud wallet to a Local wallet?**
  - We mentioned before. You have to create new account for the Local wallet. You can create the Local wallet by using an [**advanced form**] link on Welcome to BitShares form. After you created new Local wallet, send your funds from your old account (Cloud wallet) to new account (Local wallet). And create a backup!!
* **I have a Cloud wallet. Do I have to save my private keys information somewhere?**
  - Not necessary. Because the Could wallet always do it for extra security. Also lets you login without exposing your owner key, you can login using only the active key.
* **Can I change a Cloud wallet password?**
  - Yes.
  - Go to [How to change a password if using a Cloud Wallet](https://github.com/bitshares/bitshares-ui/wiki/Cloud-Wallet-Login-and-changing-password) : from BitShares UI wiki
* **Can I change a Local wallet password?**
  - Yes.
  - Go to [**Settings**] - [**Password**] - Change your password. Use this page .
* **There is [Create Account] in a Side navigation menu. Can I create and add new account in the same wallet I logged in?**
  - Yes. However, the account you logged in must have a LifeTime Membership (LTM) stats.

[Next](backup_local_wallet.html "4. Backups and Restore your Wallet") [Previous](create_account.html "2. Create a BitShares Wallet")

***

© Copyright 2018, BitShares Blockchain Foundation.

          Revision `237abdab`.

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

