# synchronous and asynchronous code in javascript

> 동기(sync)는 다음 코드의 실행은 이전 코드의 작업이 끝날 때 까지 대기한다. 순차적으로 실행된다. 비동기는(async)는 다른 동작이 처리되는 동안 작업을 할 수 있다.

자바스크립트에서 모든 코드는 싱글 스레드에 특성으로 인해 동기식이다. 그러나 비동기 작업(XMLHttpRequest 나 setTimeout)은 native code(브라우저 API)에 의해 컨트롤 되기 때문에 메인 스레드 외부에서 실행된다. 그러나 프로그램의 콜백은 여전히 동기식으로 실행된다.

자바스크립트는 "event loop" 모델을 기반으로 하는 concurrency model 을 가지고 있다.

'alert'와 같은 함수는 메인 스레드를 block 한다. 그래서 사용자가 닫을 때까지 사용자의 입력이 등록될 수 없다.

## 자바스크립트 이벤트 루프

### 자바스크립트?

> a single-threaded non-blocking asynchronous concurrent language. i have a call stack, an event loop, a callback queue, some other apis and stuff

v8? 크롬의 런타임? 싱글 스레드? 콜백?

![javascript event loop](https://cdn-images-1.medium.com/max/752/1*7GXoHZiIUhlKuKGT22gHmA.png)

v8 런타임과 브라우저가 제공하는 웹 API 가 있다. 브라우저는 DOM, AJAX, timeout 등과 함께 evnet loop 와 콜백 큐를 가지고 있다.

**one thread == one call stack == one thing at a time**

싱글 스레드 런타임을 가지고 있다는 말은 결국 한 번에 하나의 싱글 콜 스택만을 가지고 있다는 말이다. 하나의 프로그램은 동시에 하나의 코드만 실행할 수 있다는 것이다.

### blocking

blocking 은 느리게 동작하는 코드이며 느린 동작이 스택에 남아있는 것을 보통 블로킹이라고 말하게 된다.

프로그래밍 언어에서 싱글 스레드라는 것은 루비 같은 언어와는 달리 여러개의 스레드를 사용하지 않는다는 의미이다. 웹브라우저에서 코드가 실행되고 있기 때문에 브라우저에서 다른 작업을 할 수 없다. 브라우저는 모든 리퀘스트가 완료될 때까지 멈춰있다. 유려한 UI 를 만드려면 콜스택을 멈추게 해서는 안된다.

### asynchronous callback

브라우저 혹은 노드에는 블로킹 함수가 거의 없다. 대부분 비동기로 만들어졌다. 이는 어떤 코드를 실행하면 결국 콜백을 받고 이걸 나중에 실행한다는 말이다.

```js
console.log("hi");
setTimeout(function() {
  console.log("there");
}, 5000);
console.log("js");
```

setTimeout console.log 로 'hi' 를 출력하도록 하면 console.log 는 큐에 등록되고 'js'를 먼저 출력한다. 5 초 뒤에 there 을 찍게 된다.

자바스크립트는 한 번에 하나밖에 할 수 없는데 어떻게 가능할까? 여기서 이벤트 루프와 동시성이 역할을 하게 된다.

### Concurrency & the Event Loop

자바스크립트는 다른 코드를 실행시키는 동안 Ajax 요청을 실행할 수 없다. setTimeout 역시 마찬가지다.
하지만 우리가 이걸 동시에 할 수 있는 이유는 브라우저는 단순 런타임 이상을 의미하기 때문이다.
브라우저가 API 를 제공하기 때문에 자바스크리립트에서 호출할 수 있는 스레드를 효과적으로 지원한다. 여기에 동시성이 들어오는 것이다.

**setTimeout 을 호출하면 일어나는 일**

setTimeout 은 브라우저에서 제공하는 API 이다. v8 소스코드에 존재하지 않는다. 자바스크립트가 실행되는 런타임 환경에 존재하는 별도의 API 이다.
브라우저가 타이머를 실행시키고 카운트 다운을 시작한다. setTimeout 호출 자체는 완료되었다는 의미이고, 스택에서 함수를 지울 수 있다. 그 다음 'js'를 출력하고 지워진다. 이제 Web API 에서 실행하고 있는 타이머가 남아있다. 5 초 뒤에 타이머가 종료된다. 모든 Web API 는 작동이 완료되면 콜백을 테스크 큐에 푸시한다.

이벤트 루프란 무엇일까? 이벤트 루프의 역할은 콜 스택과 테스크 큐를 주시하는 것이다. 스택이 비어있으면, 큐의 첫번째 콜백을 자바스크립트 영역인(v8 엔진) 스택에 쌓아 효과적으로 실행할 수 있게 해준다. 이제 console.log('there')을 실행한다.

`setTimeout 0`를 샐행하는 이유는 일반적으로 스택이 비어있을 떄까지 기다리게 하기 위해서이다.(스택에 마지막까지 지연시키는 이유)

timeout 이 설정된 시간대로 동작하지 않을 수 있다. 다만 딜레이되는 최소의 시간만을 지정할 수 있다.

**콜백**

```js
//synchronous
[1, 2, 3, 4].forEach(function(i) {
  console.log(i);
});
//asynchronous
function asyncForEach(array, cb) {
  array.forEach(function() {
    setTimeout(cb, 0);
  });
}
asyncForEach([1, 2, 3, 4], function(i) {
  console.log(i);
});
```

콜백은 다른 함수가 부르는 함수이다. 혹은 앞으로 큐에 쌓일 비동기식 콜백이라고 묘사할 수 있다.
forEach 함수의 경우, 함수를 실행시키기는 한다. 콜백이라고 할 수 있지만 비동기적으로 실행하지는 않는다. 자신의 자체적 스택에서 실행시킨다.

첫 번째 블록의 경우 스택을 차지한다. 실행이 다 끝날 때 까지
반대로 비동기 버전은 여러 개의 콜백을 큐에 쌓고, 스택이 비워지면 실제로 쌓인 콜백들이 실행된다.

브라우저는 자바스크립트로 하는 무언가로 인해 제약을 받는다. 브라우저는 기본적으로 화면을 매 16.6 밀리세컨드, 즉 1 초에 60 프레임을 리페인트하는 것이 정상이다. 그게 제일 빠른것이다. 그러나 스택에 코드가 있으면 렌더링을 못한다. 렌더도 하나의 콜백처럼 행동하기 때문이다.
스택이 비워질때까지 기다려야 한다. 다른 점이라면, 렌더는 콜백에 비해 더 높은 우선순위를 갖는다. 매 16 밀리세컨드마다 큐에 렌더가 들어가고, 스택이 깨끗해진 후에야 렌더링을 한다.

각 배열 요소에 대해 오래 걸리는 처리를 해야한다고 치면, 렌더는 막히게 된다. 렌더가 막히면, 화면의 텍스트를 선택하거나 선택해서 반응을 보거나 하는게 불가능하다. 렌더에게 각 요소 중간중간에 렌더가 끼어들 수 있는 기회를 줄 수 있다. 큐가 async 를 통해 쌓여있기 떄문에

event loop 를 막지말라고 할 때 바로 이런 현상을 뜻하는 것이다.

스크롤 이벤트는 DOM 에서 매우 자주 일어난다. document.scroll 가 일어날 때 애니메이션을 넣으면 큐에 엄청나게 많은 콜백을 쌓는다. 그리고 매번 이걸 처리하면서 각각의 느린 프로세싱이 일어날 때 마다, 스택을 채우지는 않지만, 큐를 이벤트로 가득 채운다. 이런 동작을 알고있으면, 매 몇초마다 혹은 유저가 스크롤을 멈출 때 까지 작업량을 줄인다든지 하는 결정을 내릴 수 있다.

## 참고

[Philip Roberts: What the heck is the event loop anyway? | JSConf EU](https://2014.jsconf.eu/speakers/philip-roberts-what-the-heck-is-the-event-loop-anyway.html)
