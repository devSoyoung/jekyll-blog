---
layout: post
title:  "[HTML] Shadow DOM 개념, 사용방법"
date:   2019-05-07
categories: HTML
---

Shadow DOM의 개념과 사용 방법에 대해 정리한 글입니다.

## Shadow DOM의 필요성
### global로 모든 것을 공유하는 DOM
HTML 문서의 모든 요소와 스타일로 구성되어 있는 DOM은 하나의 global 범위 내에 존재한다. 그렇기 때문에, 모든 Element는 document 객체의 `querySelector()` 메서드로 접근할 수 있다. CSS 또한 document 내의 모든 해당하는 엘리먼트에 적용된다. 

이처럼 DOM이 global 영역으로 공유하는 것은 문서에 **일괄적으로 무언가를 적용**하기 매우 편리하게 해준다.

하지만, 글로벌 스타일에 영향을 받지 않는 독립적인 요소를 만들 수 없다는 점에서 불편하다. 외부의 것을 가져왔을 때 기존의 스타일에 현재 DOM의 스타일이 덮어씌워지기 때문에 원치 않는 스타일이 적용될 수 있다. 

### frame, iframe
위에서 말한 문제를 해결하기 위해, 즉 현재 DOM에 영향 받지 않는 요소를 추가하기 위해 `<frame>` 이나 `<iframe>` 태그를 사용해 왔다. *(frame 태그는 HTML5에서 사용하지 않는 deprecated된 태그임.)*

iframe은 내부 프레임(inline frame)이라는 의미로, 하나의 HTML 문서 내에 다른 HTML 문서를 보여주고자 할 때 사용한다. 하지만 `<iframe>` 태그를 사용하면 다음과 같은 단점이 있다.

* http 요청이 한차례 더 일어난다.
* 별도의 페이지이기 때문에, 소비되는 리소스도 높고 느리다.
* **iframe의 주소가 같은 도메인이 아닌 경우 접근이 불가능하다.**

여기에서 소개할 shadow-dom을 사용하면 위의 단점 없이 기존 DOM에 독립적인 요소를 만들 수 있다.

***
## Shadow DOM 사용방법
Shadow DOM은 HTML5에서 지원하는 **[웹 컴포넌트](https://developer.mozilla.org/ko/docs/Web/Web_Components)**이다. 아래에 Shadow DOM을 사용할 때 핵심적인 개념(생성, 사용 등)이나 헷갈렸던 것을 간단히 정리했다.

### Shadow Root 생성
DOM의 Element에 `attachShadow()`를 사용하면 Shadow Root를 생성해준다. Shadow Root 아래에 생성된 Element들은 기존 DOM의 영역과 별개로 관리된다.
```js
const newAreaEl = document.querySelector(`.new-area`);
newAreaEl.attachShadow({ mode: `open`});
```
```html
<div class="new-area">
    #shadow-root (open)
</div>
```
이렇게 하면 `newAreaEl` 하위에 shadow-root가 생성된다. `newAreaEl.shadowRoot`를 이용해서 shadow DOM에 원하는 요소를 추가할 수 있다.

> Shadow Root가 붙어있는 Element를 Shadow Host라고 부른다.

```js
newAreaEl.shadowRoot.appendChild(document.createElement(`a`));
```
```html
<div class="new-area">
    #shadow-root (open)
        <a></a>
</div>
```

`newAreaEl`에서 바로 요소를 추가하면 기존 DOM에 속한다.
```js
newAreaEl.appendChild(document.createElement(`div`));
```
```html
<div class="new-area">
    #shadow-root (open)
        <a></a>
    <div></div>
</div>
```

`newAreaEl`의 innerHTML을 바꾸면 shadowRoot도 사라질 것이라고 생각했는데, shadow-root를 제외한 `newAreaEl`의 하위 노드들만 변경되었다.
```js
newAreaEl.innerHTML = `<p>clear inner html</p>`;
```
```html
<div class="new-area">
    #shadow-root (open)
        <a></a>
    <p>clear inner html</p>
</div>
```

### Shadow DOM에 스타일 적용하기
기존 DOM과 분리 적용되는 스타일을 적용하려면 `<style>` 태그로 삽입해주거나, `<link>` 태그로 스타일 파일을 로딩하면 된다.

```js
newAreaEl.shadowRoot.innerHTML = `<link rel="stylesheet" href="${cssFile}" />`;

const styleEl = document.createElement(`style`);
styleEl.innerText = `...`;
newAreaEl.shadowRoot.appendChild(styleEl);
```

### .attachShadow의 mode
Shadow DOM을 생성하기 위해서는 `attachShadow()`을 사용하는데, 파라미터로 mode를 넘긴다. mode는 open, closed 두 가지 종류가 있다.

* [open, close에 대한 자세한 설명 포스트](https://medium.com/@emilio_martinez/shadow-dom-open-vs-closed-1a8cf286088a)

## Shadow DOM 알쓸신잡
*(알면 쓸데 많은 신비한 잡학사전)*

#### DOM과 Shadow DOM의 동일한 ID Element 사용
원래 DOM 내에서 id는 중복될 수 없는 값이지만, Shadow DOM은 다른 DOM으로 취급되기 때문에 DOM에서 사용한 id를 사용할 수 있다.

#### Shadow DOM의 중첩
Shadow DOM 내에 또 다른 Shadow DOM을 생성해서 DOM, 부모 Shadow DOM과 독립적인 별개의 DOM을 중첩할 수 있다.

***
## 참고링크
* [(번역) Shadow DOM은 무엇일까?](https://wit.nts-corp.com/2019/03/27/5552)
* [웹 컴포넌트(3) — 쉐도우 돔(#SHADOW DOM)](https://kyu.io/ko/%EC%9B%B9-%EC%BB%B4%ED%8F%AC%EB%84%8C%ED%8A%B83%E2%80%8A-%E2%80%8A%EC%89%90%EB%8F%84%EC%9A%B0-%EB%8F%94shadow-dom/)