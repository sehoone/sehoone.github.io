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
private nexus repository를 통해서 dependency를 가져올때, 인터넷이 되는 환경에서는 proxy repository를 사용하면 간단히 dependency를 가져올 수 있다.   
   
하지만 망분리된 오프라인 환경에서 nexus를 사용하면 dependency를 오프라인 환경에 반입해서 nexus에 업로드 해야한다.   

이때, Spring + Gradle 프레임워크에서 private nexus를 참조하게끔 하는 방법을 공유합니다.   

# 1. build.gradle   
- dependency 를 아래의 nexus를 참조하여 가져오게끔 합니다.   
build.gradle   
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
settings.gradle
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

