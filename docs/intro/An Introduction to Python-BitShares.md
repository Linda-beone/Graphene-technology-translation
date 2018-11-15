原文链接：https://steemit.com/python/@full-steem-ahead/howto-an-introduction-to-python-bitshares

译者：bt666

# 如何:Python-BitShares简介 #

在实现使用程序来监控见证人节点的丢块，并自动将签名密钥切换到备份见证人节点的过程中，我向社区询问比特股社区是否已经存在这样的程序。[ @RoelandP](https://steemit.com/@roelandp)回应了他基于[@ xeroc](https://steemit.com/@xeroc)的Python-BitShares库开发的程序。 非常感谢这些真正为Steemit和比特股做出巨大贡献的人。

关于这个多功能且全面的Python-BitShares库，我发现了一些非常重要的事项，下面我将在这里加以描述。我花了一整天时间来解决我面临的这些问题。 然而，有了我提供的这些信息，任何人都可以很容易避免这些困难。 回过头来看，我遇到的主要问题是找到库运行的正确的上下文，以及如何删除SQLite钱包数据库，从而创建一个可以添加私钥的数据库。

**预备/先决条件**

我可能是Python编程的新手，但我发现它相对容易上手并且可以让你更加高效。如果你没有编程或技术背景，那么这可能是个挑战，但如果你坚持下去，你所开发的技能将在未来许多年里为你提供很大的帮助，不仅适用于区块链工作，也适用于许多其他领域。

我在此假定读者可以轻松获取比特股帐户的私钥并使用计算机和编程。您的经验越丰富，这个过程就越容易。所有代码和讨论仅适用于Linux，但是一旦在您选择的平台上安装了Python，操作系统就无关紧要了。

本文中使用的代码非常简单。它只做一件事：监控见证人是否丢块，当发生丢块时，调用update_witness API在比特股区块链上广播一条消息，从而激活备用见证人节点。我在很多全节点上进行了操作，我想扩展Roeland程序从而使用多个备份节点。将来我可能会进一步扩展程序，以便使用多个可切换的监视器来提供更高的容错能力，并为监视器本身提供冗余性。除了文本编辑器和比特股帐户，该账户具有可支付手续费的余额，以下链接提供了您所需的基本要素：

Python-BitShares on github: [http://python-bitshares.readthedocs.io/en/latest/](http://python-bitshares.readthedocs.io/en/latest/)
Uptick: [http://uptick.readthedocs.io/en/latest/](http://uptick.readthedocs.io/en/latest/)
Python: [https://www.python.org/about/gettingstarted/](https://www.python.org/about/gettingstarted/)

请注意，库的文档是非常新的，虽然它提供的很多信息仍然缺乏重要的细节。 它主要是一些小的代码片段的集合，很少有完整的例子。 该库应该编写的很快，其中只包含了要点，相关细节留到以后再进行补充。 没有什么好方法可以鼓励大家使用。 我发现浏览文档很困难，搜索功能也非常有限。 文档也可以组织地更好。 例如我无法找到这个页面，它应该在QuickStart这部分下，但事实却不是。 本文旨在解决其中一些问题。 我将提供一个总结并将其提交到github，以便xeroc可以更新他的文档。

**安装Python-BitShares**

我遇到的第一个问题是如何安装库。我更喜欢用pip来安装python包，但我找不到合适的名称来使用。 Xeroc和其他人通过几个名称来引用python-bitshares库：PyBitShares，python-bitshares，piston，uptick，python-graphene ......并非所有这些都指向相同的东西，这让人觉得困惑。因此，我的第一次安装尝试是从github下载了python-bitshares库的zip压缩包，并进行了手动安装。请注意，我上面提到的安装链接提供了有关手动安装或使用pip（pip用于python3或pip3）的信息。使用pip安装时使用的正确名称是“bitshares”，如：

pip3 install bitshares

虽然这不是必需的，但您可能会发现uptick程序很有用。有关单例的uptick程序的文档比库更稀少。它是一个单独的python程序，可与python-bitshares一起使用，可以帮助您创建钱包并为其添加私钥，以供使用python-bitshares构建的程序使用。我一开始使用了这个程序，不过后来发现并不需要使用这个切换程序。

**创建钱包**

要使用python-bitshares做很多有用的事情，你需要一个带有一些BTS余额的比特股账户，来支付大多数比特股活动所需的交易费用。文档中的任何地方均未提及此隐含要求。这似乎是一个显而易见的要求，但直到经过多次试验和实验后我才想到这一点。错误消息可能非常容易误导人，例如当我调用update_witness API时出现“MissingKeyError”。实际问题不是缺失密钥，而是提供的密钥无效，因为它没有BTS余额。当我意识到需要付费来广播update_witness API消息，而我使用的私钥对应的比特股账户并没有资金，突破就来了。所以我为我的见证人帐户添加了活动密钥。您也可以使用您的账户所有者密钥，但这对见证人来说是不好的做法。我不确定是否需要见证人账户，或者是否可以使用任何具有BTS余额的比特股账户。update_witness API调用可能仅限于见证人帐户或终身会员（LTM）帐户。

实际上我写了一个非常简短的程序来创建一个新钱包并在其中安装私钥。你也可以用uptick做到这一点。这里重要的一点是，你需要设置一个python-bitshares钱包来包含你的见证人私钥。确保您使用的帐户具有BTS余额。我不确定钱包是什么时候创建的，但是花了很长时间才终于弄清楚如何从“不存在钱包”的情况下开始。 Uptick也没有“删除钱包”命令。除非您的程序在没有钱包的情况下启动，并且创建了一个钱包并添加了密钥，否则您将需要某种类型的独立程序来设置钱包。 python-bitshares库使用SQLite数据库来存储各种数据，比如你的私钥。不确定它有多安全，但还有另一种方法可以提供不涉及SQLite数据库的密钥。有关这方面的更多信息，请参阅python-bitshares文档。这意味着将安全问题从SQLite转移到您的自初始化程序了。

您可以使用下面的walletInit.py程序创建一个新钱包并为其添加私钥。将walletPassword，privateKey和URL更改为全节点，然后运行该程序。它会报告是否找到钱包，如果没有，它会创建一个钱包。再次运行它会添加你的私钥。如果您需要删除钱包，可以删除“os.remove”该行的注释，或手动运行命令：`rm -rf~.local / share / bitshares / bitshares.sqlite`。

    #!/usr/bin/env python3
    
    from bitshares import BitShares
    import os
    
    walletPassword = "YourUltraSecretWalletPassword"
    privateKey = "5K.....................................1Aw"
    
    bitshares = BitShares("ws://localhost:port/ws")
    
    #os.remove("/home/yourAccountName/.local/share/bitshares/bitshares.sqlite")
    
    if bitshares.wallet.created():
    print("A wallet already exists, good. Adding your BitShares private key...")
    bitshares.wallet.unlock(walletPassword)
    bitshares.wallet.addPrivateKey(privateKey)
    else:
    print("No wallet exists yet, creating one now...")
    bitshares.wallet.newWallet(walletPassword)
    
    print("\nYou should see many values printed below on successful install,")
    print("including recently_missed_count and next_maintenance_time\n")
    print(bitshares.info(), "\n")

**btsNodeSwitcher\.py**

RoelandP编写的程序可以在github上找到：[https：//github.com/roelandp/Bitshares-Witness-Monitor/blob/master/witnesshealth.py](https：//github.com/roelandp/Bitshares-Witness-Monitor/blob/master/witnesshealth.py)。 它还包括通知功能，您可能会发现它更适合您的需求。 它非常简单，易于修改以处理多个故障转移节点。 这是我创建的适合我需求的版本：

    #!/usr/bin/env python3
    #
    # This modified script from roelandp will monitor a witness'
    # missed blocks and broadcast an update_witness msg when they
    # excede the number defined by the "tresholdwitnessflip".
    #
    import sys
    import time
    from bitshares.witness import Witness
    from bitshares import BitShares
    
    # witness/delegate/producer account name to check
    witness= "YourBitSharesWitnessAcccount"   # This account must have a BTS balance to pay fees
    walletpwd  = "YourUptickWalletPassword"   # Your UPTICK / bitshares.sqlite wallet unlock code
    witnessurl = "https://bitsharestalk.org/" # Your Witness Announcement url
    
    # Array of alternate signing keys we'll switch to in round-robin fashion upon failure
    backupsigningkeys = [ "BTS5...........................................qjTpc1",
      "BTS8...........................................MNWcfX",
      "BTS5...........................................ZtjU9P",
      "BTS6...........................................gXZ2nU" ]
    
    websocket   = "ws://localhost:port/ws" # API node to connect to!
    
    next_key= int(0)# Index of next signing key to use when a failure is detected
    check_rate  = int(30)   # Frequency in seconds between each sample of missed blocks
    loopcounter = int(0)# Increments each time the missed block count is sampled  
    startmisses = int(-1)   # Holds number of misses (set to -1 to initialize counters)
    currentmisses   = int(0)# Current number of missed blocks according to blockchain 
    counterOnLastMiss   = int(0)# Last loopcounter value we missed a block on
    resetAfterNoMisses  = int(20)   # If no missed blocks for this many samples reset counters
    tresholdwitnessflip = int(3)# after how many blocks the witness should switch to different signing key
    #currentmisses   = int(999) # To test the update_witness call set this to current actual misses -1
    #startmisses = int(1000)# To test set this to the current number of missed blocks
    #tresholdwitnessflip = int(0)   # To test set this to zero
    
    # Setup node instance
    bitshares = BitShares(websocket)
    
    # Check how many blocks the witness has missed and switch signing keys if required
    def check_witness():
    global counterOnLastMiss
    global lastMissedSample
    global currentmisses
    global startmisses
    global next_key
    status = Witness(witness, bitshares_instance=bitshares)
    current_key = status['signing_key']
    missed = status['total_missed']
    print("\r%d samples, missed=%d, key=%.16s..." % (loopcounter, missed, current_key), end='')
    
    if start misses == -1:   # Initialize counters
    startmisses = currentmisses = missed
    counterOnLastMiss = loopcounter  # Reset periodically to prohibit switching on
     #  small accumulated misses over long periods 
    if missed > currentmisses:
    print("\nMissed another block!  (%d)" % missed)
    currentmisses = missed
    counterOnLastMiss = loopcounter
    if (currentmisses - startmisses) >= tresholdwitnessflip:
    # Flip witnesses to backup
    print("Time to switch! (using key %s)" % backupsigningkeys[next_key])
    bitshares.wallet.unlock(walletpwd)
    bitshares.update_witness(witness, url=witnessurl, key=backupsigningkeys[next_key])
    time.sleep(6) # wait for 2 witness cycles
    status = Witness(witness, bitshares_instance=bitshares)
    if current_key != status['signing_key']:
    current_key = status['signing_key']
    print("Witness updated. Now using " + current_key)
    startmises = -1
    next_key = (next_key + 1) % len(backupsigningkeys)
    else:
    print("Signing key did not change! Will try again in %d seconds" % check_rate)
    else:
    # If we haven’t missed any for awhile reset counters 
    if loopcounter - counterOnLastMiss >= resetAfterNoMisses:
    startmisses = -1
    
    
    # Main Loop
    if __name__ == '__main__':
    if not bitshares.wallet.created():
    print("Please use uptick or your own program to create a proper wallet.")
    exit(-2)
    while True:
    check_witness()
    sys.stdout.flush()
    loopcounter += 1
    time.sleep(check_rate)

圣诞节这天更新，在跑了20个样本且没有丢块后重置计数器。

**就是这样。 谢谢阅读！**