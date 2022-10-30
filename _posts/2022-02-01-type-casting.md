---
layout: post
title: "[Javascript]타입 변환"
author: "Wayne"
tags: javascript study
excerpt_separator: <!--more-->
---

<span style="color:rgba(0,0,0,0)">조건문에 boolean이 아닌 값을 넣어도 작동하는 이유</span>

<!--more-->

<br/><br/><br/>

# 타입 변환

자바스크립트의 모든 값에는 타입이 있고, **개발자가 의도적으로 타입을 변환시키는 것을 명시적 타입변환/ 타입캐스팅** 이라고한다. 반면 **자바스크립트의 엔진에 의해 암묵적으로 타입이 자동 변환되는 것을 암묵적 타입 변환/타입 강제 변환**이라고 한다.

원시값은 변경 불가능한 값이므로, 타입 변환이 기존 원시값을 직접 변경하는 것은 아니다. 기존 원시값을 사용해 다른 타입의 새로운 원시값을 생성한다.

```js
// 타입캐스팅, 명시적 타입 변환

let num = 100;
let str = num.toString(); // 숫자를 문자열로 타입 캐스팅한다.
console.log(typeof str, str); // string 100

// num 변수의 값이 변경된 것은 아니다.
console.log(typeof num, num); // number 100

// 암묵적 타입 변환

let num = 100;
var str = num + ""; // 문자열 연결 연산자는 숫자 타입 x의 값을 바탕으로 새로운 문자열을 생성한다.
console.log(typeof str, str); // string 100

// x 변수의 값이 변경된 것은 아니다.
console.log(typeof num, num); // number 100
```

## 1. 암묵적 타입 변환

암묵적 타입 변환은 자바스크립트 엔진이 표현식을 에러 없이 평가하기 위해 피연산자의 값을 연산자에 맞게 타입 변환한 새로운 값을 평가하는 순간에 사용하고 버린다.

#### 1) 문자열 타입 변환

코드의 문맥상 문자열이어야 하는 값이 문자열이 아닌 경우 문자열로 타입 변환하여 사용한다.

```js
// 코드의 문맥상 피연산자가 문자열이어야 하는 경우

// 1개 이상의 문자열 피연산자와 + 연산자
"10" +
  2 // 템플릿 리러털 // -> '102'
  `1 + 1 = ${1 + 1}`; // -> "1 + 1 = 2"
```

일반적으로 문자열 피연산자와 문자열이 아닌 피연산자가 + 연산자로 평가되는 경우 그대로 문자열로 반환한다

```js
1 +
  "" - // -> "1"
  1 +
  ""; // -> "-1"
NaN + ""; // -> "NaN"
```

하지만 심벌과 객체 처럼 예외 경우도 있으니 유의한다.

```js
// 심벌 타입
(Symbol()) + '' // -> TypeError: Cannot convert a Symbol value to a string

// 객체 타입
({}) + ''           // -> "[object Object]"
Math + ''           // -> "[object Math]"
[] + ''             // -> ""
[10, 20] + ''       // -> "10,20"
(function(){}) + '' // -> "function(){}"
Array + ''          // -> "function Array() { [native code] }"
```

### 2) 숫자 타입 변환

산술 연산자와 비교 연산자는 수학적 연산을 하거나, 숫자의 크기를 비교하여 불리언 값을 만드는 것이므로, 피연산자가 숫자 타입이어야 문맥적으로 맞는 표현식이 된다. 또한 단항 연산자(+피연산자)와 같은 경우 숫자 타입으로 자동 변환한다

```js
+"" + // -> 0
  "0" + // -> 0
  "1" + // -> 1
  // true는 숫자타입으로 1
  true + // -> 1
  // false와 null은 숫자타입으로 0
  false + // -> 0
  null + // -> 0
  // undefined은 0이 아니라 NaN이 반환됨 주의
  undefined + // -> NaN\
  //심벌 타입은 아얘 변환 못함
  Symbol() + // -> typeError: Cannot convert a Symbol value to a number
  // 객체 타입은 빈배열을 제외하고 NaN으로 변환
  {} + // -> NaN
  [] + // -> 0 주의
  [10, 20] + // -> NaN
  function () {}; // -> NaN
```

### 3) Boolean타입 변환

