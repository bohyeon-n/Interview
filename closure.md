# What is a closure? Can you give a useful example of one?

> 클로저는 다른 함수의 안에서 정의된 함수이다. 함수가 특정 스코프에 접근할 수 있도록 의도적으로 그 스코프에서 정의하는 경우가 많다. 스코프를 함수 주변으로 좁히는(closing)하는 것이라고 생각해도 좋다.

```js
let globalFunc; // 정의되지 않은 전역 함수
{
  let blockVar = "a"; // 블록 스코프에 있는 변수
  globalFunc = function() {
    console.log(blockVar);
  };
}
globalFunc();
```

globalFunc 는 블록 안에서 값을 할당받았다. 이 블록 스코프와 그 부모인 전역 스코프가 클로저를 형성했다. globalFunc 를 어디서 호출하든, 이 함수는 클로저에 들어있는 식별자에 접근할 수 있다.

globalFunc 를 호출하면, 이 함수는 스코프에서 빠져나왔음에도 불구하고 blockVar 에 접근할 수 있다. 일반적으로 스코프에서 빠져나가면서 해당 스코프에서 선언한 변수는 메모리에서 제거해도 안전하다. 하지만 여기서는 스코프 안에서 함수를 정의했고 해당 함수는 스코프 밖에서도 참조할 수 있으므로 자바스크립트는 스코프를 계속 유지한다.

즉, 스코프 안에서 함수를 정의하면 해당 스코프는 더 오래 유지된다. 또한 일반적으로는 접근할 수 없는 것에 접근할 수 있는 효과도 있다.

```js
let f; // 정의되지 않은 함수
{
  let o = { note: "Safe" };
  f = function() {
    return o;
  };
}
let oRef = f();
oRef.note = "Not so safe after all";
```
