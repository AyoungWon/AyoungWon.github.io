---
layout: post
title: "[Javascript]제어문"
author: "Wayne"
tags: javascript study
excerpt_separator: <!--more-->
---

<span style="color:rgba(0,0,0,0)">카테고리 설명</span>

<!--more-->

<br/><br/><br/>

# 제어문

자바스크립트는 위에서 아래로 순차적으로 실행된다. 하지만 특정 조건이나 상황에 따라 해당 코드를 실행하지 않거나, 실행거나, 반복해야하거나 하는 경우가 발생한다. 이때 우리는 제어문을 사용해 처리할 수 있다.
제어문을 사용하면 코드의 실행 흐름을 인위적으로 제어할 수 있다.

## 1. 블록문

코드를 중괄호로 묶은 것을 의미한다. 자바스크립트는 블록문을 하나의 실행 단위로 취급한다. 블록문은 단독으로 사용할 수 있으나 일반적으로 제어문이나 함수를 정의할 때 사용한다.

```js
{
  let foo = "foo";
}

function sum(a, b) {
  return a + b;
}
```

## 2. 조건문

주어진 조건식의 평가 결과에 따라 블록문의 실행을 결정하는 문을 말한다. 조건식은 불리언 값으로 평가될 수 있는 표현식이여야 하며, 조건식이 불리언 값이 아닌 값으로 평가되는 경우 암묵적으로 불리언 값으로 강제 변환되어 실행할 코드 블록을 결정한다.

### 1) if else

`if...else`문은 조건식을 평가하여 true이면 `if`문의 코드 블럭이, false이면 `else`문의 코드 블럭이 실행된다.

```js
if (조건식) {
  // 조건이 참이면 여기 실행
} else {
  //조건식이 거짓이면 여기 실행
}

let isAdult;
let age = 100;

if (age > 19) {
  isAdult = true;
} else {
  isAdult = false;
}
```

만약에 나눠야하는 조건식이 1개가 아니라면 `else if`를 사용하여 조건과 블록문을 여러개로 나눌 수 있다.

```js
if (조건식1) {
  // 조건식1이 참이면 여기 실행
} else if (조건식2) {
  //조건식 1은 거짓이고 조건식2는 참이면 여기 실행
} else {
  //조건식 모두 거짓이면 여기 실행
}

let stage;
let age = 18;

if (age < 8) {
  stage = "kindergarden";
} else if (age >= 8 && age < 14) {
  stage = "elementary school";
} else if (age >= 14 && age < 17) {
  stage = "middle school";
} else {
  stage = "high school";
}
```

`if...else`문에서 `else if`와 `else`는 옵션 값이다. 즉, `if`문만 있어도 조건문은 실행된다.

### 2) switch

`switch`문은 평가를 받는 표현식이 주어지고 그 값과 일치하는 표현식을 갖는 **case문으로 실행 흐름을 옮긴다.(흐름을 옮긴다는 것은 그 부분만 실행하는 것이 아니라, case문으로 이동 후, 자바스크립트 실행 순서에 따라 아래로 쭉 실행한다)**
<br/>
`case`에 일치하는 표현식이 없는 경우,` default`문으로 실행 순서가 이동한다. **default는 옵션 값**이므로 없을 수도 있다.

```js
switch (expr) {
  case "Oranges":
    //expr가 'Oranges'면 여기 코드가 실행
    break;
  case "Mangoes":
    //expr가 'Mangoes'면 여기 코드가 실행
    break;
  case "Papayas":
    //expr가 'Papayas'면 여기 코드가 실행
    break;
  default:
  //expr이 위 case를 모두 만족하지 못할 때 여기 코드가 실행
}
```

<span class="bg_highlight">만약 각 case마다 break가 없다면, 해당 case로 이동한 후에 코드 블럭을 실행하고 난 뒤, switch문을 빠져나오는 것이 아니라, 자바스크립트 실행 순서에 따라 해당 case 밑의 코드를 순서대로 실행한다(fall through). </span>

```js
const expr = "Apples";
let fruits;

switch (expr) {
  case "Oranges":
    fruits = "Oranges";
  case "Apples":
    fruits = "Apples";
  case "Bananas":
    fruits = "Bananas";
  case "Cherries":
    fruits = "Cherries";
  case "Mangoes":
  case "Papayas":
    fruits = "Papayas";
}

console.log(fruits); // 'Papayas'
// 'Apples'가 할당된 뒤 break가 없으므로 'Bananas', 'Cherries','Papayas' 순서대로 재할당 됨
```

> `break`는 레이블문, 반복문, switch문의 코드 블록을 탈출하는 역할을 한다.

## 3. 반복문

반복문은 조건식의 평가 결과가 true인 경우 코드 블록을 반복해서 실행하는 것을 말한다. 즉, 조건식이 거짓일때 까지 코드블록을 반복한다.

### 1) while

`while`문은 소괄호 안의 조건이 만족하면 블록 안의 코드를 반복적으로 실행한다.

```js
let n = 0;
let x = 0;
while (n < 3) {
  //n이 3보다 크거나 같아지면 반복을 멈춤
  n++;
  x += n;
}
```

### 2) for

`for`문은 `while`문과 다르게 시작 조건과 변수의 증감을 소괄호 안에 표현한다.

```js
let step;
for (step = 0; step < 5; step++) {
  // Runs 5 times, with values of step 0 through 4.
  console.log("Walking east one step");
}
```

### Reference

> [모던 자바스크립트 deep dive](https://wikibook.co.kr/mjs/) <br/> [MDN Control_flow_and_error_handling](https://developer.mozilla.org/ko/docs/Web/JavaScript/Guide/Control_flow_and_error_handling) <br/> [MDN Statements](https://developer.mozilla.org/ko/docs/Web/JavaScript/Reference/Statements)
