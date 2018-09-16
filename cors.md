# CORS

> **Cross-Origin Resource Sharing** 또는 **CORS** 는 서로 다른 출처간의 통신, cross-origin 요청을 안전하게 보낼 수 있는 방법을 정한 표준이다. 첫 번 째 리소스가 서빙된 도메인과 다른 도메인으로부터의 리소스 요청을 제한하는 것을 허락하는 메커니즘을 말한다.

### Same-origin Policy(동일 출처 정책)

> 웹페이지에서 리소스를 불러올 때, 리소스의 출처가 웹페이지의 출처와 같으면 안전하다고 보고, 출처가 다르면 해당 리소스는 안전하지 않다고 보는 원칙이다. 서버 A 에서 호스팅되는 문서는 서버 A 에 있는 다른 문서하고만 상호작용할 수 있다.

여기서 '출처'란 **'프로토콜 + 도메인 + 포트번호'**의 결합을 가리킨다. 세 개가 다 같아야 동일 출처라고 할 수 있다.
`http://www.example.com/hello-world.html` url 의 프로토콜( HTTP), host(example.com), 그리고 포트(80)가 같아야만 동일한 출처라고 한다.

### 왜 동일출처 정책이 필요할까?

많은 웹사이트가 쿠키를 사용하여 인증 또는 세션 정보를 추적할 수 있다. 쿠키는 쿠키가 생성시의 특정 도메인에 바인딩된다. 도메인에 대한 모든 HTTP 호출(ajax 호출, 이미지, html 페이지등)에서 브라우저는 해당 도메인에서 생성 된 쿠키를 첨부한다. 즉 https://examplebank.com에 로그인하면, 쿠키가 https://examplebank.com 에 저장된다. 은행페이지가 SPA 라면, AJAX 로 통신하게 위해 https://examplebank.com/api 를 생했을 수도 있다.
만약 same-oirign policy 가 적용되지 않은 상태에서 http://examplebank.com에 로그인 한 후 https://evil.com으로 이동한다면 해커가 은행 쿠키에 직접 접근할 수 없더라도, 해커 웹사이트가 인증된 악성 AJAX 를 http://examplebank.com/api로 /withdraw post 요청을 보내 은행 계좌에서 예금을 인출할 위험이 있다.
이는 브라우저가 해당 도메인에 대한 모든 HTTP 호출에 대해 http://example.com에 바인드 된 쿠키를 자동으로 첨부하기 때문이다. 그래서 http://evil.com에서 http://examplebank.com으로 AJAX 호출을 할 때도 쿠키를 첨부되는 것이다.

아마도 SPA APP 에서는 http://mydomain.com 은 http://api.mydomain.com으로 AJAX call 을 할 것이다. Cross-Origin Resource Sharing 은 이러한 cross domain request 를 가능하게 해준다.

### CORS

http://domain-a.com으로부터 전송되는 HTML 페이지가 `<img>src`속성을 통해 http://domain-b.com/image.jpg, 다른 출처로부터 리소스를 읽어온다. 이 때 cross-origin HTTP 요청을 하게 된다. HTTP 요쳥은 기본적으로 Cross-Site HTTP Request 가 가능하다.
그러나 `<script></script>`로 둘러싸여 있는 스크립트에서 생성된 Cross-Site HTTP Request 는 Same Origin Policy 를 적용받기 떄문에 Cross-Site HTTP Request 가 불가능하다. 즉 프로토콜, 호스트명, 포트가 같아야만 요청이 가능하다.

IE8 이상의 모던 웹 브라우저는 cross-origin 요청에 대해 여러가지 제한을 두고 있다. cross-oirign 요청을 허용하려면, 서버가 특별한 형태의 응답을 전송해야 한다. 만약 서버가 cross-origin 요쳥을 허용하지 않으면, 웹 브라우저는 에러를 발생시킨다.

#### CORS 에 관여하는 응답 헤더

- Access-Control-Allow-Origin
- Access-Control-Expose-Headers
- Access-Control-Max-Age
- Access-Control-Allow-Credentials
- Access-Control-Allow-Methods
- Access-Control-Allow-Headers

#### CORS 에 관여하는 요청 헤더

- Origin
- Access-Control-Request-Method(preflighted 전용)
- Access-Control-Request-Headers(preflighted 전용)

CORS 구현은 보안측면에서 중요하다. 알 수 없는 도메인에 의한 호출을 차단하고 허용된 메소드외의 리소스를 변경하거나 삭제할 수 있는 요청은 거부할 수 있다.

#### CORS - Safe, Unsafe

GET, HEAD 요청은 safe(읽기 전용)이기 때문에 서버에 요청이 도달한다고 해서 서버의 상태에 영향을 미칠 일은 없으므로, 웹 브라우저는 일단 해당 요쳥을 보내본다. 만약 서버가 cross-origin 요청을 허용한다고 응답하면 응답을 그대로 사용하고, 그렇지 않으면 에러를 낸다. POST, PUT, PATCH, DELETE 등의 메소드는 요청이 서버에 전송되는 것 자체가 위험흐므로, 실제 요청을 보내기 전에 서버가 cross-origin 요청을 허용하는지를 알아보기 위해 시험적으로 요청을 한 번 보내본다. 이 요청을 `preflighted request`라고 한다.

#### CORS with credentials

cross-origin 요청에 기본적으로 쿠키가 포함되지 않으나, XMLHTTPRequest 혹은 fetch 를 통해서 요청을 보낼 때 쿠키를 포함시키는 옵션을 줄 수 있고 이 때 CORS 요건이 더 엄격해진다.

### 정리

프론트엔드와 API 서버를 같은 도메인으로 제공한다.
불가피하게 둘을 다른 도메인으로 제공해야 한다면 CORS 를 허용한다(cors 미들웨어를 사용하면 간단함)
CORS 를 허용하는 경우, 쿠키를 쓸 수는 있으나 보안 상 허점이 생기기 쉽고 사용하기도 불편하므로 보통 JWT(JSON Web Tokens) 와 같은 토큰 방식의 인증을 사용한다.

## 참고자료

[FDS 수업자료 ](https://fds9.github.io/fds-nodejs-http/2-2-3-jwt.html)
[What is CORS? (or Cross Origin Resource Sharing)](https://medium.com/@buddhiv/what-is-cors-or-cross-origin-resource-sharing-eccbfacaaa30)

[콘텐츠 보안 정책](https://developers.google.com/web/fundamentals/security/csp/?hl=ko)

[Cross-Origin Resource Sharing (CORS) - MDN](https://developer.mozilla.org/en-US/docs/Web/HTTP/CORS#Preflighted_requests)

(https://www.moesif.com/blog/technical/cors/Authoritative-Guide-to-CORS-Cross-Origin-Resource-Sharing-for-REST-APIs/)
