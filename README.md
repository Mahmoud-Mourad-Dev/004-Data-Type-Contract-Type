# 004-Data-Type-Contract-Type
Every contract defines its own type.
When you define a contract in Solidity, that contract becomes a type that can be used to create instances of itself, interact with other contracts, and call functions on those contracts.
```solidity
// SPDX-License-Identifier: GPL-3.0
pragma solidity >=0.4.16 <0.9.0;

contract A {
    uint public ipadPrice = 1000;

}
contract B{
    A public a;
}
```
```solidity
contract A {
    function someFunction() public {}
}

contract B {
    function anotherFunction() public {}
}

contract C {
    A public aContract;
    B public bContract;

    function callA() public {
        aContract.someFunction();  // Allowed, as aContract is of type A
        // bContract.someFunction();  // Compile-time error, bContract is of type B
    }
}
```
```solidity

// SPDX-License-Identifier: MIT
pragma solidity >=0.4.16 <0.9.0;
contract A {
function say() public pure returns(string memory){
    return "hello";
}
}
contract B{
    A a = new A();
    function get() public view returns(string memory){
        return a.say();

        
    }

}
```

```solidity

// SPDX-License-Identifier: MIT
pragma solidity >=0.4.16 <0.9.0;

contract Parent {
    function sayHello() public pure returns (string memory) {
        return "Hello from Parent!";
    }
}

contract Child is Parent {
    function sayHi() public pure returns (string memory) {
        return "Hi from Child!";
    }
}

contract Example {
    function callParentFunction() public  returns (string memory) {
        Child child = new Child();
        Parent parent = child; // تحويل ضمني من Child إلى Parent
        return parent.sayHello();  // يمكن استدعاء دوال العقد الأب
    }
}
```
Contracts can be explicitly converted to and from the address type.

```solidity
//SPDX-License-Identifier: MIT
pragma solidity >=0.4.16 <0.9.0;
contract MyContract {
    // Function to get the contract's address
    function getAddress() public view returns (address) {
        return address(this);  // تحويل صريح من العقد إلى عنوانه
    }
}
```
Converting an Address to a Contract:
```solidity
//SPDX-License-Identifier: MIT
pragma solidity >=0.4.16 <0.9.0;
pragma solidity ^0.8.0;

contract Token {
    function balanceOf() public pure returns (uint) {
        return 100;  // Example function to return a balance
    }
}

contract Wallet {
    function getTokenBalance(address tokenAddress) public pure returns (uint) {
        Token token = Token(tokenAddress);  // Convert address to Token contract type
        return token.balanceOf();    // Call a function on the Token contract
    }
}
```
Contracts do not support any operators.
The members of contract types are the external functions of the contract including any state variables marked as public
```solidity

//SPDX-License-Identifier: MIT
pragma solidity >=0.4.16 <0.9.0;

contract MyContract {
        // Public state variable
        uint public myNumber = 42;

    // Public function that is part of the contract's external interface
    function externalFunction() public pure returns (string memory) {
        return "This is an external function";
    }
}


contract AnotherContract {
    function callExternalFunction(MyContract contractInstance) public pure returns (string memory) {
        return contractInstance.externalFunction();  // Accessing an external function
    }

    function getPublicVariable(MyContract contractInstance) public view returns (uint) {
        return contractInstance.myNumber();  // Accessing a public state variable via its getter
    }
}
```
For a contract C you can use type(C) to access type information about the contract.
In Solidity, type(C) is a built-in feature that allows you to access type information about the contract C. It provides information related to the contract type, such as its runtime properties like the contract’s creation code and runtime code. This is particularly useful for advanced functionality such as deploying contracts or analyzing contract bytecode.
```solidity

//SPDX-License-Identifier: MIT
pragma solidity ^0.8.0;

contract MyContract {
    uint public value;

    
}

contract Deployer {
    function getCreationCode() public pure returns (bytes memory) {
        return type(MyContract).creationCode;  // Returns the creation code of MyContract
    }
}
```
Getting the Contract Runtime Code:
type(MyContract).runtimeCode provides the actual executable code for MyContract after deployment.
This can be useful for verifying the contract code or interacting with contracts in a more low-level way
```solidity
//SPDX-License-Identifier: MIT
pragma solidity ^0.8.0;
pragma solidity ^0.8.0;

contract MyContract {
    uint public value;

    constructor(uint _value) {
        value = _value;
    }
}

contract Deployer {
    function getRuntimeCode() public pure returns (bytes memory) {
        return type(MyContract).runtimeCode;  // Returns the runtime code of MyContract
    }
}
```







