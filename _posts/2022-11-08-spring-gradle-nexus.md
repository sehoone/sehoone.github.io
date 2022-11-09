---
title: "[Spring_Gradle] private nexus 참조(for offline 폐쇠망 환경)"
last_modified_at: 2022-07-08T16:01:04-04:00
categories:
  - Spring
tags:
  - update
toc: true
toc_label: "Spring"
---
인터넷이 되는 환경에서는 meven repository(mavenCentral()) 에서 dependency를 가져올 수 있다.   
하지만 망분리된 오프라인 환경에서는 private nexus를 참조해서 dependency를 가져와야한다.   

이때, Spring + Gradle 프레임워크에서 private nexus를 참조하게끔 하는 방법을 공유합니다.   

# 1. build.gradle   
- dependency 를 아래의 nexus를 참조하여 가져오게끔 합니다.   
![image](/assets/images/spring-nexus/spring_nexus1.png){: width="30%" height="30%"}   

- build.gradle   
```gradle
repositories {
	// mavenCentral()
	maven {
		allowInsecureProtocol true	// privte nexus가 https를 사용안할경우 추가
		url "http://10.240.100.1:2000/repository/my-maven-hosted/"	// privete nexus url
	}
}
```

# 2. settings.gradle   
- plugin을 사용할경우, 아래의 nexus를 참조하여 가져오게끔 합니다.   
![image](/assets/images/spring-nexus\spring_nexus2.png){: width="30%" height="30%"}  

- settings.gradle
```gradle
pluginManagement {
    repositories {
        maven {
            allowInsecureProtocol true	// privte nexus가 https를 사용안할경우 추가
            url "http://10.240.100.1:2000/repository/my-maven-hosted/"	// privete nexus url
        }
    }
}
```
소스에서 plugin 사용한경우 예시(build.gradle)    
```gradle
plugins {
	id 'org.springframework.boot' version '2.7.1'
	id 'io.spring.dependency-management' version '1.0.11.RELEASE'
	id 'java'
}
```

# 3. vscode gradle (vscode를 사용할 경우)
- local build를 할때, gradle bin을 참조하는데 이떄도 외부 url을 참조하게되는데 이 경우에는 bin파일 자체를 가져와서 환경을 만들어주면 offline 환경에서 사용가능합니다
## 3-1 환경 확인
- {project}\gradle\wrapper\gradle-wrapper.properties 을 보면 아래와 같이 외부 주소를 참조하고 있습니다   
- distributionUrl 에 로컬 디렉토리를 넣어서 해봤는데, 윈도우 환경에서는 경로정의가 잘 안되는 듯합니다.(해보다가 실패..) 그래서 그냥 'wrapper/dists' 를 가져와서 셋팅하였습니다.    
```xml
distributionBase=GRADLE_USER_HOME
distributionPath=wrapper/dists
distributionUrl=https\://services.gradle.org/distributions/gradle-7.4.1-bin.zip
zipStoreBase=GRADLE_USER_HOME
zipStorePath=wrapper/dists
```

## 3-2 gradle bin local pc에 복사
- 온라인 환경에서 아래의 gradle bin을 local pc에 복사
- 경로: 'C:\Users\{사용자}\.gradle\wrapper'  .gradle 경로 전체를 가져와도 되긴한데 해당 경로에 dependency 들이 모두 있어서 용량이 많이 나갈 수 있습니다. 그래서 wrapper 만 가져오고 local pc에 .gradle\wrapper 경로와 동일하게 위치 시킵니다    