### Analysis
We hand-write the required opcodes to create our smart contract.Since the desired behavior of our smart contract is to always return 42 regardless of what inputs/function calls are provided to it, we can keep the smart contract simple:
~~~
;; Store the number 42 into memory offset 0
PUSH 42
PUSH 0
MSTORE
;; Memory: 000000000000000000000000000000000000000000000000000000000000002a

;; Return a value that's 32 bytes large stored in memory offset 0
PUSH 32 
PUSH 0
RETURN
~~~
Save this assembly into a file called contract.txt and get the runtime bytecode by passing the file to evm compile:
~~~
$ evm compile contract.txt
602a60005260206000f3
~~~

Confirm the bytecode works as expected:
~~~
$ evm --code 602a60005260206000f3 run
0x000000000000000000000000000000000000000000000000000000000000002a
~~~

Now we need to write the creation bytecode. The return result of the creation bytecode should be our runtime bytecode:
~~~
;; Store the runtime bytecode into memory offset 0
PUSH 0x602a60005260206000f3
PUSH 0
MSTORE
;; Memory: 00000000000000000000000000000000000000000000602a60005260206000f3

;; Return a value that's 10 bytes large starting in memory offset 32 - 10 = 22
;; Note: runtime contract bytecode size is hardcoded as 10 bytes.
PUSH 10
PUSH 22
RETURN
;; Memory: 00000000000000000000000000000000000000000000602a60005260206000f3
;; Offset: └────────────────────22────────────────────┘│                  │
;; Return:                                             └────────10────────┘
~~~
Save the creation assembly into creation.txt and get the creation bytecode:
~~~
$ evm compile creation.txt
69602a60005260206000f3600052600a6016f3
~~~
Test the creation bytecode:
~~~
$ evm --code 69602a60005260206000f3600052600a6016f3 run
0x602a60005260206000f3
~~~

### The Exploit
Run the code in Developer Tools:
~~~
await web3.eth.sendTransaction({from:accounts[0],data:'0x69602a60005260206000f3600052600a6016f3'})

await contract.setSolver('0x000...0000'); // Replace with the smart contract you deployed
~~~