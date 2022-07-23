---
layout: post
title: "[Javascript]표현식과 문"
author: "Wayne"
tags: javascript
excerpt_separator: <!--more-->
---

<span style="color:rgba(0,0,0,0)">코드의 작은 단위에 대하여!</span>

<!--more-->

<br/><br/><br/>

자바스크립트를 이루는 다양한 코드 조각들의 기본적인 명칭과 의미에 대해서 알아보자!

# 값(Value)

값은 표현식이 평가되어 생성된 결과를 말한다. 여기서 평가란 식을 해석해서 값을 생성하거나 참조하는 것을 의미한다. <br/>변수는 하나의 **값**을 저장하기 위해 확보한 메모리 공간이라고 했으므로 여기서 변수에 할당되는 것은 값이다.

```javascript
let result = 10 + 20;
//변수 result에 할당 되는 것은 표현식인 10 + 20가 아니라, 결과 값인 30
```

# 리터럴(Literal)

리터럴은 사람이 이해할 수 있는 문자 또는 약속된 기호를 사용해 값을 생성하는 표기법이다. 숫자뿐만 아니라 문자, 미리 약속된 기호 모두 리터럴이다.(3, 10, true, false ]...) 자바스크립트엔진은 해당 리터럴 코드가 실행되는 시점에 리터럴을 평가해 리터럴 값을 생성한다.
<br/>
<br/> 참고로 자바스크립트에서 객체나 배열, 함수의 표기법도 미리 약속된 표기법이므로 리터럴이다.

```javascript
function(){
  //함수 리터럴
};

const object ={
  0:"이것은",
  1:"객체"
};
```

# 표현식(Expression)

**값으로 평가될 수 있는 문**이다. 즉, 표현식이 평가되면 새로운 값을 생성하거나 기존 값을 참조한다. 리터럴도 값으로 평가되므로 리터럴도 표현식이다. **리터럴, 식별자, 연산자, 함수 호출 등**은 모두 평가되어 값이 될 수 있으므로 표현식이 될 수 있다.

```javascript
let height = 168;
let sum = 10000 + 1000;

//168, 10000 + 1000 둘다 평가되어 값을 생성하므로 표현식

let targetNumber = 30;
let sum = targetNumber;

// targetNumber도 값 30으로 인식됨으로 표현식

const getFamilyName = () => {
  return "Kim";
};
const name = "minsu" + getFamilyName(); // "minsu Kim"

//getFamilyName()도 평가되어 kim이란 값으로 인식되므로 표현식
```

# 문(Statement)

문은 프로그램을 구성하는 기본 단위이자 **최소 실행 단위**이다. 문은 문법적인 의미를 가지며 문법적으로 더 이상 나눌 수 없는 코드 요소인 토큰으로 이루어진다. 키워드, 식별자, 연산자, 리터럴, 세미콜론 등은 모두 의미를 가지며 문법적으로 더 이상 나눌 수 없기 때문에 토큰이다.
<br/><br/>
문은 명령문이라고도 부른다. 즉 문은 컴퓨터에 내리는 명령이다. 문이 실행되면 명령이 실행되고 무언가가 일어나게 된다. 문의 종류에는 **선언문, 할당문, 조건문, 반복문 등**이 있다.

```javascript
let name = firstName + lastName;
//let, name, =, firstName, +, lastName 모두 개별적인 토큰

let name; //변수 선언문

name = "minsu kim" //할당문

const foo = () => {} //함수 선언문

if(index === 0){console.log("first")} //조건문

for(let 1 = 0; i < 100; i++){console.log(i);} //반복문
```

# 표현식인 문과 표현식이 아닌 문

표현식은 그 자체로 문이 될 수도 있고, 혹은 문의 일부일 수도 있다.

```javascript
let name; //변수 선언문은 값으로 평가될 수 없으므로 표현식이 아니다.

name = firstName + lastName;
// firstName, lastName, name = firstName + lastName 모두 표현식
// name = firstName + lastName 은 표현식이면서 문이다.
```

표현식인 문과 표현식이 아닌 문을 구별하는 것은 변수에 할당해 보면 알 수 있다.

```javascript
var myName = var name;
//syntaxError: Unexpected token var
```

# 세미콜론(;)

**세미콜론은 문의 종료를 나타낸다.** 자바스크립트 엔진은 세미콜론으로 문이 종료한 위치를 파악하고 순차적으로 문을 실행한다. 따라서, 문을 끝낼 때는 세미콜론을 붙여야 한다. 단, 0개 이상의 문을 중괄호로 묶은 코드블록은 뒤에 세미콜론을 붙이지 않는다.<br/><br/>

세미콜론은 옵션값으로 생략가능 하다. 자바스크립트 엔진이 스스로 문의 끝이라고 생각하는 지점에 세미콜론을 자동으로 삽입하는 기능(ASI)이 있기 때문이다. 다만, 엔진이 예상한 문의 종료 지점과 개발자가 의도한 문의 종료 지점이 서로 상이할 수 있으므로 세미콜론 사용을 권장하는 분위기이다.

### Reference

> [모던 자바스크립트 deep dive](https://wikibook.co.kr/mjs/)
> [MDN Statements](https://developer.mozilla.org/ko/docs/Web/JavaScript/Reference/Statements)
