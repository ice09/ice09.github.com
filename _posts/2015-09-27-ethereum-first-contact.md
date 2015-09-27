= Introducing Ethereum-Frontier on Ubuntu
:toc:
:toc-placement!:

toc::[]
An introduction to the link:http://ethereum.org[Ethereum Frontier-Release].

== Prerequisites

* Install link:https://ethereum.org/cli[Ethereum CLI] with according to Geth installation.
* Start a private network
** Follow link:https://blog.ethereum.org/2015/07/27/final-steps/[instructions] to create genesis_block.json
*** Use this hash in creation: 0x11bbe8db4e347b4e8c937c1c8370e4b5ed33adb3db69cbdb7a38e1e50b1b82fa (why?)
*** Download link:https://raw.githubusercontent.com/ethereum/genesis_block_generator/master/mk_genesis_block.py[Python script] to local file mk_genesis_block.py
[source,bash]
----
curl -O https://bootstrap.pypa.io/get-pip.py
sudo python get-pip.py
python mk_genesis_block.py --extradata 0x11bbe8db4e347b4e8c937c1c8370e4b5ed33adb3db69cbdb7a38e1e50b1b82fa > genesis_block.json
----

NOTE: script has to be started several times, maybe using parameter --insight in between

** Change minimal difficulty ("difficulty") in genesis_block.json to "0x000100000"

== Using Geth

* Start Geth in private network
[source,bash]
----
geth --networkid 77777 --genesis genesis_block.json --datadir ~/.ethereum_experiment console
----

* Create accounts
----
personal.newAccount("first passphrase")
personal.newAccount("second passphrase")
----
** Note the generated hashes, these are the account identifies

var primaryAccount = web3.eth.accounts[0]

changes in genesis_block.json
[source,javascript]
----
"alloc": {
    "INSERT_ACCOUNT_HASH_HERE": {
        "balance": "1337000000000000000000"
    },
scroll down
"gasLimit": "0x13880000"
----

 var sender    = web3.eth.accounts[0];
 var recipient = web3.eth.accounts[1];
 var amount = web3.toWei(0.01, "ether");
 web3.eth.sendTransaction({from: sender, to: recipient, value: amount})


* show pending transactions

 eth.getBlock("pending", true).transactions


* unlock recipient (necessary?)

 personal.unlockAccount(recipient);


* mining to finish transaction

 miner.setEtherbase(eth.accounts[0])
 miner.start(8)


* get balance
 web3.eth.getBalance(recipient)


sudo add-apt-repository ppa:ethereum/ethereum
sudo apt-get update
sudo apt-get install solc
which solc

* start client & listen to cosmo.to

 $ geth --networkid 77777 --genesis genesis_block.json --datadir ~/.ethereum_experiment --rpc --rpcaddr="localhost" --mine --rpcport="8080" --rpccorsdomain="http://meteor-dapp-cosmo.meteor.com" --maxpeers=0 console
