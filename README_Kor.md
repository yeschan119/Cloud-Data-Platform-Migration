# Cloud Data Platform Migration  
### Azure 데이터 파이프라인 → AWS 분산 분석 아키텍처 전환

Azure 기반 데이터 처리 및 리포팅 시스템을 AWS 네이티브 아키텍처로 재설계한 프로젝트입니다.

단순 마이그레이션이 아닌, 데이터 파이프라인과 리포팅 엔진을 포함한 전체 구조 재설계 프로젝트입니다.

---

## 📌 프로젝트 개요

기존 시스템 구성:

- Azure Event Hubs
- Stream Analytics
- Azure Data Lake
- Azure Data Factory
- Azure SQL Server
- Power BI

목표:

- 확장성 개선
- 분산 스냅샷 자동화
- 이벤트 기반 오케스트레이션
- 인프라 비용 최적화
- 운영 복잡도 감소

---

## 🔵 기존 아키텍처 (Azure)

### 데이터 흐름

Event → Data Lake → Data Factory ETL → Azure SQL → Power BI

### 특징

- 배치 중심 처리
- ETL과 리포팅 강결합 구조
- 스냅샷 확장성 부족
- 정적 인프라 운영
- Vendor Lock-in

---

## 🟠 신규 아키텍처 (AWS)

### 핵심 구성 요소

- .NET API (Trigger Layer)
- AWS Lambda
- DynamoDB (메타데이터 저장소)
- Amazon SQS (작업 큐)
- AWS Step Functions (오케스트레이션)
- EC2 Auto Scaling Worker (Node.js)
- Amazon S3 (스냅샷 저장)
- Amazon QuickSight (리포팅)
- Amazon RDS (MySQL)

---

## 📸 분산 스냅샷 엔진 설계

이번 프로젝트의 핵심은 분산 스냅샷 생성 시스템 구현입니다.

### 스냅샷 처리 흐름

1. .NET API → Lambda 트리거
2. DynamoDB 메타데이터 확인
3. 스냅샷 미존재 시 메타 생성
4. SQS에 작업 분배
5. Step Functions 오케스트레이션
6. EC2 Worker가 QuickSight 렌더링
7. S3에 스냅샷 저장
8. DynamoDB 상태 업데이트

### 설계 포인트

- Idempotent 처리
- 실패 복구 로직
- 메타데이터 기반 상태 전이
- Auto Scaling
- Scale-to-zero 비용 최적화

---

## 🚀 마이그레이션 전략

### 1. 데이터 계층 전환
- Azure Data Lake → Amazon S3
- Azure SQL → Amazon RDS

### 2. 오케스트레이션 재설계
- Azure Data Factory → Step Functions + Lambda

### 3. 리포팅 엔진 교체
- Power BI → Amazon QuickSight

### 4. 병렬 처리 도입
- SQS 기반 분산 워커 모델

---

## ⚙️ 기술적 핵심 요소

### 이벤트 기반 아키텍처
스케줄 기반 ETL에서 이벤트 기반 오케스트레이션 구조로 전환.

### 수평 확장 구조
SQS 기반 작업 분산 및 EC2 Auto Scaling 적용.

### 메타데이터 일관성
DynamoDB를 활용한 중복 방지 및 상태 관리.

### 비용 최적화
- 작업 완료 후 자동 Scale-down
- S3 스냅샷 재사용
- 유휴 인프라 최소화

---

## 📈 개선 결과

| 항목 | Azure (기존) | AWS (개선) |
|------|--------------|------------|
| 스냅샷 생성 | 미지원 | 분산 자동화 |
| 오케스트레이션 | Data Factory | Step Functions |
| 확장성 | 제한적 | 수평 확장 |
| 리포팅 | Power BI | QuickSight |
| 비용 모델 | 고정 인프라 | Auto Scaling |

---

## 🛠 기술 스택

### Backend
- .NET Core
- Node.js

### AWS
- Lambda
- Step Functions
- EC2
- SQS
- DynamoDB
- S3
- QuickSight
- RDS (MySQL)

### 기존 Azure
- Azure Data Factory
- Azure Data Lake
- Power BI
- Azure SQL Server

---

## 🧠 아키텍처 철학

이 프로젝트는 단순한 클라우드 이전이 아닌:

- 이벤트 기반 처리
- Stateless 설계
- 분산 작업 관리
- 메타데이터 중심 오케스트레이션
- 인프라 효율 최적화

를 목표로 한 재설계 프로젝트입니다.

---

## 🎯 이력서 요약용 문장

Azure 기반 ETL 및 리포팅 시스템을 AWS 분산 분석 아키텍처로 재설계.

- SQS 기반 병렬 스냅샷 엔진 설계
- Step Functions 오케스트레이션 구현
- Power BI → QuickSight 전환
- Auto Scaling 기반 비용 최적화
