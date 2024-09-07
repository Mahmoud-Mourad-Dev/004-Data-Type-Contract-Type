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
