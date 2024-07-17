# TechToken Smart Contract
TechToken is an ERC20 token smart contract implemented on the Ethereum blockchain. It provides functionality for issuing tokens, burning tokens, claiming devices with tokens, and more.

## Description
This smart contract allows users to manage tokens and claim devices based on their token balance. It includes mechanisms for token issuance, destruction, device claiming, and transferring tokens between addresses.

## Getting Started
To interact with this smart contract, you can use Remix, an online Solidity IDE. Follow these steps to get started:

Open Remix: Go to Remix IDE.

Create a New File: Click on the "+" icon in the left-hand sidebar and save the file with a .sol extension (e.g., TechToken.sol).

Copy and Paste Code: Copy the following Solidity code into the file:

solidity
Copy code
// SPDX-License-Identifier: MIT
pragma solidity ^0.8.18;

import "@openzeppelin/contracts/token/ERC20/ERC20.sol";
import "@openzeppelin/contracts/access/Ownable.sol";

contract TechToken is ERC20, Ownable {
    string[] private deviceCatalog = [
        "Laptop", 
        "Smartphone", 
        "Headphones", 
        "Smartwatch", 
        "VR Gear", 
        "Bluetooth Speaker", 
        "Tablet", 
        "Drone", 
        "Game Console", 
        "Camera"
    ];
    address private shopAddress;
    mapping(address => string) private claimedDevices;

    event TokensIssued(address indexed recipient, uint256 quantity);
    event TokensDestroyed(address indexed holder, uint256 quantity);
    event DeviceClaimed(address indexed user, string device);

    constructor() ERC20("TechToken", "TCH") {
        shopAddress = msg.sender; 
    }

    function issueTokens(address recipient, uint256 quantity) external onlyOwner {
        _mint(recipient, quantity);
        emit TokensIssued(recipient, quantity);
    }

    function destroyTokens(uint256 quantity) external {
        _burn(msg.sender, quantity);
        emit TokensDestroyed(msg.sender, quantity);
    }

    function claimDevice() external {
        uint256 userBalance = balanceOf(msg.sender);
        require(userBalance > 0, "Not enough tokens to claim a device");

        _burn(msg.sender, userBalance);

        string memory device = deviceCatalog[_random() % deviceCatalog.length];

        claimedDevices[msg.sender] = device;

        emit DeviceClaimed(msg.sender, device);
    }

    function getClaimedDevice(address user) external view returns (string memory) {
        return claimedDevices[user];
    }

    function transfer(address recipient, uint256 quantity) public virtual override returns (bool) {
        require(recipient != address(0), "ERC20: transfer to the zero address");
        _transfer(_msgSender(), recipient, quantity);
        return true;
    }

    function _random() private view returns (uint256) {
        return uint256(keccak256(abi.encodePacked(block.timestamp, blockhash(block.number - 1))));
    }

    function updateShopAddress(address newShopAddress) external onlyOwner {
        shopAddress = newShopAddress;
    }
}
Compile: Click on the "Solidity Compiler" tab in the left-hand sidebar, ensure the compiler version is set to ^0.8.18, and click on the "Compile TechToken.sol" button.

Deploy: Switch to the "Deploy & Run Transactions" tab, select TechToken from the dropdown menu, and click on "Deploy" to deploy the contract.

## Interacting with the Contract
Once deployed, you can interact with the contract using the following functions:

issueTokens(address recipient, uint256 quantity): Allows the owner to issue tokens to a specified recipient.

destroyTokens(uint256 quantity): Allows any holder to destroy their own tokens.

claimDevice(): Allows a user to claim a device based on their token balance. Tokens are burned upon claiming.

transfer(address recipient, uint256 quantity): Allows users to transfer tokens to another address.

getClaimedDevice(address user): Retrieves the device claimed by a specific user.

# Authors
West Pagbilao

GitHub: @WestPagbilao4

# License
This project is licensed under the MIT License. See the LICENSE file for details.
