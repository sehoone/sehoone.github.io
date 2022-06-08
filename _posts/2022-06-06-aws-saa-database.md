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

## 2-1. RDS
- PostgreSQL / MySQL / Oracle / SQL Server
- EC2 인스턴스 및 EBS 볼륨 유형 및 크기를 프로비저닝 필요
- 읽기 전용 복제본 및 다중 AZ 지원
- IAM, Security Groups, KMS , SSL 보안지원
- CloudWatch를 통한 모니터링
- 사용 사례: 관계형 데이터 세트(RDBMS/OLTP) 저장, SQL 쿼리 수행, 트랜잭션 삽입/업데이트/삭제 가능

## 2-2. RDS for SA(Solutions Architect)
- 운영: 장애 조치 발생 시 약간의 가동 중지 시간, 유지 관리 발생 시 읽기 전용 복제본/ec2 인스턴스/복원 EBS의 확장은 수동 개입
- 보안: AWS는 OS 보안을 담당하고 우리는 SSL을 사용하여 KMS, 보안 그룹, IAM정책 설정, DB의 사용자 권한 부여를 담당
- 안정성: 다중 AZ 기능, 장애 발생 시 장애 조치
- 성능: EC2 인스턴스 유형, EBS 볼륨 유형, 읽기 전용 복제본 추가 기능에 따라 다름. 인스턴스의 스토리지 자동 확장 및 수동 확장
- 비용: 프로비저닝된 EC2 및 EBS를 기준으로 시간당 지불

## 3-1. Aurora
- PostgreSQL/MySQL용 호환 API
- 데이터는 3개의 AZ에 걸쳐 6개의 복제본에 보관
- 자동 복구 기능
- 다중 AZ, Auto Scaling 읽기 전용 복제본. 읽기전용 복제본은 전역 가능
- Auto Scaling 10GB ~ 128TB의 스토리지
- RDS와 동일한 보안/모니터링/유지 관리 기능
- Serverless ‒ 예측할 수 없는/간헐적인 워크로드용
- 사용 사례: RDS와 동일하지만 유지 관리가 덜함/유연성/성능 향상

## 3-2. RDS for SA(Solutions Architect)
- 운영: 적은 운영, Auto Scaling 스토리지
- 보안: AWS는 OS 보안을 담당하고 우리는 SSL을 사용하여 KMS, 보안 그룹, IAM 정책설정, DB의 사용자 권한 부여를 담당
- 안정성: 다중 AZ, 고가용성, RDS 이상 가능, Aurora 서버리스 옵션, Aurora 다중마스터 옵션
- 성능: 아키텍처 최적화로 인한 5배 성능(AWS 기준). 최대 15개의 읽기 전용 복제본(RDS의 경우 5개만)
- 비용:  EC2 및 스토리지 사용량을 기준으로 시간당 지불. Oracle과 같은 엔터프라이즈급 데이터베이스에 비해 비용이 낮을 수 있음

## 4-1. ElastiCache
- 관리형 Redis/Memcached(RDS와 유사한 제품이지만 캐시용)
- 인메모리 데이터저장소, 밀리초 미만의 지연 시간
- EC2 인스턴스 유형을 프로비저닝해야 함
- 클러스터링(Redis) 및 다중 AZ 지원, 읽기 전용 복제본(샤딩)
- IAM, Security Groups,KMS, Redis Auth를 통한 보안
- 백업 / 스냅샷 / 특정 시점 복원 기능
- CloudWatch를 통한 모니터링
- 사용 사례: 키/값 저장, 빈번한 읽기, 적은 쓰기, DB 쿼리에 대한 캐시 결과, 웹 사이트에 대한 세션 데이터 저장, SQL을 사용 불가

## 4-2. ElastiCache for SA(Solutions Architect)
- 운영: RDS와 동일
- 보안: OS보안을 담당하는 AWS, SSL을 사용하여 KMS, 보안 그룹, IAM 정책, 사용자(RedisAuth) 설정을 담당
- 안정성: 클러스터링, 다중 AZ
- 성능: 메모리에서 밀리초 미만의 성능, 샤딩을 위한 읽기 전용 복제본, 매우 인기 있는 캐시 옵션
- 비용: EC2 및 스토리지 사용량을 기준으로 시간당 지불

## 5-1. DynamoDB
- AWS 독점 기술, 관리형 NoSQL 데이터베이스
- 서버리스, 프로비저닝된 용량, 자동 크기 조정
- 키/값 저장소로 ElastiCache를 대체할 수 있음(예: 세션 데이터 저장)
- 기본적으로 고가용성, 다중 AZ, 읽기 및 쓰기가 분리됨, 읽기 캐시용 DAX
- 보안, 인증 및 권한 부여가 IAM을 통해 수행
- 백업/복원 기능, 전역 테이블 기능
- CloudWatch를 통한 모니터링
- 사용 사례: 서버리스 애플리케이션 개발(작은 문서 100s KB), 분산 서버리스 캐시, 사용 가능한 SQL 쿼리 언어 없음

## 5-2. DynamoDB for SA
- 운영: 작업 불필요, 자동 확장 기능, 서버리스
- 보안:  IAM 정책, KMS 암호화, SSL전송을 통한 완전한 보안
- 안정성: 다중 AZ, 백업
- 성능: 한 자릿수 밀리초 성능, 읽기 캐싱을 위한 DAX
- 비용: 프로비저닝된 용량 및 스토리지 사용량에 따라 지불(용량을 미리 추측할 필요 없음‒ Auto Scaling 사용 가능)

