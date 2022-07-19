---
title: "[NEXUS] Nexus Repository maven/npm dependency bulk upload (for offline 폐쇠망 환경)"
last_modified_at: 2022-07-19T16:01:04-04:00
categories:
  - NEXUS
tags:
  - update
toc: true
toc_label: "NEXUS"
---
   
private nexus repository를 통해서 dependency를 가져올때, 인터넷이 되는 환경에서는 proxy repository를 사용하면 간단히 dependency를 가져올 수 있다.   
   
하지만 망분리된 오프라인 환경에서 nexus를 사용하면 dependency를 오프라인 환경에 반입해서 nexus에 업로드 해야한다.   
이 글은 망분리된 환경에 dependency업로드를 위해 파일준비 및 일괄 업로드 방법을 공유하려 합니다.   


## 1-1. maven repository 생성
