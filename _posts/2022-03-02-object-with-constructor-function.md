---
layout: post
title: "[Javascript]생성자 함수와 객체"
author: "Wayne"
tags: javascript
excerpt_separator: <!--more-->
---

<span style="color:rgba(0,0,0,0)">생성자 함수로 생성한 객체 VS 리터럴 객체</span>

<!--more-->

<br/><br/><br/>

객체를 생성하는 방식은 하나가 아니다. 가장 일반적으로 사용하는 객체 리터럴 방식이다. 만약 생성자 함수로 객체를 생성하면 리터럴 방식과 무엇이 다를까?

## 1. 생성자 함수

`new` 연산자와 함께 `Object` 생성자 함수를 호출하면 빈 객체를 생성하여 반환한다. 빈 객체를 생성한 이후 프로퍼티 또는 메서드를 추가하여 객체를 완성할 수 있다.

```js
// 빈 객체의 생성
const person = new Object();

// 프로퍼티 추가
person.name = "Won";
person.sayHello = function () {
  console.log("Hi! My name is " + this.name);
};

console.log(person); // {name: "Won", sayHello: ƒ}
person.sayHello(); // Hi! My name is Won
```

생성자 함수에 의해 생성된 객체를 **인스턴스**라고 한다. 자바스크립트는 `Object` 생성자 함수 외에도 `String`, `Number`, `Boolean`, `Function`, `Array`,`Date`, `RegExp`, `Promise` 등의 빌트인 생성자 함수를 제공한다.

```js
// String 생성자 함수에 의한 String 객체 생성
const strObj = new String("Won");
console.log(typeof strObj); // object
console.log(strObj); // String {"Won"}

// Number 생성자 함수에 의한 Number 객체 생성
const numObj = new Number(123);
console.log(typeof numObj); // object
console.log(numObj); // Number {123}

// Boolean 생성자 함수에 의한 Boolean 객체 생성
const boolObj = new Boolean(true);
console.log(typeof boolObj); // object
console.log(boolObj); // Boolean {true}

// Function 생성자 함수에 의한 Function 객체(함수) 생성
const func = new Function("x", "return x * x");
console.log(typeof func); // function
console.dir(func); // ƒ anonymous(x)

// Array 생성자 함수에 의한 Array 객체(배열) 생성
const arr = new Array(1, 2, 3);
console.log(typeof arr); // object
console.log(arr); // [1, 2, 3]

// RegExp 생성자 함수에 의한 RegExp 객체(정규 표현식) 생성
const regExp = new RegExp(/ab+c/i);
console.log(typeof regExp); // object
console.log(regExp); // /ab+c/i

// Date 생성자 함수에 의한 Date 객체 생성
const date = new Date();
console.log(typeof date); // object
console.log(date); // Mon May 04 2020 08:36:33 GMT+0900 (대한민국 표준시)
```

## 2. Object 생성자 함수

### 1) 객체 리터럴에 의한 객체 생성 방식의 문제점

**객체 리터럴에 의한 객체 생성 방식은 단 하나의 객체만 생성한다.** 따라서 동일한 프로퍼티를 갖는 객체를 여러 개 생성해야 하는 경우 매번 같은 프로퍼티를 기술해야 하기 때문에 비효율적이다

```js
const circle1 = {
  radius: 5,
  getDiameter() {
    return 2 * this.radius;
  },
};

console.log(circle1.getDiameter()); // 10

const circle2 = {
  radius: 10,
  getDiameter() {
    return 2 * this.radius;
  },
};

console.log(circle2.getDiameter()); // 20
```

### 2) 생성자 함수에 의한 객체 생성 방식의 장점

생성자 함수에 의한 객체 생성 방식은 마치 객체(인스턴스)를 생성하기 위한 템플릿(클래스)처럼 <span class="bg_highlight">생성자 함수를 사용하여 프로퍼티 구조가 동일한 객체 여러 개를 간편하게 생성할 수 있다.</span>

