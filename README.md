# Restrict-US-Wallet-Validation-on-EVM-Protocol-Comibined-.
Restrict US Wallet(s) and Validator(s) from interacting with a EVM smart contract.

The following restricts Nodes in the US (other also) : https://github.com/catenacap/restrict_evm_US_other_validation
The following restricts Wallets in the US (other also) : https://github.com/catenacap/restrict_EVM_US_other_Wallets_Via_Smartcontracts/blob/main/README.md

And below you will find a alternative that restricts both US based wallets from interacting with the smart contract, or to restrict US nodes from validating any transaction for your smart contract.

-----
pragma solidity ^0.8.0;

contract USRestriction {
    mapping (address => string) private _walletCountry;
    mapping (address => string) private _nodeCountry;

    modifier onlyNonUS() {
        string memory walletCountry = _walletCountry[msg.sender];
        string memory nodeCountry = _nodeCountry[tx.origin];
        require(keccak256(bytes(walletCountry)) != keccak256(bytes("United States")) &&
                keccak256(bytes(nodeCountry)) != keccak256(bytes("United States")), "US-based access detected.");
        _;
    }

    function updateWalletCountryMapping(address wallet, string memory country) public {
        _walletCountry[wallet] = country;
    }

    function updateNodeCountryMapping(address node, string memory country) public {
        _nodeCountry[node] = country;
    }

    function myRestrictedFunction() public onlyNonUS {
        // This function can only be called from wallets owned by individuals located outside the United States
        // and validator nodes/miners located outside the United States
        // ...
    }
}

-----

The USRestriction contract combines the previous examples to restrict access to both US-based wallets and US-based validator nodes/miners.

The _walletCountry and _nodeCountry mappings associate wallet addresses and node addresses with their corresponding countries, respectively. You can update these mappings using the updateWalletCountryMapping and updateNodeCountryMapping functions, which take an address and a country as parameters.

The onlyNonUS modifier checks the country of origin of the wallet owner and the validator node/miner based on the _walletCountry and _nodeCountry mappings, respectively. If either the wallet or the node is detected as being located in the United States, the modifier reverts the transaction and prevents the function from being executed.

The myRestrictedFunction function is an example of a function that is restricted to non-US wallets and non-US validator nodes/miners. It can only be called from wallets owned by individuals located outside the United States and validator nodes/miners located outside the United States.

As with the previous examples, keep in mind that this approach relies on the accuracy and reliability of the external APIs used to retrieve the country information.
