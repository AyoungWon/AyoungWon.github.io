---
layout: post
title: "[Javascript]Object 리터럴"
author: "Wayne"
tags: javascript
excerpt_separator: <!--more-->
---

<span style="color:rgba(0,0,0,0)">객체는 어떻게 작성해야 하는가</span>

<!--more-->

<br/><br/><br/>

# 객체(Object)

자바스크립트는 **객체 기반의 프로그래밍 언어**이다. 원시 값을 제외한 나머지 모두 객체이다. 객체 타입은 다양한 타입의 값을 하나의 단위로 구성한 복합적 **자료구조**이다. 원시 타입은 변경 불가능한 값이지만, <span class="bg_highlight">객체 타입의 프로퍼티 값은 변경 가능한 값</span>이다.

## 1. 객체 리터럴

리터럴은 사람이 이해할 수 있는 문자 또는 약속된 기호를 사용하여 값을 생성하는 표기법이다. 객체 리터럴은 결국 객체를 표기하기 위한 표기법인 것이며, 중괄호({})내에 0개 이상의 프로퍼티를 작성하여 정의한다. 프로퍼티를 작성하지 않으면 빈 객체가 생성된다. **여기서 중괄호는 코드블록을 의미하지 않는 다는 것에 주의하자!**

```js
let person = {
  name: "Lee", // 프로퍼티
  sayHello: function () {
    // 메서드
    console.log(`Hello! My name is ${this.name}.`);
  },
};

console.log(typeof person); // object
console.log(person); // {name: "Lee", sayHello: ƒ}
```

## 2. 프로퍼티 특징

객체의 프로퍼티는 키(key)와 값(value)으로 구성된다. 자바스크립트의 모든 값은 프로퍼티의 값으로 사용할 수 있으며 값에 함수가 오늘 경우 특별히 메서드라고 한다.

> - key: 빈 문자열을 포함하는 모든 문자열 또는 심벌 값
> - value: 자바스크립트에서 사용할 수 있는 모든 값

```js
let person = {
  // 프로퍼티 키는 name, 프로퍼티 값은 'Lee'
  name: "Lee",
  // 프로퍼티 키는 age, 프로퍼티 값은 20
  age: 20,
};
```

### 1) key값은 객체에서 프로퍼티 값에 접근할 수 있는 식별자 역할을 한다.

하지만 반드시 식별자 네이밍 규칙을 따라야 하는 것은 아니나, 따르지 않을 경우 문자열로 만들어서(따옴표로 감싸서)사용해야하며, 네이밍 규칙을 따른 프로퍼티 키는 자바스크립트 엔진이 암묵적으로 문자열로 변환한다.<br/><br/>
(숫자로만 된 키 값은 네이밍 규칙을 위반하지만 객체의 키 값으로는 암묵적으로 문자열이 된다).

```js
let object = {
  1: "hongil",
  2: "minsu",
  3: "chulsu",
};
```

### 2) 프로퍼티 키를 동적으로 생성할 수 있다. 이럴 경우 프로퍼티 키를 동적으로 생성할 표현식을 대괄호([])로 묶어준다.

```js
const classA = ["민수", "철수", "영희", "나비"];

const nameBook = {};

classA.forEach((name, index) => {
  nameBook[index + 1] = name;
});

console.log(nameBook); //{1: "민수, 2:"철수", 3:"영희 , 4:"나비"}
```

### 3) 이미 존재하는 프로퍼티 키를 중복 선언하면 나중에 선언한 프로퍼티가 먼저 선언한 프로퍼티를 덮어쓴다.

```js
let foo = {
  name: "Won",
  name: "Kim",
};

console.log(foo); // {name: "Kim"}
```

## 3. 메서드

프로퍼티 값이 함수일 경우 일반 함수와 구분하기 위해 메서드라고 부른다. 즉, **메서드는 객체에 묶여 있는 함수를 의미**한다.

```js
let circle = {
  radius: 5, // ← 프로퍼티

  // 원의 지름
  getDiameter: function () {
    // ← 메서드
    return 2 * this.radius; // this는 circle을 가리킨다.
  },
};

console.log(circle.getDiameter()); // 10
```

## 4. 프로퍼티 접근

프로퍼티 접근 방법에는 2가지 방법이 있다.

> - 마침표 표기법(dot notation) : 마침표 프로퍼티 접근 연산자
> - 대괄호 표기법(bracket notation) : 대괄호 프로퍼티 접근 연산자

두 표기법의 차이는 네이밍 규칙을 준수한 키값의 경우 두 표기법 모두 사용할 수 있지만,** 네이밍 규칙을 준수하지 않아서 따옴표로 감싼 키값의 경우 대괄호 표기법으로만 프로퍼티에 접근할 수 있다.** 또한 네이밍 규칙을 준수하진 않았지만 키값이 숫자로만 이루어진 문자열인 경우 따옴표를 생략할 수 있다.

```js
let person = {
  'last-name': 'Won',
  1: 10
};

person.'last-name';  // -> SyntaxError: Unexpected string
person[last-name];   // -> ReferenceError: last is not defined
person['last-name']; // -> Lee

// 프로퍼티 키가 숫자로 이뤄진 문자열인 경우 따옴표를 생략할 수 있다.
person.1;     // -> SyntaxError: Unexpected number
person.'1';   // -> SyntaxError: Unexpected string
person[1];    // -> 10 : person[1] -> person['1']
person['1'];  // -> 10
```

## 5. 프로퍼티 값 갱신

존재하는 프로퍼티 키에 새로운 값을 재할당 하면 갱신된다.

```js
let person = {
  name: "Won",
};

person.name = "Kim";

console.log(person); // {name: "Kim"}
```

## 6. 프로퍼티 값 동적 생성

현재 객체에 존재하지 않는 프로퍼티에 값을 할당하면 프로퍼티가 동적으로 새로 생성되어 추가되고 값도 할당 된다.

```js
ket person = {
  name: 'Won'
};

// person 객체에는 age 프로퍼티가 존재하지 않는다.
person.age = 20;

console.log(person); // {name: "Lee", age: 20}
```

## 7. 프로퍼티 삭제

`delete` 연산자를 사용하여 프로퍼티를 삭제할 수 있다.(없는 프로퍼티에 delete 연산자를 사용해도 에러를 뱉지 않으니 주의)

```js
let person = {
  name: "Lee",
  age: 20,
};

delete person.age;

// person 객체에 address 프로퍼티가 존재하지 않음에도 에러가 발생하지 않는다.
delete person.address;

console.log(person); // {name: "Lee"}
```

## 8. 축약 표현

ES6 부터는 프로퍼티 값에 변수를 이용하는 경우, 프로퍼티 키값과 변수의 이름이 같은 경우 축약해서 사용할 수 있다.

```js
let x = 1,
  y = 2;

// 프로퍼티 축약 표현
const obj = { x, y };

console.log(obj); // {x: 1, y: 2}
```

메서드를 정의할 때도 function 키워드를 생략한 축약 표현을 사용할 수 있다.

```js
// ES6
const obj = {
  name: "Won",
  sayHi() {
    console.log("Hi! " + this.name);
  },
  //sayHi: function sayHi(){}와 같은 표현
};

obj.sayHi(); // Hi! Won
```

### Reference

> [모던 자바스크립트 deep dive](https://wikibook.co.kr/mjs/) <br/>[MDN Object](https://developer.mozilla.org/ko/docs/Web/JavaScript/Reference/Global_Objects/Object)
