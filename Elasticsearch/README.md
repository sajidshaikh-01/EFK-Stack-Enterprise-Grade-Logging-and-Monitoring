

<img width="1407" height="788" alt="image" src="https://github.com/user-attachments/assets/35ed4e9a-33cf-42fa-af56-1c9c05db7c5e" />



<img width="1406" height="734" alt="image" src="https://github.com/user-attachments/assets/43eae6c2-13f4-4dff-bf5c-8417e388408b" />



# Elasticsearch Nodes and Cluster – Complete Guide (Production Ready)

## What is an Elasticsearch Cluster?

An **Elasticsearch cluster** is a collection of one or more Elasticsearch nodes that work together to store data, provide indexing, and perform search and analytics.

### Why Cluster?

* High availability
* Horizontal scalability
* Fault tolerance
* Distributed search and storage

A cluster is identified by a **cluster name**, and all nodes with the same cluster name automatically join together.

---

## What is an Elasticsearch Node?

A **node** is a single running instance of Elasticsearch.

Each node:

* Stores data (or metadata)
* Participates in indexing and search
* Communicates with other nodes in the cluster

In production, a node usually runs on:

* One VM / EC2 instance
* One Kubernetes pod

---

## Cluster Architecture (High Level)

* Multiple nodes form a cluster
* Data is distributed across nodes using **shards**
* Replica shards provide redundancy
* Requests can hit any node

---

## Node Roles in Elasticsearch

Elasticsearch supports **role-based nodes**, meaning each node can have one or more responsibilities.

---

### 1. Master-Eligible Node

#### Role

* Manages cluster state
* Handles node joins/removals
* Manages index creation and shard allocation

#### Key Points

* Does NOT handle heavy indexing/search traffic
* Critical for cluster stability
* Usually 3 dedicated master nodes in production

Example:

```
node.roles: [ master ]
```

---

### 2. Data Node

#### Role

* Stores actual data (shards)
* Handles indexing and search queries

#### Key Points

* Heavy CPU, memory, and disk usage
* Backbone of Elasticsearch
* Scales horizontally

Example:

```
node.roles: [ data ]
```

---

### 3. Ingest Node

#### Role

* Pre-processes documents before indexing
* Executes ingest pipelines (parse, enrich, transform)

#### Use Cases

* Log parsing
* GeoIP enrichment
* Field normalization

Example:

```
node.roles: [ ingest ]
```

---

### 4. Coordinating Node

#### Role

* Acts as request router
* Distributes queries to data nodes
* Aggregates results

#### Key Points

* No data storage
* Improves query performance

Example:

```
node.roles: []
```

---

### 5. Machine Learning (ML) Node

#### Role

* Runs ML jobs
* Anomaly detection
* Forecasting

Used in advanced observability setups.

Example:

```
node.roles: [ ml ]
```

---

### 6. Transform Node

#### Role

* Runs data transformation tasks
* Creates summarized or pivoted indices

Example:

```
node.roles: [ transform ]
```

---

### 7. Remote Cluster Client Node

#### Role

* Enables cross-cluster search and replication

Example:

```
node.roles: [ remote_cluster_client ]
```

---

## Common Production Cluster Setup

### Small Production Cluster

* 3 Master nodes
* 3 Data nodes
* 1 Coordinating node

### Large Production Cluster

* 3 Dedicated Masters
* Many Data nodes
* Separate Ingest nodes
* Coordinating nodes for Kibana/API

---

## Why Dedicated Master Nodes Are Important

* Prevent split-brain scenarios
* Ensure cluster stability
* Avoid overload from search traffic

Rule:

> Always use **odd number of master nodes** (usually 3)

---

## Elasticsearch in Kubernetes (EKS Example)

* Each node runs as a pod
* StatefulSets are used
* Persistent volumes store data
* Pod disruption budgets protect masters

---


End of Document

