# The Great Solidity Cheatsheet (under construction 🚧)
The Solidity cheat sheet we've always needed upkept by a forgetful blockchain engineer 😋

## Data Locations

```memory```
- Required for reference types (arrays, structs, mappings, strings)
- Reference is a reference to original variable - if function changes value of referenced variable received, then the original variable is changed as well
- Memory variables are temporary, and are erased between external function 
calls to your contract. Think of it like your computer's hard disk vs RAM.

```
function example(string memory _) public { ... }
```

```storage```
- Variables stored permanently on the blockchain
- Most of the time you don't need to use these keywords because Solidity handles them by default. State variables (variables declared outside of functions) are by default storage and written permanently to the blockchain, while variables declared inside functions are memory and will disappear when the function call ends.
    - You will want to use them when dealing with structs and arrays

## Accessing another contract in your code

First make sure you have the contract/contract's interface imported in your code

```
// Inside contract

ExampleContract variable_name = ExampleContract(example_contract_address);

// Before contract 

import { ExampleContract } from "url...";

// Or the contract could be already written in your directory/.sol file
```


## Inheritance & Visibility

**Inheritance**

Note: contract from which other contracts inherit features is known as a base contract, while the contract which inherits the features is called a derived contract

```
// Example - (derived contract) BabyDoge inherits (base contract) Doge functionality
contract BabyDoge is Doge {...}
```

**Visibility**

```Public```

You can use a public functions can be called from anywhere; within its contract or from a contract inheriting its contract

```Private```

Can be used only inside its own contract (not outside in an inheriting contract)

```Internal```

Same as private, except that it's also accessible to contracts that inherit from this contract. The difference to public is that say a contract inheriting this contract was inherited in a whole other contract - that contract would be unable to use the internal function.

```External```

Similar to public, except that these functions can *ONLY* be called outside the contract

## Data Structures

**Arrays**

*A list of values*

Initializing:   

```Zombie[] public zombies;```

Appending:   

```zombies.push(Zombie(_name, _dna));```

Note: storing indexable data

**Mapping**

*key-value store for storing and looking up data*

Initializing:

```mapping (address => uint) public accountBalance;```

Note: use case example - for a financial app, storing a uint that holds the user's account balance

**Structs**

*Similar to a record*

Initializing

```
    struct Sandwich {
        string name;
        string status;
    }
```
Creating a struct: say you had an instance of Sandwich called "burger"

```Sandwich storage burger = Sandwich("Hamburger", "Just served")```

Accessing a struct property

``` burger.name``` --> would give you "Hamburger"

Note: useful custom data structure that can be stored in volume

## Data Types

```uint```

unsigned int - allows wider range of positive numbers, computational efficiency
    ○ It's an alias for uint256 (they're the same thing)

```int```

signed int, can be + or -

```address```

Holds a contract or wallet address in checksum format

## Other

**Variable Naming Conventions**
- Use a leading underscore for all function parameters, just as a convention to indicate that they're function parameters

```
uint256 totalSupply;

constructor(uint256 _totalSupply) public {
    totalSupply = _totalSupply;
}

// There, calling the parameter totalSupply would shadow the existing state variable totalSupply, so the leading underscore is used to avoid the naming collision.
```

## Message dot

```msg.sender```

Address of the person (or smart contract) who called the current function

```msg.value```

tbd

## Conditionals

```require```

*Makes it so that the function will throw an error and stop executing if some condition is not true*

Implementation:
```
// Example:
require(ownerZombieCount[msg.sender] == 0);
```

## Functions

**Header format**

```
function <functionName>( <parameterType><parameterName>, <parameterType><parameterName>, ...) <visibility> <functionModifier> returns (<return type><return varName>, <return type><return varName>, ...) {...}
```
- Not required
    - Returns clause
    - Function modifier (view, pure)

**Handling Multiple Returns**

Use the commas to acquire the proper return items positionally

```
function multipleReturns() internal returns (uint a, uint b, uint c) {
    return (1,2,3);
}

function getLastOfThree() internal returns (uint a) {
    uint c;

    // sets c equal to 3
    (,,c) = multipleReturns();
}
```

**Function modifiers**

```view```

Only viewing the data but not modifying it

```pure```

Not even accessing any data in the app, just does something like an arbitrary 5+5

## Events

An event is a way that DApps can get information at a specific point during smart contract (EVM) code execution. Whenever the EVM encounters a LOG opcode, Ethereum nodes emit an event that DApps and external processes can be notified of and access

```
// Initializing event object (what the event should contain when emitted)
event IntegersAdded(uint x, uint y, uint result);

// Emitting event
emit IntegersAddded(2, 2, 4)
```

## OpenZepplin Essentials

Some popular imports 

```
import "@openzeppelin/contracts/token/ERC721/ERC721.sol";
import "@openzeppelin/contracts/token/ERC20/IERC20.sol";
import "@openzeppelin/contracts/token/ERC721/extensions/ERC721URIStorage.sol";
import "@openzeppelin/contracts/utils/Counters.sol";
import "@openzeppelin/contracts/utils/math/SafeMath.sol";
```

## Interfaces

By including an interface in our contract's code your contract knows what the other contract's functions look like, how to call them, and what sort of response to expect.

**Format:** 

Same as function header, but no curly braces or any definitions

```
// Interface example

contract NumberInterface {
  function getNum(address _myAddress) public view returns (uint);
}
```
	
**Using an interface in another contract**
	
1. Define address of the actual contract you're interfacing with in a variable
2. [NameOfInterfaceContract] [variableName] = [NameOfInterfaceContract](address-containing variable from step 1.)

**Calling it**

Using the interface example in another contract

```
contract MyContract {
  address NumberInterfaceAddress = 0xab38... 
  // ^ The address of the FavoriteNumber contract on Ethereum
  NumberInterface numberContract = NumberInterface(NumberInterfaceAddress);
  // Now `numberContract` is pointing to the FavoriteNumber contract
  // *NumberInterface is from the code block example under Format

  function someFunction() public {
    // Now we can call the `getNum` function from the FavoriteNumber contract:
    uint num = numberContract.getNum(msg.sender);
    // ...and do something with `num` here
  }
}
```