제어문의 조건식에는 불리언 값으로 평가되어야 하는 값이다. **조건식 안의 값이 불리언 값이 아니더라도 암묵적으로 불리언 값으로 변환한다.** 이때, falsy한 값은 false로 변환하고 falsy가 아닌 값들은 모두 truthy한 값으로 보고 true로 변환한다.

> falsy한 값<br/> false, undefiend, null, 0 , -0, NaN, ''(빈문자열)

```js
let x1;
let x2 = null;
let x3 = 0;
let x4 = NaN;

if (x1) console.log(`${x1}은 truthy값`);
else console.log(`${x1}은 falsy값`);
//결과 => undefined은 falsy값

if (x2) console.log(`${x2}은 truthy값`);
else console.log(`${x2}은 falsy값`);
//결과 => null은 falsy값

if (x3) console.log(`${x3}은 truthy값`);
else console.log(`${x3}은 falsy값`);
//결과 => 0은 falsy값

if (x4) console.log(`${x4}은 truthy값`);
else console.log(`${x4}은 falsy값`);
//결과 => NaN은 falsy값
```

## 2. 명시적 타입 변환

타입을 변경하려는 개발자의 의도를 명확히 나타내면서 타입을 변경 하는 방법이다.

### 1) 문자열 타입 변환

`String` 생성자 함수와 `toString`함수로 문자열이 아닌 것을 문자열로 타입 변환 할 수 있다.

```js
// String 생성자 함수를 new 연산자 없이 호출
String(1); // -> "1"
String(NaN); // -> "NaN"
String(Infinity); // -> "Infinity"
String(true); // -> "true"
String(false); // -> "false"

// Object.prototype.toString
(1).toString(); // -> "1"
NaN.toString(); // -> "NaN"
Infinity.toString(); // -> "Infinity"
true.toString(); // -> "true"
false.toString(); // -> "false"
```

### 2) 숫자 타입

숫자 타입이 아닌 값을 숫자 타입으로 변경하는 방법은 여러 가지가 있다. 산술 연산자와 함께 쓰거나 `Number`, `ParseIn`t와 같은 함수를 사용하는 방법도 있다.

```js
// Number 생성자 함수를 new 연산자 없이 호출
Number("0"); // -> 0
Number("-1"); // -> -1
Number("10.53"); // -> 10.53
Number(true); // -> 1
Number(false); // -> 0

// parseInt, parseFloat 함수를 사용(문자열만 변환 가능)
parseInt("0"); // -> 0
parseInt("-1"); // -> -1
parseFloat("10.53"); // -> 10.53

// + 단항 산술 연산자
+"0"; // -> 0
+"-1"; // -> -1
+"10.53"; // -> 10.53
+true; // -> 1
+false; // -> 0

// * 산술 연산자
"0" * 1; // -> 0
"-1" * 1; // -> -1
"10.53" * 1; // -> 10.53
true * 1; // -> 1
false * 1; // -> 0
```

### 3) Boolean타입 변환

boolean 타입으로의 변환은 `Boolean` 생성자 함수를 사용하는 방법과 실제로는 `!!`나 `!`처럼 부정 논리 연산자를 사용하여 변환하는 방법을 주로 사용한다.

```js
// Boolean 생성자 함수를 new 연산자 없이 호출
// falsy한 값은 false로 반환, 나머지는 true
Boolean("x"); // -> true
Boolean(""); // -> false
Boolean("false"); // -> true
Boolean(0); // -> false
Boolean(1); // -> true
Boolean(NaN); // -> false
Boolean(Infinity); // -> true
Boolean(null); // -> false
Boolean(undefined); // -> false
Boolean({}); // -> true
Boolean([]); // -> true

// ! 부정 논리 연산자를 두번 사용하는 방법
!!"x"; // -> true
!!""; // -> false
!!"false"; // -> true 'false'가 문자열임을 주의
!!0; // -> false
!!1; // -> true
!!NaN; // -> false
!!Infinity; // -> true
// null 타입 => 불리언 타입
!!null; // -> false
!!undefined; // -> false
!!{}; // -> true
!![]; // -> true

// 부정 논리 연산자를 한번 사용하면 반대 불리언 값을 반환함
!"x"; // -> false
!""; // -> true
!"false"; // -> false
!0; // -> tur
!1; // -> false
```

### Reference

> [모던 자바스크립트 deep dive](https://wikibook.co.kr/mjs/)
