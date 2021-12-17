---
layout: post
title: "일반 함수와 화살표함수의 차이는 무엇일까?"
summary: "ES6 Arrow function에 대해 알아보자"
author: jack
date: "2021-12-18 18:58:47 GMT+0900"
category: ["JavaScript"]
tags: JavaScript
thumbnail: /assets/img/posts/thumnail_js.png
keywords: Jack, developer, es6, javascript, frontend, arrow, function, 자바스크립트, 화살표, 함수
usemathjax: false
permalink: /blog/arrow-function/
---

## let 변수와 const 변수에 대해 알아보자.

우선 es6부터 새로 도입된 `let`과 `const`에 대해 이해하기 앞서 이전에 사용되던 `var`와 변수의 범위가 무엇인지 이해할 필요가 있다.  
크게 var로 선언된 변수는 `local 변수`와 `global 변수`로 구분이 되는데 이렇게 구분이 되는 이유는 기능과 목적이 다르기 때문이다.

#### 1. Global 변수

- 다른 js파일에서 변수값 공유 및 공통 변수 개념으로 사용.
- 의도는 좋으나 처리속도가 떨어지는 문제가 있음.

기본적으로 브라우저에서 global은 `window 객체`를 지칭한다. 그렇기 때문에 global 변수로 사용되는 변수들은 window 객체의 프로퍼티로 등록되게 되어있다. global 변수를 생성하는 방법은 선언되지 않은 변수에 값을 할당하거나 global 범위에서 var 를 이용해서 변수를 선언하는방법이 있다. 아래의 예시를 통해 확인해보자.

```javascript
var varGlobal = "varGlobal";
// 또는 varGlobal = 'varGlobal';
```

<blockquote class="dev-tool">
  <div class="head">
    <p class="title arrow-down">Scope</p>
  </div>
  <div class="body">
    <p class="title arrow-down">Global</p>
    <p class="property">
      <span class="key">varGlobal</span>
      <span class="value">'varGlobal'</span>
    </p>
  </div>
</blockquote>

전역 변수를 설정하게되면 위와같이 Global scope에 변수가 저장되게되고 window 객체를 참조하여 해당 변수의 값을 불러올수 있다.
하지만 `let 변수` 같은 경우는 Global 위치에서 변수를 선언한다고 해도 `window 객체`에 설정되지 않는다.
let 변수는 블록범위 내에서 작성해야하지만 전역위치에서는 블록이 없으므로 javascript 엔진이 블록은 만들고 이를 스코프로 사용하여 설정하는 개념이다. 아래의 예시를 보자.

```javascript
let letGlobal = "letGlobal";
```

<blockquote class="dev-tool">
  <div class="head">
    <p class="title arrow-down">Scope</p>
  </div>
  <div class="body">
    <p class="title arrow-down">Script</p>
    <p class="property">
      <span class="key">letGlobal</span>
      <span class="value">'letGlobal'</span>
    </p>
    <p class="title arrow-right">Global</p>
  </div>
</blockquote>

위 이미지의 Script는 하나의 블록 개념으로 script가 작성된 모든파일에서 공유하고 이를 글로벌 변수처럼 사용할수 있다. 블록에 대해서는 3번째 절에서 더 자세히 다루겠다.

#### 2. Local 변수(함수 레벨 스코프)

- 함수 안에 선언된 변수는 그 함수에 대해서 지역(local)이 된다.
- 빠르게 식별자를 확인하기 위해 가까운 스코프의 변수를 사용하려는것.

local 변수는 함수 내부에 선언된 변수들을 일컫는 말이다. local 블록 내부에 선언된 변수들은 외부에서 접근이 불가능한 특징이 있다. 아래의 예시를 보자.

```javascript
var name = "Jack";
function localScope() {
  var name = "koh";
}
localScope();
console.log(name);
// 결과: Jack
```

<blockquote class="dev-tool">
  <div class="head">
    <p class="title arrow-down">Scope</p>
  </div>
  <div class="body">
    <p class="title arrow-down">Local</p>
    <p class="sub-title arrow-right">
      <span class="key">this</span>
      <span class="value">Window</span>
    </p>
    <p class="property">
      <span class="key">name</span>
      <span class="value">'Koh'</span>
    </p>
    <p class="title arrow-down">Global</p>
    <p class="property">
    <span class="key">name</span>
    <span class="value">'Koh'</span>
  </p>
  </div>
</blockquote>
    
개발자 도구를 살펴보면 선언된 두 변수는 서로 다른 범위에서 적용된 변수이고 출력하는부분이 Global Scope 에서 실행되기 때문에
가장 상단에 선언된 변수의 값인 Jack이 출력되게 된다. 물론 선언된 변수의 Scope의 범위에 따라 내부 Scope는 외부의 Scope의 변수에
접근할수 있고 외부 Scope에서는 내부 Scope의 변수에 접근할수 없다.

#### 3. Block 변수(블록 레벨 스코프)

