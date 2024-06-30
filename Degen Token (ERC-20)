
// 1. Minting new tokens: The platform should be able to create new tokens and distribute them to players as rewards. Only the owner can mint tokens.
// 2. Transferring tokens: Players should be able to transfer their tokens to others.
// 3. Redeeming tokens: Players should be able to redeem their tokens for items in the in-game store.
// 4. Checking token balance: Players should be able to check their token balance at any time.
// 5. Burning tokens: Anyone should be able to burn tokens, that they own, that are no longer needed.

// SPDX-License-Identifier: MIT
pragma solidity ^0.8.0;

import "@openzeppelin/contracts/token/ERC20/ERC20.sol";

contract Degencoin is ERC20 {
    struct Item {
        uint256 price;
        bool exists;
    }

    mapping(uint256 => Item) public items;
    mapping(address => mapping (uint256 => bool )) public ownedcoin;

    address public owner;

    event ItemRedeemed(address indexed player, uint256 indexed itemId, uint256 quantity);
    
    constructor() ERC20("DEGEN", "DGN") {
        owner = msg.sender;
        addItem(1, 320); // item 1: bitcoin - Price = 320 DegenTokens
        addItem(2, 480); // item 2: ether - Price = 480 DegenTokens
        addItem(3, 10500); // item 3: dogecoin - Price = 1050 DegenTokens
        addItem(4, 890); // item 4: PeiPei - Price = 890 DegenTokens
    }

    modifier onlyOwner() {
        require(msg.sender == owner, "Ownable: the caller is not the owner");
        _;
    }

    function addItem(uint256 itemId, uint256 price) internal {
        require(!items[itemId].exists, "coin already exists");
        items[itemId] = Item(price, true);
    }

    function mint(address account, uint256 amount) external onlyOwner {
        _mint(account, amount);
    }

    function burn(uint256 amount) external {
        require(balanceOf(msg.sender) >= amount, "Insufficient balance");
        _burn(msg.sender, amount);
    }

    function redeemItem(uint256 itemId, uint256 quantity) external {
        require(items[itemId].exists, "Invalid coin ID");
        require(balanceOf(msg.sender) >= items[itemId].price * quantity, "Insufficient token balance");

        _burn(msg.sender, items[itemId].price * quantity);
        ownedcoin  [msg.sender] [itemId] = true;
        emit ItemRedeemed(msg.sender, itemId, quantity);
    }

    function redeemTokens(uint256 amount) external {
        require(balanceOf(msg.sender) >= amount, "Insufficient token balance");

        _burn(msg.sender, amount);
        emit ItemRedeemed(msg.sender, 0, amount); // ItemId 0 represents redeeming tokens
    }
    function showStore() external pure returns (string memory) {
        return "1. bitcoin(320 tokens) 2. ether (480 tokens) 3. dogecoin(1050 tokens) 4. Peipei(890 tokens)";
    }
    
}
