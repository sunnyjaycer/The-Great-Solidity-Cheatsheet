# The Great Solidity Cheatsheet
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


## Visibility

```Public```

You can use a public functions can be called from anywhere; within its contract or from a contract inheriting its contract

```Private```

Can be used only inside its own contract (not outside in an inheriting contract)

```Internal```

Same as private, except that it's also accessible to contracts that inherit from this contract

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
function <functionName>( <parameterType><parameterName>, <parameterType><parameterName>, ...) <visibility> <functionModifier> returns (<return type>) {...}
```
- Not required
    - Returns clause
    - Function modifier (view, pure)

**Function modifiers**