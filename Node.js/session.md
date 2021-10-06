## 세션
쿠키의 단점을 보완한 사용자 상태를 관리하는 기법 쿠키의 단점은 브라우저에서 조작이 가능하므로 사용자가 마음만 먹으면 언제든지 값을 위조시킬 수 있다. 악의적인 값 위변조나 탈취를 방지하기 위해 중요한 정보 `key`들은 세션에서 관리하고 클라이언트에 필요할 시 세션을 식별하기 위한 키값 정도만 제공하는 방식으로 보완할 수 있다.

### 간단한 세션 구현
핵심은 클라이언트에 정보가 노출되지 않는다는 것, 서버 내에 세션 키를 이용해 값을 참조할 객체를 하나 생성한다.
아래는 그 예시인데, 사용자 이름과 만료시간을 할당했다.
```jsx
	//createServer 콜백 함수
	if(request.url === "/login"){
		const userSession = {}; //사용자 정보
		const uuid = Date.now(); //사용자 식별을 위해 중복되지 않아야한다. 
		const expireTime = new Date(); //사용자 정보
		expireTime.setMinutes(expireTime.getMinutes() + 5); //만료시간 설정
		
		userSession[uuid] = {
			name : "admin",
			expires : erpireTime
		}
	}
```

이제 세션을 성공적으로 저장했으면 해당 사용자가 재방문시 사용자 상태를 관리하기 위해 세션을 담아 전송해야한다.  
전송하는 방법은 `Set-Cookie`에 session 키값에 넣어주면 된다.  (쿠키를 사용하는 방식과 동일하지만, 각각의 key-value에 데이터를 전송하는 것이 아닌 식별을 위한 key만 전송하는 것이 차이점이다.)

```jsx
	http.createServer(async (request, response) => {
		try{
			const method = request.method;
			const url = request.url;
		
			//로그인 후 메인화면 요청
			if(method === "POST" && url === "/login"){
					let body = "";
			let userId = request.body.id.length ? request.body.id : "anonymous";
			request.on("data", (data) => { body += data; });
					/* session 처리 */
					const userSession = {}; //사용자 정보
					const uuid = Date.now(); //사용자 식별을 위해 중복되지 않아야한다. 
					const expireTime = new Date(); //사용자 정보
					expireTime.setMinutes(expireTime.getMinutes() + 5); //만료시간 설정

					userSession[uuid] = {
						name : userId,
						expires : erpireTime
					}

					response.writeHead(200, {
			Location : "/",
			"Set-Cookie": `session=${uuid} Expires=${expireTime} HttpOnly;`,
					});
			}
		} catch(e){
			console.error("error!", e);
		}
	}).listen(8088, () => {
		console.log("server start..");
	});
	response.writeHead(200, {
			"content-Type": "Application/json; charset=utf-8",
			"Set-Cookie": `session=${uuid} Expires=${expireTime.toGMTString()}`, 
	});
```

### 개선할 점
쿠키를 사용하지 않고 세션을 사용하는 것도 보안에 많이 취약하다고 한다. 오늘 작성한 방식은 사용자에게 노출되는 값을 파악할 수 없게 식별 용도로 만 작성해서 내려주기 때문이다. 

값(key)과 용도(value)가 명확히 노출되는 쿠키에 비해서는 낫다곤 하지만 위 방식으로만 작성하는 것은 실 서비스에 아래와 같은 문제점이 있다고 한다.

- 보안 취약점 : 발급 시 사용자가 언제나 값을 변조시킬 수 있음, 단순한 개인식별 값 보다 위변조 시킬 수없도록 더욱 강력한 보안이 필요 → 추후 살펴볼 `express-session`등  시크릿 키를 제공해주는 여러 라이브러리들을 통해 개선 가능하다
- 메모리 문제 : 서버를 재시작 할 때 마다 세션을 저장한 변수가 초기화 따라서 서버 내에 변수로 저장하기 보다는 실제로는 `redis`같은 NOSQL 데이터베이스를 사용하는 것을 추천한다. (읽기 속도가 빠르며, 데이터 유실 시 복구가 가능하다.)