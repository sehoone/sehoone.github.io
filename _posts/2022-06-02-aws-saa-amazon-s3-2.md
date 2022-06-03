---
title: "[AWS-SAA] Amazon S3(2)"
last_modified_at: 2022-06-02T16:01:04-04:00
categories:
  - AWS-SAA
tags:
  - update
toc: true
toc_label: "AWS-SAA"
---

## 9. Amazon S3 스토리지 클래스
- **Standard**: 기본 스토리지 클래스. 객체를 업로드할 때 스토리지 클래스를 지정하지 않으면 Amazon S3가 S3 Standard 스토리지 클래스를 할당. 성능에 민감한 사용 사례(밀리초 액세스 시간을 필요로 하는 사례)와 자주 액세스되는 데이터에 사용. ex. 빅데이터분식, 모바일/게임 어플리케이션, 컨텐츠 배포
- **Standard-Infrequent Access**: 객체 데이터를 지리적으로 분리된 여러 개의 가용 영역(AZ)에 중복되게 저장(S3 Standard 스토리지 클래스와 유사). S3 Standard-IA 객체는 가용 영역의 손실에 대한 복원력이 있어서 이 스토리지 클래스는 S3 One Zone-IA 클래스보다 뛰어난 가용성 및 복원성을 제공.
- **One Zone-Infrequent Access**: 객체 데이터를 하나의 가용 영역에만 저장하므로 S3 Standard-IA보다 더 저렴. 그러나 데이터는 지진 및 홍수와 같은 재해에 의한 가용 영역의 물리적 손실에 대해서는 복원성이 없음.   
- **Glacier Instant Retrieva**: 거의 액세스하지 않고 밀리초 단위로 검색해야 하는 데이터를 아카이브하는 데 사용. S3 Standard-IA 스토리지 클래스와 동일한 대기 시간 및 처리량 성능으로 S3 Standard-IA 스토리지 클래스보다 비용이 저렴. S3 Glacier Instant Retrieval은 S3 Standard-IA보다 데이터 액세스 비용이 더 높음.   
- **Glacier Flexible Retrieva**: 분 단위로 데이터의 일부를 검색해야 하는 아카이브에 사용. S3 Glacier Flexible Retrieval 스토리지 클래스에 저장된 데이터는 최소 스토리지 기간이 90일이며 신속 검색을 사용하여 최소 1~5분 이내에 액세스할 수 있음
- **Glacier Deep Archive**: 거의 액세스할 필요가 없는 데이터를 보관할 때 사용. S3 Glacier Deep Archive 스토리지 클래스에 저장된 데이터의 최소 스토리지 기간은 180일이고 기본 검색 시간은 12시간. 가장 저렴한 스토리지 옵션   
- **Intelligent Tierin**: 성능 영향 또는 운영 오버헤드 없이 가장 비용 효과적인 액세스 계층으로 데이터를 자동으로 이동하여 스토리지 비용을 최적화하기 위해 설계된 Amazon S3 스토리지 클래스. 변경되는 또는 알 수 없는 액세스 패턴으로 데이터를 자동으로 최적화함

## 10. Amazon S3 스토리지 클래스간 이동
- 스토리지 클래스간에 개체를 전환할 수 있음
- 자주 접근하지 않는 객체의 경우 STANDARD_IA로 이동 > 자주 접근하지 않는 객체의 경우 STANDARD_IA or DEEP_ARCHIVE 로 이동
- 수명 주기 구성을 사용하여 객체 이동을 자동화 할 수 있다.
![image](/assets/images/amzon-s3/storage-life-cycle.png){: width="80%" height="80%"}
