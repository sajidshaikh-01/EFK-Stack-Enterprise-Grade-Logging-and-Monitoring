<img width="750" height="382" alt="image" src="https://github.com/user-attachments/assets/5c7a184f-e003-49fc-8c00-0336571fd86b" />


# Kibana – Complete Guide (Visualizations, Dashboards, KQL)

## What is Kibana?

Kibana is the **visualization and analytics UI** for Elasticsearch. It allows users to **search, explore, visualize, and analyze data** stored in Elasticsearch indices.

In EFK / ELK stacks, Kibana is the **human-facing layer**.

---

## Why Kibana is Used

Without Kibana:

* Logs exist but are hard to explore
* No visual insights
* Debugging is slow

Kibana enables:

* Fast log searching
* Real-time dashboards
* Monitoring trends and errors
* Root cause analysis

---

## Kibana Architecture (High Level)

* Kibana connects ONLY to Elasticsearch
* It does not store data
* All queries go to Elasticsearch

```
User → Kibana → Elasticsearch → Results
```

---

## Core Kibana Components

### 1️⃣ Discover

Used to:

* Explore raw log documents
* Filter by fields
* Debug issues quickly

Typical usage:

* Search ERROR logs
* Filter by service / namespace / pod

---

### 2️⃣ Visualizations

Visualizations convert raw data into **charts and graphs**.

Common visualization types:

* Bar chart
* Line chart
* Pie chart
* Area chart
* Data table

Used to:

* Count errors
* Track request rates
* Analyze trends

---

### 3️⃣ Dashboards

A **dashboard** is a collection of multiple visualizations on one screen.

Dashboards provide:

* Single-pane-of-glass view
* Real-time monitoring
* Operational visibility

Example widgets:

* Error rate per service
* Logs per namespace
* Traffic trends

---

## Kibana Visualizations – How They Work

1. Kibana queries Elasticsearch
2. Elasticsearch returns aggregated data
3. Kibana renders charts

Visualizations are built using:

* Fields from index mappings
* Aggregations (count, avg, sum)

---

## What is Kibana Query Language (KQL)?

KQL is a **simple, powerful query language** used in Kibana to filter data.

### Key Characteristics

* Human-readable
* Faster than Lucene queries
* Used only for filtering (not scoring)

---

## KQL Syntax Basics

### Match a Field Value

```
level: ERROR
```

### Match Multiple Values

```
level: (ERROR or WARN)
```

### AND / OR / NOT

```
service: payment and level: ERROR
```

### Wildcards

```
service: pay*
```

### Exists Query

```
message: *
```

### Range Queries

```
duration > 500
```

### Date Range

```
@timestamp >= "now-15m"
```

---

## KQL vs Lucene (Short Comparison)

| Feature          | KQL  | Lucene  |
| ---------------- | ---- | ------- |
| Ease of use      | Easy | Complex |
| Full text search | No   | Yes     |
| Filtering        | Yes  | Yes     |
| Scoring          | No   | Yes     |

📌 KQL is preferred for dashboards and filtering

---

## Building a Dashboard in Kibana (Step-by-Step)

### Step 1: Create Data View

* Select Elasticsearch index
* Define time field

### Step 2: Create Visualizations

* Choose visualization type
* Select metrics and buckets
* Apply KQL filters

### Step 3: Create Dashboard

* Add multiple visualizations
* Arrange layout
* Apply global filters

---

## Example: Logging Dashboard

Dashboard widgets:

* Error count per service
* Logs over time
* Top namespaces by log volume
* Recent ERROR logs table

---

## Kibana in EFK Logging (Real Production)

* Fluent Bit indexes logs
* Elasticsearch stores data
* Kibana provides visibility

Teams use Kibana to:

* Debug production issues
* Monitor SLIs
* Investigate incidents

---

End of Document
