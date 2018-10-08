# How do you compare two objects in JavaScript?

두 개의 객체가 같은 값의 같은 프로퍼티를 가지고 있다고 하더라도, == 나 === 로 비교하면 같지 않다.

```js
const object1 = {
  a: 1
};
const object2 = {
  a: 1
};
object1 === object2; // false
object1 == object2; // false
```

객체를 비교할 때 객체의 레퍼런스(location memory)를 비교하기 때문이다. 반면에 원시 값은 (number, string ...)은 값을 비교한다.

두 개의 객체가 같은지 테스트하기 위해, helper function 이 필요하다. 이 함수는 각각의 객체의 프로퍼티가 같은 값인지 테스트한다. 중첩된 객체도 포함해서. 객체의 프로토타입 또한 3 번 째 인자로 true 를 전달하여 동일한지 테스트할 수 있다.

plain objects, arrays, functions, dates 그리고 원시값 이외의 데이터 구조에 대해서 동등성을 테스트하지 않는다.

```js
function isDeepEqual(obj1, obj2, testPrototypes = false) {
  if (obj1 === obj2) {
    return true;
  }

  if (typeof obj1 === "function" && typeof obj2 === "function") {
    return obj1.toString() === obj2.toString();
  }

  if (obj1 instanceof Date && obj2 instanceof Date) {
    return obj1.getTime() === obj2.getTime();
  }

  if (
    Object.prototype.toString.call(obj1) !==
      Object.prototype.toString.call(obj2) ||
    typeof obj1 !== "object"
  ) {
    return false;
  }

  const prototypesAreEqual = testPrototypes
    ? isDeepEqual(
        Object.getPrototypeOf(obj1),
        Object.getPrototypeOf(obj2),
        true
      )
    : true;

  const obj1Props = Object.getOwnPropertyNames(obj1);
  const obj2Props = Object.getOwnPropertyNames(obj2);

  return (
    obj1Props.length === obj2Props.length &&
    prototypesAreEqual &&
    obj1Props.every(prop => isDeepEqual(obj1[prop], obj2[prop]))
  );
}
```

- 숫자, 문자열과 같은 원시 타입은 값을 비교한다.
- 객체는 레퍼런스로 비교한다.

# Pass By Value And Pass By Reference In JavaScript

### Pass by Value

함수는 변수의 값을 인수로 직접 전달하여 호출된다. 함수 안에서 인수의 값을 바꿔도 함수 밖에는 영향을 주지 않는다.

자바스크립트는 항상 pass by value 이다.

```js
function callByValue(var1, var2) {
  console.log(`Inside Call by Value Method`);
  var1 = 100;
  var2 = 200;
  console.log(`var1 = ${var1}, var2 = ${var2}`);
}

let var1 = 10;
let var2 = 20;
console.log(`Before Call by Value Method`);
console.log(`var1 = ${var1}, var2 = ${var2}`);

callByValue(var1, var2);

console.log("After Call By Value Method");
console.log(`var1 =${var1}, var2 = ${var2}`);

// Before Call by Value Method
// var1 = 10, var2 = 20
// Inside Call by Value Method
// var1 = 100, var2 = 200
// After Call By Value Method
// var1 =10, var2 = 20
```

그러나 변수가 배열을 포함한 객체를 가리키고 있다면 값은 객체에 대한 참조이다.

### Pass by Reference

참조 전달에서는 함수는 reference/address 를 직접 전달하여 호출 된다. 함수 안에서 인수를 바꾸면 함수밖에 영향을 준다. 자바스크립트 객체와 배열은 pass by reference 이다.

```js
function callByReference(varObj) {
  console.log("Inside Call By Reference Method");
  varObj.a = 100;
  console.log("varObj");
}
const varObj = { a: 1 };
console.log("Before Call by Reference Method");
console.log(varObj);

callByReference(varObj);
console.log("After Call by Reference Method");
console.log(varObj);

// Before Call by Reference Method
// { a: 1 }
// Inside Call By Reference Method
// varObj
// After Call by Reference Method
// { a: 100 }
```

메소드의 인수로 객체나 배열을 전달하면 객체의 값이 바뀔 수 있다.

## 참고 자료

[[Javascript] Pass By Value And Pass By Reference In JavaScript](https://codeburst.io/javascript-pass-by-value-and-pass-by-reference-in-javascript-fcf10305aa9c)

```

```
