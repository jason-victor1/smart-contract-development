# Solidity and Smart Contract Development

This repo covers the essentials of developing, deploying, and managing smart contracts on the Ethereum blockchain.

## Key Topics Covered

- **[Basics of Remix IDE](#basics-of-remix-ide)**
- **[Compiler Directives and SPDX License Identifiers](#compiler-directive)**
- **[Writing and Compiling Smart Contracts](#writing-the-smart-contract)**
- **[Managing Data Locations: Storage, Memory, and Calldata](#data-locations-in-solidity)**
- **[Error and Warning Management](#error-and-warning-management)**
- **[Understanding Function Visibility and Variable Scope](#variable-scope)**
- **[Using Pure and View Functions](#pure-and-view-keywords)**
- **[Storing Multiple Values with Arrays and Structs](#storing-multiple-numbers-with-arrays-and-structs)**
- **[Contract Creation and Deployment](#contract-creation-and-deployment)**

## Basics of Remix IDE

### Introduction to Remix
Remix IDE is an Integrated Development Environment for Solidity smart contract development. It provides tools to visualize, compile, and deploy smart contracts.

### Getting Started
1. **Open Remix**: Visit [Remix IDE](https://remix.ethereum.org/).
2. **Clean Up**: Optionally remove existing files.
3. **Create New File**: Create a new file, e.g., `SimpleStorage.sol`.

### Compiler Directive
Specify the Solidity compiler version using the `pragma` directive.
- **Example**:
  ```solidity
  pragma solidity ^0.8.26;
  ```

### SPDX License Identifier
Include an SPDX License Identifier for legal clarity.
- **Example**:
  ```solidity
  // SPDX-License-Identifier: MIT
  pragma solidity ^0.8.26;
  ```

### Writing the Smart Contract
Define your contract using the `contract` keyword.
- **Example**:
  ```solidity
  // SPDX-License-Identifier: MIT
  pragma solidity ^0.8.26;

  contract SimpleStorage {
      uint256 favoriteNumber;
      
      function store(uint256 _favoriteNumber) public {
          favoriteNumber = _favoriteNumber;
      }
  }
  ```

### Compiling
1. **Select Compiler**: In Remix, go to the Solidity Compiler tab.
2. **Choose Version**: Match the version specified in your Solidity file.
3. **Compile**: Hit the Compile button. Address any errors highlighted by Remix.

## Data Locations in Solidity

### Calldata and Memory
- **Temporary Variables**: Exist only during function execution.
- **Memory**:
  - Default for most variable types.
  - Must specify for strings and arrays.
  - Example:
    ```solidity
    string memory variableName = "someValue";
    ```
- **Calldata**:
  - Read-only, cheaper than memory.
  - Used for input parameters.
  - Example:
    ```solidity
    function addPerson(string calldata _name, uint256 _favoriteNumber) public {
        listOfPeople.push(Person(_favoriteNumber, _name));
    }
    ```

### Storage
- **Persistent Variables**: Retain values between function calls and transactions.
- **Example**:
  ```solidity
  contract MyContract {
      uint256 favoriteNumber; // storage variable
  }
  ```

### Variable Scope
- **Scope**: Context within which a variable is defined and accessible.
- **Example**:
  ```solidity
  function addPerson(string memory _name, uint256 _favoriteNumber) public {
      uint256 test = 0; // stored in memory or stack
      listOfPeople.push(Person(_favoriteNumber, _name));
  }
  ```

## Error and Warning Management

### Warnings vs Errors
- **Warnings**: Code can compile and deploy but should be addressed.
- **Errors**: Must be resolved before deployment.
- **Color Indicators**:
  - **Red**: Compilation error.
  - **Yellow**: Warning, double-check the code.

### Resources
- AI tools (ChatGPT, Phind, Bard)
- GitHub Discussions
- Stack Exchange Ethereum
- Peeranha

## Pure and View Keywords

### Definitions
- **View**: Reads state but doesn’t modify it.
- **Pure**: Neither reads nor modifies state.
- **Examples**:
  ```solidity
  function retrieve() public view returns(uint256){
      return favoriteNumber;
  }

  function retrieve() public pure returns(uint256){
      return 7;
  }
  ```

## Storing Multiple Numbers with Arrays and Structs

### Arrays
- **Example**:
  ```solidity
  uint256[] list_of_favorite_numbers;
  ```

### Structs
- **Example**:
  ```solidity
  struct Person {
      uint256 my_favorite_number;
      string name;
  }
  Person[] public list_of_people;

  function add_person(string memory _name, uint256 _favorite_number) public {
      list_of_people.push(Person(_favorite_number, _name));
  }
  ```

## Contract Creation and Deployment

### Avoiding Costly Iterations
If we want to know just one person's favorite number (e.g. Chelsea's), but our contract holds a (long) array of Person, we would need to iterate through the whole list to find the desired value:

```solidity
list_of_people.add(Person("Pat", 7));
list_of_people.add(Person("John", 8));
list_of_people.add(Person("Mariah", 10));
list_of_people.add(Person("Chelsea", 232));
/* go through all the people to check their favorite number.
If name is "Chelsea" -> return 232
*/
```

Iterating through a long list of data is usually expensive and time-consuming, especially when we do not need to access elements by their index.

### Mapping
To directly access the desired value without the need to iterate through the whole array, we can use mappings. Mappings are sets of unique keys linked to a value, similar to hash tables or dictionaries in other programming languages. Looking up a name (key) will return its corresponding favorite number (value).

#### Defining a Mapping
A mapping is defined using the `mapping` keyword, followed by the key type, the value type, the visibility, and the mapping name. In our example, we can construct an object that maps every name to its favorite number:

```solidity
mapping (string => uint256) public nameToFavoriteNumber;
```

Previously, we created an `addPerson` function that was adding a `Person` struct to an array `list_of_people`. Let's modify this function and add the `Person` struct to a mapping instead of an array:

```solidity
nameToFavoriteNumber[_name] = _favoriteNumber;
```

#### Important Notes
- Mappings have a constant time complexity for lookups, meaning that retrieving a value by its key is done in constant time.
- The default value for all key types is zero. For example, `nameToFavoriteNumber["ET"]` will be equal to 0.

#### Conclusion
Mappings can be a versatile tool to increase efficiency when attempting to find elements within a larger set of data.

### Deploying My First Contract

#### Preparation
Deploy the contract to a real testnet, which fully simulates a live blockchain environment without using real Ether. 

##### Caution
Do not deploy the contract immediately to a testnet. Write tests, carry out audits, and ensure the robustness of your contract before deploying it to production.

##### Compilation Check
Before deploying, always perform a compilation check to ensure that the contract has no errors or warnings and is fit for deployment.

#### Deployment on a Testnet
1. Go into the deployment tab and switch from the local virtual environment (Remix VM) to the Injected Provider - MetaMask. This allows Remix to send requests and interact with your MetaMask account.

   ![Switch Environment](https://github.com/jason-victor1/smart-contract-development/blob/main/%231%20change%20environment%20to%20injector%20metamask.png?raw=true)

2. Ensure you have enough Sepolia ETH in your account, which you can obtain from a faucet. Once your balance is sufficient, you can proceed by clicking the "Deploy" button.

3. MetaMask will ask to sign and send the transaction on the testnet. The deployment transaction is displayed on Etherscan.

   ![Deployed Smart Contract](https://github.com/jason-victor1/smart-contract-development/blob/main/%232%20deployed%20to%20testnet.png?raw=true)

#### Interacting with the Deployed Contract
1. Since the contract has been deployed, we can now interact with it and update the blockchain. For example, if you want to store a number, you can do so by clicking the button 'store'. MetaMask will ask for another transaction confirmation to update the favorite number.

   ![Update Smart Contract](https://github.com/jason-victor1/smart-contract-development/blob/main/%233%20update%20the%20contract%20using%20the%20store%20function.png?raw=true)

2. Check the details on Etherscan at the deployed address:

   ![Etherscan Transaction](https://github.com/jason-victor1/smart-contract-development/blob/main/%234%20store%20function%20success%20on%20etherscan.png?raw=true)

### View and Pure Functions
View and pure functions will not send transactions. 

Go back to Remix IDE and click the retrieve function which shows the value used in the store function.

   ![Retrieve Function Value](https://github.com/jason-victor1/smart-contract-development/blob/main/%235%20stored%202500%20on%20chain.png?raw=true)

Go back to Etherscan and search by the smart contract address. Both transactions are listed:

   ![Etherscan Transactions Overview](https://github.com/jason-victor1/smart-contract-development/blob/main/%236%20%20both%20transaction%20for%20the%20smart%20contract.png?raw=true)

