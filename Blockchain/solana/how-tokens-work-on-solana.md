---
tags: blockchain, solana
author: Tran Khac Vy
---

## How tokens work on solana - explain for EVM developers
> **Let's say you want to create a new type of fungible token, mint some to yourself, and then transfer some to your friend. What would you do?**

- As a EVM developer, you have to deploy a new ERC20 smart contract.
- On solana, you don't need to create new contract. These is a single token program (which deployed by solana team) which responsible for creating, minting and transfering tokens. 
- In order to create new token, you send the `create` instruction to the token program. This creates a new `mint account`. Each type of tokens is associated with exactly one `mint account` which holds metadata about the token (likes `total supply`, `decimals`, `mint authority` - who allowed to mint, `freeze authority` - who allowed to freeze account )


![](https://i.imgur.com/vuOcgc7h.png)


- You've just created a new token but you don't own any amount of this token yet. From `mint account`, you have to create a `token account`. A `token account` stores how many tokens a particular user has, for a particular type of token. 

![](https://i.imgur.com/JrCKbIfh.jpg)


- Now, you have a `mint account` and a `token account`. Let's mint some tokens. To mint, you just send the `mint` instruction to the token program, which tells the program how many tokens to mint and whom to mint them to. Only one user is allowed to mint token of particular type (the `mint authority` which mentioned above)

- To transfer tokens, no surprises, you send the `transfer` instruction to the token program, which tells it how many tokens to transfer and whom to transfer them to. Note that the recipient must also own a `token account` for the type of token you're transferring.

![](https://i.imgur.com/C2fZ6Whh.jpg)

- What about NFTs? To create a NFT, you also use the same token program (what!!!), but these are some differences in how they are created and minted.
- As you know, a NFT is just a token which has one `total supply` and zero `decimal`. To create a NFT, you just need to create a `mint account` which has zero `decimal`. After that, you mint only one token of this NFT and disable future minting. This ensures there will only ever be one.
- In practice, most people use Candy Machine to create NFTs, which abstracts all this complexity away.
- But how can I config the name, symbol for my token? To do that, you need to create a pull request to [Solana Token Registry](https://github.com/solana-labs/token-list). Include a json file contains your token metadata (chain id, address, symbol, logo, name ...). Click [here](https://github.com/solana-labs/token-list) for more information.

## Refs
- https://spl.solana.com/token
- https://spl.solana.com/associated-token-account
-  https://twitter.com/pencilflip/status/1454141877972779013