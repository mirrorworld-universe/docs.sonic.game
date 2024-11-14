---
description: This page introduces Sorada, Sonic's Data Solution.
---

# Introduction

## Blockchain's Data Layer Problem

Blockchain networks aggregate data through `blocks`, `transactions`, and `metadata`.

Querying APIs are openly available and well-documented for fetching these data directly from the network.

This may work for small decentralized applications (DApps) with minimal historical data needs, but for bigger historical data requirements the baseline APIs fail to meet 2 main business requirements:

* Performance
* Scalability

### Performance

Companies have barely any control over the response times and the data operation optimizations of the baseline querying API endpoints provided by the network.

This is critical for business requirements because performance greatly impacts [website speed, search engine optimization (SEO), and conversion rate](https://www.cloudflare.com/learning/performance/more/website-performance-conversion-rates/).

### Scalability

Similar to performance, companies cannot optimally adapt to the demands of their users with elastic provisioning due to the limitation of the baseline Querying API endpoints provided by the network.

This is another critical business requirement because scalability goes beyond performance and impacts the financial feasibility of operating.

### How Blockchain Indexing help solve this problem

Blockchain Indexing is the process of querying historical data directly from the network and storing them in in-house provisioned infrastructure.

A typical and simplified Blockchain Indexing pipeline would have the following flow:

1. Use network API to fetch `blocks` and `transactions`.
2. Store `blocks` and `transactions` to internal storage infrastructure.
3. Use internally provisioned data query endpoints for products as opposed to querying directly from the network

This way, the Company has internal control over the Performance and Scalability of its historical data needs.

## A Sonic Case Study: 85% Archival Reads

According to Sonic's internal [Hypergrid](https://docs.sonic.game/developers/hypergrid-framework/hypergrid-infrastructure) Infrastructure monitoring, Sonic experienced around **85% Archival Read Requests vs 15% Write Requests**.

These read requests span across `getTransaction`, `getBlock`, `getSignaturesForAddress`.

While this is a solid metric of demand for Sonic, the engineering team started experiencing bottlenecks around `Bandwidth` and `Storage` costs for requests directly funneled towards the Hypergrid infrastructure.

These bottlenecks were solved by decoupling typical archival read requests away from Sonic's main Hypergrid infrastructure and into **Sorada**.

## What is Sorada?

Sorada is Sonic's **Data Solution** with the main objective of decoupling **Archival Read Requests** away from **Network Write Requests.**

By decoupling archival read requests away from the main Sonic Hypergrid and into a separate data-optimized infrastructure, `Bandwidth` and `Storage` bottlenecks were mitigated.

