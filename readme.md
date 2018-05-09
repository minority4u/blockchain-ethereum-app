# Hint
Do not use the provided accounts for real transactions in the main-ethreum-network!!!

Ethereum test-network ready to use. This demo consists of:

* Three mining nodes
* External Contracts owned by each account to encapusalte the minning accounts
* More than 1800 already mined blocks
* Lots of past transactions stored in the logfiles and history
* Own Token (Unversity Votes) deployed as Smart-Contract with tokens/vote rights splitted between all accounts and their public contracts
* DAO (University Mannheim). Every owner of the University token is automatically member of the DAO and is allowed to create proposals and votes.

The Smart Contract code is stored in "_infos/". They are based on the Ethereum-Templates: https://www.ethereum.org/token



# Network structure:


![Devnet structure](https://github.com/minority4u/Ethereum_network/blob/master/_infos/images/Structure.png "Devnet Structure") 

# Project files and folder
Description of all important files wthin the project

## _infos directory 
	For example screenshots and further network infos as the accunts.txt

## devnet
	Root of our Ethereum network contains all network related data

### boot.key
	Bootnode file

### genesis.json
	Network initial definition, this is our first block, created with ppupeth.
	This network uses the proof of authority concept.

### node0, node1 and node2
	..* Our three working Nodes. All of them are allowed to mine.
	..* Every Node consists of the following files: geth (contains all blockchain-data), history (consists all past javascript-console transactions comands), keystore (consists of the public account-UTC-files, for easy administration node0/keystore consists all three keystore files), password.txt (password file for the account of this node to simplify the startprocedure)

### node0.log, node1.log and node2.log
	Logfiles for each Node to make every transaction persistent.

## readme.md
	Setup instructions


# Setup 
Tested with OSX 10.13.4

## Requirements
* Ethereum Wallet (https://github.com/ethereum/mist/releases)
* the bootnode executeable needs to be available (https://github.com/ethereum/homebrew-ethereum/pull/114)

## Steps
* 'Git clone'
* 'cd Ethereum_network/devnet'
* start bootnode:

>bootnode -nodekey boot.key -verbosity 9 -addr :30310

* Bootnode should return it name, write it down or copy it, example output: 

>bootnode:
enode://6119af548bfdc5948a8e440f50b054ae75be7f9cbf3ef8d0681a905cc7cd326587690d22cfc2b7c1bdf8ece179ee1e45fc3b988d54287443534cd4f72602d859@[::]:30310

* Start a new terminal with three tabs and go to the devnet dir

* Start Node0 - no mining, multiple keystore files for later Ethereum Wallet usage, --> adjust the bootnode-param

> geth --datadir node0/ --etherbase '7d82095715752f45b048d056a4383e013a4bf2ee' --syncmode 'full' --port 30320 --rpc --rpcaddr 'localhost' --rpcport 8500 --rpcapi 'personal,db,eth,net,web3,txpool,miner' --bootnodes 'enode://6119af548bfdc5948a8e440f50b054ae75be7f9cbf3ef8d0681a905cc7cd326587690d22cfc2b7c1bdf8ece179ee1e45fc3b988d54287443534cd4f72602d859@127.0.0.1:30310' --networkid 42 --gasprice '1' --unlock '7d82095715752f45b048d056a4383e013a4bf2ee' --password node0/password.txt console 2>>node0.log

* Start Node1, single keystore file, mining --> adjust the bootnode-param

> geth --datadir node1/ --syncmode 'full' --port 30321 --rpc --rpcaddr 'localhost' --rpcport 8501 --rpcapi 'personal,db,eth,net,web3,txpool,miner' --bootnodes 'enode://6119af548bfdc5948a8e440f50b054ae75be7f9cbf3ef8d0681a905cc7cd326587690d22cfc2b7c1bdf8ece179ee1e45fc3b988d54287443534cd4f72602d859@127.0.0.1:30310' --networkid 42 --gasprice '1' --unlock '58f998ac0db4cd278459f83ab2ad7a0bbddd48ef' --password node1/password.txt --mine console 2>>node1.log

* Start Node2 single keystore file, mining --> adjust the bootnode-param

> geth --datadir node2/ --syncmode 'full' --port 30322 --rpc --rpcaddr 'localhost' --rpcport 8502 --rpcapi 'personal,db,eth,net,web3,txpool,miner' --bootnodes 'enode://6119af548bfdc5948a8e440f50b054ae75be7f9cbf3ef8d0681a905cc7cd326587690d22cfc2b7c1bdf8ece179ee1e45fc3b988d54287443534cd4f72602d859@127.0.0.1:30310' --networkid 42 --gasprice '1' --unlock '7092fbc7f89c81e4118c481f822cc6e128fc6365' --password node2/password.txt --mine console 2>>node2.log

* Start a new terminal with three tabs for our logs and open all three logfiles

> tail -f node0.log
> tail -f node1.log
> tail -f node2.log

* Start the Ethereum Wallet, --> adjust the "--prc param" to the working dir of your Node0

> open -a /Applications/"Ethereum Wallet.app" --args  --rpcport "8500" --rpc $HOME/Git/Ethereum:network/devnet/node0/geth.ipc

* Check the botnode console, can you see every working Node?

* After 1-2 minutes Node1 and Node2 should start mining, you can start the miner of Node0 with:

> miner.start()

* To follow all transactions of the University-Votes-Token and the University-DAO, please go to the "contracts"-Tab and add the two smart contract-accounts:

Token: 

> 0xb296046B30bFfE02d0111D824Aa86a0D65ec95B8

DAO: 

> 0xCf67708873388542Db6B8A32A65FDAD02782C000

* Enjoy! ;)


# Delete all past block data = new init of the genesis-block
* stop all Nodes and the bootnode
* remove the geth and history folders
* geth --datadir node0/ init genesis.json
* geth --datadir node1/ init genesis.json
* geth --datadir node2/ init genesis.json
* startup the bootnode and the working nodes


# Some initial help
## Help if Port is in Use:
https://github.com/ethereum/mist/wiki#bind-address-already-in-use

## Example-usage of the RPC Interface via curl or postman
curl http://localhost:8500 -X POST --data '{"jsonrpc":"2.0","method":"eth_getBalance","params":["0x7d82095715752f45b048d056a4383e013a4bf2ee","latest"],"id":1}'

## Example Javascript-Console comands
* eth.sendTransaction({'from':eth.coinbase, 'to':'0x7092fbc7f89c81e4118c481f822cc6e128fc6365', 'value':web3.toWei(3, 'ether')})
* eth.coinbase
* eth.blockNumber
* eth.getBalance(eth.coinbase)

>function checkAllBalances() {
     var totalBal = 0;
     for (var acctNum in eth.accounts) {
         var acct = eth.accounts[acctNum];
         var acctBal = web3.fromWei(eth.getBalance(acct), "ether");
         totalBal += parseFloat(acctBal);
         console.log("  eth.accounts[" + acctNum + "]: \t" + acct + " \tbalance: " + acctBal + " ether");
     }
     console.log("  Total balance: " + totalBal + " ether");
 };

