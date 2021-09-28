## 쿠키
HTTP 트랜잭션에는 상태가 없다`unstate`. 웹 사이트에 접속하는 모든 사용자 요청마다 동일 사용자인지 식별할 방법이 없는데, 이를 보완하는 방식이 예전부터 여러 방법이 존재했는데 쿠키는 오늘날에서 많이 사용하는 방식 중 하나이다. 이전에는 `IP`를 활용한 방법을 많이 사용했다고 하는데 아래와 같은 문제들이 있다.
+ 사용자 기준이 아닌 접속 PC의 IP이기 때문에 정확한 사용자 구별이 어려움
+ ISP가 제공하는 IP 대역을 사용하는 경우, 매번 다른 주소를 받으므로 추후 식별이 어려움
+ NAT 장비 등 보안을 위해 IP주소를 점점 private하게 관리하기 때문에 실 사용자 IP인지 파악이 어려움

사용자가 처음 웹 사이트에 방문하면 당연히 서버에서는 해당 사용자가 어떤 사용자인지 전혀 알지 못한다. 따라서 로그인 기능을 통해 인가 요청이 성공적으로 수행되면 HTTP 응답 헤더에 쿠키 값을 내려주는데 **key=value** 형태로 되어있다. 자세한 형식은 다음과 같다.
```bash
    Cookie: user-name="Jiny"; auth="1"
```
### 세션 쿠키와 지속 쿠키
+ **session cookie :** 사용자가 브라우저를 닫으면 삭제
+ **persistent cookie** : 사용자 PC 드라이브에 저장 (비휘발성) 

### SetCookie
`node.js`에서는 writeHead 메서드로 쿠키를 내려줄 수 있다. 쿠키를 사용할때의 주의사항으로는 한글문자열을 취급할 때, 반드시 인코딩(encodeURIComponent)과정을 거쳐야한다. 반대로 요청객체에서 받아올때도 디코딩(decodeURIComponent)과정이 필요하다.
```js
response.writeHead(200, {"Set-Cookie": `user-name=${encodeURIComponent(userName);}`});
```

### GetCookie
쿠키 응답을 받은 사용자가 다시 요청을 보내면 서버 측에서도 쿠키를 식별해야한다. 아쉽게도 `node.js`에서는 파싱 라이브러리를 쓰지 않는 이상 쿠키 값을 알아서 빼내올 함수가 존재하지 않는다. `request.headers.cookie`에 해당 접속자의 쿠키 정보를 확인할 수 있는데 직접 파싱하는 함수를 구현해야한다.
```js
//쿠키 형식(request.headers.cookie) : _ga=GA1.1.55873572.1627649409; _ga_6TN7PCJZTQ=GS1.1.1631890653.16.1.1631891304.0; user-id=asddsa
const getCookie = (cookies, key) => {
    const cookieArray = cookies.split(";");
    let value = "";
    cookieArray.forEach((element) => {
        if (element.split("=")[0].trim() === key) {
            return (value = element.split("=")[1]);
        }
    });
    return value.length ? decodeURIComponent(value) : value; //한글을 대비한 디코딩
};
```

해당 함수를 바탕으로 원하는 쿠키 값이 있는지 확인할 수 있다.
```js
if (method === "POST" && url === "/write") {
    //쿠키 값 탐색
    const userId = getCookie(request.headers.cookie, "user-id");
    let body = "";
    //쿠키 값 존재시 해당 사용자 ID, 아니면 익명 ID로 값 저장
    let writer = userId.length ? userId : "anonymous";
    request.on("data", (data) => {
        body += data;
    });

    return request.on("end", () => {
        if (body.length) {
            body = JSON.parse(body);
            store.boardlist.push({
                title: body.title,
                context: body.context,
                writer: writer,
                writeDate: new Date().toISOString().substr(0, 10),
            });

            //응답 성공 시 메인 화면으로 redirect
            response.writeHead(302, {
                Location: "/",
            });
            response.end();
        } else {
            response.writeHead(500, { "content-Type": "Application/json; charset=utf-8" });
            response.end(
                JSON.stringify({
                    result: false,
                    message: "NOT Value",
                })
            );
        }
    });
}
```