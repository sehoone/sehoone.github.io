---
title: "[AWS-SAA] EC2 Database"
last_modified_at: 2022-06-06T16:01:04-04:00
categories:
  - AWS-SAA
tags:
  - update
toc: true
toc_label: "AWS-SAA"
---

## 1. 데이터베이스 유형
- RDBMS (= SQL / OLTP): RDS, Aurora. 조인에 적합
- NoSQL database: DynamoDB (~JSON), ElastiCache (key / value pairs), Neptune (graphs) – 조인없음. SQL없음
- Object Store: S3 (for big objects) / Glacier (for backups / archives)
- Data Warehouse (= SQL Analytics / BI): Redshift (OLAP), Athena
- Search: ElasticSearch (JSON) – 자유 텍스트, 구조화되지 않은 검색
- Graphs: Neptune – 데이터간의 관계 표시

## 2. RDS
- PostgreSQL / MySQL / Oracle / SQL Server
- EC2 인스턴스 및 EBS 볼륨 유형 및 크기를 프로비저닝 필요
- 읽기 전용 복제본 및 다중 AZ 지원
- IAM, Security Groups, KMS , SSL 보안지원
- CloudWatch를 통한 모니터링
- 사용 사례: 관계형 데이터 세트(RDBMS/OLTP) 저장, SQL 쿼리 수행, 트랜잭션 삽입/업데이트/삭제 가능

## 3. RDS for SA(Solutions Architect)
- 운영: 장애 조치 발생 시 약간의 가동 중지 시간, 유지 관리 발생 시 읽기 전용 복제본/ec2 인스턴스/복원 EBS의 확장은 수동 개입
- 보안: AWS는 OS 보안을 담당하고 우리는 SSL을 사용하여 KMS, 보안 그룹, IAM정책 설정, DB의 사용자 권한 부여를 담당
- 안정성: 다중 AZ 기능, 장애 발생 시 장애 조치
- 성능: EC2 인스턴스 유형, EBS 볼륨 유형, 읽기 전용 복제본 추가 기능에 따라 다름. 인스턴스의 스토리지 자동 확장 및 수동 확장
- 비용: 프로비저닝된 EC2 및 EBS를 기준으로 시간당 지불

## 2. Aurora
- PostgreSQL/MySQL용 호환 API
- 데이터는 3개의 AZ에 걸쳐 6개의 복제본에 보관
- 자동 복구 기능
- 다중 AZ, Auto Scaling 읽기 전용 복제본. 읽기전용 복제본은 전역 가능
- Auto Scaling 10GB ~ 128TB의 스토리지
- RDS와 동일한 보안/모니터링/유지 관리 기능
- Serverless ‒ 예측할 수 없는/간헐적인 워크로드용
- 사용 사례: RDS와 동일하지만 유지 관리가 덜함/유연성/성능 향상

## 3. RDS for SA(Solutions Architect)
- 운영: 적은 운영, Auto Scaling 스토리지
- 보안: AWS는 OS 보안을 담당하고 우리는 SSL을 사용하여 KMS, 보안 그룹, IAM 정책설정, DB의 사용자 권한 부여를 담당
- 안정성: 다중 AZ, 고가용성, RDS 이상 가능, Aurora 서버리스 옵션, Aurora 다중마스터 옵션
- 성능: 아키텍처 최적화로 인한 5배 성능(AWS 기준). 최대 15개의 읽기 전용 복제본(RDS의 경우 5개만)
- 비용:  EC2 및 스토리지 사용량을 기준으로 시간당 지불. Oracle과 같은 엔터프라이즈급 데이터베이스에 비해 비용이 낮을 수 있음

## 2. ElastiCache
- 관리형 Redis/Memcached(RDS와 유사한 제품이지만 캐시용)
- 인메모리 데이터저장소, 밀리초 미만의 지연 시간
- EC2 인스턴스 유형을 프로비저닝해야 함
- 클러스터링(Redis) 및 다중 AZ 지원, 읽기 전용 복제본(샤딩)
- IAM, Security Groups,KMS, Redis Auth를 통한 보안
- 백업 / 스냅샷 / 특정 시점 복원 기능
- CloudWatch를 통한 모니터링
- 사용 사례: 키/값 저장, 빈번한 읽기, 적은 쓰기, DB 쿼리에 대한 캐시 결과, 웹 사이트에 대한 세션 데이터 저장, SQL을 사용 불가

## 3. ElastiCache for SA(Solutions Architect)
- 운영: RDS와 동일
- 보안: OS보안을 담당하는 AWS, SSL을 사용하여 KMS, 보안 그룹, IAM 정책, 사용자(RedisAuth) 설정을 담당
- 안정성: 클러스터링, 다중 AZ
- 성능: 메모리에서 밀리초 미만의 성능, 샤딩을 위한 읽기 전용 복제본, 매우 인기 있는 캐시 옵션
- 비용: EC2 및 스토리지 사용량을 기준으로 시간당 지불