```js
// 생성자 함수
function Circle(radius) {
  // 생성자 함수 내부의 this는 생성자 함수가 생성할 인스턴스를 가리킨다.
  this.radius = radius;
  this.getDiameter = function () {
    return 2 * this.radius;
  };
}

// 인스턴스의 생성
const circle1 = new Circle(5); // 반지름이 5인 Circle 객체를 생성
const circle2 = new Circle(10); // 반지름이 10인 Circle 객체를 생성

console.log(circle1.getDiameter()); // 10
console.log(circle2.getDiameter()); // 20
```

일반 함수와 동일한 방법으로 생성자 함수를 정의하고 `new` 연산자와 함께 호출하면 해당 함수는 생성자 함수로 동작한다. 만약 `new` 연산자와 함께 생성자 함수를 호출하지 않으면 생성자 함수가 아니라 일반 함수로 동작한다.

```js
// new 연산자와 함께 호출하지 않으면 생성자 함수로 동작하지 않는다. 일반 함수로서 호출된다.
const circle3 = Circle(15);

// 일반 함수로서 호출된 Circle은 반환문이 없으므로 암묵적으로 undefined를 반환한다.
console.log(circle3); // undefined

// 일반 함수로서 호출된 Circle내의 this는 전역 객체를 가리킨다.
console.log(radius); // 15
```

### 3) 생성자 함수의 인스턴스 생성 과정

생성자 함수로 인스턴스를 만들때 어떤 일련의 과정을 통해 인스턴스가 만들어지는 알아보자.

#### (1) 인스턴스 생성과 `this` 바인딩

생성자 함수를 호출하면 암묵적으로 빈 객체가 생성된다. 이 빈 객체가 바로 생성자 함수가 생성한 인스턴스다. 그리고 암묵적으로 생성된 빈 객체, 즉 인스턴스는 `this`에 바인딩 된다. 이 바인딩 처리는 함수 몸체의 코드의 런타임 이전에 실행된다.

```js
function Circle(radius) {
  // 1. 암묵적으로 빈 객체가 생성되고 this에 바인딩된다.
  console.log(this); // Circle {}

  this.radius = radius;
  this.getDiameter = function () {
    return 2 * this.radius;
  };
}
```

#### (2) 인스턴스 초기화

`this`에 바인딩되어 있는 인스턴스에 프로퍼티나 메서드를 추가하고 생성자 함수가 인수로 전달받은 초기값을 인스턴스 프로퍼티에 할당하여 초기화하거나 고정값을 할당한다.

```js
function Circle(radius) {
  // 2. this에 바인딩되어 있는 인스턴스를 초기화한다.
  this.radius = radius;
  this.getDiameter = function () {
    return 2 * this.radius;
  };
}
```

#### (3) 인스턴스 반환

생성자 함수 내부의 모든 처리가 끝나면 완성된 인스턴스가 바인딩된 this가 암묵적으로 반환된다.

```js
function Circle(radius) {
  this.radius = radius;
  this.getDiameter = function () {
    return 2 * this.radius;
  };
}

// 인스턴스 생성. Circle 생성자 함수는 암묵적으로 this를 반환한다.
const circle = new Circle(1);
console.log(circle); // Circle {radius: 1, getDiameter: ƒ}
```

**만약 this가 아닌 다른 객체를 명시적으로 반환되면 this가 반환되지 못하고 return 문에 명시한 객체가 반환된다.**

```js
function Circle(radius) {
  this.radius = radius;
  this.getDiameter = function () {
    return 2 * this.radius;
  };

  // 명시적으로 객체를 반환하면 암묵적인 this 반환이 무시된다.
  return {};
}

const circle = new Circle(1);
console.log(circle); // {}
```

명시적으로 **원시 값을 반환하면 원시 값 반환은 무시되고 암묵적으로 this가 반환**된다.

```js
function Circle(radius) {
  this.radius = radius;
  this.getDiameter = function () {
    return 2 * this.radius;
  };

  // 명시적으로 원시값을 반환하면 원시값 반환은 무시되고 암묵적으로 this가 반환된다.
  return 100;
}

const circle = new Circle(1);
console.log(circle); // Circle {radius: 1, getDiameter: ƒ}
```

