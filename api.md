# REST(Representational State Transfer)

> REST 는 웹상의 컴퓨터 시스템간에 표준을 제공하기 위한 아키텍처 스타일이다. 네트워크 아키텍처 원리는 자원을 정의하고 자원에 대한 주소를 지정하는 방법 전반을 일컫는다.

그럼 왜 REST architectural style 이 왜 필요한 것일까?

## SEPARATION OF CLIENT AND SERVER

REST 아키텍처 스타일에서 클라이언트와 서버는 서로에 대해 알지 못해도 독립적으로 수행될 수 있다. 즉 클라이언트 측의 코드가 서버 작동에 영향을 미치지 않고 언제든지 변경될 수 있다는 것을 의미한다. 서버 또한 클라이언트의 작동에 영향을 미치지 않고 서버측의 코드를 변경할 수 있다.

서버와 클라이언트가 서로에게 보낼 메시지의 포맷을 알고 있다면 클라이언트와 서버가 서로 분리되어 동작할 수 있게 된다. 데이터 저장 문제와 사용자 인터페이스의 문제를 분리하면, 플랫폼간 인터페이스의 유연성을 향상시키고, 서버의 구성요소를 단순화할 수 있다.

REST 인터페이스를 사용하여, 다른 클라이언트가 동일한 엔드포인트(여기서는 url 패턴)로 동일한 응답을 수신할 수 있다.

## STATELESSNESS

REST 패러다임을 따르는 시스템에서 서버는 클라이언트의 상태를 알 필요가 없다. 이전 메시지를 보지 않고도 서버와 클라이언트 모두 수신 된 모든 메시지를 이해할 수 있다.

## COMMUNICATION BETWEEN CLIENT AND SERVER

REST 아키텍처에서 클라이언트는 리소스를 얻고, 수정하기 위해 요청을 보낸다. 그리고 서버는 요청에 대한 응답을 보낸다.

요청과 응답을 보내는 방법의 표준은 다음과 같다.

### MAKING REQUESTS

클라이언트가 서버에서 데이터를 얻거나 수정하기 위해 서버에 요청을 해야 한다.
요청은 다음으로 구성된다.

- 수행 할 작업의 종류를 정의하는 HTTP 메소드
- 요청에 대한 정보를 전달하는 헤더
- 리소스에 대한 경로
- 데이터가 포함 된 바디(옵션)

#### HTTP VERBS

- GET -- 리소스 검색 (by id)
- POST -- 리소스 생성
- PUT -- 특정 리소스 업데이트 (by id)
- DELETE - 특정 리소스 삭제 (by id)

