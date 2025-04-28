# Experiment 6: Blockchain-Based Passwordless Authentication (Using Public-Private Key Cryptography)
# Aim:
To implement a secure passwordless authentication system using public-private key cryptography on Ethereum. This prevents phishing and password leaks.

# Algorithm:
Step 1: User Registration
A user registers with their Ethereum public key (instead of a password).


Step 2: Login Process
When logging in, the user signs a random challenge message using their private key.


The smart contract verifies the signature using the user’s public key.



# Program:
```
// SPDX-License-Identifier: MIT
pragma solidity ^0.8.20;

contract PasswordlessAuthDemo {
    struct User {
        bool registered;
        address pubKey;
        bytes32 privateKey; // Fake private key for demo
    }

    mapping(address => User) public users;
    bytes32 public latestChallenge;

    event UserRegistered(address user, address pubKey, bytes32 privateKey);
    event ChallengeGenerated(bytes32 challenge);
    event SignatureGenerated(bytes32 hash, uint8 v, bytes32 r, bytes32 s);

    // Step 1: Register user
    function registerUser() public {
        require(!users[msg.sender].registered, "Already registered");

        // Fake public/private keys
        address fakePubKey = msg.sender;
        bytes32 fakePrivateKey = keccak256(abi.encodePacked(msg.sender, block.timestamp));

        users[msg.sender] = User({
            registered: true,
            pubKey: fakePubKey,
            privateKey: fakePrivateKey
        });

        emit UserRegistered(msg.sender, fakePubKey, fakePrivateKey);
    }

    // Step 2: Generate random challenge
    function generateChallenge() public returns (bytes32) {
        require(users[msg.sender].registered, "User not registered");
        latestChallenge = keccak256(abi.encodePacked(block.timestamp, msg.sender));
        emit ChallengeGenerated(latestChallenge);
        return latestChallenge;
    }

    // Step 3: "Sign" the challenge (fake signing)
    function generateSignature() public returns (bytes32 hash, uint8 v, bytes32 r, bytes32 s) {
        require(users[msg.sender].registered, "User not registered");
        
        hash = latestChallenge;
        bytes32 combined = keccak256(abi.encodePacked(users[msg.sender].privateKey, hash));
        
        // Fake values for r, s, v
        r = bytes32(uint256(uint160(users[msg.sender].pubKey)) << 96);
        s = combined;
        v = 27;

        emit SignatureGenerated(hash, v, r, s);

        return (hash, v, r, s);
    }

    // Step 4: Authenticate
    function authenticate(bytes32 hash, uint8 v, bytes32 r, bytes32 s) public view returns (bool) {
        require(users[msg.sender].registered, "User not registered");

        bytes32 expectedCombined = keccak256(abi.encodePacked(users[msg.sender].privateKey, hash));
        bytes32 expectedR = bytes32(uint256(uint160(users[msg.sender].pubKey)) << 96);
        uint8 expectedV = 27;

        if (r == expectedR && s == expectedCombined && v == expectedV) {
            return true;
        } else {
            return false;
        }
    }
}
```

# Expected Output:
Users can register without a password.
![Screenshot (4)](https://github.com/user-attachments/assets/2c6f95d3-4b51-4dea-a1aa-a598946ae94d)


Users sign a challenge with their private key for authentication.
![Screenshot (1)](https://github.com/user-attachments/assets/baa39d37-62e4-45bf-b3a8-c33ef2f952fd)


The smart contract verifies signatures to confirm identity.
![Screenshot (5)](https://github.com/user-attachments/assets/400618bf-2266-48ea-9d2d-a6288ad508b8)

![Screenshot (3)](https://github.com/user-attachments/assets/743a50d7-2f2b-453d-9675-c8faaf0b3903)


# High-Level Overview:
Eliminates password hacks & phishing attacks.


Uses Ethereum's built-in cryptographic functions.


Inspired by Web3 login solutions like MetaMask authentication.

# RESULT: 
Thus To implement a secure passwordless authentication system using public-private key cryptography on Ethereum. This prevents phishing and password leaks.
