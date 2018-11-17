---
layout: specification
title: 2019 May 15 Network Upgrade Specification
date: 2018-10-08
activation: 1557921600
version: 0.01
---

## Summary

When the median time past [1] of the most recent 11 blocks (MTP-11) is greater than or equal to UNIX timestamp 1557921600, Bitcoin Cash will execute an upgrade of the network consensus rules according to this specification. Starting from the next block these consensus rules changes will take effect:

* Enable opcodes as specified in 2019-may-opcodes.md
* Change the Merkle tree structure to Merklix-Metadata Tree.
* Enforce "Minimal Data Push" rule
* Enforce "Null Dummy" rule
* Improve signature operations (Sigops) counting method [TBD] 

The following are not consensus changes, but are recommended changes for Bitcoin Cash implementations:

* Automatic replay protection for future upgrade

## OpCodes

The opcodes OP_MUL, OP_RSHIFT, OP_LSHIFT, and OP_INVERT will be enabled as specified in [2019-may-opcodes.md](2019-may-opcodes.md) [2].

## Merklix Metadata Tree

The value of the Merkle Root Hash field in the block header will be re-defined as specified in [merklix-metadata.md](merklix-metadata.md).

## Minimal Data Push

For a transaction to be valid, the smallest possible push operation must be used when possible. Pushing data using an operation that could be encoded in a shorter way makes the script evaluate to false. This is the same as Bitcoin BIP 62 rule #3 [3, 4].

## Null Dummy

The dummy stack element consumed by OP_CHECKMULTISIG and OP_CHECKMULTISIGVERIFY must be the empty byte array. Anything else makes the script evaluate to false immediately. This is the same as Bitcoin BIP 62 rule #7 [3, 4, 5].

## Automatic Replay Protection

When the median time past [2] of the most recent 11 blocks (MTP-11) is less than UNIX timestamp 1571140800 (Nov 15, 2019 upgrade) Bitcoin Cash full nodes MUST enforce the following rule:

 * `forkid` [6] to be equal to 0.

When the median time past [1] of the most recent 11 blocks (MTP-11) is greater than or equal to UNIX timestamp 1571140800 (Nov 15, 2019 upgrade) Bitcoin Cash full nodes implementing the Mayr 2019 consensus rules SHOULD enforce the following change:

 * Update `forkid` [6] to be equal to 0xFF0001.  ForkIDs beginning with 0xFF will be reserved for future protocol upgrades.

This particular consensus rule MUST NOT be implemented by Bitcoin Cash wallet software. Wallets that follow the upgrade should not have to change anything.

## References

[1] Median Time Past is described in [bitcoin.it wiki](https://en.bitcoin.it/wiki/Block_timestamp). It is guaranteed by consensus trules to be monotonically increasing.

[2] https://github.com/bitcoincashorg/bitcoincash.org/blob/master/spec/2019-may-opcodes.md

[3] [BIP 62](https://github.com/bitcoin/bips/blob/master/bip-0062.mediawiki)

[4] [Script Flags](https://github.com/Bitcoin-ABC/bitcoin-abc/blob/058a6c027b5d4749b4fa23a0ac918e5fc04320e8/src/script/script_flags.h)

[5] [BIP 147](https://github.com/bitcoin/bips/blob/master/bip-0147.mediawiki)

[6] The `forkId` is defined as per the [replay protected sighash](replay-protected-sighash.md) specification.

