# Azure Billing Cost Optimizer

This project provides a **cost optimization solution** for managing billing records stored in **Azure Cosmos DB** by implementing a **hot-cold data archival strategy** using **Azure Blob Storage**.

---
 Problem Statement

The original architecture stores over 2 million billing records in Cosmos DB, which has grown expensive over time. Records older than 3 months are rarely accessed but still need to be available when requested.

---

 Goals

-  Reduce Cosmos DB storage and Request Unit (RU) costs
-  Maintain full data availability
-  Ensure zero downtime and no data loss
-  Keep existing API contracts unchanged
-  Easy to maintain and serverless

---

Solution Overview

I implemented a **hybrid storage architecture**:

- **Hot Tier (Cosmos DB):** Recent records (last 3 months)
- **Cold Tier (Blob Storage):** Older records (archived as JSON)

A serverless Azure Function acts as a **proxy**, checking Cosmos DB first and falling back to Blob if not found.

---
Technologies Used

- **Azure Cosmos DB (serverless)**
- **Azure Blob Storage (cold tier)**
- **Azure Functions (Python)**
- **Azure Timer Trigger**
- **Azure SDK for Python**
- **RBAC or SAS for Blob security**

---

 Folder Structure

```
azure-billing-optimization/
├── archive_function/           # Archives old records to Blob
│   └── __init__.py
├── read_proxy/                 # Read logic with Cosmos fallback to Blob
│   └── __init__.py
├── architecture/
│   └── architecture-diagram.png  # Architecture diagram (replace placeholder)
├── README.md
```

 1. Archival Function

- Runs daily via a timer trigger
- Moves records older than 3 months to Blob Storage
- Deletes them from Cosmos DB

 2. Read Proxy Function

- On read request:
  - Checks Cosmos DB
  - If not found → fetches from Blob Storage
- Seamless to clients (no change in API response)

---
 Architecture Diagram

![Architecture Diagram](./architecture/architecture-diagram.png)


---

Deployment guide

### Prerequisites

- Azure Subscription
- Cosmos DB and Blob Storage set up
- Azure Function App with Python runtime

Steps

1. **Deploy both `archive_function/` and `read_proxy/` as Azure Functions**
2. **Set up environment variables** (Cosmos URI, keys, blob connection)
3. **Configure Timer Trigger** to run archival daily
4. **Secure Blob Storage** using SAS or RBAC

---

- Up to **70% reduction in Cosmos DB costs**
- Improved performance for recent data
- Long-term records remain accessible on-demand
- No change to client-side APIs

---



