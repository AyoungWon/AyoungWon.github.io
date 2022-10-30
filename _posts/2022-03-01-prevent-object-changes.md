---
layout: post
title: "[Javascript]객체의 변경을 막는 방법들"
author: "Wayne"
tags: javascript study
excerpt_separator: <!--more-->
---

<span style="color:rgba(0,0,0,0)">어떻게 객체의 변경을 막을수 있을까?</span>

<!--more-->

<br/><br/><br/>

# 객체의 변경 방지

자바스크립트는 객체의 변경을 방지하는 다양한 메서드를 제공한다. 3가지의 메서드가 있는데, 이 메서드들은 객체의 변경을 금지하는 강도가 조금씩 다르다. 간단히 표로 보자면 아래와 같다.

| 구분           | 메서드                     | 프로퍼티 추가 | 프로퍼티 삭제 | 프로퍼티 값 읽기 | 프로퍼티 값 쓰기 | 프로퍼티 어트리뷰트 재정의 |
| -------------- | -------------------------- | :-----------: | :-----------: | :--------------: | :--------------: | :------------------------: |
| 객체 확장 금지 | `Object.preventExtensions` |       X       |       O       |        O         |        O         |             O              |
| 객체 밀봉      | `Object.seal`              |       X       |       X       |        O         |        O         |             X              |
| 객체 동결      | `Object.freeze`            |       X       |       X       |        O         |        X         |             X              |

하나하나씩 자세히 살펴보자.

## 1. `Object.preventExtensions`

`Object.preventExtensions` 메서드는 객체의 확장을 금지한다. 즉, 객체의 프로퍼티 추가가 금지된다. property의 동적 추가와 `Object.definedProperty` 메서드 이용하여 추가하는 것 모두 금지된다. 확장 여부는 `Object.isExtensible` 메서드로 확인할 수 있다.

```js
const person = { name: "Lee" };

// person 객체는 확장이 금지된 객체가 아니다.
console.log(Object.isExtensible(person)); // true

// person 객체의 확장을 금지하여 프로퍼티 추가를 금지한다.
Object.preventExtensions(person);

// person 객체는 확장이 금지된 객체다.
console.log(Object.isExtensible(person)); // false

// 프로퍼티 추가가 금지된다.
person.age = 20; // 무시. strict mode에서는 에러
console.log(person); // {name: "Lee"}

// 프로퍼티 정의에 의한 프로퍼티 추가도 금지된다.
Object.defineProperty(person, "age", { value: 20 });
// TypeError: Cannot define property age, object is not extensible

// 프로퍼티 추가는 금지되지만 삭제는 가능하다.
delete person.name;
console.log(person); // {}
```

## 2. `Object.seal`

`Object.seal` 메서드는 객체를 밀봉한다. 밀봉된 객체는 읽기와 쓰기만 가능하다. 밀봉된 여부는 `Object.isSealed`로 확인할 수 있다.

```js
const person = { name: "Lee" };

// person 객체는 밀봉(seal)된 객체가 아니다.
console.log(Object.isSealed(person)); // false

// person 객체를 밀봉(seal)하여 프로퍼티 추가, 삭제, 재정의를 금지한다.
Object.seal(person);

// person 객체는 밀봉(seal)된 객체다.
console.log(Object.isSealed(person)); // true

// 밀봉(seal)된 객체는 configurable이 false다.
console.log(Object.getOwnPropertyDescriptors(person));

//{name: {value: "Lee", writable: true, enumerable: true, configurable: false}}

// 프로퍼티 추가가 금지된다.
person.age = 20; // 무시. strict mode에서는 에러
console.log(person); // {name: "Lee"}

// 프로퍼티 삭제가 금지된다.
delete person.name; // 무시. strict mode에서는 에러
console.log(person); // {name: "Lee"}

// 프로퍼티 값 갱신은 가능하다.
person.name = "Kim";
console.log(person); // {name: "Kim"}

// 프로퍼티 어트리뷰트 재정의가 금지된다.
Object.defineProperty(person, "name", { configurable: true });
// TypeError: Cannot redefine property: name
```

## 3.`Object.freeze`

`Object.freeze` 메서드는 객체를 동결한다. 즉, 동결된 객체는 읽기만 가능하다. 동결 여부는 `Object.isFrozen`로 확인할 수 있다.

```js
const person = { name: "kim" };

// person 객체는 동결(freeze)된 객체가 아니다.
console.log(Object.isFrozen(person)); // false

// person 객체를 동결(freeze)하여 프로퍼티 추가, 삭제, 재정의, 쓰기를 금지한다.
Object.freeze(person);

// person 객체는 동결(freeze)된 객체다.
console.log(Object.isFrozen(person)); // true

// 동결(freeze)된 객체는 writable과 configurable이 false다.
console.log(Object.getOwnPropertyDescriptors(person));

//{name: {value: "kim", writable: false, enumerable: true, configurable: false}}

// 프로퍼티 추가가 금지된다.
person.age = 20; // 무시. strict mode에서는 에러
console.log(person); // {name: "kim"}

// 프로퍼티 삭제가 금지된다.
delete person.name; // 무시. strict mode에서는 에러
console.log(person); // {name: "kim"}

// 프로퍼티 값 갱신이 금지된다.
person.name = "won"; // 무시. strict mode에서는 에러
console.log(person); // {name: "kim"}

// 프로퍼티 어트리뷰트 재정의가 금지된다.
Object.defineProperty(person, "name", { configurable: true });
// TypeError: Cannot redefine property: name
```

## 4. 불변 객체

앞의 3개의 메서드들은 중첩 객체까지는 영향을 주지 못한다. 따라서 `Object.freeze` 메서드로 객체를 동결하여도 중첩 객체까지 동결할 수는 없다.

```js
const person = {
  name: "Lee",
  address: { city: "Seoul" },
};

Object.freeze(person);

console.log(Object.isFrozen(person)); // true

console.log(Object.isFrozen(person.address)); // false

person.address.city = "Busan";
console.log(person); // {name: "Lee", address: {city: "Busan"}}
```

객체의 중첩 객체까지 동결하여 변경이 불가능한 불변객체로 구현하려면 객체를 값으로 갖는 **모든 property에 재귀적으로** `Object.freeze` 메서드를 호출하면 된다.

```js
function deepFreeze(target) {
  // 객체가 아니거나 동결된 객체는 무시하고 객체이고 동결되지 않은 객체만 동결한다.
  if (target && typeof target === "object" && !Object.isFrozen(target)) {
    Object.freeze(target);
    Object.keys(target).forEach((key) => deepFreeze(target[key]));
  }
  return target;
}

const person = {
  name: "Lee",
  address: { city: "Seoul" },
};

deepFreeze(person);

console.log(Object.isFrozen(person.address)); // true

person.address.city = "Busan";
console.log(person); // {name: "Lee", address: {city: "Seoul"}}
```

### Reference

> [모던 자바스크립트 deep dive](https://wikibook.co.kr/mjs/)<br/> [MDN defineProperty](https://developer.mozilla.org/ko/docs/Web/JavaScript/Reference/Global_Objects/Object/defineProperty) <br/>[MDN Object.freeze](https://developer.mozilla.org/ko/docs/Web/JavaScript/Reference/Global_Objects/Object/freeze)