### 4) 내부 메서드 `[[Call]`]과 `[[Construct]]`

함수는 객체이므로 일반 객체와 동일하게 동작할 수 있다. 함수 객체는 일반 객체가 가지고 있는 내부 슬롯과 내부 메서드를 모두 가지고 있기 때문이다.

```js
// 함수는 객체다.
function foo() {}

// 함수는 객체이므로 프로퍼티를 소유할 수 있다.
foo.prop = 10;

// 함수는 객체이므로 메서드를 소유할 수 있다.
foo.method = function () {
  console.log(this.prop);
};

foo.method(); // 10
```

하지만 일반 객체는 호출할 수 없지만 **함수는 호출할 수 있다.** 따라서 함수 객체는 일반 객체가 가지고 있는 내부 슬롯과 내부 메서드는 물론 함수로서 동작하기 위해 함수 객체만을 위한 `[[Environment]]`, `[[FormalParameters]]` 등의 내부 슬롯과 `[[Call]]`, `[[Construct]] `같은 내부 메서드를 추가로 가지고 있다.
<br/>
함수가 일반 함수로서 호출되면 함수 객체의 내부 메서드 `[[Call]]`이 호출되고 new 연산자와 함께 생성자 함수로서 호출되면 내부 메서드를 `[[Construct]]`가 호출된다.

> 내부 메서드 `[[Call]]`을 갖는 함수 객체를 callable이라 하며, 내부 메서드 `[[Construct]]`를 갖는 함수 객체를 constructor, `[[Constructor]]`를 갖지 않는 함수 객체를 non-constuctor라고 부른다.<br/><br/> callable은 호출할 수 있는 객체, 즉 함수를 말하며, constructor는 생성자 함수로서 호출할 수 있는 함수, non-constructor는 객체를 생성자 함수로서 호출할 수 없는 함수를 의미한다. **호출할 수 없는 객체는 함수 객체가 아니므로 함수로서 가능하는 객체, 즉 함수 객체는 반드시 callable이어야 한다.**

```js
function foo() {}

// 일반적인 함수로서 호출: [[Call]]이 호출된다.
foo();

// 생성자 함수로서 호출: [[Construct]]가 호출된다.
new foo();
```

### 5) constructor와 non-constructor의 구분

함수 객체는 constructor일 수도 있고 non-construct일 수도 있다.
즉, 모든 함수 객체는 호출할 수 있지만 모든 함수 객체를 생성자 함수로서 호출할 수 있는 것은 아니다.
<br/>
자바스크립트 엔진은 함수 정의를 평가하여 함수 객체를 생성할 때 함수 정의 방식에 따라 함수를 constructor와 non-constructor로 구분한다.

> - constructor: 함수 선언문, 함수 표현식, 클래스
> - non-constructor: 메서드(ES6 메서드 축약 표현), 화살표 함수

```js
// 일반 함수 정의: 함수 선언문, 함수 표현식
function foo() {}
const bar = function () {};
// 프로퍼티 x의 값으로 할당된 것은 일반 함수로 정의된 함수다. 이는 메서드로 인정하지 않는다.
const baz = {
  x: function () {},
};

// 일반 함수로 정의된 함수만이 constructor이다.
new foo(); // -> foo {}
new bar(); // -> bar {}
new baz.x(); // -> x {}

// 화살표 함수 정의
const arrow = () => {};

new arrow(); // TypeError: arrow is not a constructor

// 메서드 정의: ES6의 메서드 축약 표현만을 메서드로 인정한다.
const obj = {
  x() {},
};

new obj.x(); // TypeError: obj.x is not a constructor
```

## 3. `new` 연산자

`new` 연산자와 함께 함수를 호출하면 해당 함수는 생성자 함수로 동작한다. 따라서 `new` 연산자와 함께 호출하는 함수는 non-constructor가 아닌** constructor**여야 한다. `new` 연산자와 함께 함수를 호출할시 `[[Construct]]`가 호출된다.

