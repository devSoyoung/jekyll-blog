---
layout: post
title: "[JS] HTMLCollection과 NodeList 차이점"
date:   2019-03-26
categories: Javascript
---

Javascript의 Element Collection인 HTMLCollection과 NodeList의 공통점, 차이점을 정리한 글입니다.

## 공통점과 차이점 요약
* **공통점** : DOM nodes의 모음
* **차이점** : 제공하는 메소드, 포함하고 있는 돔 노드의 타입이 다름
	* `NodeList` : 모든 타입의 노드를 다 가질 수 있지만
	* `HTMLCollection` : element 노드만 가질 수 있음
	
## [HTMLCollection](https://developer.mozilla.org/ko/docs/Web/API/HTMLCollection)
요소(elements)의 일반적인 모음을 나타냄(유사배열)
### 속성
* `.length` : 컬렉션 항목의 갯수를 반환
* `.[이름명]` : 노드 목록 중 해당 이름을 가진 요소를 바로 참조할 수 있음(`-`가 이름 중간에 있는 경우는 불가능)

### 메서드
* `.item()` : 파라미터로 받은 인덱스의 노드를 반환하며, 범위를 벗어난 인덱스인 경우 null을 반환
* `.namedItem()` : 파라미터로 받은 문자열과 일치하는 이름의 노드를 반환하며, 해당 이름을 가진 노드가 없는 경우 null을 반환

### 주의사항
* 순수 숫자로 된 id를 인식할 수 없음
	* HTML5에서는 허용하지만, 배열 형식의 접근과 충돌할 수 있어서 HTMLCollection에서는 지원하지 않음
* 멤버(노드 목록)을 이름(`.name`)과 인덱스(`.item()`)로 직접 노출함
* HTML ID는 유효한 문자로 **:** 와 **.** 을 포함할 수 있는데, 속성에 접근하기 위해서는 대괄호 표기법을 사용

```js
var elem1, elem2;

// document.forms은 HTMLCollection이다.
elem1 = document.forms[0];
elem2 = document.forms.item(0);

alert(elem1 === elem2); // "true"

elem1 = document.forms.myForm;
elem2 = document.forms.namedItem("myForm");

alert(elem1 === elem2); // "true"

elem1 = document.forms["named.item.with.periods"];
```

## [NodeList](https://developer.mozilla.org/ko/docs/Web/API/NodeList)
`element.childNodes` 속성, `document.querySelectorAll` 메서드로 반환되는 노드의 모음
* 유사배열이지만 `.forEach()`를 사용하여 반복할 수 있고 `Array.from()`으로 배열로 변환할 수도 있음
	* 일부 구형 브라우저의 경우 지원하지 않으며, `Array.prototype.forEach()`를 사용하면 됨

### 속성
* `.length` : NodeList의 갯수를 반환

### 메서드
* `.item()` : 파라미터로 받은 인덱스의 노드를 반환하며, 범위를 벗어난 인덱스인 경우 null을 반환
* `.entries()` : iterator를 반환하여 모든 키(index)/값(node) 쌍을 순회할 수 있도록 함
* `.forEach()` : 요소(element)마다 한 번씩, 인자로 전달 받은 함수를 실행하여 요소를 인수(argument)로 함수에 전달
* `.keys()` : 콜렉션에 포함된 모든 키(unsigned int)를 통과할 수 있는 iterator 를 반환 (*왜 쓰는지 모르겠음*)
* `.values()` : 콜렉션에 포함된 모든 값(node)를 통과할 수 있는 iterator를 반환 (*왜 쓰는지 모르겠음*)

### 주의사항
* `Node.childNodes`의 NodeList는 라이브 콜렉션으로, DOM의 변경사항을 실시간으로 반영함
* `document.querySelectorAll()`의 NodeList는 정적 콜렉션으로, DOM이 변경되어도 collection의 내용에는 영향을 주지 않음

```javascript
const staticNList = document.querySelectorAll('div');
const dynamicNList = document.body.childNodes;

// > dynamicNList
// NodeList(33) [text, script, text, ul#nav-access, text, comment, text, header#main-header.header-main, ...]

// > staticNList
// NodeList(52) [div.nav-toolbox-wrapper, div#nav-tech-submenu.submenu.js-submenu, div.submenu-column, div#nav-learn-submenu.submenu.js-submenu, ...]

const div = document.createElement('div');
document.body.appendChild(div);

// > dynamicNList
// NodeList(34) [text, script, text, ul#nav-access, text, comment, text, header#main-header.header-main, ...]

// > staticNList
// NodeList(52) [div.nav-toolbox-wrapper, div#nav-tech-submenu.submenu.js-submenu, div.submenu-column, div#nav-learn-submenu.submenu.js-submenu, ...]
```
