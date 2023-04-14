---
title: "[git] fork merge 소스 충돌"
last_modified_at: 2023-04-14T16:01:04-04:00
categories:
  - git
tags:
  - update
toc: true
toc_label: "git"
---
fork repository 로 개발을 진행할때, 원본 repository 에 merge confict 가 발생 했을때 해결방법 공유합니다.   
요약하면 fork repository에 upstram(원본) 브랜치를 가져와서 conflict를 해결하고 머지를 합니다

# 1. upstram(원본) 브랜치 가져오기
## 1-1 remote 확인
```
git remote -v
```
## 1-2 upstram(원본)을 remote에 추가 및 확인
```
git remote add upstream "원본git"
git fetch upstream "원본git의 merge target branch"
git branch -a
```

예시
```
git remote add upstream https://github.com/sehoone/spring-mci-sample.git
git fetch upstream main
git branch -a
```

# 2. 소스 충돌 해결
1번을 진행하면 branch 를 remote에서 가져올수 있어서 merge를 해결

# 3. 원본저장소에 merge
2번에서 소스충돌을 해결했으므로 원본 저장소와 충돌없이 merge가 됩니다
