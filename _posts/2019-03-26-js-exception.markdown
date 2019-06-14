---
layout: post
title: "[JS] Javascript 예외처리: try, catch, finally"
date:   2019-03-26
categories: Javascript
---

Javascript의 예외처리 방법에 대해 정리한 글입니다.

## 예외처리의 필요성
예상치 못하게 발생한 에러를 잘 처리해야 프로그램 전체가 멈추지 않고 동작할 수 있음

## Try, Catch, Finally
```javascript
try {
	// 실행하고 싶은 코드
	// 여기서 실행하다가 에러가 발생하면 catch 블록으로 이동
	throw {전달할 e 값};		// catch 블록으로 이동
}
catch (e) {
	// 에러가 발생했을 때,
	// 에러를 파라미터로 받아서 처리함
}
finally {
	// 가장 마지막에 항상 실행되어야 할 코드
}
```
* try는 반드시 있어야 함
* catch나 finally는 둘 중 하나만 있어도 실행 가능
* `throw` : 에러를 발생시키는 명령어

### 에러 처리 과정
throw가 발생하면 catch 구문을 찾아서 이동
* 현재 블록에 catch나 finally가 없는 경우 상위 블록이나 호출한 함수로 이동
* catch가 존재하지 않는 경우, finally를 실행하고, catch 될 수 있는 구문을 찾아서 이동
* catch 구문에서 에러가 처리되고, 이후 코드를 실행
