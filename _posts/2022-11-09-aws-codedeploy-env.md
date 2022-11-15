---
title: "[AWS] AWS codedeploy 환경변수 설정"
last_modified_at: 2022-07-09T16:01:04-04:00
categories:
  - AWS
tags:
  - update
toc: true
toc_label: "AWS"
---
AWS codedeploy 통해서 서버 배포를 할떄, 배포 서버 환경별로 실행 변수를 구분하기위한 방법을 공유합니다.    

테스트 환경
- OS: Red Hat Enterprise Linux 8.6
- application: spring-boot 2.6.7

# 1. 현상
## 서버 환경별 구분필요(개발/운영)
- spring-boot 는 실행할떄 아래와 같이 '-Dspring.profiles.active=dev' 에 dev or prod 구분하여 application-dev.yml or application-prod.yml 를 참조하게끔 할 수 있는데, codedeploy가 실행할떄 서버 환경별로 명령어를 변경하여 실행할 상황이 생겼습니다.   

- dev (개발계)
```yml
java -Dspring.profiles.active=dev -Dserver.port=8000 -jar my-application.jar
```

- prod (운영계)
```yml
java -Dspring.profiles.active=prod -Dserver.port=8000 -jar my-application.jar
```

# 2. 환경변수 정의
## 2-1 환경변수 추가
- os에서 /etc/profile.d/* 파일을 os 부팅시점에 실행하기떄문에 해당 경로에 shell을 추가합니다.
```shell
vi /etc/profile.d/codedeploy.sh
```

- codedeploy.sh 에 적용하고자하는 환경변수를 추가합니다.
```shell
export MY_WAS_PROFILE=dev
```

## 2-2. 환경변수 적용
- 서버 재부팅 없이 적용하기 위해서 source 를 수행합니다.
```shell
source /etc/profile
```

## 2-3. 환경변수 적용 확인
- echo 를 통해서 정의한 환경변수가 print되는지 확인합니다.   
```shell
echo $MY_WAS_PROFILE
```

## 2.4 codedeploy 서비스 재시작
- code deploy 환경변수 적용 관련 다른 포스트들에서는 서비스 재시작은 안하던데, 저의 경우는 codedeploy agent 서비스를 재시작을 해야 적용되었습니다.
```shell
sudo service codedeploy-agent restart
```

# 3. codedeploy appspec 정의
- aws codedeploy에 appspec.yml, shell, jar 을 tar로 묶어서 전달합니다
- appspec.yml   
```yaml
version: 0.0
os: linux
files:
  - source: /jar
    destination: /swhome/my-server-home/

permissions:
  - object: /
    pattern: "**"
    owner: wasadmin
    group: wasadmin

hooks:
  ApplicationStop:
    - location: /bin/my-was-shutdown.sh
      timeout: 60
      runas: wasadmin
  ApplicationStart:
    - location: /bin/my-was-startup.sh
      timeout: 60
      runas: wasadmin
```

- my-was-shutdown.sh   
```shell
if [ $(ps -ef|grep my-application |grep -v grep|wc -l) -gt 0]; then pkill -ef -15 my-application;fi
```

- my-was-startup.sh   
```shell
java -Dspring.profiles.active=$MY_WAS_PROFILE -Dserver.port=8000 -jar /swhome/my-server-home/my-application.jar
```