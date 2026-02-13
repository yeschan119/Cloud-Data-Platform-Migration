# Cloud Data Platform Migration  
### Azure Data Pipeline → AWS Distributed Analytics Architecture

Re-architected an enterprise Azure-based data processing and reporting system into a scalable, event-driven AWS analytics platform.

This was not a lift-and-shift migration — it was a full redesign of the data pipeline, orchestration model, and reporting engine.

---

## 📌 Project Overview

The legacy system was built on:

- Azure Event Hubs
- Stream Analytics
- Azure Data Lake
- Azure Data Factory
- Azure SQL Server
- Power BI

The goal was to migrate and improve the system using AWS-native services while achieving:

- Horizontal scalability
- Distributed snapshot generation
- Event-driven orchestration
- Infrastructure cost optimization
- Reduced operational complexity

---

## 🔵 Legacy Architecture (Azure)

### Flow

Event → Data Lake → Data Factory ETL → Azure SQL → Power BI

### Characteristics

- Batch-driven ETL
- Tight coupling between transformation and reporting
- Limited scalability for snapshot generation
- Vendor lock-in
- Static infrastructure allocation

---

## 🟠 Re-Architected Architecture (AWS)

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

---

## 📸 Distributed Snapshot Engine

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

---

## 🚀 Migration Strategy

### 1. Data Layer Migration
- Azure Data Lake → Amazon S3
- Azure SQL → Amazon RDS

### 2. Orchestration Redesign
- Azure Data Factory → Step Functions + Lambda

### 3. Reporting Engine Replacement
- Power BI → Amazon QuickSight

### 4. Parallelization
- Introduced SQS-based distributed worker model

---

## ⚙️ Technical Highlights

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

---

## 📈 Improvements

| Area | Azure (Before) | AWS (After) |
|------|---------------|-------------|
| Snapshot Generation | Not supported | Distributed & Automated |
| Orchestration | Data Factory | Step Functions |
| Scalability | Limited | Horizontal Scaling |
| Report Engine | Power BI | QuickSight |
| Cost Model | Static resources | Auto scaling |

---

## 🛠 Tech Stack

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

---

## 🧠 Architecture Philosophy

This project focused on:

- Event-driven execution
- Stateless processing
- Distributed workload management
- Metadata-driven orchestration
- Infrastructure efficiency

---

## 🎯 Resume Summary

Rebuilt Azure-based ETL and reporting system into AWS distributed analytics architecture.

- Designed SQS-based parallel snapshot engine
- Implemented Step Functions orchestration
- Migrated reporting from Power BI to QuickSight
- Reduced infrastructure idle cost via auto scaling
