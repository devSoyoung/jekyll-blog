---
layout: post
title:  "[Node.js] koa-router 사용하기"
date:   2019-05-12
categories: Node.js
---

지난 글 [Koa 프레임워크 시작하기](https://devsoyoung.github.io/node.js/2019/05/11/koa-api-tutorial.html)에 이어서 koa-router에 대해 정리한 글입니다.

## koa-router

API 엔드포인트를 정의하기 위해서 koa-router를 사용합니다. 요청 URI에 따라서 다른 작업을 수행할 수 있도록 해주는 것입니다. Koa 프레임워크에 내장되어 있지 않기 때문에, 별도로 설치해주어야 합니다.

    $ npm i koa-router
    
## koa-router 사용하기

koa-router도 하나의 미들웨어로 app에 연결해서 사용할 것입니다. 아래와 같이 `index.js` 파일을 만들어봅니다.

```js
const Koa = require('koa');
const Router = require('koa-router');

const router = new Router();
const app = new Koa();

router.get('/', (ctx, next) => {
    ctx.body = 'Home';
});
router.get('/hello', (ctx, next) => {
    ctx.body = 'Hello, World!';
});

app.use(router.routes());
app.use(router.allowedMethods());

app.listen(3000, () => {
    console.log('server is listening to port 3000');
});
```

이렇게 하고 서버를 동작한 후, 3000번 포트로 접속하면 'Home' 메세지가 뜹니다. `localhost:3000/hello`로 접속하면 'Hello, World!'라는 메세지를 볼 수 있습니다.

## Parameter
