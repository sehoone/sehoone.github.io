---
title: "[etc] udemy 강의 한글자막 적용방법(한글자막 미지원 강의)"
last_modified_at: 2025-05-25T16:01:04-04:00
categories:
  - etc
tags:
  - update
toc: true
toc_label: "etc"
---
현재기준(2025/05/25)으로 udemy 한글자막 크롬 플러그인이 동작을 안해서 찾아보니 스크립트를 사용하는 방법도 공유가 되어있는데, 
스크립트도 동작안하길래 스크립를 수정해서 공유합니다

## 1. udemy 강의 영상 오른쪽하단에 자막설정 > 동영상 아래표시 선택
![image](/assets/images/etc-udemy/udemy-setting1.png){: width="70%" height="70%"}  

## 2. udemy 강의 영상에서 대본 표시 선택
![image](/assets/images/etc-udemy/udemy-setting2.png){: width="70%" height="70%"}  

## 3. 화면 마우스 오른쪽 버튼 > 한국어 번역 선택(크롬 한글번역)
![image](/assets/images/etc-udemy/udemy-setting3.png){: width="70%" height="70%"}  

## 4. 크롬 개발자 모드 > console 탭에서 아래의 스크립트를 붙여넣기
```javascript
let lastText = '';
function check() {
    const toEl = document.querySelector('.well--container--afdWD span');
    const fromEl = document.querySelector('p[data-purpose="transcript-cue-active"] span');
    if(!toEl || !fromEl) return;
    let currentText = fromEl.innerText;
    if (lastText !== currentText) { toEl.innerText = currentText } lastText = fromEl.innerText
}

window.i = setInterval(check, 200)
```
![image](/assets/images/etc-udemy/udemy-script.png){: width="70%" height="70%"} 

## 동작안할경우 확인사항
- 스크립트가 번역된 대본을 영상 아래로 넣는 구조라서 1번에서 대본이 안보이거나 3번에서 대본이 번역이 안되었다면 한글자막 노출이 안됩니다.
- chrome 개발자 도구를 사용할 수 있어야 되는데,    
udemy에서 화면 수정같은걸로 querySelector가 값이 바뀔수도 있어서 개발자 도구의 셀렉터로 영상아래 엘리먼트(toEl), 대본 하이라이트 엘리먼트(fromEl) 의 querySelector값을 수정합니다.
