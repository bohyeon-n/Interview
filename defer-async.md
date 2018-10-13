# What are defer and async attributes on a `<script>` tag?

## 어떤 속성도 없는 `<script>`

`defer`와 `async` 두 속성 모두 없는 경우, 스크립트가 로드되고 동기식으로 실행된다. 문서를 파싱하다가 스크립트 태그를 만나면 스크립트의 로드가 끝날 때까지 문서의 파싱을 중지한다. 스크립트는 다운로드 된 순서대로 실행된다.

`defer` 와 `async` 없이 스크립트를 로드하는 방법

![without defer of async, script in 'head'](https://flaviocopes.com/javascript-async-defer/without-defer-async-head.png)

## `<script defer>`

`defer` 속성은 문서를 파싱하는 동안에 스크립트를 로드하지만 `DOMCounterLoaded` 이벤트 리스너 내에서 실행하는 것처럼 문서가 파싱될때까지 스크립트를 실행하지 않는다.
`defer` 스크립트는 순서대로 실행된다.

![whit defer, script in 'head'](https://flaviocopes.com/javascript-async-defer/with-defer.png)

**DOMContentLoaded?**

DOMContentLoaded 이벤트는 stylesheets, image, subframes 로드가 완료되기를 기다리지 않고 처음 HTML 문서를 완전히 로드되고 파싱될 때 발생한다. 이와는 달리 `load`이벤트는 full-loaded page 를 detect 할 때 사용한다.

## `<script async>`

`async`속성은 문서가 파싱되는 동안에 다운로드한다. 다운 로드가 끝나면 스크립트가 실행을 위해 HTML 파서가 일시중지한다. `async` 스크립트는 반드시 순서대로 실행되는 것이 아니다.

![with async, script in head](https://flaviocopes.com/javascript-async-defer/with-async.png)

`async`와 `defer`속성은 스크립트에 `scr`속성이 있을 때만 사용해야 한다.

```js
<script scr="myscript.js" />
<script src="myscript.js" defer/>
<script src="myscript.js" async/>
```

- `<head>`안에 `defer`스크립트를 넣으면 브라우저는 페이지를 파싱하고 있는 동안 스크립트를 다운로드할 수 있으므로 body 끝 부분 앞에 스크립트를 배치하는 것보다 더 나은 옵션이다.

- 스크립트가 서로 의존하면 `defer`를 사용해라

- 스크립트가 독립적이면 `async`를 사용해라

- DOM 의 준비가 필요해 content 가 DOMContentLoaded 리스너에 놓이지 않는경우 `defer`를 사용해라

## 참고자료

[EFFICIENTLY LOAD JAVASCRIPT WITH DEFER AND ASYNC](https://flaviocopes.com/javascript-async-defer/)
[]
