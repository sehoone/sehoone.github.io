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

# 1. maven
## 1-1. maven repository 생성
- admin config(왼쪽상단 톱니바퀴) > Repository(왼쪽 네비게이션) > Create repository  
![image](/assets/images/nexus/nexus_maven_repo1.png){: #magnific width="100%" height="100%"}

- maven2(hosted) 선택 > repository 정보 입력. 각각 의미 있는 설정값이지만 이글의 목적과는 관련없으니 name(ex. test-maven-hosted), blob store(default) 정보만 입력   
![image](/assets/images/nexus/nexus_maven_repo2.png){: #magnific width="100%" height="100%"}

## 1-2. 일괄 업로드 shell 실행
- **로컬 레파지토리 디렉토리에서 shell을 실행하여 일괄 upload. 폐쇠망 환경에서 디펜던시 업로드를 위해서는 로컬 레파지토리와 shell을 사용하여 일괄업로드**
- https://github.com/sehoone/nexus-utils/tree/main/bulk-import-lib
- 위의 github 레파지토리에서 'mavenimport.sh' 을 로컬 maven repository 에 복사(ex. C:/Users/{로컬pc사용자명}/.m2/repository/)
- 실행: ./mavenimport.sh -u {nexusId} -p {password} -r {respositoryUrl} (ex. ./mavenimport.sh -u admin -p open1404! -r http://localhost:5001/repository/test-maven-hosted/)   
![image](/assets/images/nexus/nexus_maven_repo3.png){: #magnific width="100%" height="100%"}

# 2. npm
## 2-1 npm blob 생성
- admin config(왼쪽상단 톱니바퀴) > Blob Stores(왼쪽 네비게이션) > Create Blob Store > type file 선택 > 생성
![image](/assets/images/nexus/nexus_npm_repo1.png){: #magnific width="100%" height="100%"}

## 2-2 npm repository 생성
- admin config(왼쪽상단 톱니바퀴) > Repository(왼쪽 네비게이션) > Create repository  
![image](/assets/images/nexus/nexus_maven_repo1.png){: #magnific width="100%" height="100%"}

- npm(hosted) 선택 > repository 정보 입력. name(ex. test-npm-hosted), blob store(default) 정보 입력   
![image](/assets/images/nexus/nexus_npm_repo2.png){: #magnific width="100%" height="100%"}

## 2-3 npm dependency pack
- node_module을 npm dependency 업로드를 위해서 tgz파일로 packing. npm dependency는 1개의 라이브러리 내부에서 dependency를 가져오는 경우가 있어서 일괄 tgz로 만드는 작업이 필요합니다.
- https://github.com/sehoone/nexus-utils/tree/main/node-module-pack-all
- 위의 레파지토리를 clone 받아서 아래의 경로 수정하여 'yarn run start' 를 실행
```javascript
const NODE_MODULES = "node_modules";
const BASE_PATH = "/dev/temp";  // packing 하려는 원본경로(BASE_PATH + NODE_MODULES 로 경로값이 합쳐집니다.)
const TARGET_PATH = "/dev/temp/module_test2"; // packing한 tgz파일이 생성될 target경로
```
- 실행: yarn run start

## 2-4 일괄 업로드 shell 실행
- **packing 디렉토리에서 shell을 실행하여 일괄 upload. 폐쇠망 환경에서 디펜던시 업로드를 위해서는 packing디렉토리와 shell을 사용하여 일괄업로드**
- https://github.com/sehoone/nexus-utils/tree/main/bulk-import-lib
- 위의 github 레파지토리에서 'npmimport.sh' 을 2-3에서 생성한 TARGET_PATH 에 복사(ex. C:/dev/temp/module_test2/) 후 아래의 실행1/2 스크립트를 순서대로 입력
- 실행1: npm login --registry http://localhost:5001/repository/test-npm-hosted/
- 실행2: ./npmimport.sh -r http://localhost:5001/repository/test-npm-hosted/