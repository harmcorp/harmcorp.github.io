---

title: "javascript hoist ?"
date:
  created: 2019-08-01
tags:
  - javascript
---

javascript 내부적으로 변수가 코드 상위로 끌어 올려지는 것을 말하는 것 같다  
어려운 개념이 아닌 것 같아서 예제 코드로 갈음한다.

```javascript
function whatIsHoist() {
  console.log(x); //undefined
  var x = 100;
  console.log(x); //100
}
/* 위와 같은 코드에서 변수 x 는
 * 내부적으로 Hoist(끌어올려지게) 되어
 * 아래와 같은 코드가 된다. */
function whatIsHoist() {
  var x;
  console.log(x); //undefined
  x = 100;
  console.log(x); //100
}
```

> `ECMAScript6` 에서 포함된 `let` 키워드로 만든 변수는 `hoist` 을 불허한다고 한다.

```javascript
console.log(a);
var a;
console.log(b); //VM383:3 Uncaught ReferenceError: Cannot access 'b' before initialization
let b;
```
