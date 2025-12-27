<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/0e1e3213-b644-41b6-9ebd-cd0aef70868a" />


# Elasticsearch Mapping – Explained with Types and Examples (Beginner → Production)

## Why Mapping is Important

**Mapping** defines **how documents and their fields are stored and indexed** in Elasticsearch.

If index & document answer *where data lives*, mapping answers:

* What is the data type?
* How should it be indexed?
* How should it be searched?

Bad mapping = slow queries, wrong search results, high cost.

---

## What is Mapping in Elasticsearch?

A **mapping** is a **schema definition** for an index.

It defines:

* Field names
* Field data types (text, keyword, number, date, etc.)
* How each field is indexed and searched

### Simple Definition

> Mapping is like a **schema in SQL**, but more flexible.

---

## Mapping vs SQL Schema (Analogy)

| SQL      | Elasticsearch |
| -------- | ------------- |
| Database | Cluster       |
| Table    | Index         |
| Column   | Field         |
| Row      | Document      |
| Schema   | Mapping       |

---

## Example Document (Log Event)

```json
{
  "timestamp": "2025-01-15T10:30:22Z",
  "level": "ERROR",
  "service": "payment-service",
  "message": "Payment failed due to timeout",
  "duration": 250,
  "success": false
}
```

Mapping defines how each field above is treated.

---

## Types of Mapping in Elasticsearch

### 1️⃣ Dynamic Mapping

Elasticsearch **automatically detects field types** when documents are indexed.

#### How it works

* First document arrives
* Elasticsearch inspects field values
* Mapping is created automatically

#### Example

```json
"duration": 250  → integer
"success": false → boolean
```

#### Pros

* Easy to start
* No upfront configuration

#### Cons (IMPORTANT)

* Wrong field types possible
* Hard to change later

📌 **Not recommended for strict production data**

---

### 2️⃣ Explicit (Static) Mapping

You **manually define the mapping** before indexing documents.

#### Example

```json
PUT logs-payment
{
  "mappings": {
    "properties": {
      "timestamp": { "type": "date" },
      "level": { "type": "keyword" },
      "service": { "type": "keyword" },
      "message": { "type": "text" },
      "duration": { "type": "integer" },
      "success": { "type": "boolean" }
    }
  }
}
```

#### Pros

* Full control
* Predictable performance
* Best for production

#### Cons

* Needs planning

---

### 3️⃣ Dynamic Templates (Advanced)

Rules that define mapping behavior **for future fields**.

#### Example

```json
"dynamic_templates": [
  {
    "strings_as_keywords": {
      "match_mapping_type": "string",
      "mapping": {
        "type": "keyword"
      }
    }
  }
]
```

Used when:

* Logs have dynamic fields
* You want consistent mapping rules

---

## Common Field Data Types (VERY IMPORTANT)

| Type    | Use Case                            |
| ------- | ----------------------------------- |
| text    | Full-text search (logs, messages)   |
| keyword | Filtering, aggregation, exact match |
| integer | Numeric values                      |
| float   | Decimal values                      |
| date    | Time-based data                     |
| boolean | true/false                          |
| object  | Nested JSON                         |

---

## text vs keyword (MOST ASKED QUESTION)

### text

* Tokenized
* Used for full-text search
* Example: log messages

### keyword

* Not tokenized
* Used for filtering & aggregations
* Example: service name, log level

📌 Best practice:

```
"message": text
"service": keyword
```

---

## How Mapping Works Internally

1. Index created (with or without mapping)
2. Mapping stored in cluster state
3. Documents indexed
4. Fields analyzed based on mapping
5. Queries use mapping to search correctly

---

## Can Mapping Be Changed?

### Rules

* You can ADD new fields
* You CANNOT change existing field types

If wrong mapping happens:

* Create a new index
* Reindex data

---

## Mapping in EFK Logging (Real Production)

Typical choices:

* message → text
* level → keyword
* service → keyword
* timestamp → date

Fluent Bit often sends JSON logs that respect predefined mappings.

---

## Production Best Practices

* Avoid dynamic mapping in prod
* Use index templates
* Use dynamic templates for logs
* Keep mappings consistent
* Avoid too many fields (mapping explosion)

---

## Interview-Ready Answer 🏆

"Mapping in Elasticsearch defines how document fields are stored and indexed. It specifies field data types and search behavior. Elasticsearch supports dynamic mapping, explicit mapping, and dynamic templates, with explicit mappings preferred in production for predictable performance."

-





