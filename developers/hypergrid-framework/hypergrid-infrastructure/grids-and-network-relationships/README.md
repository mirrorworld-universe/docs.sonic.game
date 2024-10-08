---
description: >-
  HyperGrid's network architecture: Shared State Network, Grid instances, and
  Solana Base Layer relationships for scalable dApp deployment.
---

# Grids and Network Relationships

<figure><img src="../../../../.gitbook/assets/image (2).png" alt=""><figcaption><p>HyperGrid Shared State Network Data Flow</p></figcaption></figure>

The HyperGrid framework is designed to support a wide array of application-specific networks, or decentralized applications, with a particular focus on high-demand applications like games, DeFi, AI Agents, e.t.c. within its ecosystem. This architecture aims to:

1. Reduce strain on the Solana Base Layer's performance
2. Minimize performance conflicts and competition for block-space inclusion between Base Layer and the various domain-specific dApps.

{% hint style="info" %}
HyperGrid makes it possible for dApps or operators to either instantiate their own Grid instance for their specific application or implement their own domain-specific SVM network.

[Click this link to learn more about Grids](grids.md).
{% endhint %}

### Key Features

* **Flexibility for Grid Network Creators**: Developers can choose between:
  * Using the HyperGrid public network
  * Horizontally scaling to create dedicated networks tailored to specific needs
* **Performance and Cost Optimization**: The choice between public and dedicated networks is based on developers' assessments of performance requirements and associated costs.
* **Network Independence**: Developers can deactivate their respective networks without affecting others in the ecosystem.

### Operational Framework

* **Validation**: Each network autonomously handles the validation of its transactions and state changes.
* **Logging**: Transaction and state alteration logs are maintained independently by each network.
* **Retrieval**: Data retrieval processes are executed autonomously for each network.

### Integration with Solana

The linkage between HyperGrid and the Solana Base Layer ensures that all operations, while processed independently within each Grid instance, ultimately anchor to Solana for final consensus and security.

This architecture provides a scalable and flexible environment for dApp deployment, allowing developers to optimize their applications' performance while leveraging the security and stability of the Solana blockchain.
