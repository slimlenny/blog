+++
title = "Obelisk: Skycoin Consensus Algorithm | Information Pages"
tags = [
    "Overview",
    "Consensus",
    "Obelisk",
]
bounty = 10
date = "2017-09-09"
categories = [
    "Overview",
]
author = "johnstuartmill"
+++

![Obelisk The Skycoin Consensus Algorithm](/img/obelisk-the-skycoin-consensus-algorithm.png)

<!-- MarkdownTOC autolink="true" bracket="round" -->

- [Consensus highlights](#consensus-highlights)
    - [Why consensus](#why-consensus)
    - [High scalability and low-energy consumption](#high-scalability-and-low-energy-consumption)
    - [Robust to coordinated attacks](#robust-to-coordinated-attacks)
    - [The “51-percent Attack”](#the-%E2%80%9C51-percent-attack%E2%80%9D)
    - [Hidden IP addresses](#hidden-ip-addresses)
    - [Independence of clock synchronization](#independence-of-clock-synchronization)
    - [Two type of nodes: Consensus and Block-making](#two-type-of-nodes-consensus-and-block-making)
- [How Skycoin Consensus Algorithm works](#how-skycoin-consensus-algorithm-works)
- [References](#references)

<!-- /MarkdownTOC -->


## Consensus highlights

### Why consensus

Skycoin Consensus Algorithm ("Obelisk") synchronizes the state of Skycoin
blockchain across all the network nodes. This results in consistent accounting,
i.e. when calculating coin balance for a given public key (or address)
yields same value at each node that performed the calculation.

### High scalability and low-energy consumption

By design, the algorithm is a scalable and computationally-inexpensive
alternative to proof-of-work, therefore both the consensus algorithm and
block-making can be run on a budget hardware that have low price and low
energy consumption, thus making the cryptocurrency network more robust
to possible centralization attempts (i.e. via node being affordable to
general public).

### Robust to coordinated attacks

Our Consensus Algorithm (i) converges fast, (ii) requires minimal
network traffic, and (iii) can withstand a large-scale coordinated
attack by a well-organized network of malicious nodes. The algorithm is
non-iterative, fast, can be run on a sparse network with only
nearest-neighbor connectivity (e.g. on Mesh Network), and works well in
the presence of cycles in the connectivity graph (i.e. DAG-type
connectivity is *not* required).

### The “51-percent Attack”

In a limited sense, the base version of the Algorithm can be subject to
this attack. Specifically, when the modified or malicious nodes who are
in majority broadcast a protocol-compliant and UTXO-compliant candidate
block, such block wins the consensus. However, a block with any sort of
non-compliance is dropped by the (unmodified) Algorithm before the block
gets a chance to participate in consensus trial.

Consensus nodes can optionally utilize a Web-of-Trust concept in such a
way that consensus-related messages that come from unknown nodes (i.e.
signed by untrusted public keys) are ignored.

When Web-of-Trust mode is enabled, starting a very large number of
malicious consensus nodes in order to (a) cause blockchain fork or (b)
disrupt the consensus process would have little effect, unless a vast
majority of Web-of-Trust members unwittingly include those malicious
nodes into their local lists of trusted nodes.

### Hidden IP addresses

The nodes are addressed by their cryptographic public key. Node’s IP
address is only known to the nodes to which it is connected directly.

### Independence of clock synchronization

The Algorithm does not use “wall clock” (i.e. calendar date/time).
Instead, block sequence numbers that are extracted from validated
consensus- and blockchain- related messages are used to calculate node’s
internal time. This can be informally called “block clock”.

### Two type of nodes: Consensus and Block-making

A consensus node receives its input from one or more block-making nodes.
The algorithms for consensus and block-making are separate, yet they
both operate on same data-structures. We mention block-making where it
helps understanding the Consensus Algorithm and how it integrates with
the rest of the system.

Both type of nodes always performs authorship verification and fraud
detection of incoming date. Fraudulent or invalid messages are detected,
dropped and never propagated; peer nodes engaged in suspicious
activities are disconnected from, and their public keys are banned.

## How Skycoin Consensus Algorithm works

For exposition purposes only, the following description assumes that (i)
each node is both block-maker and consensus participant, and (ii)
consensus-related messages generated by non-trusted nodes are accepted,
i.e. no filtering based on web-of-trust is performed. A full
implementation (i.e. without these simplifying assumptions) will be
available in Skycoin GitHub repository. For simulation results and a
detailed, diagrammatic example of one consensus trial, see [\[1\]](#references). A
simulation of a network with trust relationship, although using a
different than Skycoin algorithm, can be found in [\[2\]](#references). The
description of Skycoin Consensus Algorithm follows.

1.  *Block-making*. Each block-making node collects new
    transactions, verifies them against the UTXO of the desired sequence
    number, packages the compliant transactions into a new block, and
    broadcasts the block to the network.

2.  *Collecting blocks*. Each consensus node collects the
    blocks generated by block-makers, and puts them into a container
    (separate from blockchain) keyed by block’s sequence number.

3.  *Selecting winning block*. Each consensus node, upon
    receiving a sufficiently large number[^1] of candidate blocks or
    upon meeting other criteria, finds the block that was made by the
    largest number of block-makers. Ties are resolved deterministically.
    Such block is labeled “local winner”[^2] and is appended to the
    local blockchain. The key-value pair corresponding to the block
    sequence number of the local winner is deleted, thus
    reclaiming storage. Hash code of local winner
    is broadcast/announced.

4.  *Verification step*. Each node keep statistics on local
    winners reported by other nodes. When local winners have been
    reported by all or most of the nodes[^3], the node determines the
    global winner for the particular sequence number. If the global
    winner is the local winner, then the node continues to function
    as above. Otherwise the nodes decides, based on external data and
    local logs, between (a) re-synchronizing itself with the network
    or (b) dropping from participating in consensus and/or block-making
    or (c) keeping its blockchain and requesting an emergency stop.

[^1]: This is a configurable parameter of in the Algorithm.
[^2]: Under certain ideal conditions, local winners (for a given block
    sequence number) are all the same, i.e. include identical set of
    transactions. The difference arises due to network latency, high
    frequency of transactions, out-of-sequence message delivery, message
    loss, malfunctioning or malicious nodes etc.
[^3]: This number can be determined, for example, by asking trusted
    nodes to report public keys of their trusted nodes, recursively.

## References

\[1\] johnstuartmill et al. A Distributed Consensus Algorithm for
Cryptocurrency Networks.
<https://github.com/skycoin/whitepapers/blob/master/whitepaper_skycoin_consensus_v01_jsm.pdf>
2016

\[2\] Houwu Chen and Jiwu Shu. Sky: an Opinion Dynamics Framework and Model
for Consensus over P2P Network.
<https://github.com/skycoin/whitepapers/blob/master/Sky-%20Opinion%20Dynamics%20Based%20Consensus%20for%20P2P%20Network%20with%20Trust%20Relationships.pdf>
201?

*Read more:*

* *[Skycoin Consensus Algorithm Whitepapers](https://www.skycoin.net/whitepapers)*
* *[Obelisk The Skycoin Consensus Algorithm](/statement/obelisk-skycoin-consensus-algorithm/)*
