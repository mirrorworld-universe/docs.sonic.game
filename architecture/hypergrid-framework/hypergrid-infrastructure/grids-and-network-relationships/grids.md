# Grids

## Definition

A Grid in the HyperGrid architecture is a semi-autonomous network within the larger HyperGrid framework, designed to host and operate particularly high-demand application-specific use cases. Examples include Gaming, DeFi, on-chain AI Agents or applications-specific networks.

Sonic is an example of a Grid optimized for high-performance gaming. A Grid can take on the form a single node, or an entire cluster

## Characteristics of a Grid

&#x20;Here are the key characteristics of a Grid:

1. **Semi-autonomous operation**: Each Grid functions independently to a large extent, but remains connected to the overall HyperGrid system.
2. **Dedicated resources**: A Grid has its own set of components, including:
   * A ZK-coprocessor for managing grid-specific operations
   * A local BlockStore (also referred to as "Bank - local") for handling account and program data
   * Runtime environments (like Sonic SVM Runtime and Sonic EVM Runtime)
   * A Concurrent Merkle Tree Generator for efficient state transitions
3. **State sharing**: Grids interact with the HyperGrid Shared State Network, synchronizing states to maintain consistency across the system.
4. **User interaction**: Users can interact directly with a Grid, sending transactions and receiving responses.
5. **Scalability**: Grids allow for horizontal scaling, enabling developers to create dedicated networks tailored to their specific requirements.
6. **Performance isolation**: By separating dApps into different Grids, the system can mitigate performance conflicts between applications and reduce strain on the Solana Base Layer.
7. **Flexibility**: Developers can choose to use the public HyperGrid network or create their own dedicated Grid based on performance needs and cost considerations.
8. **Independent lifecycle**: A Grid can be activated or deactivated without affecting other Grids in the system.
9. **SVM Client Diversity:** HyperGrid is client-agnostic, meaning that it can be used with any SVM client implementation. This makes it possible for network operators on Solana to implement high-performance SVM clients while relying on HyperGrid's Shared State Network (HSSN) for consensus and interoperability.

In essence, a Grid serves as a scalable, flexible, and semi-independent cluster of nodes or an environment within the HyperGrid system, designed to host and manage specific dApps while maintaining a connection to the overall HyperGrid architecture and the Solana Base Layer.
