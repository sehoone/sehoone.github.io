---
title: "[SSH] SSH 파일 업로드(scp file upload)"
last_modified_at: 2022-11-16T16:01:04-04:00
categories:
  - SSH
tags:
  - update
toc: true
toc_label: "SSH"
---
파일 업로드를 위해 FTP를 사용하기 어려운 상황이라면 SSH기반의 파일 업로드 방법을 공유합니다   

# 문법
- scp -P {포트} {로컬파일명} {계정@IP}:{복사경로}

# 예시
- scp -P 22 localfile.tar ec2-user@10.240.111.222:/swhome/ 
