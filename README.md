# Cloud Data Platform Migration  
### Azure Data Pipeline → AWS Distributed Analytics Architecture

[한국어 🇰🇷](README_Kor.md)

---

## 🚀 Executive Summary

Re-architected an enterprise Azure-based ETL and reporting platform into a **scalable, event-driven AWS analytics system**.

This was not a lift-and-shift migration — it was a **full redesign** of:

- Data pipeline
- Orchestration model
- Snapshot generation engine
- Reporting architecture

### Key Outcomes

- ⚡ Event-driven distributed architecture
- 📸 Distributed snapshot generation engine
- ☁ Horizontal scalability
- 💰 Infrastructure cost optimization
- 🔄 Metadata-driven orchestration
- 📊 Power BI → Amazon QuickSight migration

---

## 🏗 High-Level Transformation

**Before (Azure)**  
Event → Data Lake → Data Factory → Azure SQL → Power BI  

**After (AWS)**  
Event → Lambda → SQS → Step Functions → EC2 Workers → S3 → QuickSight  

---

# 🔍 Detailed Sections (Click to Expand)

---

<details>
<summary><strong>📦 Legacy Architecture (Azure)</strong></summary>

### Components

- Azure Event Hubs
- Stream Analytics
- Azure Data Lake
- Azure Data Factory
- Azure SQL Server
- Power BI

### Flow

Event → Data Lake → Data Factory ETL → Azure SQL → Power BI

<img width="3285" height="1820" alt="Azure" src="https://github.com/user-attachments/assets/0366a4f7-1947-4afb-b665-981aca2ba150" />

### Characteristics

- Batch-driven ETL
- Tight coupling between transformation and reporting
- Limited scalability for snapshot generation
- Vendor lock-in
- Static infrastructure allocation

</details>

---

<details>
<summary><strong>☁ Re-Architected Architecture (AWS)</strong></summary>

<img width="845" height="584" alt="ETL" src="https://github.com/user-attachments/assets/7d040226-7696-40ef-90f0-acda229120b0" />

### Core Components

- .NET API (Trigger Layer)
- AWS Lambda
- DynamoDB (Metadata Store)
- Amazon SQS (Job Queue)
- AWS Step Functions (Orchestration)
- EC2 Auto Scaling Workers (Node.js)
- Amazon S3 (Snapshot Storage)
- Amazon QuickSight (Reporting)
- Amazon RDS (MySQL)

</details>

---

<details>
<summary><strong>📸 Distributed Snapshot Engine</strong></summary>

One of the key architectural improvements was implementing a distributed snapshot generation system.

### Snapshot Flow

1. .NET API triggers Lambda
2. Lambda checks metadata in DynamoDB
3. If snapshot not exists → create metadata
4. Job pushed to SQS
5. Step Functions orchestrate execution
6. EC2 workers render QuickSight reports
7. Snapshot stored in S3
8. Metadata updated in DynamoDB

### Key Design Considerations

- Idempotent execution
- Failure recovery handling
- Metadata state transitions
- Auto scaling & scale-to-zero
- Cost-efficient rendering

</details>

---

<details>
<summary><strong>🔄 Migration Strategy</strong></summary>

### 1. Data Layer Migration
- Azure Data Lake → Amazon S3
- Azure SQL → Amazon RDS

### 2. Orchestration Redesign
- Azure Data Factory → Step Functions + Lambda

### 3. Reporting Engine Replacement
- Power BI → Amazon QuickSight

### 4. Parallelization
- Introduced SQS-based distributed worker model

</details>

---

<details>
<summary><strong>⚙ Technical Highlights</strong></summary>

### Event-Driven Architecture
Replaced scheduled ETL-centric model with event-triggered orchestration.

### Horizontal Scalability
Snapshot jobs distributed through SQS and processed via EC2 Auto Scaling.

### Metadata Consistency
Used DynamoDB to maintain atomic state transitions and prevent duplicate processing.

### Cost Optimization

- Auto scale-down after job completion
- Snapshot reuse via S3
- Reduced idle infrastructure cost

</details>

---

<details>
<summary><strong>📈 Improvements Comparison</strong></summary>

| Area | Azure (Before) | AWS (After) |
|------|---------------|-------------|
| Snapshot Generation | Not supported | Distributed & Automated |
| Orchestration | Data Factory | Step Functions |
| Scalability | Limited | Horizontal Scaling |
| Report Engine | Power BI | QuickSight |
| Cost Model | Static resources | Auto scaling |

</details>

---

<details>
<summary><strong>🛠 Tech Stack</strong></summary>

### Backend
- .NET Core
- Node.js

### AWS Services
- Lambda
- Step Functions
- EC2
- SQS
- DynamoDB
- S3
- QuickSight
- RDS (MySQL)

### Legacy Stack
- Azure Data Factory
- Azure Data Lake
- Power BI
- Azure SQL Server

</details>

---

<details>
<summary><strong>🏛 Architecture Philosophy</strong></summary>

This project focused on:

- Event-driven execution
- Stateless processing
- Distributed workload management
- Metadata-driven orchestration
- Infrastructure efficiency

</details>

---

## 🎯 Resume Summary

Rebuilt Azure-based ETL and reporting system into AWS distributed analytics architecture.

- Designed SQS-based parallel snapshot engine
- Implemented Step Functions orchestration
- Migrated reporting from Power BI to QuickSight
- Reduced infrastructure idle cost via auto scaling

---

## 🏁 Conclusion

This migration demonstrates:

- Deep cloud architecture redesign capability
- Distributed system design
- Event-driven orchestration
- Scalable analytics infrastructure
- Cost-aware cloud engineering

A complete transformation from batch ETL to distributed, event-driven analytics platform.
