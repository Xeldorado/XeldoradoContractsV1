# [Xeldorado Protocol](nft.xeldorado.live)
Open source implementation of Xeldorado - A general purpose Creator Social Token (CST) Protocol that implements Creator Economy by connecting Creator Tokens with:
- <b>Liquidity Pair of CreatorToken-BaseToken</b> : Gateway between Creator Economy and External World. Creator can choose from a list of options to select best suited BaseToken. For e.g. USDC, DAI, WETH, WBTC, BUSD, etc. 
- <b>NFTs that are pegged to CreatorToken</b> : Price of 1 NFT will be constant w.r.t. CreatorToken and every time a new NFT is added to the vault same number of CreatorTokens will be minted to enuse the peg. 
- <b>Bank</b> : for NFT backed lending borrowing of Creator Tokens
- <b>DAO</b> : for Creator community governance

## Smart Contracts Flow Diagram

![Samrt Contract Flow Diagram](./Images/XeldoradoCoreSmartContractDesign.png)

## Features 

- [x] Creator Vault
  - [x] Creator Add/Mint NFTs to vault contract
  - [x] Launch Creator Token, make it available for community to buy. Liquidity is bought not rented. Since CreatorTokens offer NFT, DAO, Banking related utilities they do have value since day one of launch especially to access Vault NFTs.
  - [x] Price of NFTs is pegged to certain number of tokens at the time of launch and stays same forever so scope of rise in NFT prise is only from rise in token price of CreatorToken
  - [x] Same number of tokens are minted every time an NFT is added to vault, these tokens are transferred to DAO contract and can be used for Airdrop, FLO, Grants for community [batch supported]
  - [x] Holder of CreatorTokens can redeem NFT and tokens worth NFT price will be burnt from his wallet [batch supported]
  - [x] Holder of CreatorTokens can return his redeemed NFT and tokens worth NFT price will be minted to his wallet [batch supported]
  - [x] 2 NFTs of a creator can be swapped for no fees [batch supported]
  - [x] Creator/Community can add Further Liquidity to Pair Contract by sale of CreatorTokens from DAO contract. Vault Contract hosts this sale.
  - [x] Vault can own millions of NFTs. There are no arrays storing Vault's NFT data about redeemed NFTs. Instead a simple mapping of vaultId to NFT Contract and vaultId to TokenId is used and to know whether an NFT is redeemed a simple ownerOf() function call for a given vaultId will help determine. This is done to avoid For Loops over large arrays which will run out of gas after few thousand NFTs. Hence avoiding scaling bottleneck.
- [x] Creator Pair
  - [x] Constant Product based AMM for CreatorToken - BaseToken pair
  - [x] Swap between tokens
- [x] Creator Vesting Vault
  - [x] Tokens allocated to creator at the start are vested over a period of time, 2 years by default with 3 months of cliff period during which the vested token count will be increasing just that creator cannot redeem them due to cliff.
- [x] Creator DAO  
  - [x] Token holders can make proposals 
  - [x] Creator can choose managers and those managers will be given allowances. Using these allowances they can pay folks/employees, they hire, for specific task either on Pay Per Task basis or monthly salaries. Managers can transfer from their allowances to the employee's allowance value. 
  - [x] 4 types of proposals 
    - [x] Airdrops, before CreatorToken launch as it is, after launch with voting and approval 
    - [x] FLO proposals to increase size of market to ensure greater liquidity in Pair contract
    - [x] Allowances Proposals to decide amount allocated per manager. Single proposal can handle multiple managers' allowances.
    - [x] General Proposal will contain link to their DAO Forum's proposal page where detailed discussions can take place. Results of these proposals will be acted upon by community members in good faith.
  - [x] Token holders can vote for each of the proposal
  - [x] Result of voting is based on number of CreatorTokens held by the voter. For first 3 cases only 2 choices, no or yes. For General Proposal any number of choices are allowed. 
- [x] Migration to V2: 
  - [x] Pair, DAO, Vault and Bank contracts contains Token and NFT assets which is transfered via Migration Contract to V2 version of Pair, DAO, Vault and Bank contracts
  - [x] Before migration of a Creator Community's assets, community members must vote for or against mirgration and migration contract will be made public much in advance. Voting is implemented in CreatorToken contract. This is done to ensure security of assets and decentralisation of decision making. This is important since in Xeldorado Protocol liquidity isn't rented its owned.
  - [x] If voted no, then assets will stay in V1, current version, of Xeldorado Protocol
  - [x] There can be 2 types of migration, partial and complete. In partial migration only one or two parts maybe updated like only Vault contract updated and hence needs asset transfer only for Vault's V2 version. In full migration all 4 contracts will be updated and maybe entire Factory and CreatorFactory may also be updated. 
  - [x] Migration function in Pair, DAO, Vault and Bank are implemented with check on voting status passed and only after that migration contract, that has been voted, is allowed to transfer asset by deploying V2 version of the contract.
  - [x] Migration function also updates all dependent contracts with new address of V2 contract. For e.g. if Vault contract gets migrated to V2,then its dependent contract CreatorToken, CreatorNFT, CreatorDAO and CreatorFactory will get updated with V2 address of vault variable.
