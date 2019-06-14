---
layout: post
title:  "[Node.js] Koa 프레임워크 시작하기"
date:   2019-05-11
categories: Node.js
---

Node.js의 대표적인 프레임워크 중 하나인 Koa에 대해서 간단하게 정리한 글입니다.

## What is Koa framework?
Koa 대표적인 Node.js의 프레임워크인 Express를 만든 팀이 제작한 웹 프레임워크입니다. Express보다 더 작고, 더 표현적이고, 더 튼튼한 기초가 되는 것을 목표로 제작되었다고 합니다. [[공식사이트 바로가기]](https://koajs.com/)

## Koa 설치하기

Koa는 비동기 처리를 위해 자바스크립트 최신 문법인 async/await를 사용하기 때문에, ES6 이상을 지원하는 node 버전 7.6.0 이상이 필요합니다.

    $ nvm install 7
    $ npm i koa
    $ node my-koa-app.js
    
이렇게 하면, `my-koa-app.js` 를 실행할 수 있습니다.

## Hello, world!
간단한 Hello, World 앱으로 Koa 프레임워크의 기능을 확인해봅시다.

```js
// my-koa-app.js
const Koa = require('koa');
const app = new Koa();

app.use(async ctx => {
    ctx.body = 'Hello, World!';
});

app.listen(3000, () => {
    console.log('Connect!');
});
``` 

Koa 프레임워크를 `require()`로 가져온 후, app을 생성합니다. `use()`로 앱이 실행되었을 때 동작(Hello, world 출력)을 지정해준 후 `listen()`을 실행해 3000번 포트로 서버를 동작시킵니다. 콜백을 전달해서 서버가 실행된 후 동작을 지정해줄 수도 있습니다. 여기에서는 console에 로그를 찍는 동작을 수행해 보았습니다.

> 다른 사용 중인 애플리케이션과 겹치지 않는 포트를 쓰면 됩니다. 일반적으로 개발할 때 3000번 포트를 많이 사용하기 때문에 여기에서도 관례적으로(?) 3000번 포트를 사용했습니다. 이미 3000번 포트를 사용중이라면 3001, 3002 등 다른 포트를 사용해도 됩니다. (사실 저 코드는 공식 문서에서 가져왔습니다. :P)

이렇게 하고 `node my-koa-app.js`를 실행하면 console에는 'Connect!'라는 메세지가 뜹니다. localhost의 3000번 포트로 접속하면 'Hello, World!' 메세지도 확인할 수 있습니다.

이 코드에서 우리는 `use()`를 눈여겨보아야 합니다. 미들웨어라는 중요한 개념이 등장하기 때문입니다.

### Middleware

Koa는 여러 미들웨어가 연결되어 구성됩니다. `app.use()`를 통해 사용할 미들웨어를 애플리케이션에 등록해줄 수 있습니다. `use()`의 파라미터로 넘겨준 미들웨어 함수는 `ctx`와 `next`를 전달받습니다.

* `ctx` : 웹 요청과 응답에 대한 정보를 저장
* `next` : 다음 미들웨어를 실행시키는 함수

미들웨어에서 `next`를 실행하게 되면 다음 미들웨어로 전달되고, 실행하지 않으면 해당 미들웨어에서 요청 처리를 완료하고 응답하게 됩니다. 미들웨어는 등록한 순서대로 실행됩니다. 아래 예제를 확인하면 더 쉽게 이해할 수 있습니다.

```js
const Koa = require('koa');
const app = new Koa();

app.use((ctx, next) => {
    console.log('middleware 1');
    next();
});

app.use((ctx, next) => {
    console.log('middleware 2');
});

app.use((ctx, next) => {
    console.log('middleware 3');
});

app.listen(3000, () => {
    console.log('Connect!');
});
```

이렇게 미들웨어를 여러 개 연결해서 3000번 포트로 접속하면 1, 2번 미들웨어가 순차적으로 실행되는 것을 알 수 있습니다.

### next()
`next()`는 프로미스를 반환합니다. 그래서 다음 미들웨어가 실행이 끝난 후 동작을 지정해줄 수 있습니다. 아래 코드를 보면서 확인해봅시다. 

```js
const Koa = require('koa');
const app = new Koa();

app.use((ctx, next) => {
    console.log('--- middleware 1 ---');
    ctx.body = 'Hello, World!';
    console.log("middleware 1's body:", ctx.body);
    next()
        .then(() => {
            console.log("Body Message Changed!");
        });
});

app.use((ctx, next) => {
    console.log('--- middleware 2 ---');
    ctx.body = 'Goodbye, World!';
    console.log("middleware 2's body:", ctx.body);
});

app.listen(3000);
```
    
    // 실행결과
    --- middleware 1 ---
    middleware 1's body: Hello, World!
    --- middleware 2 ---
    middleware 2's body: Goodbye, World!
    Body Message Changed!

이렇게 하면 처음에는 'Hello, World!'로 메세지가 지정되고, 이후에는 'Goodbye, World!'로 메세지가 지정됩니다. 두 번째 미들웨어가 끝난 후에 콘솔에는 'Body Message Changed!'가 출력됩니다.

ES7에서 추가된 async/await 문법을 사용하면 콜백을 없앨 수 있습니다.

```js
const Koa = require('koa');
const app = new Koa();

app.use( async (ctx, next) => {
    console.log('--- middleware 1 ---');
    ctx.body = 'Hello, World!';
    console.log("middleware 1's body:", ctx.body);

    await next();
    console.log("Body Message Changed!");
});

app.use((ctx, next) => {
    console.log('--- middleware 2 ---');
    ctx.body = 'Goodbye, World!';
    console.log("middleware 2's body:", ctx.body);
});

app.listen(3000);
```

동일한 코드를 async/await으로 수정한 결과입니다. 마찬가지로 잘 동작합니다.

## nodemon으로 자동 서버 재시작
지금까지의 내용을 수행하면서 코드를 수정하면 바뀐 내용을 적용하기 위해서 실행중이던 node 서버를 종료하고 다시 실행해야 했습니다. 한두번은 괜찮았지만 반복할수록 귀찮음과 짜증이 증가합니다. **nodemon** 사용하면 수정을 감지해서 자동으로 서버를 재실행시켜줍니다. 

먼저, nodemon을 설치해봅니다.

    $ npm install -g nodemon
    
다른 프로젝트에서도 nodemon을 사용할 수 있도록 global로 설치했습니다. 이 프로젝트에서만 사용하고 싶다면, global 옵션 없이 설치하면 됩니다. 

아래 명령어를 입력하면 `helloWorld.js` 파일이 수정될 때마다 알아서 서버를 재실행해줍니다.

    $ nodemon helloWorld.js

일반적으로 프로젝트 구조를 잡을 때 src 파일 안에 Javascript 코드를 관리하므로 src 폴더를 만들고, 그 안에 `index.js` 파일을 생성합시다. 그리고나서 아래 명령어를 다시 실행해봅니다. (`index.js` 파일 안에는 아까 `helloWorld.js`에 작성한 코드를 옮겨서 실행했습니다.)

    $ nodemon --watch src/ src/index.js 
    
index.js 파일을 수정하면 서버가 재실행됩니다. src 폴더 내에 새로운 파일을 추가해도 서버가 재실행됩니다. `src` 디렉토리의 모든 변화를 감지하기 때문에 그렇습니다.

이 명령어를 매번 개발을 시작할때마다 치면 귀찮기 때문에, `package.json` 파일에 script를 추가해줍시다.

    "scripts": {
        "test": "echo \"Error: no test specified\" && exit 1",
        "start": "nodemon --watch src/ src/index.js"
    },
      
`package.json` 파일의 **scripts**에 "start"를 추가하고 아까 입력한 명령어를 입력해줍니다. 그리고 터미널에서 `npm run start`를 입력하면 동일하게 동작하는 것을 확인할 수 있습니다.