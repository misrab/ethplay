# ethplay

playing with a private ethereum testnet




## launching


- Use a random networkid e.g. 9347. Use a custom datadir i.e. .misrab_testnet 

`geth --networkid 12345 --genesis ~/test/genesis.json --datadir ~/.ethereum_experiment console`


Here is our own version:

`geth --networkid 9347 --genesis test_genesis.json --datadir ~/.misrab_testnet --nodiscover --maxpeers 0 console`


- logging: `geth console 2>>geth.log`. Extra screen `geth attach`



## genesis block

above you'll notice . 

"If you want to create a private network you should, for security reasons, use a different genesis block (a database that contains all the transactions from the Ether sales). You can read our [announcement](https://blog.ethereum.org/2015/07/27/final-steps/) blog post on how to generate your file. In the near future we will provide better ways to get other genesis blocks."


Get [mk_genesis_block.py](https://raw.githubusercontent.com/ethereum/genesis_block_generator/master/mk_genesis_block.py)

python mk_genesis_block.py --extradata 0x11bbe8db4e347b4e8c937c1c8370e4b5ed33adb3db69cbdb7a38e1e50b1b82fa > ~/test/genesis.json


## creating a private testnet

http://adeduke.com/2015/08/how-to-create-a-private-ethereum-chain/


https://forum.ethereum.org/discussion/4049/why-does-mining-on-a-private-net-not-work-with-small-machines


## doing stuff

[install solidity compiler](http://solidity.readthedocs.org/en/latest/installing-solidity.html)



```
	// create an account

	personal.newAccount("i am just testing")
	"0xbd42fb9d264697dd3429d8c70cab185bd45ceaa7"


	web3.eth.accounts
	var primaryAccount = web3.eth.accounts[0]

	web3.eth.getBalance(primaryAccount)

	// send ether
	eth.sendTransaction({from:eth.accounts[0], to:eth.accounts[1], value: web3.toWei(0.05, "ether")})






```

## installing solidity compiler on OSX
[see](https://ethereum.gitbooks.io/frontier-guide/content/contract_greeter.html)

brew install cpp-ethereum
brew linkapps cpp-ethereum
which solc


admin.setSolc("/usr/local/bin/solc")
eth.getCompilers()

## contract process

var greeterSource = '...'
var greeterCompiled = web3.eth.compile.solidity(greeterSource) // solidity
var _greeting = "Hello World!" // args
// instance
var greeterContract = web3.eth.contract(greeterCompiled.greeter.info.abiDefinition);

// submit to network
var greeter = greeterContract.new(_greeting,{from:web3.eth.accounts[0], data: greeterCompiled.greeter.code, gas: 1000000}, function(e, contract){
  if(!e) {

    if(!contract.address) {
      console.log("Contract transaction send: TransactionHash: " + contract.transactionHash + " waiting to be mined...");

    } else {
      console.log("Contract mined! Address: " + contract.address);
      console.log(contract);
    }

  }
})


// then mine like
miner.start(1)
admin.sleepBlocks(1);
miner.stop()

web3.eth.getBlock("latest").difficulty
miner.hashrate


// should get
Contract transaction send: TransactionHash: 0x00e1eef490c3572e24fd41d615ca7a8806006c9128c722a1a910ab32f891af70 waiting to be mined...
// then
Contract mined! Address: 0x6e0f774eca64fa5dc9b699762e6aa27a18a431a9
[object Object]



## contract greeter

// see greeter.sol

[turn into one line string](http://www.textfixer.com/tools/remove-line-breaks.php)


greeter.greet();

// other people need
greeterCompiled.greeter.info.abiDefinition;
greeter.address;


// cleanup
greeter.kill.sendTransaction({from:eth.accounts[0]})



## registrars
https://ethereum.gitbooks.io/frontier-guide/content/contract_namereg.html


var tokenName = "MyFirstCoin"
registrar.addr(tokenName) // if 0x00 then it's free
registrar.reserve.sendTransaction(tokenName, {from: eth.accounts[0]});
// check if mine
registrar.owner(myName) // tokenName?
// set name
registrar.setAddress.sendTransaction(tokenName, token.address, true,{from: eth.accounts[0]});


// share with someone
token = eth.contract(abiDef).at(address | registrar.addr("MyFirstCoin"))


## event watcher
var event = token.CoinTransfer({}, '', function(error, result){
  if (!error)
    console.log("Coin transfer: " + result.args.amount + " tokens were sent. Balances now are as following: \n Sender:\t" + result.args.sender + " \t" + token.coinBalanceOf.call(result.args.sender) + " tokens \n Receiver:\t" + result.args.receiver + " \t" + token.coinBalanceOf.call(result.args.receiver) + " tokens" )
});



