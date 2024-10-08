---
description: >-
  Comprehensive guide to the Hypergrid Shared Sequencer Network (HSSN)
  dashboard, detailing its key features and data visualization across multiple
  pages.
---

# HSSN Explorer Overview

## Introduction

The Sonic Hypergrid Shared Sequencer Network (HSSN) is a Cosmos-based network that operates between the Grid and the Solana Base Layer. It manages state for all communications, including inter-grid interactions and periodic synchronization of block data from the grid as a rollup to the Solana base layer.

To monitor the HSSN's operations, a dedicated dashboard has been provisioned.&#x20;

{% hint style="info" %}
You can access the HSSN Explorer dashboard at: [https://explorer-hssn.hypergrid.dev/](https://explorer-hssn.hypergrid.dev/)
{% endhint %}

{% embed url="https://drive.google.com/file/d/13y-B5pgrdlKZ5HvMqUvviO-uPJCg_ike/view?usp=sharing" %}

## Dashboard Pages

### 1. Home Page

<figure><img src="../../.gitbook/assets/image (5).png" alt=""><figcaption></figcaption></figure>

The Home page provides an overview of the entire HSSN:

* Latest Block Height
* Latest Block Time
* Network Name (currently "hypergridssn")
* Number of Validators
* Connection diagram showing communication flow between grids and Solana through the HSSN

### 2. Grids Page



Displays all nodes in the HSSN:

* Name
* Role (Solana, HSSN, Sonic Grid, or Grid)
* RPC interface
* Public key
* Data account
* Start time

### 3. Syncs Page

<figure><img src="../../.gitbook/assets/image (7).png" alt=""><figcaption></figcaption></figure>

Shows synchronization data between Grid nodes and Solana:

* ID
* Grid name
* Account information
* Slot number
* Transaction hash
* Creator account

### 4. Blocks Page

<figure><img src="../../.gitbook/assets/image (8).png" alt=""><figcaption></figcaption></figure>

Presents blockchain information within the HSSN:

* Block tab: Height, App Hash, Number of transactions, Creation time
* Transactions tab: TX Hash, Result, Messages, Height, Time

<figure><img src="../../.gitbook/assets/image (9).png" alt=""><figcaption><p>Transactions Tab</p></figcaption></figure>

### 5. Validators Page

<figure><img src="../../.gitbook/assets/image (10).png" alt=""><figcaption></figcaption></figure>

Provides information about HSSN validators:

* Validator name
* Status
* Voting power

### 6. Shared Accounts Page

<figure><img src="../../.gitbook/assets/image (12).png" alt=""><figcaption></figcaption></figure>

Lists communication status information within the HSSN:

* Address
* Source node
* Slot
* Account info
* Creator
* Version

### 7. Fee Page

Displays fee bills and settlement records:

* Grid Block Fee tab: Original transaction fee records
* Fee Settlement tab: Periodic settlement records based on fee bills

<figure><img src="../../.gitbook/assets/image (13).png" alt=""><figcaption><p>Fee Page</p></figcaption></figure>

<figure><img src="../../.gitbook/assets/image (14).png" alt=""><figcaption><p>Fee Settlement Page</p></figcaption></figure>

## Relationship to Other Components

The HSSN Dashboard provides crucial insights into the operations of the HSSN, which plays a central role in the Grid ecosystem. It offers visibility into:

* Communication between Grids
* Synchronization with the Solana Base Layer
* Gas fee calculations and settlements

By providing this comprehensive view, the explorer dashboard enhances transparency and facilitates efficient management of the entire Grid ecosystem.
