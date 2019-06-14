---
layout: post
title: "[JS] 이벤트 등록: addEventListener()"
date:   2019-03-26
categories: Javascript
---

Javascript에서 가장 많이 수행하는 event 등록, event 관련 메소드에 대해 정리한 글입니다.

## 이벤트 등록하기
이벤트 대상(target)의 `addEventListener` 메소드로 지정한 이벤트가 대상에 전달되었을 때 실행할 함수를 등록할 수 있음
* **대상**: Element, Document, Window, XMLHTTPRequest 등, 이벤트를 지원하는 모든 대상

```javascript
target.addEventListener(type, listener[, options]);
target.addEventListener(type, listener[, useCapture]);
target.addEventListener(type, listener[, useCapture, wantsUntrusted]);
```

* **type** : 반응할 이벤트의 종류를 나타내는 문자열, 대소문자 구분
* **listener** : 지정한 이벤트가 발생했을 때 알림을 받는 객체, `EventListener` 인터페이스나 함수를 구현하는 객체여야 함
* **options** :
* **useCapture** :
* **wantsUntruste** : 

### 이벤트 등록 방법 비교

클릭 이벤트를 등록하는 방법에는 아래 두 가지 경우가 있음 

```js
const targetEl = document.getElementById('targetId');

// addEventListener 메소드로 등록하는 경우
targetEl.addEventListener('click', function() {
  console.log('event occur');
});

// 직접 onclick 속성에 등록하는 경우
targetEl.onclick = function() {
  console.log('event occur');
});
```

`addEventListener` 메소드를 사용해서 이벤트 핸들러를 등록하는 경우, 같은 이벤트에 여러 개의 핸들러를 등록할 수 있음

> 한 이벤트에 대해서 복수의 이벤트 리스너를 연결한 경우, onclick에 바로 함수를 전달하는 것처럼 하면 다른 이벤트 리스너가 지워지기 때문에, `addEventLinster`를 사용하는 것이 좋음

### 여러 개의 엘리먼트에 하나의 리스너 등록하기
이벤트 리스너를 함수로 만들고, 각 엘리먼트의 이벤트 리스너에 동일한 함수를 전달

```html
<input type="button" id="target1" value="button1" />
<input type="button" id="target2" value="button2" />
```
```js
const target1El = document.getElementById('target1');
const target2El = document.getElementById('target2');

function btnListener(event) {
  switch(event.target.id) {
    case 'target1':
      window.alert(1);
      break;
    case 'target2':
      window.alert(2);
      break;
  }
}

target1El.addEventListener('click', btnListener);
target2El.addEventListener('click', btnListener);
```

## event 관련 메소드
### preventDefault()
기본으로 설정되어 있는 동작의 실행을 막음
* submit을 클릭한 경우 preventDefault()로 기본 제출 동작을 막고 원하는 작업 수행 가능

### stopPropagation()
이벤트 버블링이나 캡쳐링으로 상/하위 태그로 전파되는 것을 막음
* 클릭한 태그와 관련된 동작만 수행할 수 있도록 함

***
## 참고링크
* [MDN - EventTarget.addEventListener](https://developer.mozilla.org/ko/docs/Web/API/EventTarget/addEventListener)
