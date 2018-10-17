# cache busting 의 목적과 방법

![cache-busting](https://cdn.keycdn.com/support/wp-content/uploads/2016/06/cache-busting.webp)

>

브라우저는 임시적으로 웹사이트에 파일을 저장하는 캐시가 있어서 페이지를 전환하거나 같은 페이지를 다시 로드할 때 다시 다운로드할 필요가 없다. 서버는 주어진 시간동안 파일을 저장하도록 브라우저에 알려주는 헤더를 보내도록 설정되어있다. 이것은 웹 사이트의 속도를 크게 높일 수 있다.

그러나 웹사이트를 업데이트 하는 경우, 문제가 생길 수 있다. 사용자는 아직 업데이트 되기 전의 파일을 가지고 있기 때문이다. 캐시 된 CSS 파일 및 Javascript 파일이 더 이상 존재하지 않거나 이동되었거나 이름이 변경된 요소를 참조하는 경우 업데이트 되지 않고 오래전 기능을 유지하거나 웹 사이트를 중단시킬 수 있다.

Cache busting 은 브라우저에게 새로운 파일을 다운로드 하도록하는 프로세스이다. cache busting 은 그 전 파일과는 다른 이름으로 지정하여 사용할 수 있다.

# cache busting 방법

- query string

```js
src="js/script.js" => src "js/script.js?v=2"
```

- file name versioning

```
style.v2.css;
```

- file path versioning

```
v2/style.css;
```

file name versioning 과 file path versioning 은 cache busting 방법으로 권장된다. caching 메커니즘을 방해하지 않으면서 수정된 파일을 반영하도록 쉽게 업데이트할 수 있다.

그러나 query string 방법은 caching 문제를 일으킨다고 알려져 있다. 일부 프록시 또는 CDN 은 쿼리 문자열이 스트링이 포함 된 파일을 캐시할 수 없으므로 사용하지 않는 것이 좋다.

cache busting 에 어떤 방법을 사용하던 파일 이름/ 경로가 수정되면 파일을 참조하는 HTML 도 업데이트해야한다.

# Cache Busting Example

1. CSS(style.css)파일의 만료 기간을 1 년으로 설정하였다. 즉 한 번 사용자의 브라우저에 캐시되면 브라우저는 1 년 동안 origin server 를 다시 확인하지 않아도 된다.

2. 3 개월 후에 style.css 파일을 수정하기로 하였다. css 파일을 수정하여 이 파일을 같은 이름으로 서버에 재 업로드하였다. 그러나 브라우저는 파일이 수정된 사실을 알지 못하며 계속 이전 버전의 style.css 을 전달한다.

3. 반면에 cache busting 을 구현하면 즉 파일의 이름을 style.v2.css 와같이 수정하고 이 변경 사항을 반영하도록 페이지의 HTML 을 업데이트 하면 브라우저는 갱신해야 하는 새 파일이 있음을 알게 되고 즉시 파일을 갱신 한다.
