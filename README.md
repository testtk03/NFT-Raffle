# NFT-Raffle
NFT Raffle
// SPDX-License-Identifier: MIT
pragma solidity ^0.8.20;

import "@openzeppelin/contracts/token/ERC721/IERC721.sol";

contract NFTRaffle {
    IERC721 public nft;
    uint256 public tokenId;
    address[] public players;

    constructor(address _nft, uint256 _tokenId) {
        nft = IERC721(_nft);
        tokenId = _tokenId;
    }

    function enter() public payable {
        require(msg.value == 0.01 ether, "Need 0.01 ETH");
        players.push(msg.sender);
    }

    function draw() public {
        uint256 winner = uint256(keccak256(abi.encodePacked(block.timestamp))) % players.length;
        nft.transferFrom(msg.sender, players[winner], tokenId);
    }
}
