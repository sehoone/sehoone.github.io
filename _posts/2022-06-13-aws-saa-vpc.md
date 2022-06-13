---
title: "[AWS-SAA] VPC"
last_modified_at: 2022-06-13T16:01:04-04:00
categories:
  - AWS-SAA
tags:
  - update
toc: true
toc_label: "AWS-SAA"
---

## 1. VPC 란
- VPC(Virtual Private Cloud) 사용자가 정의한 가상 네트워크로 AWS 리소스
- 가상 네트워크는 AWS의 확장 가능한 인프라를 사용한다는 이점과 함께 고객의 자체 데이터 센터에서 운영하는 기존 네트워크와 매우 유사

## 2. VPC 요약
- **VPC**: 사용자의 AWS 계정 전용 가상 네트워크
- **subnet**: VPC의 IP 주소 범위. AZ에 연결되어 CIDR을 정의
- **CIDR**: 인터넷 프로토콜 주소 할당 및 라우팅 집계 방법. IP의 범위를 나타냄. ex. 10.0.0.0/32 = 10.0.0.0, 10.0.0.0/24 = 10.0.0.0 ~ 10.0.0.255
- **Internet Gateway**: VPC의 리소스와 인터넷 간의 통신을 활성화하기 위해 VPC에 연결하는 게이트웨이. VPC 수준에서 IPv4 및 IPv6 인터넷 액세스 제공
- **Route Tables**: 네트워크 트래픽을 전달할 위치를 결정하는 데 사용하는 라우팅이라는 이름의 규칙 집합. 서브넷에서 연결, VPC엔드포인트, IGW, VPC 피어링으로의 경로를 추가하려면 편집 필요
- **Bastion Host**: SSH에 대한 퍼블릭 EC2 인스턴스. 프라이빗 서브넷의 EC2 인스턴스에 대한 SSH 연결
- **NAT Instances**: 프라이빗 서브넷의 인스턴스가 인터넷, 다른 VPC 또는 온프레미스 네트워크에 연결되도록 허용하는 퍼블릭 서브넷의 EC2 인스턴스. 프라이빗 서브넷의 EC2 인스턴스에 대한 인터넷 액세스를 제공. 퍼블릭 서브넷에서 설정하려면 소스/대상 확인 플래그 비활성화 필요
- **NAT Gateway**: 프라이빗 서브넷의 EC2 인스턴스가 인터넷, 다른 VPC 또는 온프레미스 네트워크에 연결되도록 허용하는 관리형 AWS 서비스. AWS에서 관리하며 프라이빗 EC2 인스턴스에 대한 확장 가능한 인터넷 액세스 제공. IPv4 전용
- **Private DNS + Route 53**: DNS 확인 + DNS 호스트 이름(VPC) 활성화
- **NACL**: 서브넷에서 들어오고 나가는 트래픽을 제어하기 위해 방화벽 역할을 수행하는 VPC에 대한 선택적 보안 계층. 상태 비저장(stateless). 인바운드 및 아웃바운드에 대한 서브넷 규칙, Ephemeral포트 사용
- **Security Groups**: 상태 저장(stateful). EC2 인스턴스 수준에서 작동
- **Reachability Analyzer**: AWS 간의 네트워크 연결 테스트 수행
- **VPC Peering**: 비중첩 CIDR, 비전이성을 사용하여 두 VPC 연결
- **VPC Endpoints**:  private access AWS Services 제공(S3, DynamoDB, CloudFormation, SSM) within a VPC
- **VPC Flow Logs**:  VPC/서브넷/ENI 수준에서 ACCEPT 및 REJECT 트래픽, 공격 식별에 도움, Athena 또는 CloudWatch Logs를 사용하여 분석
- **Site-to-Site VPN**:  DC(direct connect)에 고객 게이트웨이, VPC에 가상 사설 게이트웨이 설정. VPN을 사용하여 공용 인터넷을 통한 사이트 간 연결
- **AWS VPN CloudHub**:  hub-and-spoke VPN 모델로 사이트와 연결
- **Direct Connect**:  VPC에서 가상 프라이빗 게이트웨이를 설정하고 직접 프라이빗 연결을 설정
- **Direct Connect Gateway**:  서로 다른 AZ에 대한 많은 VPC 다이렉트 연결
- **AWS PrivateLink / VPC Endpoint Services**: VPC와 고객 VPC를 비공개로 연결. VPC 피어링, 퍼블릭 인터넷, NAT 게이트웨이, 라우팅 테이블이 필요하지 않음. Network Load Balancer 및 ENI와 함께 사용
- **ClassicLink**:  EC2-Classic EC2 인스턴스를 VPC에 비공개로 연결
- **Transit Gateway**:  VPC, VPN 및 DX에 대한 전이적 피어링 연결
- **Traffic Mirroring**:  분석을 위해 ENI에서 네트워크 트래픽 복사
- **Egress-only Internet Gateway**:  송신 전용 인터넷 게이트웨이. NAT Gateway와 비슷하지만 IPv6만 사용가능