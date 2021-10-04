## Express.js
기존 `http`모듈로 서버를 구축하는 것 보다 프레임워크를 사용해 서버를 구축하는 것이 더욱 효율적이다. `node.js`생태계에서도 압도적으로 express.js를 사용한다. 혹여나 다른 프레임워크를 찾는 다면 아래 프레임워크를 찾아보는 것도 좋다.
+ [**koa**](https://koajs.com/)
+ [**hapi**](https://hapi.dev/)

### 설치하기
npm 명령어를 통해 express 프레임워크 설치
```bash
npm install express
```

### 서버구축
express모듈을 통해 기존 `http`모듈로 구축했을 때 처럼 if문으로 분기처리하지 않아도 쉽게 http-method요청을 받을 수 있다.
```js
const express = require("express");
const app = express();

//get method
app.get("/", (req, res) => {
    res.send("hello express.js");
});

//post method
app.post("/write", (req,res)=>{
		/* code */
});

app.listen(8080, () => {
    console.log("start express server");
});
```

### 모니터링
`nodemon`은 개발 생산성을 높여주는 모니터닝 모듈이다. 보통 파일이 변경되면 서버를 종료하고 다시 실행시키는데, 자동으로 js파일들을 모니터링해서 파일이 수정되면 알아서 restart해준다. 콘솔 환경에서 실행하기 때문에 설치 시 `global`환경으로 설치해 주면 된다.
```bash
#설치
npm install -g nodemon 

#nodemon으로 앱 실행
nodemon app
```
nodemon으로 앱을 실행하면, 소스가 바뀌어도 재시작 할 필요 없이 알아서 restart가되는것을 볼 수 있다.
```text
> npm-test@1.0.0 start
> nodemon app
[nodemon] 2.0.13
[nodemon] to restart at any time, enter `rs`
[nodemon] watching path(s): *.*
[nodemon] watching extensions: js,mjs,json
[nodemon] starting `node app.js`
start express server
```

## sendFile
express에서는 fs모듈 필요 없이 `sendFile`메서드로 html파일을 브라우저에게 전달할 수 있다.
```text
project
├── app.js
├── html
│   └── about.html
├── package-lock.json
└── package.json
```
```js
app.get("/about", (req, res) => {
    res.sendFile(path.join(__dirname, "/html/about.html"));
});
```