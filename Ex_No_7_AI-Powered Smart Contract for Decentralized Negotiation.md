# Experiment 7: AI-Powered Smart Contract for Decentralized Negotiation
# Name : T Thrishendra
# Reg no : 212223230227
# Aim:
# To create a smart contract that integrates AI logic for automated negotiation in decentralized commerce. The contract adjusts price and conditions dynamically based on real-time market trends using an on-chain AI model.

# Algorithm:
## Step 1:
AI-Powered Dynamic Pricing
Seller lists an item with a minimum price and negotiation range.

## Step 2: 
Buyer submits an offer price.

## Step 3:
AI logic (simulated using Solidity algorithms) evaluates the price based on:

## Step 4: 
Market demand (tracked using on-chain transactions).

## Step 5:  
Historical transaction data.
## Step 6:

Time-based price fluctuations.


## Step 7: Smart Contract Counteroffer
The contract automatically generates a counteroffer if the buyer’s price is within the negotiation range.


If the buyer accepts, the transaction is executed on-chain.


## Step 8: Settlement and Price Learning
Every completed transaction updates the price learning algorithm to refine future pricing decisions.



# Program:
```
// SPDX-License-Identifier: MIT
pragma solidity ^0.8.20;

contract AIPoweredNegotiation {
    struct Item {
        address seller;
        uint256 minPrice;
        uint256 maxPrice;
        uint256 basePrice;
        bool sold;
    }

    mapping(uint256 => Item) public items;
    uint256 public itemCount;

    event ItemListed(uint256 itemId, uint256 basePrice);
    event OfferMade(uint256 itemId, address buyer, uint256 offer);
    event CounterOffer(uint256 itemId, uint256 counterOffer);
    event Sold(uint256 itemId, address buyer, uint256 finalPrice);

    function listItem(uint256 _basePrice, uint256 _minPrice, uint256 _maxPrice) public {
        require(_minPrice <= _basePrice && _basePrice <= _maxPrice, "Invalid price range");
        
        items[itemCount] = Item(msg.sender, _minPrice, _maxPrice, _basePrice, false);
        emit ItemListed(itemCount, _basePrice);
        itemCount++;
    }

    function makeOffer(uint256 _itemId, uint256 _offerPrice) public payable {
        Item storage item = items[_itemId];
        require(!item.sold, "Item already sold");
        require(msg.value == _offerPrice, "Incorrect offer amount");

        emit OfferMade(_itemId, msg.sender, _offerPrice);

        uint256 aiCounterOffer = dynamicPricing(item.basePrice, item.minPrice, item.maxPrice, _offerPrice);

        if (_offerPrice >= aiCounterOffer) {
            item.sold = true;
            payable(item.seller).transfer(_offerPrice);
            emit Sold(_itemId, msg.sender, _offerPrice);
        } else {
            payable(msg.sender).transfer(_offerPrice); // Refund buyer
            emit CounterOffer(_itemId, aiCounterOffer);
        }
    }

    function dynamicPricing(uint256 base, uint256 min, uint256 max, uint256 offer) private pure returns (uint256) {
        if (offer >= max) return max;
        if (offer >= base) return base;
        uint256 counter =(base+offer)/2;
        return counter < min ? min : counter; // Enforce a Minimum bound
        
    }
}

```

# Expected Output:
Buyers submit offers, and the contract auto-negotiates the price.

![image](https://github.com/user-attachments/assets/620f6491-7f2f-4798-91f5-b45d3a54dffd)

If the buyer’s offer is fair, the deal is executed.
![image](https://github.com/user-attachments/assets/1e43896c-1a21-4893-afc9-93f46f8f7667)

If the offer is too low, the contract suggests a counteroffer.


![image](https://github.com/user-attachments/assets/d03d4689-7577-4682-9bee-70f52b8b2722)


# High-Level Overview:
First-of-its-kind AI-powered pricing contract.


Mimics real-world price negotiations using dynamic on-chain pricing.


Can be extended to AI oracles for real-time market data.


Inspired by AI-enhanced commerce and eBay-like decentralized auctions.

# RESULT:

Thus,Zero-Knowledge Proof (ZK) Private Voting System has been created and successfully executed.