- [ ] Creator Bank( Not needed on day one, can be integrated after launch)
  - [ ] Bank contract needs price data from CreatorToken to decide upon interest rates. Hence, Each creator can have different interest rates
  - [ ] Community members, holders of CreatorToken can stake their CreatorTokens for some yield
  - [ ] NFT Loans: Holder of NFTs from Creator Vault can use it as collateral for borrowing CreatorTokens, since we have price data of CreatorTokens and also price of single NFT is fixed w.r.t. to CreatorToken we can estimate realistic valuation of NFTs
  - [ ] Flash Loans on Creator Tokens 
- [ ] External Contract Functions
  - [ ] 2 NFTs of different creator with same BaseTokens can be swapped with small fees 
  - [ ] If someone owns an NFT, you can buy it for premium by placing a bid (Auction smart contract will be placed separately) 
  - [ ] Base Token Pairs: Use a Curve kind of multi token pairs for BaseTokens for swap. This will help NFTs of creator that don't have same base token. Liquidity will be added by us using a small fraction of profit being earnt in BaseTokens. Swap in these pools will have no fees. 

## Documentation

Detailed Explanations for each contracts and functions: [Understand](https://nft.xeldorado.live/index.html#xeldoradoprotocol)

<!-- <a href="https://nft.xeldorado.live/index.html#xeldoradoprotocol" target="_blank"><img src="https://nft.xeldorado.live/images/logo.png" width="150" height="30"/></a> -->

White paper for Xeldorado Protocol is currently under development.

## Tests

For tests please refer to [`README`](./test/README.md) from test folder.

## Licensing

The primary license for Xeldorado Contracts V1 is the Business Source License 1.1 (`BUSL-1.1`), see [`LICENSE`](./LICENSE). 

<h3>
    
```diff
!!! Overall this repository is not available for production use !!!
```

</h3>

### Exceptions

- All files in `contracts/interfaces/` are licensed under `GPL-2.0-or-later` (as indicated in their SPDX headers), see [`contracts/interfaces/LICENSE`](./contracts/interfaces/LICENSE)
- Several files in `contracts/libraries/` are licensed under `GPL-2.0-or-later` (as indicated in their SPDX headers), see [`contracts/libraries/LICENSE`](contracts/libraries/LICENSE)
- All files in `contracts/test` remain unlicensed.

## Community

<a href="https://discord.gg/ExMb82zpnB" target="_blank"><img src="https://nft.xeldorado.live/images/discord.png" width="80" height="80"/></a>&emsp;&emsp;&emsp;
<a href="https://t.me/xeldorado" target="_blank"><img src="https://nft.xeldorado.live/images/telegram.png" width="80" height="80"/></a>&emsp;&emsp;&emsp;
<a href="https://twitter.com/XeldoradoLabs" target="_blank"><img src="https://nft.xeldorado.live/images/twitter.png" width="80" height="80"/></a>&emsp;&emsp;&emsp;
<a href="https://www.reddit.com/r/Xeldorado_/" target="_blank"><img src="https://nft.xeldorado.live/images/reddit.png" width="80" height="80"/></a>

## Responsible disclosure

At Xeldorado, we consider the security of our systems a top priority. But even putting top priority status and maximum effort, there is still possibility that vulnerabilities can exist. 

In case you discover a vulnerability, we would like to know about it immediately so we can take steps to address it as quickly as possible.  

If you discover a vulnerability, please do the following: 

    E-mail your findings to xeldorado.nft@gmail.com; 

    Do not take advantage of the vulnerability or problem you have discovered; 

    Do not reveal the problem to others until it has been resolved; 

    Do not use attacks on physical security, social engineering, distributed denial of service, spam or applications of third parties; and 

    Do provide sufficient information to reproduce the problem, so we will be able to resolve it as quickly as possible. Complex vulnerabilities may require further explanation so we might ask you for additional information. 

We will promise the following: 

    We will respond to your report within 3 business days with our evaluation of the report and an expected resolution date; 

    If you have followed the instructions above, we will not take any legal action against you in regard to the report; 

    We will handle your report with strict confidentiality, and not pass on your personal details to third parties without your permission; 

    If you so wish we will keep you informed of the progress towards resolving the problem; 

    In the public information concerning the problem reported, we will give your name as the discoverer of the problem (unless you desire otherwise); and 

    As a token of our gratitude for your assistance, we offer a reward for every report of a security problem that was not yet known to us. The amount of the reward will be determined based on the severity of the leak, the quality of the report and any additional assistance you provide.  

## Remarks

- Currently,
    - we won't be having any exchange token but the core contract has support for discounted fees based on the number of exchange tokens owned. This is done to ensure smooth future integration of exchange token next year after our protocol gains some traction.
    - we haven't implemented the logic for creator's bank contract. There must be some buffer between Creator Token launch and starting of Bank to mitigate risk and establish real valuations for NFTs as well as Creator Tokens which is much needed for using them as collateral for lending borrowing. However The intigration will require only deploying of Bank contract by creator and updating creatorBank value in XeldoraroCreatorFactory contract.


