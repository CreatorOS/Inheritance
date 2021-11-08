# Inheritance

Solidity supports multiple inheritance. Contracts can inherit other contract by using the `is` keyword.

Function that is going to be overridden by a child contract must be declared as `virtual`.

Function that is going to override a parent function must use the keyword `override`.

```
contract A {
    function foo() public pure virtual returns (string memory) {
        return "A";
    }
}

// Contracts inherit other contracts by using the keyword 'is'.
contract B is A {
    // Override A.foo()
    function foo() public pure virtual override returns (string memory) {
        return "B";
    }
}

contract C is A {
    // Override A.foo()
    function foo() public pure virtual override returns (string memory) {
        return "C";
    }
}
```

## Inheriting from multiple contracts

Contracts can inherit from multiple parent contracts.

- When a function is called that is defined multiple times in different contracts, parent contracts are searched from right to left, and in depth-first manner.

Here, `D.foo()` returns "C" since C is the right most parent contract with function `foo()`.
While `E.foo()` returns "B" since B is the right most parent contract with function `foo()`.

```
contract D is B, C {
    function foo() public pure override(B, C) returns (string memory) {
        return super.foo();
    }
}

contract E is C, B {
    function foo() public pure override(C, B) returns (string memory) {
        return super.foo();
    }
}
```

## Order of inheritance

- Order of inheritance is important.

- You have to list the parent contracts in the order from “most base-like” to “most derived”.
- Swapping the order of A and B will throw a compilation error.

```
/* Graph of inheritance
    A
   / \
  B   C
 / \ /
F  D,E

*/



contract FInh is A, B {
    function foo() public pure override(A, B) returns (string memory) {
        return super.foo();
    }
}
```
