---
description: >-
  Overview of the HyperGrid Shared State Network (HSSN), its architecture, and
  its role in managing state and communication across Grids and the Solana base
  layer.
---

# HyperGrid Shared State Network (HSSN)

## Introduction

The HyperGrid Shared State Network (HSSN) is a crucial component in the Grid ecosystem. It serves as a consensus layer, communication hub and state management cluster, facilitating interactions between Grids and the Solana base layer. This network manages the state for all communications, including periodic synchronization of block data from Grid Rollups to the Solana base layer.

![HSSN Architecture Overview](<../../.gitbook/assets/RPC Summit - HyperGrid (1).png>)

### Key Components and Features

1. **HSSN Architecture**: Built on the Cosmos framework, ensuring reliability and security in cross-chain communications.
2. **Data Structures**: Manages states between Grids and the Solana base layer, including:
   * Grid registration
   * Communication data sources
   * Versioning
   * Read/write states
3. **Extended Account Data Fields**: Augments native Solana base layer account data fields to accommodate new fields managed by HSSN, ensuring synchronization with Grid account states.
4. **Refactored Grid RPC**: Enables direct communication between Grids and the HSSN, facilitating interoperability within the ecosystem.
5. **Gas Charging and Allocation Mechanism**:
   * Users pay gas fees for certain Grid requests
   * A specialized Grid (Sonic Grid) runs a gas calculation program
   * Centrally manages gas across the entire Grid ecosystem

![Gas Charging and Allocation Mechanism](../../.gitbook/assets/hssn-gas-engine.png)

### Interactions and Data Flow

1. **Grid-to-HSSN Communication**:
   * Grids send state updates to HSSN
   * HSSN synchronizes states across Grids
2. **HSSN-to-Solana Base Layer**:
   * Periodic synchronization of block data as rollups
   * Management of extended account data fields
3. **Inter-Grid Communication**:
   * HSSN facilitates state sharing between different Grids
4. **Gas Management**:
   * Sonic Grid calculates and allocates gas fees
   * Regular synchronization of gas billing records with HSSN
   * Allocation of funds to Grid accounts, HSSN accounts, and official accounts based on billing records

### Relationship to Other Components

The HSSN plays a central role in the Grid ecosystem, interacting closely with:

* Individual Grids
* Solana Base Layer
* Gas Calculation Program (Sonic Grid)

By serving as a consensus layer for state management and communication, the HSSN enhances the scalability, interoperability, and efficiency of the entire Grid ecosystem.
