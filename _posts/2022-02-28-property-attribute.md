---
layout: post
title: "[Javascript]Property Attribute"
author: "Wayne"
tags: javascript
excerpt_separator: <!--more-->
---

<span style="color:rgba(0,0,0,0)">객체의 프로퍼티는 어떤 속성이 있을까?</span>

<!--more-->

<br/><br/><br/>

## 1. 내부 슬롯과 내부 메서드

property atrribute를 이해하기 위해서는 먼저 내부 슬롯과 내부 메서드의 개념에 대해서 알고 있어야 한다.
<br/>
내부 슬롯과 내부 메서드는 자바수크립트 엔진의 구현 알고리즘을 설명하기 위해 사용하는 의사 프로퍼티와 의사 메서드다. ECMAscript 사양에 등장하는 이중 대괄호`[[...]]`로 감싼 이름들이 내부 슬롯과 내부 메서드다.

**내부 슬롯과 내부 메서드는 자바스크립트 엔진에서 실제로 동작하지만 개발자가 직접 접근할 수 있도록 외부 공개된 객체의 프로퍼티가 아니다. 다만, 일부 내부 슬롯과 메서드에 한하여 간접적으로 접근할 수 있다.**

예를들어, 모든 객체는 `[[protorype]]`이라는 내부 슬롯을 갖는다. 이 내부 슬롯의 경우 `__proto__`를 통해 간접적으로 접근할 수 있다.

```js
const o = {};

// 내부 슬롯은 자바스크립트 엔진의 내부 로직이므로 직접 접근할 수 없다.
o.[[Prototype]] // -> Uncaught SyntaxError: Unexpected token '['

// 단, 일부 내부 슬롯과 내부 메서드에 한하여 간접적으로 접근할 수 있는 수단을 제공하기는 한다.
o.__proto__ // -> Object.prototype
```

## 2. propery attribute와 property descriptor

자바스크립트는 property를 생성할 때 property의 상태를 나타내는 attribute를 기본값으로 자동 정의한다. 이 attribute에는 property의 값`[[Value]]`, 값의 갱신 여부`[[Writable]]`, 열거 가능 여부`[[Enumerable]]`, 재정의 가능 여부`[[Configurable]]`가 있다.
<br/>
이러한 것들은 내부 슬롯이므로 직접적으로 property attribute에 접근할 수는 없지만 Object.getOwnPropertyDescriptor메서드를 사용하여 간접적으로 확인할 수 있다.

```js
const person = {
  name: "Won",
};

console.log(Object.getOwnPropertyDescriptor(person, "name"));
// {value: "Won", writable: true, enumerable: true, configurable: true}
```

`Object.getOwnPropertyDescriptor `메서드는 property attribute 정보를 제공하는 descriptor 객체를 반환한다. 만약 존재하지 않는 property나 상속받은 property에 대한 descriptor를 요구하면 `undefined`를 반환한다.

`Object.getOwnPropertyDescriptor`는 하나의 property에 대해 객체를 반환하지만 `Object.getOwnPropertyDescriptors`를 사용하면 객체의 모든 property attribute 정보를 제공하는 객체를 반환한다.

```js
const person = {
  name: "Won",
};

person.age = 30;

console.log(Object.getOwnPropertyDescriptors(person));
/*
{
  name: {value: "Won", writable: true, enumerable: true, configurable: true},
  age: {value: 30, writable: true, enumerable: true, configurable: true}
}
*/
```

## 3. 데이터 프로퍼티와 접근자 프로퍼티

객체의 프로퍼티는 데이터 프로퍼티와 접근자 프로퍼티로 구분할 수 있다. 자세히 알아보자.

```js
const person = {
  // 데이터 프로퍼티
  firstName: "john",
  lastName: "smith",

  // fullName은 접근자 함수로 구성된 접근자 프로퍼티다.
  // getter 함수
  get fullName() {
    return `${this.firstName} ${this.lastName}`;
  },
  // setter 함수
  set fullName(name) {
    [this.firstName, this.lastName] = name.split(" ");
  },
};
```

### 1) 데이터 프로퍼티

**키와 값으로 구성된 일반적인 프로퍼티**다. 데이터 프로퍼티는 다음과 같은 attribute를 가진다. 이 attribute는 자바스크립트 엔진이 property가 생성될 때 자동으로 정의한다.

