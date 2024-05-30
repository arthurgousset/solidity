# Solidity (Cheat Sheet)

> [!NOTE]  
> This is a cheat sheet on Solidity (mostly notes-to-self). They are incomplete by default.

## Mappings

Source: ChatGPT

Mappings in Solidity are a type of data structure that allows you to store and retrieve values
associated with specific keys. They are often used for associative arrays or dictionaries.

1. **Storage and Access**: Mappings are used to store key-value pairs. You can access the value
   associated with a key using the syntax `mappingName[key]`.

1. **No Length or Iteration**: Mappings in Solidity do not have a length property or a way to
   iterate over all key-value pairs. Unlike arrays, you cannot directly determine the number of
   entries in a mapping or iterate through them without additional data structures or helper
   functions.

1. **Custom Getters and Setters**: While Solidity provides automatic getters for public variables,
   including mappings, these getters only allow you to retrieve values by their key. To work with
   mappings more extensively (e.g., getting all keys or values), you need to implement custom
   functions.

## Arrays

Source: ChatGPT

Arrays in Solidity are a fundamental data structure used to store collections of elements:

1.  they can be either **fixed-size** or **dynamic**. **Fixed-size arrays** have a constant length
    defined at compile time, while **dynamic arrays** can change size at runtime.
1.  they can support various types of elements, including **primitive types** like `uint` and
    `address`, as well as **user-defined types** such as structs.
1.  they can be stored in **storage** or **memory**. **Storage arrays** are stored permanently on
    the blockchain, making them expensive to modify due to gas costs. **Memory arrays** are
    temporary and only exist during the execution of a function, making them cheaper to use.
1.  they can be indexed, iterated over, and manipulated using familiar operations from other
    programming languages.

**Fixed-size arrays**:

- Defined with a fixed number of elements.
- Size cannot be changed after declaration.
- Syntax:

  ```sol
  type[length] arrayName;
  ```

- For example:

  ```solidity
  pragma solidity ^0.8.0;

  contract FixedSizeArrayExample {
      // Declare a fixed-size array of 5 unsigned integers
      uint[5] public fixedArray;

      constructor() {
          // Initialize the array with some values
          fixedArray[0] = 10;
          fixedArray[1] = 20;
          fixedArray[2] = 30;
          fixedArray[3] = 40;
          fixedArray[4] = 50;
      }

      // Function to get the length of the fixed-size array
      function getLength() public view returns (uint) {
          return fixedArray.length; // Always returns 5
      }

      // Function to get an element at a specific index
      function getElement(uint index) public view returns (uint) {
          require(index < fixedArray.length, "Index out of bounds");
          return fixedArray[index];
      }
  }
  ```

**Dynamic arrays**:

- Uses functions like `push` and `pop` to modify the array.
- Size can be changed at runtime.
- Syntax:

  ```sol
  type[] arrayName;
  ```

- For example:

  ```solidity
  pragma solidity ^0.8.0;

  contract DynamicArrayExample {
      // Declare a dynamic array of unsigned integers
      uint[] public dynamicArray;

      constructor() {
          // Initialize the dynamic array with some values
          dynamicArray.push(10);
          dynamicArray.push(20);
          dynamicArray.push(30);
      }

      // Function to add an element to the dynamic array
      function addElement(uint value) public {
          dynamicArray.push(value);
      }

      // Function to remove the last element from the dynamic array
      function removeLastElement() public {
          require(dynamicArray.length > 0, "Array is empty");
          dynamicArray.pop();
      }

      // Function to get the length of the dynamic array
      function getLength() public view returns (uint) {
          return dynamicArray.length;
      }

      // Function to get an element at a specific index
      function getElement(uint index) public view returns (uint) {
          require(index < dynamicArray.length, "Index out of bounds");
          return dynamicArray[index];
      }
  }
  ```

**Storage vs Memory Arrays**:

- **Storage Arrays**: Persistent and stored on the blockchain. They are more costly to manipulate
  due to gas fees.
- **Memory Arrays**: Temporary and exist only during the execution of a function. They are cheaper
  to use since they are not stored on the blockchain.

- For example:

  ```solidity
  pragma solidity ^0.8.0;

  contract ArrayInMemoryExample {
      // Declare a storage array
      uint[] public storageArray;

      constructor() {
          // Initialize the storage array with some values
          storageArray.push(1);
          storageArray.push(2);
          storageArray.push(3);
      }

      // Function to demonstrate memory array usage
      function manipulateMemoryArray() public view returns (uint[] memory) {
          // Declare a memory array with the same length as storageArray
          uint[] memory memoryArray = new uint[](storageArray.length);

          // Copy elements from storageArray to memoryArray
          for (uint i = 0; i < storageArray.length; i++) {
              memoryArray[i] = storageArray[i];
          }

          // Modify the memory array (this won't affect the storage array)
          memoryArray[0] = 100;

          return memoryArray; // Return the modified memory array
      }
  }
  ```

## Compiler warnings

### Warning (2462): Visibility for constructor is ignored.

Source: ChatGPT

This warning indicates that specifying visibility for a constructor in Solidity is unnecessary and
is ignored by the compiler. In Solidity versions 0.7.0 and later, constructors are implicitly
`internal`, meaning they can only be called within the contract itself or derived contracts, making
the visibility keyword redundant.

In your code, the constructor of the `RegistryIntegrationTest` contract is marked with `public`
visibility. To resolve this warning, simply remove the visibility keyword from the constructor.

```diff
- constructor() public {
+ constructor() {
  // constructor body
}
```

### Warning (2018): Function state mutability can be restricted to view

Source: ChatGPT

This warning suggests that the state mutability of a function could be restricted to `view` if the
function does not modify the state of the blockchain. A `view` function promises not to alter the
state, which allows the Ethereum Virtual Machine (EVM) to optimize how it handles the function.

In your code, the function `test_shouldHaveAddressInRegistry` does not modify any state; it only
reads from the registry and logs some information. To resolve this warning, change the function's
mutability to `view` to explicitly state that it does not alter the state.

```diff
- function test_shouldHaveAddressInRegistry() public {
+ function test_shouldHaveAddressInRegistry() public view {
  // function body
}
```
