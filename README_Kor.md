# Cloud Data Platform Migration  
### Azure 데이터 파이프라인 → AWS 분산 분석 아키텍처 전환

[English](README.md)

---

## Executive Summary

Azure 기반 데이터 처리 및 리포팅 시스템을  
**AWS 네이티브 분산 분석 아키텍처로 전면 재설계한 프로젝트**.

단순 Lift-and-Shift가 아닌,

- 데이터 파이프라인 구조
- 오케스트레이션 모델
- 스냅샷 생성 엔진
- 리포팅 아키텍처

를 포함한 **전체 구조 재설계 프로젝트**.

### 핵심 성과

- ⚡ 이벤트 기반 분산 아키텍처 구축
- 📸 분산 스냅샷 자동 생성 엔진 설계
- ☁ 수평 확장 구조 구현
- 💰 인프라 비용 최적화
- 🔄 Power BI → Amazon QuickSight 전환

---

## 전환 구조 요약

**기존 (Azure)**  
Event → Data Lake → Data Factory → Azure SQL → Power BI  

**개선 (AWS)**  
Event → Lambda → SQS → Step Functions → EC2 Workers → S3 → QuickSight  

---

# 상세 내용 (클릭하여 펼치기)

---

<details>
<summary><strong>📦 기존 아키텍처 (Azure)</strong></summary>

### 기존 시스템 구성

- Azure Event Hubs
- Stream Analytics
- Azure Data Lake
- Azure Data Factory
- Azure SQL Server
- Power BI

### 데이터 흐름

Event → Data Lake → Data Factory ETL → Azure SQL → Power BI

<img width="3285" height="1820" alt="Azure" src="https://github.com/user-attachments/assets/dc3cddc0-3508-46ef-84c3-fac71857be96" />

### 특징

- 배치 중심 처리 구조
- ETL과 리포팅이 강하게 결합된 구조
- 스냅샷 확장성 부족
- 정적 인프라 운영
- Vendor Lock-in 문제

</details>

---

<details>
<summary><strong>☁ 신규 아키텍처 (AWS)</strong></summary>

<img width="845" height="584" alt="ETL" src="https://github.com/user-attachments/assets/bf8e8426-c5b9-4cba-bc81-7383f5ab0430" />

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

</details>

---

<details>
<summary><strong>📸 분산 스냅샷 엔진 설계</strong></summary>

이번 프로젝트의 핵심 개선 사항은 **분산 스냅샷 생성 시스템 구현**.

### 스냅샷 처리 흐름

1. .NET API → Lambda 트리거
2. DynamoDB 메타데이터 확인
3. 스냅샷 미존재 시 메타데이터 생성
4. SQS에 작업 분배
5. Step Functions 오케스트레이션 실행
6. EC2 Worker가 QuickSight 리포트 렌더링
7. S3에 스냅샷 저장
8. DynamoDB 상태 업데이트

### 설계 핵심 포인트

- Idempotent 처리 구조
- 실패 복구 로직
- 메타데이터 기반 상태 전이 관리
- Auto Scaling
- Scale-to-zero 비용 최적화
- Snapshot 재사용 전략

</details>

---

<details>
<summary><strong>🔄 마이그레이션 전략</strong></summary>

### 1️⃣ 데이터 계층 전환

- Azure Data Lake → Amazon S3
- Azure SQL → Amazon RDS

### 2️⃣ 오케스트레이션 재설계

- Azure Data Factory → Step Functions + Lambda

### 3️⃣ 리포팅 엔진 교체

- Power BI → Amazon QuickSight

### 4️⃣ 병렬 처리 구조 도입

- SQS 기반 분산 워커 모델 설계
- EC2 Auto Scaling 기반 작업 처리

</details>

---

<details>
<summary><strong>⚙️ 기술적 핵심 요소</strong></summary>

### 이벤트 기반 아키텍처

- 스케줄 중심 ETL 모델 제거
- 이벤트 트리거 기반 실행 구조로 전환

### 수평 확장 구조

- SQS 기반 작업 분산
- EC2 Auto Scaling 적용
- 작업 완료 후 자동 Scale-down

### 메타데이터 일관성 관리

- DynamoDB 기반 상태 관리
- 중복 실행 방지
- 원자적 상태 전이 관리

### 비용 최적화 전략

- 유휴 인프라 최소화
- Snapshot 재사용
- Scale-to-zero 구조

</details>

---

<details>
<summary><strong>📈 개선 결과 비교</strong></summary>

| 항목 | Azure (기존) | AWS (개선) |
|------|--------------|------------|
| 스냅샷 생성 | 미지원 | 분산 자동화 |
| 오케스트레이션 | Data Factory | Step Functions |
| 확장성 | 제한적 | 수평 확장 |
| 리포팅 | Power BI | QuickSight |
| 비용 모델 | 고정 인프라 | Auto Scaling |

</details>

---

<details>
<summary><strong>🛠 기술 스택</strong></summary>

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

</details>

---

<details>
<summary><strong>🏛 아키텍처 철학</strong></summary>

이 프로젝트는 단순한 클라우드 이전이 아닌:

- 이벤트 기반 실행 모델
- Stateless 처리 구조
- 분산 작업 관리
- 메타데이터 중심 오케스트레이션
- 인프라 효율성 극대화

를 목표로 한 **재설계 프로젝트**.

</details>

---

## 이력서 요약용 문장

Azure 기반 ETL 및 리포팅 시스템을 AWS 분산 분석 아키텍처로 재설계.

- SQS 기반 병렬 스냅샷 엔진 설계
- Step Functions 오케스트레이션 구현
- Power BI → QuickSight 전환
- Auto Scaling 기반 인프라 비용 최적화

---

## 🏁 결론

이 프로젝트는:

- 클라우드 아키텍처 재설계 능력
- 분산 시스템 설계 경험
- 이벤트 기반 오케스트레이션 이해
- 확장 가능한 분석 플랫폼 구축 역량
- 비용 효율적인 인프라 설계

Azure 기반 배치 ETL 시스템을  
AWS 분산 이벤트 기반 분석 플랫폼으로 전환한  
엔터프라이즈 수준의 클라우드 아키텍처 프로젝트.
