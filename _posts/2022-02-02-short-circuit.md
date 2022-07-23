---
layout: post
title: "[Javascript]단축 평가"
author: "Wayne"
tags: javascript
excerpt_separator: <!--more-->
---

<span style="color:rgba(0,0,0,0)">javascript 개발에서 가장 많이 쓰는 꿀팁이라고!</span>

<!--more-->

<br/><br/><br/>
개인적으로 프론트개발자로서 리액트로 개발시 조건문이나 렌더링에서` && ?? ?. ||`와 같은 연산자를 쓸 일이 많았는데 왜 이렇게 작동하는가에 대해서는 명확히 모르고 이렇게 쓰면 이렇게 작동한다 정도 수준으로만 알고 사용하고 있었다. 이번 챕터에서 튿히 단축 평가를 공부하면서 이유를 확실하게 알게되어 너무 좋았다. 자바스크립트 개발자라면 꼭 알아야 하는 챕터라고 생각된다.

# 단축 평가

논리 연산의 결과를 결정하는 피연산자를 타입 변환하지 않고 그대로 반환하는 것이다. 단축 평가는 표현식을 평가하는 도중에 평가 결과가 확정된 경우 나머지 평가 과정을 생략한다.<br/>

### 1) 논리 연산자를 사용한 단축 평가

```js
// 논리합(||) 연산자
"Cat" || "Dog"; // -> "Cat" , 평가가 "Cat"에서 참이 되었으므로 평가 종료
false || "Dog"; // -> "Dog" , 평가가 "Dog"에서 참이 되었으므로 평가 종료
"Cat" || false; // -> "Cat" , 평가가 "Cat"에서 참이 되었으므로 평가 종료

// 논리곱(&&) 연산자
"Cat" && "Dog"; // -> "Dog" , 평가가 "Dog"까지 와서 참이 되엇으므로 평가 종료
false && "Dog"; // -> false , 평가가 false에서 거짓이 되었으므로 평가 종료
"Cat" && false; // -> false , 평가가 false에서 거짓이 되었으므로 평가 종료
```

하지만 조건문에서 논리 연산자와 비교 연산자와 함께 사용했을때 `boolean` 값이 반환된 것을 본적이 있을 것이다.
이때도 사실 논리 연산자의 단축 평가가 마찬가지로 적용 된 것인데, 결과를 결정한 좌항 혹은 우항의 비교 연산 결과 값인 `boolean` 값을 반환한 것이기 때문이다.

```js
// 논리 연산자와 비교 연산자를 함께 사용
"Cat".length > 0 && typeof "Dog" === "string"; // -> true
"Cat" || typeof "Dog" === "string"; // -> "Cat"
typeof "Dog" !== "string" && typeof "Cat" === "string";
```

단축 평가를 사용하면 `if`문을 대체할 수 있다.

```js
let done = true;
let message = "";

// if문
if (done) message = "완료";

// 단축평가
message = done && "완료"; // done이 true라면 message에 '완료'를 할당
console.log(message); // 완료
```

```js
let done = true;
let message = "";

// if문
if (!done) message = "미완료";

// 단축평가
message = done || "미완료"; // done이 false라면 message에 '미완료'를 할당
console.log(message); // 미완료
```

객체일 것이라고 예상하는 변수가 객체가 아닌 `null`이나 `undefined`가 아닌지 확인하고 프로퍼티를 참조할때 유용하게 사용할 수 있다.

```js
let elem = null;

let value = elem && elem.value; // -> null
// value = elem ?  elem.value :  elem  같은 의미
```

함수 매개변수의 기본값을 설정하는 용도로도 사용할 수 있다. 함수 내부에서 사용하는 변수의 값을 매개변수 인자로 전달하지 않으면 `undefined`가 할당되는데, 당연히 함수 내부에서 사용되는 값에 아무 값도 없으니 에러가 난다. 이것을 예방하기 위해 논리합 연산자로 기본값을 할당할수 있다.

```js
// 단축 평가를 사용한 매개변수의 기본값 설정
function getStringLength(str) {
  str = str || ""; // str = srt ? srt : ''와 값은 뜻, str이 있으면 str, 없으면 ''.
  return str.length;
}

getStringLength(); // -> 0
getStringLength("hi"); // -> 2
```

### 2) 옵셔널 체이닝 연산자(Optional chaining) `?.`

`?.`좌항의 피연산자가 `null` 또는 `undefined`인 경우 `undefined`를 반환하고, 아니면 우항의 프로퍼티를 참조를 이어간다.

```js
let elem = null;

let value = elem?.value;
// elem이 null 또는 undefined이면 undefined를 반환하고,
// 그렇지 않으면 우항의 프로퍼티 참조를 이어간다.
// value = elem ? elem.value : undefined;
console.log(value); // undefined
```

> `&&`연산자와 `?.`연산자가 다른 점은 `&&`은 좌항의 피연산자가 **falsy**한 값이면 좌항의 값을 다시 리턴하지만, `?.` 연산자는 **falsy**한 값이라도 `undefined`나 `null`이 아니라면 프로퍼티 참조를 진행한다.

```js
let emptyStr = "";

// 문자열의 길이(length)를 참조한다.
let length = emptyStr && emptyStr.length;

// 문자열의 길이(length)를 참조하지 못한다.
console.log(length); // '', emptyStr이 falsy한 값이기 때문에

length = emptyStr?.length;
console.log(length); // 0
```

### 3) null 병합 연산자 `??`

`??`좌항의 피연산자가 `null` 또는 `undefined`인 경우 우항의 피연산자를 반환하고, 그렇지 않으면 좌항의 피연산자를 반환한다. 주로 기본값을 설정해줄 때 매우 유용하다.

```js
let foo = null ?? "default string";
console.log(foo); // "default string"
```

> 논리 연산자의 `||`와 다른 점은 `||`은 **falsy**한 값이면 모두 우항의 값을 반환하는 반면, `??`연산자는 `undefined`나 `null`이 아닌 **falsy**값일 경우 좌항의 값을 반환한다는 점이다.

```js
let foo = "";

let result1 = foo ?? "default string";
let result2 = foo || "default string";
console.log(result1, result2); // "", 'default string'
```

### Reference

> [모던 자바스크립트 deep dive](https://wikibook.co.kr/mjs/)
