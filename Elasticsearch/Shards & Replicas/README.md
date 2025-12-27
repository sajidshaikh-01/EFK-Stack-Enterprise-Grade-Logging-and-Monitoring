<img width="1407" height="788" alt="image" src="https://github.com/user-attachments/assets/a9025e77-3da6-4578-ae0f-163f45a3f585" />


# Elasticsearch Shards and Replicas – Explained with Architecture (Beginner → Production)

## Why Shards and Replicas Exist

Elasticsearch is designed for **large-scale, distributed data**. Shards and replicas are the core mechanisms that allow Elasticsearch to:

* Scale horizontally
* Handle node failures
* Perform fast searches

---

## What is a Shard?

A **shard** is a **physical partition of an index**.

### Key Points

* An index is divided into multiple shards
* Each shard is a fully functional mini-index
* Shards are distributed across nodes

### Why shards are needed

* One machine cannot store or search unlimited data
* Shards allow data to be split and processed in parallel

### Example

```
Index: logs-payment
Primary shards: 3

Shard 0 → Node A
Shard 1 → Node B
Shard 2 → Node C
```

---

## Types of Shards

### 1️⃣ Primary Shard

* Original shard where data is written
* Required for indexing
* Number is fixed at index creation

### 2️⃣ Replica Shard

* Copy of a primary shard
* Used for high availability and read scaling
* Can be changed anytime

---

## What is a Replica?

A **replica** is a **copy of a primary shard** stored on a different node.

### Why replicas are important

* Protect data if a node fails
* Improve search performance
* Enable zero-downtime maintenance

---

## How Shards and Replicas Work Together

Example:

```
Index: logs-payment
Primary shards: 2
Replicas: 1

Node A → Primary Shard 0
Node B → Primary Shard 1
Node C → Replica of Shard 0
Node D → Replica of Shard 1
```

Total shards:

```
2 primary + 2 replicas = 4 shards
```

---

## Data Flow During Indexing

1. Client sends log
2. Coordinating node decides shard
3. Data written to primary shard
4. Primary shard copies data to replica shard
5. Write acknowledged

---

## Data Flow During Search

1. Search query hits coordinating node
2. Query sent to all relevant shards
3. Each shard searches locally
4. Results aggregated and returned

---

## Node Failure Scenario (VERY IMPORTANT)

### Without replica (replicas = 0)

* Node fails → shard lost
* Data unavailable

### With replica (replicas = 1)

* Node fails
* Replica promoted to primary
* No data loss
* Cluster continues normally

---

## Production Best Practices

### Shard count

* Avoid too many shards
* Recommended shard size: 10–50 GB

### Replica count

* Dev: 0 replicas
* Prod: at least 1 replica

### Why too many shards is bad

* High memory usage
* Slow cluster state updates

---

## Shards and Replicas in Kubernetes (EKS)

* Each shard lives inside a pod
* Shards are spread across nodes
* Pod rescheduling triggers shard relocation

---

