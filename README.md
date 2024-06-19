# The-style

To deploy a smart contract to the Ethereum network, you can use the Truffle framework along with the Infura service to interact with the Ethereum network. Here’s a step-by-step guide and a sample script to achieve this:

### Step-by-Step Guide

1. **Install Prerequisites:**
   - Ensure you have Node.js and npm installed.
   - Install Truffle and Ganache CLI globally:
     ```sh
     npm install -g truffle
     npm install -g ganache-cli
     ```

2. **Set Up a Truffle Project:**
   - Create a new directory for your project and initialize a Truffle project:
     ```sh
     mkdir MyDapp
     cd MyDapp
     truffle init
     ```

3. **Create a Smart Contract:**
   - In the `contracts` directory, create a file `MyContract.sol` with your Solidity code. For example:
     ```solidity
     // contracts/MyContract.sol
     pragma solidity ^0.8.0;

     contract MyContract {
         string public message;

         constructor(string memory initialMessage) {
             message = initialMessage;
         }

         function updateMessage(string memory newMessage) public {
             message = newMessage;
         }
     }
     ```

4. **Create Migration Script:**
   - In the `migrations` directory, create a new file `2_deploy_contracts.js`:
     ```js
     // migrations/2_deploy_contracts.js
     const MyContract = artifacts.require("MyContract");

     module.exports = function (deployer) {
         deployer.deploy(MyContract, "Hello, Ethereum!");
     };
     ```

5. **Configure Truffle to Use Infura:**
   - Install necessary dependencies:
     ```sh
     npm install @truffle/hdwallet-provider
     ```

   - Update the `truffle-config.js` to use Infura and HDWalletProvider. Replace `YOUR_INFURA_PROJECT_ID` and `YOUR_MNEMONIC` with your actual Infura project ID and wallet mnemonic:
     ```js
     // truffle-config.js
     const HDWalletProvider = require('@truffle/hdwallet-provider');
     const infuraKey = "YOUR_INFURA_PROJECT_ID";
     const mnemonic = "YOUR_MNEMONIC";

     module.exports = {
         networks: {
             rinkeby: {
                 provider: () => new HDWalletProvider(mnemonic, `https://rinkeby.infura.io/v3/${infuraKey}`),
                 network_id: 4,       // Rinkeby's id
                 gas: 4500000,        // Rinkeby has a lower block limit than mainnet
                 gasPrice: 10000000000
             },
         },

         compilers: {
             solc: {
                 version: "0.8.0",    // Fetch exact version from solc-bin (default: truffle's version)
             }
         }
     };
     ```

6. **Deploy the Contract:**
   - Make sure you are in the root of your Truffle project directory and run:
     ```sh
     truffle migrate --network rinkeby
     ```

### Sample Deployment Script

Here's a complete deployment script assuming you've followed the above steps:

```sh
#!/bin/bash

# Set up project directory
mkdir MyDapp
cd MyDapp

# Initialize Truffle project
truffle init

# Create smart contract
cat <<EOL > contracts/MyContract.sol
pragma solidity ^0.8.0;

contract MyContract {
    string public message;

    constructor(string memory initialMessage) {
        message = initialMessage;
    }

    function updateMessage(string memory newMessage) public {
        message = newMessage;
    }
}
EOL

# Create migration script
cat <<EOL > migrations/2_deploy_contracts.js
const MyContract = artifacts.require("MyContract");

module.exports = function (deployer) {
    deployer.deploy(MyContract, "Hello, Ethereum!");
};
EOL

# Install HDWalletProvider
npm install @truffle/hdwallet-provider

# Configure truffle-config.js
cat <<EOL > truffle-config.js
const HDWalletProvider = require('@truffle/hdwallet-provider');
const infuraKey = "YOUR_INFURA_PROJECT_ID";
const mnemonic = "YOUR_MNEMONIC";

module.exports = {
    networks: {
        rinkeby: {
            provider: () => new HDWalletProvider(mnemonic, \`https://rinkeby.infura.io/v3/\${infuraKey}\`),
            network_id: 4,
            gas: 4500000,
            gasPrice: 10000000000
        },
    },

    compilers: {
        solc: {
            version: "0.8.0",
        }
    }
};
EOL

# Deploy contract
truffle migrate --network rinkeby
```

Replace `YOUR_INFURA_PROJECT_ID` and `YOUR_MNEMONIC` with your actual Infura project ID and wallet mnemonic before running the script. This script sets up the project, creates the contract and migration files, configures the Truffle project for the Rinkeby test network, and deploys the contract.