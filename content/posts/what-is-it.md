---
title: "What Is It?"
date: 2021-07-29T12:09:20+02:00
draft: false
---
Catamaran swap is a three leg swap, two transactions happen on the Stacks chain, one on the Bitcoin network.

1. Define Stacks asset to swap, define bitcoin amount and receiver, put assets into escrow
1. Do Bitcoin transaction
1. Verify Bitcoin transaction and release Stacks asset if successful.

# Define Stacks assets
The create swap transaction defines the swap amounts, what seller wants to sell (STX amount, nft details, ft amount). This assets are put into escrow in the contract. The bitcoin amount in sats and receiving btc address are defined as well

# Send Bitcoins
Then the buyer sends the bitcoin to the specified address.

# Verify BTC transaction
The btc transaction details is submitted to the stacks chain. If the details can be confirmed by the contract the stacks assets are released.
The stacks blockchain has a view on the block header hash of the bitcoin blockchain.
If you provide the stacks block height (or a bitcoin block height in the future), the stacks chain knows the block header hash. The contract can now verify that the provided parts of the bitcoin block header of the block that contains the transaction (merkle root, etc) are correct by hashing the provided parts and comparing the hashes.
Next the transaction, the contract can verify that the tx (provided with all the details like ins and outs) was included in the block by hashing the provided details and using that together with the provided merkle proof to calculate the merkle root (it is just about hashing, https://learnmeabitcoin.com/technical/merkle-root). If the calcuated hash matches the merkle root in the verified block header, the contract know that the tx was indeed included in the block.

# Limitations
There are currently some limitations when we have to fall back to trustful swap due to technical limitations:
* the bitcoin transaction must not happen during a flash block
* the bitcoin transaction must not be larger than 1024 bytes and must not have more than 8 ins and 8 outs.

The flash block problem will be resolved in 2.1

# Add your own swap type
The validation happens in the clarity-bitcoin-lib contract, therefore it is relatively easy to write new types of swaps or other triggers.
