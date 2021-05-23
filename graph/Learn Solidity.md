- ## Metadata
    - Type: [[tooling]]
    - Domain: [[Learn Ethereum]] 
- ## Introduction
    - This is one of the many [[Locations of Knowledge]]. In this category, you can find __insights__ that I am discovering, **as I learn*Solidity**.  With Solidity one can create a [[decentralized application]] , an application that runs on [[Learn Ethereum]] that is.
- ## How to read this category 
    - The insights are not meant to be all-inclusive, but complementary resources. Follow the instructions at the start of the Insights.
    - Insights are meant to **greatly accelerate** your learning process of the original material that they accompany. 
    3. This should be collaborative. If you have any questions, just jump into **Discord:** https://discord.gg/vJ5FRbQg. 
    - If you want to enrich the content, let's chat in the **Discord** channel.
    5. If you are unfamiliar with Roam Research, visit [[Learn Roam Research]] to learn more about it.
    - Think of this as a trunk of a tree. A body of knowledge that branches into different subjects. All subjects (branches) have [[external resources]], so you can research further into the subject.
        - For example, click on the following image for an example from [[Learn Solidity]], where the insights were generated while following [Crypto Zombies](https://cryptozombies.io/) 
        - ![](https://firebasestorage.googleapis.com/v0/b/firescript-577a2.appspot.com/o/imgs%2Fapp%2FSymposium%2F4zl4lDkQFV.jpeg?alt=media&token=4c3ef3b3-c08a-46fc-acf3-8c245b0effc4)
- ## Insights
    - ## Start here
        - The following insights were generated while following the course [Cryptozombies](https://cryptozombies.io/en/course/). 
            - **Start from lesson 1.**
            - **If things are obvious to use, just pass over them**. The goal of this exercise is not to get some proof, but rather learn. If you skip something that you don't know, you are doing a disservice to yourself 🤷‍♂️
    - ### Cryptozombies - Solidity Path Lesson 1-5
        - `_variable` means that the variable is a function's argument. It's a convention to make the code more readable. 
            - [[external resources]]
                - [StackOverflow](https://ethereum.stackexchange.com/questions/1351/what-is-the-purpose-of-underscores-in-front-of-parameters) 
                - [Naming Conventions in Solidity](https://github.com/crytic/slither/wiki/Detector-Documentation#conformance-to-solidity-naming-conventions)
        - Storing a string in `Memory` vs `Storage`
            - In solidity, a variable can be potentially stored in 3 different places:
                - “**storage**”, where all the contract state variables reside. Every contract has its own storage and it is persistent between function calls and quite expensive to use.
                - “**memory**”, this is used to hold temporary values. It is erased between (external) function calls and is cheaper to use.
                - **stack**, which is used to hold small local variables. It is almost free to use, but can only hold a limited amount of values.
            - Strings can be stored in either `memory` or `storage`. For strings, the **default location** is **storage**. 
                - [[external resources]]
                    - [Where are strings stored in Solidity?](https://ethereum.stackexchange.com/questions/28333/where-are-strings-stored-in-solidity)
            - Different data structures have different default locations:
                - State variables are always in storage
                - function arguments are always in memory
                - local variables of struct, array or mapping type reference storage by default
                - local variables of value type (i.e. neither array, nor struct nor mapping) are stored in the stack
            - This has considerable implications when optimising a contract for gas costs. You want to use the cheapest possible solution for every single problem, even when defining a variable. 
                - Relevant [[external resources]] for reading:
                    - https://eth.wiki/en/faqs/subtleties
                    - https://ethereum.stackexchange.com/questions/1701/what-does-the-keyword-memory-do-exactly
                    - https://medium.com/coinmonks/what-the-hack-is-memory-and-storage-in-solidity-6b9e62577305
            - When using `memory` and `storage` together, we must be careful so that we don't have unwanted results. A  `memory` definition of a `storage` object will create a local copy of that object, while a `storage` definition will reference the original object. 
                - [[external resources]]
                    - [What the hack is Memory and Storage in Solidity?
](https://medium.com/coinmonks/what-the-hack-is-memory-and-storage-in-solidity-6b9e62577305)
        - When to use `memory` keyword?
            - Storage is in essence the state of your contract. It's expensive to use this. 
            - Memory is best to be used locally in functions, as a temporal save to be used inside the function.
                - Read more on the differences between `memory` and `storage` [[external resources]]
                    -  [Ethereum Solidity: Memory vs Storage & When to Use Them](https://medium.com/coinmonks/ethereum-solidity-memory-vs-storage-which-to-use-in-local-functions-72b593c3703a) 
                    - [In Ethereum Solidity, what is the purpose of the “memory” keyword?](https://stackoverflow.com/questions/33839154/in-ethereum-solidity-what-is-the-purpose-of-the-memory-keyword)
        - `uint` variable types:
            - `uint` is an alias for `uint256`. `uint256` means that it's an `unsigned` `integer` that takes up `256` bits, thus it can represent from `0` to `2^255-1 = 1,157920892e77`
            - When using `uint` in structs:
                - This is an important consideration for [[ethereum gas cost]]. When creating the smart contract, solidity will store variables of the same type together, in increments of `32 bytes`. 
            - In general, Solidity will reserve `32 bytes = 256 bits` of storage for every `uint`, even if it is a `uint8`. Packing works  **only** in structs.
            - So, the design pattern that we need to keep in mind is to use the smallest type that covers our use-case. If we want to count up to 10, we don't need a `uint256`, we can store the state in a `uint8`. 
            - [[external resources]]
                - [Solidity Docs](https://docs.soliditylang.org/en/v0.4.21/types.html)
                - [Tight Variable Packing](https://fravoll.github.io/solidity-patterns/tight_variable_packing.html)
                - [Optimisation in Solidity part 1](https://medium.com/coinmonks/gas-optimization-in-solidity-part-i-variables-9d5775e43dde)
        - [[ethereum gas cost]] **considerations**.
            -  It's the sum of the cost of all the operations of the contract. 
            - #.rm-block--blue An interesting #thought on the previous ideas is that writing inefficient code (e.g assigning 32bits for a number that will take up to 8bits) costs literally money. We hardly think about these kinds of optimizations most of the time, but in Solidity, it should be top of mind.
            - Although this is a more advanced feature, we can further optimise our code by using `bitwise` operations. Meaning that we perform operations directly on binary numbers. This is optimal when the state of `something` has a binary representation.
                - At this point, it's interesting to keep it in mind. We will dive into this kind of optimisation at some later point.
                - [[external resources]]
                    - [using bitmaps for efficient solidity smart contracts](https://hiddentao.com/archives/2018/12/10/using-bitmaps-for-efficient-solidity-smart-contracts)
                    - [Efficient bit packing](https://ethereum.stackexchange.com/questions/77099/efficient-bit-packing)
            - Modifying an array is an expensive operation. We should try to use alternative ways to get the same functionality. __For example:__
                - Removing an item is particularly expensive because you have to swift all elements to the left (except if the item is the last one)
                - This is where `view` functions come in handy
                    - You have an array of all the items and then you create dynamic views of that array using a `view` function. 
                    - When called externally, the `view` function is free and I can change the view based on some parameter. 
                    - This is a relevant concept to how `views` work in a [[database]]
                    - Read more about `view` functions in **Function Modifiers**
                - [[external resources]]
                    - [Array or mapping, which costs more gas?](https://ethereum.stackexchange.com/questions/37549/array-or-mapping-which-costs-more-gas)
        - When defining a [Solidity Struct](https://www.tutorialspoint.com/solidity/solidity_structs.htm) you don't have to remember the order. There are different ways to call a specific attribute.
            - [[external resources]] [StackOverflow](https://ethereum.stackexchange.com/questions/1511/how-to-initialize-a-struct/1512)
        - **What are events?**
            - Smart contracts can __emit__ events, which are picked up by applications that are listening for them. In a way, it's an aditional way for smart contracts to communicate with apps that live outside of the blockchain. 
                - They are an abstraction on top of __logs__. Events are published in the form of logs.
            - They are used to signal about changes in the blockchain.
            - We can add the attribute `indexed` to up to three parameters which add them to a special data structure known as `topics`, instead of the data part of the log. Basically, we make the event more prominent. 
            - For example:
                - [web3.js](https://docs.ethers.io/v5/) app that subscribes to `indexed` events:
                    - ```javascript
var options = {
    fromBlock: 0,
    address: web3.eth.defaultAccount,
    topics: ["0x0000000000000000000000000000000000000000000000000000000000000000", null, null]
};
web3.eth.subscribe('logs', options, function (error, result) {
    if (!error)
        console.log(result);
})
    .on("data", function (log) {
        console.log(log);
    })
    .on("changed", function (log) {
});
``` 
                - Solidity code
                    - ```javascript
// SPDX-License-Identifier: GPL-3.0
pragma solidity >=0.4.21 <0.9.0;

contract ClientReceipt {
    event Deposit(
        address indexed _from,
        bytes32 indexed _id,
        uint _value
    );

    function deposit(bytes32 _id) public payable {
        // Events are emitted using `emit`, followed by
        // the name of the event and the arguments
        // (if any) in parentheses. Any such invocation
        // (even deeply nested) can be detected from
        // the JavaScript API by filtering for `Deposit`.
        emit Deposit(msg.sender, _id, msg.value);
    }
}```
            - Events can also be used to **cheaply** store information in the form of logs. Applications access the data of an event by reading the logs of a contract. 
                - Logs are much cheaper than `storage`.
                - **Caveat:** Logs are **only **accessible by off-blockchain applications and not accessible by other contracts. This limits their functionality. 
                - While `cameCase` is used in Solidity, events use `CapitalCase`. 
            - [[external resources]]
                - [Events -- Solidity docs](https://docs.soliditylang.org/en/latest/contracts.html#events)
                - [LOGGING DATA FROM SMART CONTRACTS WITH EVENTS](https://ethereum.org/ig/developers/tutorials/logging-events-smart-contracts/)
                - [A Guide to Events and Logs in Ethereum Smart Contracts](https://consensys.net/blog/developers/guide-to-events-and-logs-in-ethereum-smart-contracts/)
        - **String comparison**
            - It is not possible to directly compare strings in Solidity.
            - We need to hash the __binary encoding__ of the strings and then compare the hashes.
            - ```javascript
keccak256(abi.encodePacked((a))) == keccak256(abi.encodePacked((b)))```
            - [[external resources]]
                - [how to compare strings in solidity?](https://ethereum.stackexchange.com/questions/30912/how-to-compare-strings-in-solidity)
        - **Function Modifiers**
            - **Solidity** has a concept called **function modifier**. It creates additional features on the **function** or apply some restriction on the **function**
            - They are used so that we don't repeat certain functionality (e.g `require(x)`). It allows for cleaner code and better readability.
                - We could have the same functionality by defining a function and then calling it from every function, but this would result in **less** readable code. 
            - There are **custom** modifiers, which are created by the user, and **built-in**.
            - Built-in modifiers:
                - **Pure**: The function will not read or write the storage state
                - **View**: The function will not write the storage state (but can read it)
                - **View, Pure** functions have great applicability in optimizing [[ethereum gas cost]], as they **don't cost anything** when called externally, that is from outside the contract. As they don't change the state of the blockchain, they don't require any transactions. 
                    - The default behavior is that the [[EVM]] will execute the function and then roll back any changes that it makes to the state. Thus, in effect only "reading" the state will produce any meaningful results. It uses gas but without spending it so it can defend itself from infinite loops.  **It is used temporarily but not paid.**
                    - **caveat**: If the function is called internally from another function that is not a `view` or `pure`, **then it will cost gas **because the calling function will still need to alter the state. It has to be called externally so that it doesn't cost any gas. 
                    - When executing a smart contract function that is `view, pure, constant`, in essence we `call` the function without performing a transaction. There is a difference between `call` and sending a `transaction` to a function, since the second is broadcasted to the network and update the state of the blockchain. See **Transactions vs Calls**
                    - **Why use view, pure?**
                        - The value of these types of functions is that they don't cost any gas when called externally. Thus we can use them for functions that we expect to call from apps **outside of the blockchain**, which may need to read the contract for some data. 
            - Custom Modifers:
                - We can define our own modifiers, we can give any feature that we want to a function. Mostly, we use modifiers to restrict certain functions to certain addresses (e.g owners).
                    - ```javascript
  modifier onlyOwnerOf(uint _zombieId) {
    require(msg.sender == zombieToOwner[_zombieId]);
    _;```
                    - The `_;` part signifies to Solidity where the function's code should start. It can be at the start or at the end of the modifier, so the modifier's code can be placed at any point in the function's codepath. In general, it's simpler to put it at the start. 
            - Modifiers that are defined in libraries, can't be used in contracts that inherit (for inheritance, check **What is contract inheritance?** ) from them. The way to implement such functionality is to apply the modifier to a library function and then call the function. See [Solidity modifiers in library?](https://ethereum.stackexchange.com/questions/68529/solidity-modifiers-in-library)
            - [[external resources]]
                - [Solidity docs](https://docs.soliditylang.org/en/v0.5.3/contracts.html#function-modifiers)
                - [Tutorialspoints](https://www.tutorialspoint.com/solidity/solidity_function_modifiers.htm)
                - [Solidity modifiers: good or bad?](https://ethereum.stackexchange.com/questions/67502/solidity-modifiers-good-or-bad)
                - [Solidity modifiers in library?](https://ethereum.stackexchange.com/questions/68529/solidity-modifiers-in-library)
                - [Solidity Tutorial : all about Modifiers
](https://jeancvllr.medium.com/solidity-tutorial-all-about-modifiers-a86cf81c14cb)
        - **Internal Transactions**
            - > Internal transactions, despite the name (which isn't part of the yellow paper; it's a convention people have settled on), aren't actual transactions, and aren't included directly in the blockchain; they're value transfers that were initiated by executing a contract. 

As such, they're not stored explicitly anywhere: they're the effects of running the transaction in question on the blockchain state. Blockchain explorers like etherscan obtain them by running a modified node with an instrumented EVM, which record all the value transfers that took place as part of transaction execution, storing them separately.
                - [[external resources]]
                    - [Normal transactions VS. Internal transactions in etherscan](https://ethereum.stackexchange.com/questions/6429/normal-transactions-vs-internal-transactions-in-etherscan)
                    - [How to get contract internal transactions](https://ethereum.stackexchange.com/questions/3417/how-to-get-contract-internal-transactions)
        - **Transactions vs Calls**
            - There are 2 ways to interact with a smart contract, via a __transactions__ and a __call__. 
            - [[external resources]]
                - [What is the difference between a transaction and a call?](https://ethereum.stackexchange.com/questions/765/what-is-the-difference-between-a-transaction-and-a-call)
        - **Function Visibility** **__internal, external, public and private__**
            - Solidity knows two kinds of function calls (internal ones that do not create an actual [[EVM]] call (also called a “message call”) and external ones that do)
            - __Internal__ functions can only be called inside the current contract (more specifically, inside the current code unit, which also includes internal library functions and inherited functions)
            - __external__ functions are called using their function signature and. the address of the contract.  T**hey can't be called from inside.**
            - __public__ functions are part of the contract interface and can be either called internally or via messages
            - __private__ functions and state variables are only visible for the contract they are defined in and not in derived contracts.
            - In essence: **private** is a subset of **internal** and **external** is a subset of **public**.] I
                - __Internal__ functions can be used by contracts that inherit from the original, while __private__ can not. 
                - __external__ functions can only be called from outside the contract, while __public__ can be both. 
                - __public__ is more expensive than __external__, because it copies the function inside the memory so it's available for the smart contract, while the external omits that, since it's called only externally.
                - The use of the proper visibility type is an important part of creating [[secure code]] in solidity.
                    - Read more in [Secure Development Recommendations](https://consensys.github.io/smart-contract-best-practices/recommendations/)
            - #.rm-block--blue Everything that is inside a contract is visible to all observers external to the blockchain. Making something private only prevents other contracts from accessing and modifying the information, but it will still be visible to the whole world outside of the blockchain.
            - [[external resources]]
                - [Solidity Docs](https://docs.soliditylang.org/en/v0.8.4/contracts.html)
                - [external vs public best practices](https://ethereum.stackexchange.com/questions/19380/external-vs-public-best-practices)
                - [What is the difference between an internal/external and public/private function in solidity?](https://ethereum.stackexchange.com/questions/32353/what-is-the-difference-between-an-internal-external-and-public-private-function)
                - [Secure Development Recommendations](https://consensys.github.io/smart-contract-best-practices/recommendations/)
        - **Interfaces**
            - #definition An interface explains to the compiler what functions are available to be called on an external contract without requiring the full source code of that contract to be imported.
            - ```javascript
contract KittyInterface {
  function getKitty(uint256 _id) external view returns (
    bool isGestating,
    bool isReady,
    uint256 cooldownIndex,
    uint256 nextActionAt,
    uint256 siringWithId,
    uint256 birthTime,
    uint256 matronId,
    uint256 sireId,
    uint256 generation,
    uint256 genes
  );
}

contract ZombieFeeding is ZombieFactory {

  address ckAddress = 0x06012c8cf97BEaD5deAe237070F9587f8E7A266d;
  KittyInterface kittyContract = KittyInterface(ckAddress); // this initialization is what I don't get

  function feedAndMultiply(uint _zombieId, uint _targetDna) public {
    require(msg.sender == zombieToOwner[_zombieId]);
    Zombie storage myZombie = zombies[_zombieId];
    _targetDna = _targetDna % dnaModulus;
    uint newDna = (myZombie.dna + _targetDna) / 2;
    _createZombie("NoName", newDna);
  }    
}```
                - The example is from [Crypto Zombies](https://cryptozombies.io/). It is possible that the contract which exists in the `ckAddress` has many more functions, but we tell the compiler that we only care about the `getKitty` function.
                - `KittyInterface kittyContract = KittyInterface(ckAddress);` means that it's casting the address. `kittyContract` is now a contract of type `KittyInterface` and when you call functions on that contract, they should be sent to the address `ckAddress`.
            - They are limited to what the [[smart contract ABI]] can represent. They have function definitions, but not their implementations. 
            - They are used as a basis for other contracts to inherit from. The provide better self-documentation and it allows you to follow the [Template method patter](https://en.wikipedia.org/wiki/Template_method_pattern)
            - [[external resources]]
                - [Solidity: How to know when to use Abstract Contracts vs Interfaces](https://medium.com/upstate-interactive/solidity-how-to-know-when-to-use-abstract-contracts-vs-interfaces-874cab860c56)
        - **calldata** variable storage
            - It's like memory, meaning that it's temporary. 
            - it's used only for the __arguments__ of __external__ functions. 
            - [[external resources]]
                - [When should I use calldata and when should I use memory?](https://ethereum.stackexchange.com/questions/74442/when-should-i-use-calldata-and-when-should-i-use-memory)
        - What is `abi.encodepacked`?
            - It concatenates it's input data in a __bytes array__ in memory. 
            - It is usually used when we want to `hash` something, as the hashing function requires a __bytes array__. 
            - We have come across `abi.encodepacked` and `keccak256()` hash function in **String comparison**
            - [[external resources]]
                - [Is it safe to use abi.encodePacked in production solidity code?](https://ethereum.stackexchange.com/questions/61299/is-it-safe-to-use-abi-encodepacked-in-production-solidity-code)
        - **Assert vs Require**
            - Both functions are used to check a certain requirement before advancing in the codepath.
            - 2 major considerations: Gas efficiency and Bytecode analysis
                - Gas efficiency:
                    - __assert(false)__ compiles to `0xfe`, which is an invalid opcode, **using up all remaining gas**, and reverting all changes.
                    - __require(false)__ compiles to `0xfd` which is the `REVERT` opcode, meaning it will refund the remaining gas. 
                - Bytecode analysis:
                    - `assert` creates an error of type `Panic(uint256)`. 
                    - The `require` function either creates an error without any data or an error of type `Error(string)`
                - Use:
                    - > Assert should only be used to test for internal errors, and to check invariants. **Properly functioning code should never create a Panic, not even on invalid external input**. If this happens, then there is a bug in your contract which you should fix. Language analysis tools can evaluate your contract to identify the conditions and function calls which will cause a Panic
                        - from [[external resources]], emphasis mine.
                            - [Error handling: Assert, Require, Revert and Exceptions](https://docs.soliditylang.org/en/develop/control-structures.html#error-handling-assert-require-revert-and-exceptions)
                    - So, we use `assert` to make sure that there are no errors in our code and `require` to test for certain conditions. 
                - [[external resources]]
                    - [Difference between require and assert and the difference between revert and throw](https://ethereum.stackexchange.com/questions/15166/difference-between-require-and-assert-and-the-difference-between-revert-and-thro)
                - 
        - **What is contract inheritance?** 
            - inheritance is a very popular OOP paradigm. It allows structures of data to __inherit__ attributes and functionality from other structures. It enables to repeat less code and improve code quality. 
            - ```javascript
contract parent{  
  
    // Declaring internal 
    // state varaiable  
    uint internal sum;  
        
    // Defining external function 
    // to set value of internal 
    // state variable sum
    function setValue() external {  
        uint a = 10;
        uint b = 20;
        sum = a + b;
    }  
}  
  
// Defining child contract  
contract child is parent{  
       
    // Defining external function 
    // to return value of 
    // internal state variable sum
    function getValue(
    ) external view returns(uint) {  
        return sum;  
    }  
}  ```
            - In Solidity, a contract can inherit from another country. That means:
                - A derived contract can access all non-private members including state variables and internal methods. But using this is not allowed.
                - Function overriding is allowed provided the function signature remains the same. In case of the difference of output parameters, the compilation will fail.
                - We can call a super contract’s function using a super keyword or using a super contract name.
                - In the case of multiple inheritances, function calls using super gives preference to most derived contracts.
                - [[external resources]]
                    - [Solidity - Inheritance](https://www.geeksforgeeks.org/solidity-inheritance/)
                    - [Solidity by Example - Inheritance](https://solidity-by-example.org/inheritance/)
        - **Import contract**
            - To be able to use functions or inherit (read more in **What is contract inheritance?** ) functionality from another contract, we need to import them.
            - `import './OtherContract.sol'`
        - **Security Considerations**1
            - **Math overflow and underflow. **
                - An 8-bit unsigned integer can store values between `0 `and `255 (2^8-1)`. When the result of some arithmetic falls outside that supported range, an [integer overflow](https://en.wikipedia.org/wiki/Integer_overflow) occurs. On the Ethereum Virtual Machine ([[EVM]]), the consequence of an integer overflow is that the most significant bits of the result are lost. For example, when working with 8-bit unsigned integers, `255 + 1 = 0`. This is easier to see in binary, where `1111 1111 + 0000 0001 `should be 1 0000 0000, but because only 8 bits are available, the leftmost bit is lost, resulting in a value of `0000 0000`
                    - excerpt from [[external resources]] / [How overflows work in solidity?](https://ethereum.stackexchange.com/questions/63146/how-overflows-work-in-solidity)
                - Overflow can easily happen if we use mathematical expressions without checking for the overflow (e.g `counter++`). If the loop is big enough, we could result with an overflow. 
                - **Solution**
                    - The solution is simple really. We can use the library offered by [[OpenZeppelin]], named **SafeMath**.
                        - SafeMath has different __safe__ functions for different types of `uint`. For example, you can **Import contract** for instances `SafeMath32` and `SafeMath16`.
                        - [[external resources]]
                            - [SafeMath](https://docs.openzeppelin.com/contracts/2.x/api/math)
                            - [How to Install OpenZeppelin contracts](https://docs.openzeppelin.com/contracts/4.x/)
                    - The library is rather simple. It implements math operations, while interweaving the appropriate `assert` and `require` so that we don't end up with a result that we don't want to.
                        - You can read more about these functions in **Assert vs Require**
            - **Randomness**
                - Randomness is an important security consideration, because if we design a system that is based on randomness, then the system is vulnerable in case the randomness is not completely random but __pseudorandom__. 

For example, imagine a coin-toss where if we know a secret, we can game the coin to always fall with the side that we want. 
                - A **naive** solution would be to use a `pseudorandom` function generator, e.g `rand`, combine it with `time` and always increment it before hashing the result. This would create a `random` hash, since due to the [nature of hashing](https://en.wikipedia.org/wiki/Cryptographic_hash_function), it's impossible to foresee the result. 
                    - ```javascript
function randMod(uint _modulus) internal returns(uint) {
    randNonce = randNonce.add(1);
    return uint(keccak256(abi.encodePacked(now, msg.sender, randNonce))) % _modulus;
  }
```
                    - This is not a perfect solution, since the miner can withhold the transaction be directed and instead run the randomFunction again and again, until they have the result that they want. Then, they will broadcast the transaction to the rest of the network. 
                        - That being said, it is __good enough__ for some applications. Since, the cost to actually perform such an attack could be well over the benefit.
                            - In essence, while from a point of [[computer science]] this may not be very secure, from a point of [[economics]], it's __secure enough__. 
                    - Purely with a smart contract, it's impossible to get true randomness, this is a design choice of the language/implementation itself. There are a couple of designs that can allow for randomness to enter the smart contract. Read more in the following resources:
                        - [[external resources]]
                            - [How can I securely generate a random number in my smart contract?](https://ethereum.stackexchange.com/questions/191/how-can-i-securely-generate-a-random-number-in-my-smart-contract)
            - Storage variables are part of the smart contract state. They persist beyond the execution of a function. 
                - This can be unsafe, because if you pass a storage struct inside a function, it passes a pointer, which means that you can change the original storage variable from inside the function. This could be unwanted. 
                - For example: ```javascript
contract FirstSurprise {
 
 struct Camper {
   bool isHappy;
 }
 
 mapping(uint => Camper) public campers;
 
 function setHappy(uint index) public {
   campers[index].isHappy = true;
 }
 function surpriseOne(uint index) public {
   Camper c = campers[index];
   c.isHappy = false;
 }
}```
                    - In the example above, when we run `supriseOne`, Solidity does not copy the storage variable into memory, but rather passes a `pointer` to the original struct. That means that in essence we modify the original struct that we had set `isHappy = true`.
                    - Read more about storage pointers in [Storage Pointers in Solidity](https://blog.b9lab.com/storage-pointers-in-solidity-7dcfaa536089)
                - [[best practice]]** You should never define storage variables inside a function**
                - [[external resources]]
                    - [Storage Pointers in Solidity](https://blog.b9lab.com/storage-pointers-in-solidity-7dcfaa536089)
    - ### Cryptozombies - Solidity Path lesson 6, Advanced Solidity Path lesson 1-2
        - **What is web3 ?**
            - [web3.js](https://docs.ethers.io/v5/) is one of the main #Javascript libraries that gives tools to the developer to create applications that interact with the #[[Learn Ethereum]] blockchain. 
            - In essence, the library communicates with an [[ethereum full node]] via [JSON-RPC](https://eth.wiki/json-rpc/API). It can read the blockchain or issue transactions. 
            - The pattern that we follow usually is to create a smart contract that runs on the blockchain and then create an app that works synergistically with the smart contract, as an abstraction between the smart contract and the user. 
            - There are many different libraries and tools. Take a look at [[dev tools]]
        - How to call a contract, call a function
            - When calling a function with web3, we need to specify how much __wei__ we send, not Eth.  
        - **Testing**
            - Testing is very important because in Solidity, even the slighest mistake can result in considerable loss of funds. 
            - Smart contracts are not able to change once we launch them, we must be certain of their quality. Iterative development is not that easy, we will have to launch a new contract.
            - 
- ## [[external resources]]
    - ### Raw
        - {{[[query]]: {and: [[external resources]] [[Learn Solidity]] {{[[TODO]]}} }}}
    - ### Curated
        - [[Tutorials]]
            - [The Complete Guide to Full Stack Ethereum Development](https://dev.to/dabit3/the-complete-guide-to-full-stack-ethereum-development-3j13) by [[Nader Dabit]]
            - [Crypto Zombies](https://cryptozombies.io/)
            - [Scaffold-eth](https://github.com/austintgriffith/scaffold-eth#-learning-solidity) -- Everything you need to get started building decentralised application on Ethereum ✌️ by [[Austin Griffth]]
        - [[dev tools]]
            - [hardhat](https://hardhat.org/)
                - https://medium.com/nomic-labs-blog/better-solidity-debugging-console-log-is-finally-here-fc66c54f2c4a
            - #Javascript 
                - [ethers.js](https://docs.ethers.io/v5/)
                - [web3.js](https://docs.ethers.io/v5/)
            - #Rust
                - [ethers.rs](https://github.com/gakonst/ethers-rs/)
        - [[best practices]]:
            - https://docs.openzeppelin.com/learn/
            - https://consensys.github.io/smart-contract-best-practices
            - https://github.com/paulrberg/solidity-template
            - [Secure Development Recommendations](https://consensys.github.io/smart-contract-best-practices/recommendations/)
- ## Solidity capstone project
    - [[project brainstorming]]
        - [[Flashbots]]
        - [[NFT project]]
        - [[Optimism]]
- ## Roadmap #kanban
    - {{[[kanban]]}}
        - [[project TODO]]
            - Find a real-life scenario to create a smart contract. As a Solidity capstone project
            - [The Complete Guide to Full Stack Ethereum Development](https://dev.to/dabit3/the-complete-guide-to-full-stack-ethereum-development-3j13) by [[Nader Dabit]]
            - [Crypto Zombies](https://cryptozombies.io/) Build an oracle part 2
            - [Crypto Zombies](https://cryptozombies.io/) Build an oracle part 3
        - [[project DOING]]
            - [Crypto Zombies](https://cryptozombies.io/) Build an oracle part 1
        - [[project DONE]]
            - [Crypto Zombies](https://cryptozombies.io/) Basic Solidity
            - [Crypto Zombies](https://cryptozombies.io/) Advanced Solidity
            - [Crypto Zombies](https://cryptozombies.io/) web3.js, testing
    - 
- ## Learning Log
    - The learning log is the raw notes that I take as I read/learn about this domain. These notes are then curated, refined and transferred into Insights
    - {{[[query]]: {and: [[learning log]] [[Learn Solidity]] {{[[TODO]]}} }}}
