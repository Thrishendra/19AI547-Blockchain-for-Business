# Ex_No_3_Supply Chain Transparency for Luxury Goods


## Name:T Thrishendra
## Reg no:212223230227
## Date:27/4/25

# Aim:
To develop a smart contract that tracks the supply chain of luxury goods, ensuring authenticity.

# Algorithm:
step 1.The manufacturer registers a product by providing a unique ID and name, which is stored on-chain and marked as verified.

step 2.The contract ensures that duplicate registrations are not allowed.

step 3.As the product moves through the supply chain, the current owner transfers ownership using the smart contract.

step 4.Each ownership transfer is logged with an event to maintain traceability.

step 5.Buyers can verify the product’s authenticity and current owner at any time by querying the blockchain.

step 6.This ensures transparency and prevents counterfeit goods in the supply chain.


# Program:
```
// SPDX-License-Identifier: MIT
pragma solidity ^0.8.20;

contract LuxurySupplyChain {
    struct Product {
        string name;
        address currentOwner;
        bool verified;
    }

    mapping(uint256 => Product) public products;

    event ProductRegistered(uint256 productId, string name);
    event OwnershipTransferred(uint256 productId, address newOwner);

    function registerProduct(uint256 productId, string memory name) public {
        require(products[productId].currentOwner == address(0), "Product already registered");
        products[productId] = Product(name, msg.sender, true);
        emit ProductRegistered(productId, name);
    }

    function transferOwnership(uint256 productId, address newOwner) public {
        require(products[productId].currentOwner == msg.sender, "Not the owner");
        products[productId].currentOwner = newOwner;
        emit OwnershipTransferred(productId, newOwner);
    }

    function verifyProduct(uint256 productId) public view returns (string memory, address, bool) {
        Product memory p = products[productId];
        return (p.name, p.currentOwner, p.verified);
    }
}
```
# Expected Output:
A luxury good (e.g., a Rolex watch) is registered on-chain.
![image](https://github.com/user-attachments/assets/679b717e-617b-4b3d-bfe3-49d78ab10ef1)


Ownership is transferred at every checkpoint.

![image](https://github.com/user-attachments/assets/735ae731-d59c-44a2-a575-8b97dc5911e2)

Buyers can check the authenticity before purchasing.

![image](https://github.com/user-attachments/assets/98b4bb3e-4bb3-4e64-8f68-a0275e91e4e9)

# High-Level Overview:
Helps prevent counterfeit luxury goods.

Teaches real-world supply chain use cases.

# RESULT : 

Thus, smart contract that tracks the supply chain of luxury goods, ensuring authenticity has been created and successfully executed.
