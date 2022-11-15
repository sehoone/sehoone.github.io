---
title: "[Tomcat] tomcat 환경변수 설정"
last_modified_at: 2022-07-15T16:01:04-04:00
categories:
  - Tomcat
tags:
  - update
toc: true
toc_label: "Tomcat"
---
Tomcat 을 사용하여 어플리케이션을 뛰울때, 환경변수가 필요할 경우 환경변수를 설정하는 방법을 공유합니다. 

테스트 환경
- OS: Red Hat Enterprise Linux 8.6
- WAS: apache tomcat 9.0.65
- application: servlet

# 1. 현상
- 설치형 tomcat 을 사용하여 어플리케이션 서버를 뛰울떄, 필요한 라이브러리가 환경변수를 가져오게끔 만들어져 있어서 해당 환경변수를 추가 필요하였습니다.
- 소스(LIB_CONF_PATH 환경변수 설정이 필요상태)
```java
public class Config {
  public static void loadProperties() {
    String confEnvPath = System.getenv("LIB_CONF_PATH");
    // ... 비지니스 로직
  }
} 
```

# 2. 환경변수 정의
## 2-1 환경변수 파일추가(linux)
- 경로: apache-tomcat-9.0.65/bin/setenv.sh
```shell
vi apache-tomcat-9.0.65/bin/setenv.sh
```
- setenv.sh 에 적용하고자하는 환경변수를 추가합니다.
```shell
export LIB_CONF_PATH=/my-application/conf/
```

## 2-2. 환경변수 파일추가(window)
- 경로: apache-tomcat-9.0.65/bin/setenv.bat
```shell
vi apache-tomcat-9.0.65/bin/setenv.bat
```
- setenv.bat 에 적용하고자하는 환경변수를 추가합니다.
```shell
set LIB_CONF_PATH=/my-application/conf/
```