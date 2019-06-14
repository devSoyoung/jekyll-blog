---
layout: post
title:  "오늘부터 나도 FE 성능분석가"
date:   2019-04-13
categories: Review
---

2019년 4월 11일, 네이버 프론트엔드 테크콘서트에서 진행되었던 [오늘부터 나도 FE 성능분석가](https://www.slideshare.net/NaverEngineering/naver-tech-concertfe2019-fe) 세미나를 듣고 내용을 정리한 글입니다.

## WEB 성능분석 지표
### TPS(Transition Per Second)
서버가 얼마나 많은 요청을 처리할 수 있는지를 나타내는 지표
### LAI(Loading And Interaction)
사용자 입력에 얼마나 빠르게 반응하는지를 나타내는 지표
* **초기 로딩 속도** : 얼마나 빨리 페이지를 볼 수 있는지
* **인터렉션 속도** : 동작이나 애니메이션의 매끄러운 정도

## FE 성능개선 작업 PLAN
### 대상 선정하기 : 숲을 보자
* 서비스에서 가장 많이 사용하는 화면은 무엇인가
* 서비스에서 사용자에게 가치 있는 화면은 무엇인가

### 개선 프로세스
* 측정(Measure)
* 분석(Analytic)
* 최적화(Optimize)

### 언제까지?
목표에 도달할 때까지
* **목표** : 초기 로딩 속도
* **네이버의 초기 로딩 속도** : 모바일 1.5초, PC 2초
* **구글의 초기 로딩 속도** : 사용자에게 꼭 보여주어야 하는 부분 기준 FMP

## 성능 개선 작업 - Part 1 : 초기 로딩 속도 개선하기
### 페이지 로딩 과정
서버로부터 HTML 파일을 받고, 파일에 첨부된 script, link, img 태그 등을 요청해서 받아옴

### 로딩 속도 측정/분석하기
waterfall 차트의 **높이**를 줄이고, **폭**을 줄이고, **간격**을 당긴 후 총 점검하기

#### **높이 줄이기** : Request 수 줄이기
![image](https://user-images.githubusercontent.com/42922453/56368481-7a39b580-6232-11e9-8e83-2c439b340d50.png)

* JS, CSS Merge : JS, CSS를 합쳐서 하나의 파일로 만듦
* CSS Sprite : 여러 이미지를 하나의 Sprite 이미지로 만듦
* DATA URI : 캐싱되지 않아도 될 이미지를 HTML 요청에 포함
* Lazy : 초기 로딩 시 불필요한 자원은 삭제하거나, 나중에 요청
	* 실수로 요청한 자원
	* 초기 로딩 시 불필요한 JS : `<script>` 태그 동적 삽입
	* 뷰 포트 바깥에 있는 이미지 : carousel 같은 경우

> 이미지가 전체 요청 용량의 절반 이상을 차지하므로 이 부분을 개선하는 것이 가장 효과적

브라우저는 호스트당 **동시 연결 가능한 개수**가 정해져 있음
* 요청 수가 많은 경우 : 가능한 connection 수를 초과해서 대기 발생
* 요청을 줄일수록 좋음

#### **폭 줄이기** : Request 시간 줄이기
![image](https://user-images.githubusercontent.com/42922453/56368550-a0f7ec00-6232-11e9-9d8f-e9734ec6cef7.png)

* HTTP 프로토콜마다 Connection 활용 방법이 다름
* HTTP2를 사용하자
* **TTFB(Time to First Byte)** : 오래 걸리면 서버 비즈니스 로직이 느린 것
* **Content Download**가 오래 걸릴 때 : 네트워크 속도가 낮거나 컨텐츠 크기가 큰 경우
d ㅋㅌ* minify(주석제거, 공백제거), obfuscation(변수명 변경), gzip(컨텐츠 인코딩)
	* 큰 이미지 줄이기, 이미지 메타정보 날리기

이미지가 클수록 Decode(이미지 데이터를 RGB로 변환하는 과정) 비용이 많이 듬
* 디자이너는 레티나(device-ratio: 2) 이상급 이미지를 원한다
* 요즘 브라우저는 다 레티나 이상이므로, 보이는 크기의 2배 정도의 이미지 크기로 하자

#### **간격 당기기** : Request 계단 간격 당기기
![image](https://user-images.githubusercontent.com/42922453/56368921-588cfe00-6233-11e9-9fdc-41453f02c972.png)

1. 서버로부터 HTML 문자열을 Stream으로 받음
2. `<head>` 태그에 포함된 자원을 병렬로 다운로드
3. `<head>` 태그에 포함된 자원을 모두 실행
4. `<body>` 태그부터 화면을 그리기 시작
5. DOM 구성이 완료되면 DOMContentLoaded 이벤트 발생
6. 모든 자원의 로딩 완료되면 load 이벤트 발생

* Document Load : DOM 구성 완료 시점
* Window Load : 모든 자원의 로딩 완료 시점

```html
<html>
    <head>
        <!-- A -->
    </head>
    <body>
         <p>Hello world (1) <img src="world1.png" /></p>
         <!-- B -->
         <p>Hello world (2) <img src="world2.png" /></p>
         <p>Hello world (3) <img src="world3.png" /></p>
         <p>Hello world (4) <img src="world4.png" /></p>
         <!-- C -->
    </body>
</html>
```

* A : `<head>`안의 모든 자원을 병렬로 로딩
* B : 자바스크립트나 CSS 파일 실행 시 렌더링이 block됨
* C : DOM은 다 그려졌으나, 이미지는 아직 로딩되지 않음

Frontend 성능 격언
1. head 태그에는 CSS와 필수 JS만 넣어라
2. JS는 body 태그 마지막에 넣어라
3. HTML 파일 중간중간에 JS를 넣지 마라

* async, defer 속성을 활용
	* defer : DOM 제어와 관련이 있는 스크립트
	* async : 의존성이 없는 스크립트
* CSS 파일에서 폰트, 이미지를 사용할 때, **preload**를 사용하면 CSS와 함께 로딩됨
* HTTP2 Server Push : HTML과 함께 JS, CSS, 이미지가 로딩됨

#### 총체적으로 점검하기
![image](https://user-images.githubusercontent.com/42922453/56369671-c128aa80-6234-11e9-8de1-44fe3a8cdd28.png)

1. 체감 속도 높이기
* FP(First Paint) : HEAD 태그 종료 후
* FMP(First Meaningful Paint) : Hero 엘리먼트가 보이는 시기
* Hero 엘리먼트가 무엇인지, Lazy하게 처리하면 안되는 요소가 무엇인지 고려하기

2. 균형감 찾기
* 각각의 Request를 균등한 크기로 만들기
* 지나치게 큰 CSS나 JS가 없도록