전역변수를 설정했을때 두 변수의 차이는 Global scope에 설정이 되거나 Script 블록에 설정이 된다.
let 변수가 Script라는 블록에 설정되는 이유는 let 으로 선언한 변수는 `블록 스코프`를 가진 변수라서
Block Scope 내부에 변수가 저장되어야하는데 전역에 위치한 let 변수는 블록이 존재하지 않기때문에 Script 블록내부에 생성된다.
여기서 Block이란 무엇일까? 자바스크립트의 `{}` 를 말한다. 흔히 `if문`이나 `for문`에 관련해서 사용된다.
아래의 코드를 보며 var와 let의 차이를 이해하자.

```javascript
// var 를 사용했을때
var name = "Jack";
if (name) {
  var name = "Koh";
  console.log("in :", name);
}
console.log("out :", name);
// 결과: in : Koh, out : Koh
```

<blockquote class="dev-tool">
  <div class="head">
    <p class="title arrow-down">Scope</p>
  </div>
  <div class="body">
    <p class="title arrow-down">Global</p>
    <p class="property">
      <span class="key">name</span>
      <span class="value">'Koh'</span>
    </p>
  </div>
</blockquote>

var 변수는 블록범위를 가지지 않으므로 위의 코드에서 var name 은 서로 다른 블록에 설정된 변수가 아닌 같은 변수로서 인식된다.
또한 var는 중복 선언이 혀용되기 때문에 개발시 의도치 않게 동일한 변수명을 선언하여 먼저 선언된 변수 값이 변경되는 문제점을 유발할 수 있다.

```javascript
// let 을 사용했을때
let name = "Jack";
if (name) {
  let name = "Koh";
  console.log("in :", name);
}
console.log("out :", name);
// 결과: in : Koh, out : Jack
```

<blockquote class="dev-tool">
  <div class="head">
    <p class="title arrow-down">Scope</p>
  </div>
  <div class="body">
    <p class="title arrow-down">Block</p>
    <p class="property">
      <span class="key">name</span>
      <span class="value">'Koh'</span>
    </p>
    <p class="title arrow-down">Script</p>
    <p class="property">
      <span class="key">name</span>
      <span class="value">'Jack'</span>
    </p>
    <p class="title arrow-right">Global</p>
  </div>
</blockquote>

이러한 결과가 출력되는 이유는 let 변수는 엔진이 스크립트를 읽으면서 블록을 만나게 되면 해당하는 블록 스코프를 만들고
내부에 변수를 설정하게 된다. 따라서 let 변수는 자신이 속한 block 내부에서 사용될수 있으며 외부에서는 인식할수 없다.
그러므로 위에서 선언된 name이라는 변수는 같은 이름을 사용하지만 다른범위를 가진 변수로서 서로 다른 변수다.

이어서 for문을 사용했을때 발생되는 현상에 대해서도 설명해 보겠다.

```javascript
// var 를 사용했을때
const arr = new Array(5); // length 5의 배열생성
for (var i = 0; i < arr.length; i++) {
  // 배열의 인덱스마다 자신의 인덱스를 출력하는 함수 할당
  arr[i] = function () {
    console.log(i);
  };
}
// 배열에 할당된 모든 함수 실행
arr.forEach((el) => el());
console.log("i :", i);
// 결과: 5, 5, 5, 5, 5
//       i: 5
```

```javascript
// let 을 사용했을때
const arr = new Array(5); // length 5의 배열생성
for (let i = 0; i < arr.length; i++) {
  // 배열의 인덱스마다 자신의 인덱스를 출력하는 함수 할당
  arr[i] = function () {
    console.log(i);
  };
}
// 배열에 할당된 모든 함수 실행
arr.forEach((el) => el());
console.log("i :", i);
// 결과: 0, 1, 2, 3, 4
//       Uncaught ReferenceError: i is not defined
```

위의 설명을 이해했다면 이러한 현상이 발생되는 이유는 이해가 됬을것이다.
이해가 되지 않았다면 직접 코드를 작성하며 연습해보는것이 좋다.

#### 4. const

- 값을 바꿀수 없는 변수 선언.
- 변수를 선언하면 초기값이 필수로 들어가야한다 (initializer).
- 대문자 사용이 관례(큰 의미는 없다).
- Object의 프로퍼티 값은 변경이 가능하다.
- 배열의 엘리먼트 값은 변경이 가능하다.

const 변수는 지금껏 설명했던 let의 특성을 모두 가지고 있다. 차이점이 있다면 변수가 아닌 상수로 사용되며
한번 할당한 값들은 변경할수 없는 변수이다. 변수를 사용하기 위해서는 초기값을 필수로 작성해야하며 참조자료형의
속성들에 대해서는 변경이 가능하다.

```javascript
const name = "Jack";
name = "Jack"; // 변경 불가능

const name = [1, 2, 3, 4, 5];
name[0] = 2; // 변경 가능

const name = { fistName: "Jack", lastName: "koh" };
name.lastName = "kim"; // 변경 가능
```
