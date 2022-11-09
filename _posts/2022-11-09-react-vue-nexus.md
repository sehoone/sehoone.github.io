---
title: "[React_Vue] private nexus 참조(for offline 폐쇠망 환경)"
last_modified_at: 2022-07-08T16:01:04-04:00
categories:
  - React_Vue
tags:
  - update
toc: true
toc_label: "React_Vue"
---
인터넷이 되는 환경에서는 public npm repository 에서 dependency를 가져올 수 있다.   
하지만 망분리된 오프라인 환경에서는 private nexus를 참조해서 dependency를 가져와야한다.   

이때, React or Vue 프레임워크에서 private nexus를 참조하게끔 하는 방법을 공유합니다.   

# 1. .npmrc 추가   
- .npmrc 파일을 추가하여 dependency 를 private nexus를 참조하여 가져오게끔 합니다.   
![image](/assets/images/react-nexus/react_nexus1.png){: width="30%" height="30%"}  

- .npmrc  
```yml
registry=http://10.240.100.1:2000/repository/private-npm-hosted/  // private nexus url
```

- .yarnrc 를 사용하는 경우, .yarnrc에서도 설정 가능합니다
```yml
registry "http://10.240.100.1:2000/repository/private-npm-hosted/"  // private nexus url
```

# etc. 특정 패키지만 registry 참조
- 특정 패키지만 registry 참조를 하고 싶다면 아래와 같이 패키지명과 같이 url을 작성합니다.

- .npmrc  
```yml
registry=http://10.240.100.1:2000/repository/private-npm-hosted/  // private nexus url
@mypackageName:registry=https://myserver.com/repository/npm-releases/   // specific package private nexus url
```

- .yarnrc 를 사용하는 경우, .yarnrc에서도 설정 가능합니다
```yml
registry "http://10.240.100.1:2000/repository/private-npm-hosted/"  // private nexus url
"@mypackageName:registry" "https://myserver.com/repository/npm-releases/"    // specific package private 
```
