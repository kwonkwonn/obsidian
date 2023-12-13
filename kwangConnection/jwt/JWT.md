https://datatracker.ietf.org/doc/html/rfc7519 <-- RFC7519를 준수하는 인증 시스템
요약하면, 두 집단간에 암호화된 json 파일을 전송하면서 스스로를 밝히는 인증시스템

쿠키- 세션 방식:
> 사용자가 로그인을 할 시, 서버에서는 데이터베이스에 사용자의 정보를 저장한 후, 사용자의 정보를 담은 쿠키를 브라우저에 전달한다. 
> 브라우저에서는 다음 요청부터 쿠키를 전달해서, 매 프로세스마다 인증을 할 필요가 없게 함.

JWT 방식:
> 서버에서 비밀키를 사용해서 데이터를 암호화 한 후, 이 json 파일을 사용자에게 전달함.
> 서버에서는 더이상 사용자의 쿠키를 대조하기 위한 데이터베이스를 저장 할 필요가 없어졌음. 
> 단점: 쿠키를 악의적인 사용자에게 탈취당했을때 대처 할 방법이 없음, 쿠키-세션 방식에서는 세션에 데이터를 삭제하면 되지만, 그럴 수 없음

jwt 유튜브에서 본 예시 
```
	fetch('link',{ 
	method:POST},).then(res=>{
	const token= res.data.token}
	localStorage.setItem('jwtToken', token));
```
개발자 도구의 application의 localstorage에서 확인 할 수 있음. 

한번 로컬 스토리지에 포함이 되면, request 마다 헤더에 token을 포함시켜서 전송하면 됨.

리덕스 상태에 localstorage에 저장되어 있는 토큰을 항상 포함시키면, 토큰이 만료되거나 명시적으로 삭제하지 않는경우, 매 req마다 토큰이 헤더에 포함되어서 보내짐





https://jwt.io/introduction

https://velog.io/@yaytomato/%ED%94%84%EB%A1%A0%ED%8A%B8%EC%97%90%EC%84%9C-%EC%95%88%EC%A0%84%ED%95%98%EA%B2%8C-%EB%A1%9C%EA%B7%B8%EC%9D%B8-%EC%B2%98%EB%A6%AC%ED%95%98%EA%B8%B0

https://nsinc.tistory.com/121