```js
// 생성자 함수로서 정의하지 않은 일반 함수
function add(x, y) {
  return x + y;
}

// 생성자 함수로서 정의하지 않은 일반 함수를 new 연산자와 함께 호출
let inst = new add();
// 함수가 객체를 반환하지 않았으므로 반환문이 무시된다. 따라서 빈 객체가 생성되어 반환된다.
console.log(inst); // {}

// 객체를 반환하는 일반 함수
function createUser(name, role) {
  return { name, role };
}

// 생성자 함수로서 정의하지 않은 일반 함수를 new 연산자와 함께 호출
inst = new createUser("Lee", "admin");
// 함수가 생성한 객체를 반환한다.
console.log(inst); // {name: "Lee", role: "admin"}
```

반대로 `new` 연산자 없이 생성자 함수를 호출하면 일반 함수로 호출된다. 즉 `[[Call]]`이 호출된다.

```js
// 생성자 함수
function Circle(radius) {
  this.radius = radius;
  this.getDiameter = function () {
    return 2 * this.radius;
  };
}

// new 연산자 없이 생성자 함수 호출하면 일반 함수로서 호출된다.
const circle = Circle(5);
console.log(circle); // undefined

// 일반 함수 내부의 this는 전역 객체 window를 가리킨다.
console.log(radius); // 5
console.log(getDiameter()); // 10

circle.getDiameter();
// TypeError: Cannot read property 'getDiameter' of undefined
```

> 생성자 함수를 new 연산자와 함께 호출하는 것이 아닌 일반 함수로 호출하면 생성자 함수 내부의 `this`는 전역 객체 `window`를 가르킨다. 따라서 `radius` 프로퍼티와 `getDiameter` 메서드는 전역 객체의 프로퍼티와 메서드가 된다.

#### (1) `new.target`

`new` 연산자와 함께 생성자 함수로서 호출되면 함수 내부의 `new.target`은 함수 자신을 가르킨다. `new` 연산자와 없이 일반 함수로서 호출된 함수 내부의 `new.target`은 `undefined`다. 이것을 활용해서 생성자 함수를 `new` 연산자와 함께 호출했는지 체크하여 재귀를 통해 생성자 함수로서 호출할 수 있다.

```js
// 생성자 함수
function Circle(radius) {
  // 이 함수가 new 연산자와 함께 호출되지 않았다면 new.target은 undefined다.
  if (!new.target) {
    // new 연산자와 함께 생성자 함수를 재귀 호출하여 생성된 인스턴스를 반환한다.
    return new Circle(radius);
  }

  this.radius = radius;
  this.getDiameter = function () {
    return 2 * this.radius;
  };
}

// new 연산자 없이 생성자 함수를 호출하여도 new.target을 통해 생성자 함수로서 호출된다.
const circle = Circle(5);
console.log(circle.getDiameter());
```

#### (4) 빌트인 생성자 함수

대부분의 빌트인 생성자 함수는 `new` 연산자와 함께 호출되었는지 확인한 후 적절한 값을 반환한다. 예를 들어 `Object와` `Function` 생성자 함수는 new 연산자 없이 호출해도 `new` 연산자와 함께 호출했을 때와 동일하게 동작한다. 하지만 `String`, `Number`,`Boolean` 생성자 함수는 `new` 연산자와 함께 호출했을 때와 `new` 연산자 없이 호출했을 때의 결과가 다르다.

```js
let obj = new Object();
console.log(obj); // {}

obj = Object();
console.log(obj); // {}

let f = new Function("x", "return x ** x");
console.log(f); // ƒ anonymous(x) { return x ** x }

f = Function("x", "return x ** x");
console.log(f); // ƒ anonymous(x) { return x ** x }

const str = String(123);
console.log(str, typeof str); // 123 string

const num = Number("123");
console.log(num, typeof num); // 123 number

const bool = Boolean("true");
console.log(bool, typeof bool); // true boolean
```

### Reference

> [모던 자바스크립트 deep dive](https://wikibook.co.kr/mjs/)<br/> [MDN new operator](https://developer.mozilla.org/ko/docs/Web/JavaScript/Reference/Operators/new)<br/> [MDN Function](https://developer.mozilla.org/ko/docs/Web/JavaScript/Reference/Global_Objects/Function)