[What is CRUD?](https://www.codecademy.com/articles/what-is-crud)

#### HEADERS AND ACCEPT PARAMETERS

리퀘스트 헤더에 클라이언트는 콘텐트 타입을 보낼 수 있다. 이것을 Accept 필드라고 부른다. 서버가 클라이언트가 이해할 수없거나 처리할 수 없는 데이터를 보내지 않도록 한다. 콘텐츠 타입 옵션은 MIME 유형(or Multipurpose Internet Mail Extensions, [MDN 문서](https://developer.mozilla.org/en-US/docs/Web/HTTP/Basics_of_HTTP/MIME_types)) 이다.

Accept filed 컨텐츠 형식을 지정하는 데 사용되는 MIME 형식은 type 과 subtype 으로 구성되며 슬래시(/\)로 구분한다.

예를들어 HTML 이 들어있는 text 파일은 text/html 유형으로 지정된다.

다른 유형 및 일반적으로 사용되는 subtype

- image - image/png, image/jpeg, image/gif
- audio - audio/wav, image/mpeg
- video - video/mp4, video/ogg
- application - application/json, application/pdf, application/xml, application/octet-stream

요청 헤더

```
GET /articles/24
Accept: text/html, application/xhtml
```

#### PATHS

리퀘스트에는 작업을 수행해해야 하는 리소스에 대한 경로가 포함되어야 한다. RESTful API 에서 경로는 클라이언트가 진행중인 작업이 무엇인지 알 수 있도록 설계되어야 한다.

일반적으로 경로의 첫 번째 부분은 자원의 복수 형식이어야 한다. 예를 들어 customers/1
고객들 중 1 번 아이디 처럼 계층적으로 작성해야 한다.
'store.com/customers/223/orders/12`
특정한 리소스를 얻고 싶을 때는 식별자(id)를 포함한 경로를 작성해야 한다.

### SENDING RESPONSE

#### CONTENT TYPES

서버가 데이터 페이로드를 보내는 경우, 서버는 응답 헤더에 content-type 를 반드시 포함해야 한다. content-type 헤더 필드는 클라이언트에게 바디 데이터 유형을 알려준다. content-type 요청 헤드에서와 마찬가디로 MIME 형식이다.
서버가 응답하는 content-type 은 클라이언트가 요청 accept 필드에서 지정한 옵션 중 하나여야 한다.

예를 들어서 클라이언트가 aricles 의 1 번 id 의 리소스를 요청한다면

```
GET /articles/1  HTTP/1.1
Accept: text/html, application/xhtml
```

서버는 응답 헤더와 함꼐 콘텐츠를 보내야 한다.

```
HTTP/1.1 200(OK)
Content-Type: text/html
```

클라이언트가 요청 헤더에 보냈던 콘텐츠 타입인 text/html content-type 으로 응답 바디에 리턴되고 있다는 의미이다.

#### RESPONSE CODES

서버로부터의 응답은 상태 코드를 포함하고 있다. 상태 코드는 클라이언트가 요청한 작업의 성공 상태를 클라이언트에게 알리는 코드이다. [HTTP Status Codes](https://www.restapitutorial.com/httpstatuscodes.html)

- 200(OK)
  HTTP 요청이 성공하였다.
- 201(CREATED)
  성공적으로 생성되었다,
- 204(NO CONTENT)
  요청에 성공하였고, 리턴할 응답 바디가 없다.
- 400(BAD REQUEST)
  bad request syntax, excessive size 또는 다른 클라이언트 오류로 요청을 처리 할 수 없다.
- 403(FORBIDDEN)
  클라이언트가 리소스에 대한 접근 권한이 없다.
- 404(NOT FOUND)
  리소스를 찾을 수 없다. 리소스가 삭제되었거나 더이상 존재하지 않음
- 500(INTERNAL SEVER ERROR)

각각의 HTTP 메소드는 요청이 성공했을 시에 서버에게 상태코드를 받아야 한다.

- GET - return
- POST - return 201
- PUT - return 200
- DELETE - return 204

#### EXAMPLES OF REQUESTS AND RESPONSES

`store.com` 에 요청을 보낸다면

모든 고객을 보고 싶을 때 요청은 아래와 같다.

```
GET http://fashionboutique.com/customers
Accept: application/json
```

```
Status Code: 200 (OK)
Content-type: application/json
```

그다음 고객들의 데이터가 applicaion/json 형식으로 요청된다.

새로운 고객을 생성하고 싶을 때 posting data

```
POST http://fashionboutique.com/customers
Body:
{
  “customer”: {
    “name” = “Scylla Buss”
    “email” = “scylla.buss@codecademy.org”
  }
}
```

서버는 이 객체에 대한 id 를 생성한다. 그리고 클라이언트에게 아래와 같은 헤더를 반환한다.

```
201 (CREATED)
Content-type: application/json
```

특정 고객의 정보를 보고 싶다면

```
GET http://fashionboutique.com/customers/123
Accept: application/json
```

```
Status Code: 200 (OK)
Content-type: application/json
```

그리고 id 가 123 인 고객의 정보에 대한 데이터가 application/json 형식으로 표시된다.

특정 고객의 정보를 수정하고 싶다면

```
PUT http://fashionboutique.com/customers/123
Body:
{
  “customer”: {
    “name” = “Scylla Buss”
    “email” = “scyllabuss1@codecademy.com”
  }
}
```

삭제하고 싶다면

```
DELETE http://fashionboutique.com/customers/123
```

## 참고자료

[RESTfUl-API](https://searchmicroservices.techtarget.com/definition/RESTful-API)

[What is REST?](https://www.codecademy.com/articles/what-is-rest)
