![](cover.png)

**A set of challenges to learn offensive security of smart contracts in Ethereum.**

Featuring flash loans, price oracles, governance, NFTs, lending pools, smart contract wallets, timelocks, and more!

## Play

Visit [damnvulnerabledefi.xyz](https://damnvulnerabledefi.xyz)

## Solutions

1. Unstoppable
Challenge Objective: DOS Attack on Contract

The primary objective of the initial challenge is to execute a Denial of Service (DOS) attack on the contract.

Vulnerability in flashLoan Function:

The flashLoan function contains a vulnerability identified as follows:
```solidity
uint256 balanceBefore = totalAssets();
if (convertToShares(totalSupply) != balanceBefore) revert InvalidBalance();
```
Explanation:

ERC4626 introduces a standard for tokenized vaults that track user deposit shares to determine rewards for staked tokens. In this context, the asset represents the underlying token deposited/withdrawn into the vault, while the share signifies the vault tokens minted/burned to represent user deposits.

For this challenge, 'DVT' is the underlying token, and the vault token is deployed as 'oDVT'.

Issues Identified:

The condition 
```solidity
(convertToShares(totalSupply) != balanceBefore)
```
enforces that the totalSupply of vault tokens must always equal the totalAsset of underlying tokens before any flash loan execution. This condition becomes problematic if there are alternative vault implementations diverting asset tokens to other contracts, rendering the flashLoan function inactive.

The totalAssets function is overridden to always return the balance of the vault contract 
```solidity 
(asset.balanceOf(address(this)))
```
. This introduces a separate accounting system based on tracking the supply of vault tokens.

Attack Strategy:

The attack involves creating a conflict between the two accounting systems by manually transferring 'DVT' to the vault.
```solidity
it('Execution', async function () {
        /** CODE YOUR SOLUTION HERE */
        // Sending just 2 Wei, although even 1 Wei should be enough
        await token.connect(player).transfer(vault.address, 2);
    });
```

## Help

For Q&A and troubleshooting running Damn Vulnerable DeFi, go [here](https://github.com/tinchoabbate/damn-vulnerable-defi/discussions/categories/support-q-a-troubleshooting).

## Disclaimer

All Solidity code, practices and patterns in this repository are DAMN VULNERABLE and for educational purposes only.

DO NOT USE IN PRODUCTION.
