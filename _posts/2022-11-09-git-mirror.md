---
title: "[GIT] GIT repository 복사(git repository copy_mirror)"
last_modified_at: 2022-07-09T16:01:04-04:00
categories:
  - GIT
tags:
  - update
toc: true
toc_label: "GIT"
---
git repository 를 다른 repository 로 이관 or 복사를 할떄, 단순히 소스만 복사하면 git 히스토리없이 소스만 이관되게 됩니다.   

git reposiotory 의 git history 를 포함하여 이관하는 방법을 공유합니다.   

# 1. git 미러 클론
- 이관전 git repository 를 mirror clone 합니다.
```shell
git clone --mirror {기존 git주소}
```

# 2. git url 셋팅
- 이관할 git repository 주소로 변경합니다.
- 이때, 1번에서 clone 받은 프로젝트 디렉토리로 들어가서 명령어를 수행합니다.
```shell
cd {1번에서 수행한 reposotory 디렉토리}
git remote set-url --push origin {이동할 git주소}
```

# 3. git push
- 이관할 git repository 로 push 합니다.
```shell
git push --mirror
```
