---
description: >-
  Step-by-step flow of reading data from and synchronizing updates to the Solana
  Base Layer within the HyperGrid architecture.
---

# Data Synchronization Between HyperGrid and Solana Base Layer

## Reading Data from Solana to HyperGrid

<figure><img src="../../../.gitbook/assets/image (3).png" alt=""><figcaption><p>Reading Data from Solana to HyperGrid</p></figcaption></figure>

The diagram above illustrates the following flow when performing state synchonization from Solana to a Grid on HyperGrid, like Sonic.

1. Initial loading: A pre-existing Solana program is loaded from Storage into HyperGrid's Cache.
2. User sends a read request for a specific program to HyperGrid's Sonic RPC.
3. Synchronization Program checks the Cache for the requested Program, but it's not found.
4. Synchronization Program sends a request to Solana Base Layer RPC for the Program.
5. Solana Base Layer responds with the Program data.
6. Synchronization Program receives the response and updates HyperGrid's Cache with the new Program data.
7. Synchronization Program sends the read response back to Sonic RPC.
8. Sonic RPC forwards the read response to the User.

## Synchronizing Updates Back to the Solana Base Layer

<figure><img src="../../../.gitbook/assets/image (4).png" alt=""><figcaption><p>Synchronizing Updates from Grid to Base Layer</p></figcaption></figure>

The diagram above illustrates the following flow when performing state synchonization from a Grid on HyperGrid \[like Sonic], back to Solana.

1. Initial loading: A pre-existing Program is loaded from Storage into HyperGrid's Cache.
2. User sends a write request for a specific Program to HyperGrid's Sonic RPC.
3. Synchronization Program checks the Cache for the requested Program, but it's not found.
4. Synchronization Program sends a request to lock the program on the Solana Base Layer.
5. Solana Base Layer RPC locks the requested Program.
6. Solana Base Layer responds with the Program data.
7. Synchronization program receives the response and updates HyperGrid's Cache with the new program data.
8. Synchronization Program sends a request to release the lock and write the updated data for the Program to the Solana Base Layer.
9. Solana Base Layer RPC releases the lock and writes the updated data.
10. Synchronization Program sends the write response back to Sonic RPC.
11. Sonic RPC forwards the write response to the User.
