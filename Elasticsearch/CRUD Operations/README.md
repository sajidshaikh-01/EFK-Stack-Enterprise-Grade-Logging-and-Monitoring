<img width="1406" height="734" alt="image" src="https://github.com/user-attachments/assets/30a1fdd9-824a-41ba-a942-7b33ce5f1613" />


# Elasticsearch CRUD Operations – Complete Guide (Beginner → Production)

## What is CRUD?

CRUD stands for:

* **C**reate
* **R**ead
* **U**pdate
* **D**elete

CRUD operations define **how we interact with data** stored in Elasticsearch documents.

In Elasticsearch, CRUD operations are performed **on documents inside an index**.

---

## Why CRUD Operations Are Used

CRUD operations are used to:

* Store logs and data (Create)
* Search and retrieve logs (Read)
* Modify existing data (Update)
* Remove old or incorrect data (Delete)

Without CRUD operations, Elasticsearch would just be storage with no interaction.

---

## CRUD in Elasticsearch Context

| Operation | Elasticsearch Meaning       |
| --------- | --------------------------- |
| Create    | Index a new document        |
| Read      | Search or get documents     |
| Update    | Modify an existing document |
| Delete    | Remove a document or index  |

---

## CREATE – Index a Document

Used to **add a new document** to an index.

### Example: Create Document (Auto ID)

```http
POST logs-payment/_doc
{
  "timestamp": "2025-01-15T10:30:22Z",
  "level": "INFO",
  "service": "payment-service",
  "message": "Payment completed successfully"
}
```

### Example: Create Document (Custom ID)

```http
PUT logs-payment/_doc/12345
{
  "timestamp": "2025-01-15T10:30:22Z",
  "level": "ERROR",
  "service": "payment-service",
  "message": "Payment failed"
}
```

---

## READ – Retrieve Documents

Used to **fetch or search data**.

### Get by ID

```http
GET logs-payment/_doc/12345
```

### Search Documents

```http
GET logs-payment/_search
{
  "query": {
    "match": {
      "level": "ERROR"
    }
  }
}
```

---

## UPDATE – Modify a Document

Used to **change fields of an existing document**.

### Partial Update

```http
POST logs-payment/_update/12345
{
  "doc": {
    "level": "CRITICAL"
  }
}
```

📌 Elasticsearch internally performs **read → modify → reindex**.

---

## DELETE – Remove Data

Used to **delete documents or indices**.

### Delete a Document

```http
DELETE logs-payment/_doc/12345
```

### Delete an Index (DANGEROUS)

```http
DELETE logs-payment
```

---

## CRUD Operations in EFK Logging (Real World)

| Operation | How It Happens          |
| --------- | ----------------------- |
| Create    | Fluent Bit indexes logs |
| Read      | Kibana searches logs    |
| Update    | Rare (usually avoided)  |
| Delete    | ILM deletes old indices |

📌 In logging systems:

* **Create** and **Read** are most common
* **Update** and **Delete** are minimal

---

## Important Production Notes

* Updates are expensive (reindexing)
* Logs are usually immutable
* Deletion is handled via ILM, not manual CRUD
* Direct deletes in prod are avoided

---

