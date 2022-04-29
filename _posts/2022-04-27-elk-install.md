---
title: "[Elastic Stack] 엘라스틱서치/키바나 설치 for window"
last_modified_at: 2022-04-27T16:01:04-04:00
categories:
  - Elastic Stack
tags:
  - update
toc: true
toc_label: "Getting Started"
---

## Contents

1. [elasticsearch](#elasticsearch)

   - [elasticsearch 다운로드](#elasticsearch-다운로드)
   - [elasticsearch 실행](#elasticsearch-실행)

2. [kibana](#kibana)

   - [kibana 다운로드](#kibana-다운로드)
   - [kibana 실행](#kibana-실행)

## elasticsearch

엘라스틱 스택 7.xx 대 버전을 설치를 하고자 합니다.

### elasticsearch-다운로드

1. url : [https://www.elastic.co/kr/downloads/past-releases#elasticsearch](https://www.elastic.co/kr/downloads/past-releases#elasticsearch)

2. 다운로드  
   ![image](/assets/images/elasticsearch-install1.png){: width="50%" height="50%"}

### elasticsearch-실행

1. 압축풀기
2. cmd를 사용하여 경로 이동

```
cd elasticsearch-7.17.1
```

3. 실행

```
.\bin\elasticsearch.bat
```

4. 실행확인  
   윈도우의 bat파일의 경우 기본적으로 백그라운드 실행이 안되어 cmd창을 하나 더 실행하고 다음의 명령어를 입력하거나 브라우져에서 URL로 호출하여 확인해볼 수 있다

```
curl -X GET "localhost:9200/?pretty"
```

OR 브라우져에서 URL 호출

```
http://localhost:9200/
```

![image](/assets/images/elasticsearch-install2.png){: width="70%" height="70%"}

## kibana

### kibana-다운로드

1. url : [https://www.elastic.co/kr/downloads/past-releases#kibana](https://www.elastic.co/kr/downloads/past-releases#kibana)

2. 다운로드  
   ![image](/assets/images/kibana-install1.png){: width="50%" height="50%"}

### kibana-실행

1. 압축풀기
2. cmd를 사용하여 경로 이동

```
cd kibana-7.17.1
```

3. 실행

```
.\bin\kibana.bat
```

4. 실행확인  
   브라우져에서 URL로 호출하여 확인해볼 수 있다.(엘라스틱 실행 후 키바나 실행필요)

```
http://localhost:5601/
```

![image](/assets/images/kibana-install2.png){: width="70%" height="70%"}
