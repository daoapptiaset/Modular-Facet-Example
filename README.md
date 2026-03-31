# Modular-Facet-Example
// SPDX-License-Identifier: MIT
pragma solidity ^0.8.20;

contract TokenFacet {
    struct DiamondStorage {
        mapping(address => uint256) balances;
        uint256 totalSupply;
    }

    function getStorage() internal pure returns (DiamondStorage storage ds) {
        bytes32 position = keccak256("diamond.storage.token");
        assembly {
            ds.slot := position
        }
    }

    function mint(address to, uint256 amount) external {
        DiamondStorage storage ds = getStorage();
        ds.balances[to] += amount;
        ds.totalSupply += amount;
    }

    function balanceOf(address account) external view returns (uint256) {
        return getStorage().balances[account];
    }
}