## 6-1. S3
- 객체용 키/값 저장소
- 큰 객체에는 적합하지만 작은 객체에는 적합하지 않음
- 서버리스, 무한 확장, 최대 객체 크기는 5TB
- 버전 관리, 암호화, 교차 지역 복제
- SSE-S3, SSE-KMS, SSE-C, 클라이언트 측 암호화, 전송 중인 SSL 통한 보안
- 강력한 일관성. S3 Standard, S3 IA, S3 One Zone IA, 백업용 Glacier
- 사용 사례: 정적 파일, 대용량 파일용 키 값 저장소, 웹 사이트 호스팅

## 6-2. S3 for SA
- 운영: 작업 필요 없음
- 보안:  IAM, 버킷정책, ACL, 암호화(서버/클라이언트), SSL
- 안정성: 99.999999999% 내구성/99.99% 가용성, 다중 AZ, CRR
- 성능: 읽기/쓰기 초당 수천 번으로 확장가능. 대용량 파일에 대한 전송 가속 기능
- 비용: 스토리지 사용량, 네트워크 비용, 요청 수당 지불

## 7-1. Athena
- SQL 기능이 있는 완전한 서버리스 데이터베이스
- S3에서 데이터 쿼리에 사용
- 쿼리당 지불
- 결과를 다시 S3로 출력 
- IAM을 통해 보호
- 사용 사례: 일회성 SQL 쿼리, S3에 대한 서버리스 쿼리, 로그 분석

## 7-2. Athena for SA
- 운영: 작업 불필요, 서버리스
- 보안: IAM + S3 보안
- 안정성: 관리형 서비스, Presto 엔진 사용, 고가용성
- 성능: 데이터 크기에 따라 확장되는 쿼리 
- 비용: 쿼리당/스캔된 데이터의TB당 지불, 서버리스

## 8-1. Redshift
- PostgreSQL을 기반으로 하지만 OLTP에는 사용되지 않고, OLAP ‒ 온라인 분석 처리(분석 및 데이터 웨어하우징) 
- 다른 데이터 웨어하우스보다 10배 더 나은 성능, PB 단위의 데이터 확장 
- 데이터의 열 기반 스토리지(행 기반 대신) ) 
- MPP(Massively Parallel Query Execution) 
- 프로비저닝된 인스턴스에 따라 사용한 만큼 지불 
- 쿼리를 수행하기 위한 SQL 인터페이스가 있음 
- AWS Quicksight 또는Tableau와 같은 BI 도구가 이에 통합됨
- 데이터는 S3, DynamoDB, DMS, 기타 DB에서 로드
- 백업 및 복원, 보안 VPC/IAM/KMS, 모니터링
- Redshift Enhanced VPC Routing: COPY/UNLOAD가 VPC를 통과
- 다중 AZ 모드 없음
- 사용사례: Analytics / BI / Data Warehouse   
![image](/assets/images/aws-database/redshift.png){: width="100%" height="100%"}

## 8-2. Redshift for SA(Solutions Architect)
- 운영: RDS와 유사 
- 보안:IAM, VPC, KMS, SSL(RDS와 유사) 
- 안정성: 자동 복구 기능, 교차 리전 스냅샷 복사 
- 성능: 다른 데이터 웨어하우징, 압축에 비해 10배 성능, 압축 
- 비용: 프로비저닝된 노드당 지불, 다른 웨어하우스에 비해 비용의 1/10 • Athena에 비해: 인덱스 덕분에 더 빠른 쿼리/조인/집합

## 9-1 AWS Glue
- 관리형 ETL(추출, 변환 및 로드) 서비스 
- 분석을 위한 데이터 준비 및 변환에 유용 
- 완전한 서버리스 서비스   
![image](/assets/images/aws-database/aws-glue.png){: width="100%" height="100%"}

## 10-1. Neptune
- 완전 관리형 그래프 데이터베이스
- 높은 관계데이터에 그래프 사용. ex. 소셜네트워킹
- 최대 15개의 읽기 전용 복제본으로 3개의 AZ에서 고가용성
- KMS 암호화 지원
- 특정시점 복원가능, S3로 지속 백업   
![image](/assets/images/aws-database/neptune.png){: width="80%" height="80%"}

## 10-2. Neptune for SA(Solutions Architect)
- 운영: RDS와 유사 
- 보안: IAM,VPC, KMS, SSL(RDS와 유사) + IAM 인증 
- 안정성: 다중 AZ, 클러스터링 
- 성능: 성능 향상을 위한 그래프, 클러스터링에 가장 적합 
- 비용: 노드당 지불 프로비저닝됨(RDS와 유사)

## 11-1. ElasticSearch
- 비정형 데이터 검색 및 빅데이터 에 사용
- 인스턴스 클러스터를 프로비저닝가능
- Cognito 및 IAM, KMS 암호화, SSL 및VPC를 통한 보안
- Kibana(시각화) 및 Logstash(로그 수집)와 함께 제공
- 사용사례: 검색/비정형 데이터 인덱싱

## 8-2. Redshift for SA(Solutions Architect)
- 운영: RDS와 유사 
- 보안:Cognito, IAM, VPC, KMS, SSL 
- 안정성: 다중 AZ, 클러스터링 
- 성능: ElasticSearch 프로젝트(오픈 소스) 기반, 페타바이트 규모
- 비용: 프로비저닝된 노드당 지불(유사 RDS)
