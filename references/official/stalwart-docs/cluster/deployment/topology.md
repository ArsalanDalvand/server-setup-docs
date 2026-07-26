---
title: "Topology"
source: "https://stalw.art/docs/cluster/deployment/topology"
source_format: official-html-to-markdown
language: "en"
doc_slug: "cluster/deployment/topology"
category: "cluster"
retrieved_at: "2026-07-26T15:40:57+00:00"
tags:
  - stalwart
  - mail-server
  - official-documentation
  - rust-network
aliases:
  - "Topology"
---
# Topology

      

In a Stalwart cluster, administrators control how user-facing services are distributed across nodes. This is distinct from [[cluster/configuration/roles|node roles]], which assign background maintenance tasks such as store maintenance or certificate renewal. Cluster topology focuses on which protocols (IMAP, JMAP, WebDAV, SMTP) each node serves.

Stalwart allows flexible service distribution: each node can be configured to handle one, several, or all supported protocols. This supports tuning for performance, resource usage, and fault tolerance according to real-world traffic patterns and operational goals.

Listener selection per node is driven by the [[ref/object/cluster-role|ClusterRole]] object (found in the WebUI under  Settings ›  Cluster › Roles), through its [[ref/object/cluster-role#listeners|`listeners`]] field (a [[ref/object/cluster-role#clusterlistenergroup|ClusterListenerGroup]] that names specific [[ref/object/network-listener|NetworkListener]] ids).

The sections below describe common topology strategies used in Stalwart clusters.

## Unified service model

[Section titled “Unified service model”](#unified-service-model)

In this approach, every node handles every service: IMAP, JMAP, WebDAV, and SMTP. For example, in a 1024-node deployment, each server is configured identically and can handle any client or protocol.

```
flowchart LR
    LB[Load Balancer]
    
    subgraph Cluster["1,024 Unified Nodes"]
        direction LR
        N1["Node 1SMTP+IMAP+JMAP"]
        N2["Node 2SMTP+IMAP+JMAP"]
        N3["..."]
        N1024["Node 1024SMTP+IMAP+JMAP"]
    end
    
    subgraph Storage
        FDB[(Metadata)]
        CEPH[(Blobs)]
    end
    
    LB --> Cluster
    Cluster --> Storage
```

This model suits:

Simpler management with uniform configuration
Redundancy, since any node can fail without losing service
Smaller or mid-sized environments where traffic is evenly distributed

The unified model also simplifies load balancing and reduces operational complexity, though it may be less efficient in scenarios where some services see significantly more traffic than others.

## Service-specific allocation

[Section titled “Service-specific allocation”](#service-specific-allocation)

In this model, nodes are dedicated to specific protocols. For example, a 1024-node cluster might be divided into 256 SMTP nodes, 384 IMAP nodes, 256 JMAP nodes, and 128 WebDAV nodes.

```
flowchart TB
    subgraph SMTP["SMTP Nodes (256)"]
        S1[Node 1-256]
    end
    
    subgraph IMAP["IMAP Nodes (384)"]
        I1[Node 257-640]
    end
    
    subgraph JMAP["JMAP Nodes (256)"]
        J1[Node 641-896]
    end
    
    subgraph WebDAV["WebDAV Nodes (128)"]
        W1[Node 897-1024]
    end
    
    subgraph Storage
        FDB[(Metadata)]
        CEPH[(Blobs)]
    end
    
    SMTP & IMAP & JMAP & WebDAV --> Storage
```

This separation of concerns is useful when:

Isolating workloads for performance tuning
Certain services (for example, SMTP) need different network access or security policies
Teams are structured around managing specific services

Service-specific allocation allows more granular resource planning, at the cost of more sophisticated monitoring and load balancing.

## Weighted allocation based on load

[Section titled “Weighted allocation based on load”](#weighted-allocation-based-on-load)

Clusters can also be sized according to expected usage patterns. For instance, if IMAP usage is significantly heavier than JMAP or WebDAV, the topology could look like this:

ProtocolNodesRationaleIMAP512Long-lived connections, high memorySMTP256Burst traffic, queue processingJMAP192API-heavy, mobile clientsWebDAV64Calendar and contacts, lower volume

```
flowchart LR
    subgraph Distribution
        direction TB
        IMAP["IMAP512 nodes(50%)"]
        SMTP["SMTP256 nodes(25%)"]
        JMAP["JMAP192 nodes(19%)"]
        WebDAV["WebDAV64 nodes(6%)"]
    end
```

This model balances redundancy and efficiency, allocating more resources to higher-demand services while still covering less-used protocols.

It suits:

Enterprise environments with known usage trends
Scenarios where IMAP or SMTP dominate traffic
Clusters designed to scale incrementally

## Protocol pairing model

[Section titled “Protocol pairing model”](#protocol-pairing-model)

Some organisations prefer to group complementary protocols. A common configuration looks like:

40 percent of the nodes for IMAP and JMAP
20 percent of the nodes for WebDAV
40 percent of the nodes for inbound and outbound SMTP

```
flowchart TB

    subgraph IMAP_JMAP_Nodes [IMAP + JMAP Nodes]
        direction LR
        Pair1[Node 1]
        Pair2[Node 2]
        Pair3[Node 3]
        Pair4[Node 4]
    end

    subgraph WebDAV_Nodes [WebDAV Nodes]
        direction LR
        WebDAV1[Node 5]
        WebDAV2[Node 6]
    end

    subgraph SMTP_Nodes [SMTP In + Out Nodes]
        direction LR
        SMTP1[Node 7]
        SMTP2[Node 8]
        SMTP3[Node 9]
        SMTP4[Node 10]
    end

    Pair1 --> IMAP[IMAP Service]
    Pair1 --> JMAP[JMAP Service]
    Pair2 --> IMAP
    Pair2 --> JMAP
    Pair3 --> IMAP
    Pair3 --> JMAP
    Pair4 --> IMAP
    Pair4 --> JMAP

    WebDAV1 --> WebDAV[WebDAV Service]
    WebDAV2 --> WebDAV

    SMTP1 --> SMTP[SMTP In + Out Service]
    SMTP2 --> SMTP
    SMTP3 --> SMTP
    SMTP4 --> SMTP
```

This model offers:

Reduced configuration duplication
Logical pairing of user-facing services (for example, IMAP and JMAP both serve mail clients)
Efficient use of resources while still allowing role separation

Pairing services also simplifies routing and firewall policies, especially when grouped by access pattern (client-facing versus mail-routing).

## Geographically distributed topology

[Section titled “Geographically distributed topology”](#geographically-distributed-topology)

In larger or multi-site deployments, nodes may be distributed across data centres or geographic regions, with services colocated by regional demand:

Region A: 384 nodes (IMAP, SMTP inbound)
Region B: 448 nodes (IMAP, JMAP, SMTP outbound)
Region C: 192 nodes (WebDAV, JMAP, SMTP)

```
flowchart TB
    subgraph EU["EU Region (384 nodes)"]
        EU_S[SMTP: 96]
        EU_I[IMAP: 192]
        EU_J[JMAP: 96]
    end
    
    subgraph US["US Region (448 nodes)"]
        US_S[SMTP: 112]
        US_I[IMAP: 224]
        US_J[JMAP: 112]
    end
    
    subgraph APAC["APAC Region (192 nodes)"]
        AP_S[SMTP: 48]
        AP_I[IMAP: 96]
        AP_J[JMAP: 48]
    end
    
    subgraph Global["Global Storage"]
        FDB[(MetadataMulti-region)]
        CEPH[(BlobsGeo-replicated)]
    end
    
    EU & US & APAC <--> Global
```

This approach improves:

Latency for users in different regions
Resilience to regional outages
Load isolation by physical location

Geographically distributed clusters typically rely on global load balancers, DNS-based routing, or geo-aware proxies to direct traffic efficiently.

## Choosing the right topology

[Section titled “Choosing the right topology”](#choosing-the-right-topology)

There is no one-size-fits-all topology. The best choice depends on organisational size, usage patterns, and operational preferences. One of the strengths of the Stalwart architecture is that topologies are flexible and can be adjusted over time. A simple unified model can be a starting point, with a transition to a more specialised layout as needs evolve.

Approaches can also be mixed and matched, running unified nodes alongside dedicated ones, or shifting services dynamically as demand grows.