| 프로퍼티 어트리뷰트 | 프로퍼티 디스크럽터의 프로퍼티 | 설명                                                                                                                                                                                                                                                                    |
| ------------------- | ------------------------------ | ----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| `[[Value]]`         | value                          | - 프로퍼티 키를 통해 프로퍼티 값에 접근하면 반환되는 값<br/> - 프로퍼티 키를 통해 프로퍼티 값을 변경하면 `[[Value]]`에 값을 재할당한다.                                                                                                                                 |
| `[[Writable]]`      | writable                       | - 프로퍼티 값의 변경 가능 여부를 불리언 값으로 나타낸다. <br/> - `[[Writable]]`이 `false`인 경우 해당 프로퍼티는 읽기 전용이 된다.                                                                                                                                      |
| `[[Enumerable]]`    | enumerable                     | - 프로퍼티의 열거 가능 여부를 불리언 값으로 나타낸다. <br/> - `[[Enumerable]]`이 `false`인 경우 해당 프로퍼티는 `for in`문이나 `object.keys` 메서드 등으로 열거할 수 없다.                                                                                              |
| `[[Configurable]]`  | configurable                   | - 프로퍼티의 재정의 가능 여부를 불리언 값으로 나타낸다. <br/> - `[[Configurable]]`이 `false`인 경우 해당 프로퍼티는 삭제, 값의 변경이 금지된다. 다만 `[[Writable]]`이 `true`인 경우에는 `[[Value]]`의 값을 변경하거나, `[[Writable]]`이 `false`로 만드는 것이 가능하다. |

### 2) 접근자 프로퍼티

자체적으로 값을 갖지 않고 다른 **데이터 프로퍼티의 값을 읽거나 저장할 때 호출되는 접근자 함수로 구성된 property**이다.
접근자 함수는 `getter/setter`함수라고도 부른다. 접근자 프로퍼티는 `getter`와 `setter`함수를 모두 정의할 수도 있고 하나만 정의할 수도 있다.

```js
// 데이터 프로퍼티를 통한 프로퍼티 값의 참조.
console.log(person.firstName + " " + person.lastName); // smith john

// 접근자 프로퍼티 fullName에 값을 저장하면 setter 함수가 호출된다.
person.fullName = "mark john";
console.log(person); // {firstName: "mark", lastName: "john"}

// 접근자 프로퍼티 fullName에 접근하면 getter 함수가 호출된다.
console.log(person.fullName); // mark john
```

따라서 데이터 프로퍼티인지, 접근자 프로퍼티인지는 `Object.getOwnPropertyDescriptor`메서드를 이용하여 반환된 descriptor 객체의 property가 다른 것으로 구분해야 한다.

```js
let descriptor = Object.getOwnPropertyDescriptor(person, "firstName");
console.log(descriptor);
// {value: "Heegun", writable: true, enumerable: true, configurable: true}

descriptor = Object.getOwnPropertyDescriptor(person, "fullName");
console.log(descriptor);
// {get: ƒ, set: ƒ, enumerable: true, configurable: true}

// 접근자 프로퍼티
Object.getOwnPropertyDescriptor(Object.prototype, "__proto__");
// {get: ƒ, set: ƒ, enumerable: false, configurable: true}

// 데이터 프로퍼티
Object.getOwnPropertyDescriptor(function () {}, "prototype");
// {value: {...}, writable: true, enumerable: false, configurable: false}
```

## 4. Property 정의

프로퍼티 정의란 새로운 프로퍼티를 추가하면서 property attribute를 명시적으로 정의하거나, 기존 property attribute를 재정의하는 것을 말한다. 이 프로퍼티 정의는 `Object.defineProperty` 메서드를 사용하여 정의한다. 또한 생략할 수도 있다.

```js
const person = {};

// 데이터 프로퍼티 정의
Object.defineProperty(person, "address", {
  value: "seoul",
  writable: true,
  enumerable: true,
  configurable: true,
});
```

| descriptor 객체의 프로퍼티 | 프로퍼티 어트리뷰트 | 기본 값   |
| -------------------------- | ------------------- | --------- |
| value                      | `[[Value]]`         | undefined |
| get                        | `[[Get]]`           | undefined |
| set                        | `[[Set]]`           | undefined |
| writable                   | `[[Writable]]`      | false     |
| enumerable                 | `[[Enumerable]]`    | false     |
| configurable               | `[[Configurable]]`  | false     |

`Object.defineProperty `메서드로는 하나의 property만 정의할 수 있다. `Object.defineProperties` 메서드를 사용하면 여러 개의 property를 한번에 정의할 수 있다.

```js
const person = {};

Object.defineProperties(person, {
  // 데이터 프로퍼티 정의
  firstName: {
    value: "Wayne",
    writable: true,
    enumerable: true,
    configurable: true,
  },
  lastName: {
    value: "Won",
    writable: true,
    enumerable: true,
    configurable: true,
  },
  // 접근자 프로퍼티 정의
  fullName: {
    get() {
      return `${this.firstName} ${this.lastName}`;
    },
    set(name) {
      [this.firstName, this.lastName] = name.split(" ");
    },
    enumerable: true,
    configurable: true,
  },
});
```

### Reference

> [모던 자바스크립트 deep dive](https://wikibook.co.kr/mjs/)<br/> [MDN defineProperty](https://developer.mozilla.org/ko/docs/Web/JavaScript/Reference/Global_Objects/Object/defineProperty)
