---
title: "[AWS-SAA] EC2 Storage"
last_modified_at: 2022-06-06T16:01:04-04:00
categories:
  - AWS-SAA
tags:
  - update
toc: true
toc_label: "AWS-SAA"
---

## 1. EBS 란
- EBS(Elastic Block Store) 볼륨 은 인스턴스가 실행되는 동안 연결할 수 있는 네트워크 드라이브
- 인스턴스가 종료된 후에도 데이터를 유지
- 한 번에 하나의 인스턴스에만 마운트
- 특정 가용성 영역 에 바인딩
![image](/assets/images/ec2-storage/ebs.png){: width="80%" height="80%"}

## 2. EBS 볼륨 유형
- gp2 / gp3(SSD): 다양한 워크로드에 대해 가격과 성능의 균형을 유지하는 범용 SSD 볼륨
- io1 / io2(SSD): 미션 크리티컬한 저지연 또는 고처리량 워크로드를 위한 고성능 SSD 볼륨   
![image](/assets/images/ec2-storage/ebs-type1.png){: width="100%" height="100%"}
- st1(HDD): 자주 액세스하고 처리량이 많은 워크로드를 위해 설계된 저렴한 HDD 볼륨
- sc1(HDD): 액세스 빈도가 낮은 워크로드용으로 설계된 가장 저렴한 HDD 볼륨   
![image](/assets/images/ec2-storage/ebs-type2.png){: width="100%" height="100%"}


## 3. EFS 란
- EFS(Elastic File System) 은 여러 EC2에 탑재할 수 있는 관리형 NFS(네트워크 파일 시스템)
- 다중 AZ에서 EC2 인스턴스와 작동
- 보안 그룹을 사용하여 EFS에 대한 액세스 제어
- 콘텐츠 관리, 웹 서비스, 데이터 공유 에 주로 사용
![image](/assets/images/ec2-storage/efs.png){: width="80%" height="80%"}

## 4. EBS vs EFS vs S3
- ||EFS|EBS|S3|
|:---:|:---:|:---:|:---:|
| 작업별 지연시간 | 낮음, 일관성 |낮음, 혼합 요청 유형의 경우 및 CloudFront와의 통합 | 가장 낮음, 일관성 |
|처리량 규모|초당 GB|초당 GB|초당 1GB|
|데이터 가용성/내구성|여러 AZ에 중복 저장|여러 AZ에 중복 저장|단일 AZ에 중복 저장|
|엑세스|여러 AZ에서 동시에 수천 개의 EC2 인스턴스 또는 온프레미스 서버|웹을 통한 1-100만 연결|단일 AZ의 단일 EC2 인스턴스|
|사용 사례|웹 서비스 및 콘텐츠 관리, 엔터프라이즈 애플리케이션, 미디어 및 엔터테인먼트, 홈 디렉토리, 데이터베이스 백업, 개발자 도구, 컨테이너 스토리지, 빅 데이터 분석|웹 서비스 및 콘텐츠 관리, 미디어 및 엔터테인먼트, 백업, 빅 데이터 분석, 데이터 레이크|부트 볼륨, 트랜잭션 및 NoSQL 데이터베이스, 데이터 웨어하우징 및 ETL|
{:.mbtablestyle}
  