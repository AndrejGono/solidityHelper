# Solidity

```solidity
uint square = 2 ** 10;
uint modulo = 1234 % 2;
address null = address(0); //Send tokens to burn

uint[] memory myArr = new uint[](10);

struct Zombie {
	string name;
	uint level;
}

Zombie public zombies[];

mapping (uint => address) public zombieToOwner;

returns ( // returns tuple
      uint80 roundId,
      int256 answer,
      uint256 startedAt,
      uint256 updatedAt,
      uint80 answeredInRound
);

(,,,respone) = functionCall();
```

```solidity
private/internal/external/public
pure/view //view functions are only free when they're called externally 
storage/memory
```

```solidity
require(keccak256(abi.encodePacked(a)) ==  (keccak256(abi.encodePacked("a")));
```

```solidity
event Name(uint param);
...
emit Name(_param);
```

```solidity
constructor() internal {
	_owner = msg.sender;
}
block.timestamp

import "./Y.sol";
contract X is Y {
    constructor() Y() public{
    }
}
```

```solidity
modifier onlyOwner() {
	require(isOwner());
}

modifier olderThan(uint _age, uint _userId) {
	require(age[_userId] >= _age);
	_;
}

function driveCar(uint _userId) public olderThan(16, )userId) {…}
```

```solidity
struct MiniMe {
  uint32 a; // If they are next to each other
  uint32 b; // Solidity will cluster identical data type
  uint c; // and (minimize the required storage space)
}
```

```solidity
uint in5minutes = now + 5 minutes;
uint day = 1 days;
```

```solidity
payable
require(msg.value == 0.001 ether);
```

```solidity
/// @title A contract for something
/// @author Andrej Gono
/// @notice In progress (for users)
contract GetPaid is Ownable {

	/// @notice Owner can withdrawal contract balance
  /// @param x …
  /// @param y …
  /// @return z …
  /// @dev This function does not currently check for overflows (for devs, important)
  function withdraw() external onlyOwner {
    address payable _owner = address(uint160(owner()));
    _owner.transfer(address(this).balance);
  }
}
```

```solidity
uint random = uint(keccak256(abi.encodePacked(now, msg.sender, randNonce))) % 100;
/* If I were running a node, I could publish a transaction only to my own node and not share it. 
	I could then run the coin flip function to see if I won — and if I lost, 
	choose not to include that transaction in the next block I'm solving. 
	I could keep doing this indefinitely until I finally won the coin flip and solved the next block, and profit.
	https://ethereum.stackexchange.com/questions/191/how-can-i-securely-generate-a-random-number-in-my-smart-contract
*/
```

```solidity
library SafeMath {…}
import "./safemath.sol";
using SafeMath for uint256;
using SafeMath32 for uint32;

uint test = 2;
test = test.mul(3); // test now equals 6

// assert and require will throw an error if false. 
// The difference is that require will refund the user the rest of their gas when a function fails, whereas assert will not.
```

# Web3 Frontend

```jsx
// Infura node provider
var web3 = new Web3(new Web3.providers.WebsocketProvider("wss://mainnet.infura.io/ws"));
// Metamask uses Infura's servers under the hood as a web3 provider,
web3js = new Web3(web3.currentProvider);
// ABI (Application Binary Interface) is a representation of your contracts' methods in JSON format 
// that tells Web3.js how to format function calls in a way your contract will understand.

call/send

var accountInterval = setInterval(function() {
  // Check if account has changed
  if (web3.eth.accounts[0] !== userAccount) {
    userAccount = web3.eth.accounts[0];
    // Call some function to update the UI with the new account
    updateInterface();
  }
}, 100);
```

```solidity
.send({ from: userAccount })
  .on("receipt", function(receipt) {…} // Fired when the transaction is included into a block.
	.on("error", function(error) {…} // there's an issue that prevented the transaction from being included in a block,
	 // such as the user not sending enough gas.
```

```jsx
web3js.utils.toWei("1");
```

```jsx
event NewZombie(uint zombieId, string name, uint dna);

cryptoZombies.events.NewZombie()
.on("data", function(event) {
  let zombie = event.returnValues;
  // We can access this event's 3 return values on the `event.returnValues` object:
  console.log("A new zombie was born!", zombie.zombieId, zombie.name, zombie.dna);
}).on("error", console.error);

// In order to filter events and only listen for changes related to the current user, 
// our Solidity contract would have to use the indexed keyword, like we did in the Transfer event of our ERC721 implementation:

event Transfer(address indexed _from, address indexed _to, uint256 _tokenId);

//In this case, because _from and _to are indexed, that means we can filter for them in our event listener in our front end:

// Use `filter` to only fire this code when `_to` equals `userAccount`
cryptoZombies.events.Transfer({ filter: { _to: userAccount } })
.on("data", function(event) {
  let data = event.returnValues;
  // The current user just received a zombie!
  // Do something here to update the UI to show it
}).on("error", console.error);
```

```jsx
cryptoZombies.getPastEvents("NewZombie", { fromBlock: 0, toBlock: "latest" })
.then(function(events) {
  // `events` is an array of `event` objects that we can iterate, like we did above
  // This code will get us a list of every zombie that was ever created
});

/*

Because you can use this method to query the event logs since the beginning of time, this presents an interesting use case: 
Using events as a cheaper form of storage.
If you recall, saving data to the blockchain is one of the most expensive operations in Solidity. 
But using events is much much cheaper in terms of gas.

The tradeoff here is that events are not readable from inside the smart contract itself. 
But it's an important use-case to keep in mind if you have some data you want to be historically recorded on the blockchain
so you can read it from your app's front-end.
For example, we could use this as a historical record of zombie battles — we could create an event for every time one zombie attacks another
 and who won. The smart contract doesn't need this data to calculate any future outcomes,
but it's useful data for users to be able to browse from the app's front-end.

*/
```

# Tests

```jsx
function kill() public onlyOwner {
   selfdestruct(owner());
}
```

```jsx
afterEach(async () => {
   await contractInstance.kill();
});
```
