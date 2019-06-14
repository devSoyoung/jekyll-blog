---
layout: post
title:  "빠르게 훑어보는 웹 개발 트렌드"
date:   2019-04-12
categories: Review
---

2019년 4월 11일, 네이버 프론트엔드 테크콘서트에서 진행되었던 [빠르게 훑어보는 웹 개발 트렌드](https://www.slideshare.net/NaverEngineering/naver-tech-concertfe2019) 세미나를 듣고 내용을 정리한 글입니다.

## 웹 개발 트렌드
### 서버 중심으로 개발
미리 만들어두거나 서버에서 만든 페이지를 제공

![image](https://user-images.githubusercontent.com/42922453/55972502-91baf080-5cbe-11e9-8fab-d57e27afda4c.png)

* 사용자가 요청한 화면을 서버에서 페이지 단위로 생성해서 제공
* Java Applet, Java Servlet, PHP ..

### 클라이언트 중심으로 개발
페이지를 부분적으로 갱신, 서버는 API 역할에 집중

![image](https://user-images.githubusercontent.com/42922453/55972659-e8282f00-5cbe-11e9-95ac-86db2138cdba.png)

* 일단 클라이언트를 준비하고, 추가로 필요한 데이터를 클라이언트가 주도적으로 요청
* 이미 화면에 떠있는 페이지에 요청한 데이터를 추가
* DOM에 적극적으로 개입
* **요청** - Ajax, **DOM 조작** - jQuery

### 고도화
프론트엔드 로직이 복잡해지면서 다양한 라이브러리를 적극 활용

![image](https://user-images.githubusercontent.com/42922453/55972830-39382300-5cbf-11e9-8885-8c132f1f87fa.png)

## 요즘 웹 개발
> 주의 : 모든 프로젝트에 해당하는 것은 아님, 트렌드가 향하는 방향일 뿐

* Angular, Vue.js, React.js 등 다양한 프레임워크나 라이브러리 활용
* Component 기반 개발
* Task Runner, CLI 사용

## 공부하면 좋은 것들
### 기본 지식
* **레이아웃 구성** : HTML, HTML5
* **스타일 지정** : CSS, CSS3, flex/grid model, 스타일 라이브러리, 전처리기
* **반응, 로직 처리** : JavaScript, ES6, TypeScript, 프레임워크

### 그 외
* 개발 툴 : git, github
* UI/UX, 디자인 시스템
* 데이터 시각화

## 정리
* 프론트엔드 개발 트렌드는 빠르게 변하므로 계속 공부해야 함
* Full-stack : 물리적인 한계가 존재하므로 선택과 집중을 하는 것이 효율적일 수 있음


