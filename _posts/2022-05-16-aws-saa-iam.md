---
title: "[AWS-SAA] IAM"
last_modified_at: 2022-05-16T16:01:04-04:00
categories:
  - AWS-SAA
tags:
  - update
toc: true
toc_label: "AWS-SAA"
---

## 1. IAM 이란
- ID 및 액세스 접근 권한을 관리

## 2. IAM 특징
- 기본적으로 생성된 루트 계정은 사용하거나 공유해서는 안됨(루트 계정이 아닌 IAM을 통해 계정을 생성해서 사용)
- 사용자는 조직내의 사람이며 그룹화 할 수 있다
- 그룹에는 다른 그룹이 아닌 사용자만 포함
- 사용자는 꼭 그룹에 속하지 않을 수 있고, N개의 그룹에 속할 수 있다

## 3. IAM 권한
- 사용자 또는 그룹의 정책이라는 JSON 문서로 관리가능
- AWS에서는 최소 권한 원칙을 권장함(사용자가 필요로 하는것보다 더 많은 권한을 부여하지 않음)

## 4. IAM 권한(정책) 구조
- Version: 정책 언어 버젼(항상 2012-10-17으로 고정되어 있음)
- Id: 정책에 대한 식별자(선택사항)
- Statement: 하나 이상의 개별 문(필수)
- Sid: 명령문에 대한 식별자(선택사항)
- Effect: 명령문이 액세스를 허용/거부 하는지 여부
- Principal: 이 정책이 적용된 계정/사용자/역할
- Action: 정책이 허용하거나 거부하는 작업목록
- Resource: 작업이 적용된 리소스 목록
- 예시
```json
{
    "Version": "2012-10-17",
    "Id": "AWSDirectConnectReadOnlyAccess",
    "Statement": [
        {
            "Effect": "Allow",
            "Principal": {
              "AWS": ["arn:aws:iam:123456:root"]
            },
            "Action": [
                "directconnect:Describe*",
                "directconnect:List*",
                "ec2:DescribeVpnGateways",
                "ec2:DescribeTransitGateways"
            ],
            "Resource": "*"
        }
    ]
}
```
## 5. 다단계 인증 - MFA
- MFA: Multi-factor authentication. 
- 사용자 이름과 암호 위에 추가 보호 계층을 추가하는 인증방법
- 가상 MFA 디바이스(구글 OTP, AWS AUTH), U2F, 하드웨어 열쇠고리

## 6. IAM 지침 및 모범사례
- AWS 계정 설정을 제외하고 루트 계정을 사용X
- 1인 1계정
- 사용자를 그룹에 할당하고 그룹에 권한 할당
- 강력한 암호 정책 생성(ex. 대소문자/특수문자 포함)
- MFA
- AWS 서비스에 권한을 부여하기 위한 역할 생성 및 사용
- 프로그래밍 방식 액세스(CLI/SDK)에 액세스키 사용
- IAM 자격 증명 보고서로 계정 권한 감사