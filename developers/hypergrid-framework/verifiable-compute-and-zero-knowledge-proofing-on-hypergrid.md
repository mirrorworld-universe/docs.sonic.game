---
description: An abstract of Sonic’s zk proving strategy
---

# Verifiable Compute & Zero-Knowledge Proofing on HyperGrid

## Introduction

The purpose of this document is to provide insight on HyperGrid's zk proving strategy. This concise document aims to address due diligence questions and, if satisfactory, will be expanded into the working architecture document. Relevant resources will be hyperlinked inline or at the end of this document.

## About the Grid zk-coprocessor Component

Sonic’s batch transaction processing layer for commitment to Solana L1 will be orchestrated into a component currently referred to as the **Sonic zk-coprocessor**. This unit will run on Sonic’s Grid instance, as well as on other grids orchestrated by HyperGrid.

{% hint style="info" %}
It is important to note that despite the current name (co-processor), this component's implementation paradigm differs from corresponding networks on other chains at various phases. \
\
Therefore, the name might change in the future, though it currently aligns well with our long-term strategy.
{% endhint %}

## Core Functions of the zk-coprocessor:

* **Processing and compression of all transactions.**
* **Generation of proofs for state transitions in each block.**
* **Commitment of proofs to Solana mainnet.**

This document will elaborate on the following topics:

* **Implementation phases of the Sonic zk-coprocessor.**
* **Runtime Primitives & Protocols used.**
* **A high-level breakdown of the implementation phases accompanied by relevant diagrams.**

## Implementation Phases

As the Solana ecosystem is still in the early phases of developing zero-knowledge runtime primitives, emerging solutions like **Light Protocol** provide frameworks that make a fully composable zkSVM a realistic possibility. Sonic's zk-Coprocessor implementation strategy is divided into two phases:

### Phase 1: HyperGrid Optimistic Rollup

In our upcoming mainnet release, the HyperGrid Optimistic Rollup Implementation will take priority due to its shorter implementation time. This will enable us to onboard games currently integrating with Sonic, fulfilling our minimum viable product requirements. Subsequently, we will focus on the full zero-knowledge rollup implementation of the Sonic co-processor, extending transaction processing on Sonic to achieve instantaneous finality.

#### Implementation Steps:

1. **Transactions are compressed and aggregated into a Merkle tree.**
2. **For each block, the corresponding root state hash is committed.**
3. **Zero-knowledge proofing is performed and committed to the mainnet.**

### Phase 2: Full Zero-Knowledge Rollup Integration

After launch, we plan to integrate a full zero-knowledge rollup, combining validity and consistency proofs into a single zero-knowledge proof. This will involve building zk-circuits for the SVM, leveraging tools provided by either Light Protocol or native Solana zk primitives.

## Sonic zk-Coprocessor Primitives

### Compressed Accounts

Each transaction within the Sonic rollup is represented as a **compressed account**. These accounts can be program-owned and optionally have a **permanent unique address (PDA)**. Sonic will use the **Light Compressed Account** implementation.

### Concurrent Merkle Trees

Compressed transaction states will be stored in a **Concurrent Merkle tree** data structure. Each piece of data created or consumed in a transaction represents a single leaf of a state tree. All tree leaves are recursively hashed, so only the final 32-byte root hash needs to be stored on-chain.

To verify the validity of many pieces of state (Compressed Accounts) within a single Solana transaction, Sonic will leverage **Light’s Zero-Knowledge Cryptography** to compress all state proofs into one small validity proof (about 128 bytes).

## Phase 1 – HyperGrid Optimistic Rollup

Our Go-To-Market (GTM) implementation will feature an optimistic rollup strategy that submits a validity proof to our verifier program on L1. The steps can be summarized as follows:

1. **Transactions are compressed and aggregated into a Merkle tree.**
2. **For each block, we commit the corresponding root state hash.**
3. **The Validity Proof for this block is computed on Sonic.**
4. **This new state and proof are validated through our Sonic verifier program on Solana L1.**

<figure><img src="../../.gitbook/assets/image (16).png" alt=""><figcaption></figcaption></figure>

#### Protocols Considered:

* **Light Protocol’s Verifier**
* **In-house Verifier (either built in-house or using Light Protocol’s verifier)**

If the consistency proof is valid, the latest record is committed to the base layer. If the proof is invalid, the witness/observation nodes initiate a challenge.

## Phase 2: Full Zero-Knowledge Rollup Integration

After the launch, this phase involves integrating a full zero-knowledge rollup by combining the validity and consistency proofs into a single zero-knowledge proof. The implementation steps are outlined as follows:

1. **Transactions are compressed and aggregated into a Merkle tree.**
2. **For each block, the corresponding root state hash is committed.**
3. **Zero-knowledge proofing is performed and committed to the mainnet.**

Additionally, the plan is divided into three major milestones:

* **Augment runtime to support transaction processing (building zk-circuits).**
* **zk runtime tracing by analyzing the output of zk-circuits.**
* **Proof generation from runtime execution trace.**

This phase is aimed at facilitating the instant finality of L2 transactions on Sonic.

<figure><img src="../../.gitbook/assets/image (15).png" alt=""><figcaption></figcaption></figure>

### Why Light Protocol?

In the phase where we build our own zkSVM, **Light Protocol** has already built a significant portion of the primitives required to implement a zkSVM. We are in communication with their team and are following new developments closely.

### References

* **Light Protocol Official Documentation**
* **Light Protocol System Overview**
* **Compressed Account**
* **Merkle Tree Primitive**
