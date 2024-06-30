# DegenToken
Degen Token (ERC-20): Unlocking the Future of Gaming

    
## Getting Started
```
//Your task is to create a ERC20 token and deploy it on the Avalanche network for Degen Gaming. The smart contract should have the following functionality:
// 1. Minting new tokens: The platform should be able to create new tokens and distribute them to players as rewards. Only the owner can mint tokens.
// 2. Transferring tokens: Players should be able to transfer their tokens to others.
// 3. Redeeming tokens: Players should be able to redeem their tokens for items in the in-game store.
// 4. Checking token balance: Players should be able to check their token balance at any time.
// 5. Burning tokens: Anyone should be able to burn tokens, that they own, that are no longer needed.

// SPDX-License-Identifier: MIT
pragma solidity ^0.8.0;

import "@openzeppelin/contracts/token/ERC20/ERC20.sol";

contract DegenGuns is ERC20 {
    struct Item {
        uint256 price;
        bool exists;
    }

    mapping(uint256 => Item) public items;
    mapping(address => mapping (uint256 => bool )) public ownedGuns;

    address public owner;

    event ItemRedeemed(address indexed player, uint256 indexed itemId, uint256 quantity);
    event OwnershipTransferred(address indexed previousOwner, address indexed newOwner);

    constructor() ERC20("DEGEN", "DGN") {
        owner = msg.sender;
        addItem(1, 320); // item 1: Pistol - Price = 320 DegenTokens
        addItem(2, 480); // item 2: Shotgun - Price = 480 DegenTokens
        addItem(3, 10500); // item 3: Sniper Rifle - Price = 1050 DegenTokens
        addItem(4, 890); // item 4: Assault Rifle - Price = 890 DegenTokens
        addItem(5, 1120); // item 5: Submachine Gun - Price = 1120 DegenTokens
        addItem(6, 1500); // item 6: Rocket Launcher - Price = 1550 DegenTokens
    }

    modifier onlyOwner() {
        require(msg.sender == owner, "Ownable: the caller is not the owner");
        _;
    }

    function addItem(uint256 itemId, uint256 price) internal {
        require(!items[itemId].exists, "Gun already exists");
        items[itemId] = Item(price, true);
    }

    function mint(address account, uint256 amount) external onlyOwner {
        _mint(account, amount);
    }

    function burn(uint256 amount) external {
        _burn(msg.sender, amount);
    }

    function redeemItem(uint256 itemId, uint256 quantity) external {
        require(items[itemId].exists, "Invalid item ID");
        require(balanceOf(msg.sender) >= items[itemId].price * quantity, "Insufficient token balance");

        _burn(msg.sender, items[itemId].price * quantity);
        ownedGuns [msg.sender] [itemId] = true;
        emit ItemRedeemed(msg.sender, itemId, quantity);
    }

    function redeemTokens(uint256 amount) external {
        require(balanceOf(msg.sender) >= amount, "Insufficient token balance");

        _burn(msg.sender, amount);
        emit ItemRedeemed(msg.sender, 0, amount); // ItemId 0 represents redeeming tokens
    }
}
```
## Executing Program 
    To run this program, you can use Remix, an online Solidity IDE. To get started, go to the 
    Remix website at https://remix.ethereum.org/. Once you are on the Remix website, create a 
    new file by clicking on the "+" icon in the left-hand sidebar. Save the file with a .sol extension 
    (e.g., HelloWorld.sol). Then copy the code given in the assessment.

## Author 
    NILESH SUKHWAL

## License
     This project is licensed under the MIT License - see the LICENSE file for details
