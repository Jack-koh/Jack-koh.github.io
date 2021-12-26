---
layout: post
title: "일반 함수와 화살표함수의 차이는 무엇일까?"
summary: "ES6 Arrow function에 대해 알아보자"
author: jack-koh
date: "2021-12-17 13:58:47 GMT+0900"
category: ["JavaScript"]
tags: JavaScript
thumbnail: /assets/img/posts/thumnail_js.png
keywords: Jack, developer, es6, javascript, frontend, arrow, function, 자바스크립트, 화살표, 함수
usemathjax: false
permalink: /blog/2021-12-17-arrow-function/
---

# 화살표 함수와 일반함수의 차이는 무엇일까?

---

#### 1.화살표 함수의 기본문법

화살표 함수의 가장 특징적인 부분은 문법적으로 기존의 함수보다 간결해졌다는 것이다. 이하의 설명에서 전통적으로 사용해오던 함수는 일반함수로 부르겠습니다.
우선 `일반함수`와 `화살표 함수`의 형태부터 확인해보겠습니다.

```javascript
// 일반함수
const add = function (one, two) {
  return one + two;
};
console.log(add(1, 2)); // 결과: 3

const add = (one, two) => {
  return one + two;
};
console.log(add(1, 2)); // 결과: 3
```

`function` 키워드를 이용해서 함수를 선언과 `일반함수`와는는 다르게 `화살표함수`는 `=>` 를 사용하여 함수를 선언합니다.
위의 문법만 봐서는 그렇게 큰차이를 느끼지는 못하지만 여기서 몇가지 더 살펴볼 화살표 함수의 특징이 있습니다.

```javascript
const add = (one, two) => one + two;
console.log(add(1, 2)); // 결과: 3
```

- 화살표 함수는 한줄로 사용할때 함수블록과 `return`을 생략 가능하다.
- 화살표함수에서 블록을 사용하면 `return`을 작성하지 않은것과 같다.
- 이러한 현상은 화살표이기 때문이 아니라 블록을 해석하는 js 문법때문이다.

```javascript
const add = (one, two) => {
  one + two;
};
console.log(add(1, 2)); // 결과: undefinde

const add = (one, two) => {
  return one + two;
};
console.log(add(1, 2)); // 결과: 3
```

그렇다면 { key: value } 과같은 `오브젝트` 리터럴을 형태를 `반환`하고싶다면 어떻게 해야할까요?
반환하고 싶은 오브젝트를 `({ key: value })` 형태로 소괄호로 감싸서 작성해주면됩니다.

```javascript
const add = (one, two) => ({ sum: one + two });
console.log(add(1, 2)); // 결과: { sum: 3 }
```

#### 2.화살표 함수의 구조

화살표함수는 문법적인 차이보다도 `구조적인 차이`가 더 크다고 할수 있습니다. 아래의 구조를 살펴봅시다.

```javascript
const normal = function () {};
debugger;
const arrow = () => {};
debugger;
```

<blockquote class="dev-tool-scope">
  <dl>
    <dt class="down">Script</dt>
    <dd>
      <dl>
        <dt>
          <b class="sky">normal</b>
          <span><span class="orange">f</span> ()</span>
        </dt>
        <dd>
          <b class="dark-sky">arguments</b>
          <span class="gray">null</span>
        </dd>
        <dd>
          <b class="dark-sky">caller</b>
          <span class="gray">null</span>
        </dd>
        <dd>
          <b class="dark-sky">length</b>
          <span class="purple">0</span>
        </dd>
        <dd>
          <b class="dark-sky">name</b>
          <span>normal</span>
        </dd>
        <dd>
          <dl>
            <dt>
              <b class="dark-sky">prototype</b>
              <span>{ <span class="gray">constructor</span>: f }</span>
            </dt>
          </dl>
        </dd>
      </dl>
      <dl>
        <dt>
          <b class="sky">arrow</b>
          <span>() => {}</span>
        </dt>
        <dd>
          <b class="dark-sky">length</b>
          <span class="purple">0</span>
        </dd>
        <dd>
          <b class="dark-sky">name</b>
          <span>arrow</span>
        </dd>
        <dd>
          <b class="dark-sky">arguments</b>
          <span class="white">(...)</span>
        </dd>
        <dd>
          <b class="dark-sky">caller</b>
          <span class="white">(...)</span>
        </dd>
      </dl>
    </dd>
  </dl>
</blockquote>

위의 구조를 살펴보면 화살표 함수에는 `prototype`과 `constructor`가 없다는것을 확인할수 있습니다.  
이 말은 화살표 함수는 생성자 함수로 사용할수 없다는것과 prototype에 메소드를 연결하여 확장할수 없다는것과 같습니다.  
그렇기 때문에 `화살표 함수`는 불필요한 메소드들이 연결되어있지않고 `일반함수보다 가볍습니다.`  
그리고 개인적으로는 es6에는 일반함수를 생성자로 사용하는것보단 class 문법이 추가되어 일반함수를 생성자로 사용할일이 많지 않다고 생각됩니다.  
그러므로 함수를 단독으로 사용하기에는 화살표 함수가 일반함수보다는 용이하며 이것이 화살표 함수의 특징입니다.

#### 3.화살표 함수의 arguments

일반함수에서는 유동적인 인수를 `arguments` 라는것을 사용해서 처리했습니다.
아래의 예제 코드를 보겠습니다.

```javascript
const normal = function () {
  const args = arguments;
  console.log(args);
};
normal(1, 2, 3);
// 출력: Arguments(3) [1, 2, 3, callee: …]
```

위와같이 `일반함수에서는` `arguments` 는 Array-like 형태의 객체(Array가 아니다. iterable)를 이용하여 함수내의 모든 인수를 참조할수 있습니다.

반면 `화살표함수`에는 ReferenceError가 발생합니다. 즉 arguments를 사용할수 없습니다. 그렇다면 유동적인 인수를 다룰때 화살표 함수는 어떻게 다뤄야할까요?  
화살표 함수는 arguments 사용하지 못하는 대신 `rest` 파라미터를 제공합니다. 아래의 코드를 살펴보겠습니다.

```javascript
const arrow = (...rest) => {
  console.log(rest);
};
arrow(1, 2, 3);
// 출력: [1, 2, 3]

const arrow = (a, ...rest) => {
  console.log(rest);
};
arrow(1, 2, 3);
// 출력: [2, 3]
```

위의 코드와 같이 `rest`는 문자의미 그대로 명시되지 않은 모든 인수들을 Array 형태로 담에서 참조할수 있습니다.

#### 4.화살표 함수의 this

우선 일반함수가 `this`를 참조하는 방식을 살펴보겠습니다. 일반함수는 함수가 실행이 되었을때 자신을 실행시킨 오브젝트를 `this`로 참조합니다.

```javascript
var name = "Jack";
const normal = function () {
  console.log(this.name);
};
normal();
// 출력: 'Jack'
```

우선 함수가 스스로 실행을 하게되었을때의 `주체`는 기본적으로 `window객체`가 됩니다.
그러므로 위의 코드에서 this는 함수를 실행시킨 주체인 this window 오브젝트를 출력하게 됩니다.

```javascript
var name = "Jack";
const normal = function () {
  console.log(this.name);
};
const obj = {
  name: "Koh",
  normal: normal,
};
obj.normal();
// 출력: 'Koh'
```

이번 코드에서는 normal 함수를 obj라는 객체에 담아서 normal 함수를 실행시켰습니다. `obj` 오브젝트가 normal을 실행시킨 `주체`이기때문에 this는 obj를 가르킵니다.

반면 기본적으로 화살표 함수는 this를 가지고있지 않습니다. 그렇다면 화살표 함수가 가르키는 this는 누구일까요?

화살표함수가 this를 참조하는 방식에 대해 알아보겠습니다. 우선 화살표 함수는 `lexical(정적) 스코프의` this를 참조합니다. `lexical`스코프란 엔진이 해석할때, 화살표 함수를 만나면 function오브젝트를 생성하고 화살표 함수가 속한 스코프를 생성된 오브젝트에 바인딩을 합니다. 그리고 오브젝트에 바인딩된 스코프의 this를 화살표함수가 this로 사용하게 됩니다.

쉽게 말해 화살표함수는 자신이 선언된 환경의 this를 참조하고 this가 존재하지 않다면 상위 스코프의 this를 참조합니다.

설명도 어렵기때문에 이론적으로는 이해하기 어렵습니다. 아래의 코드를 살펴보겠습니다.

```javascript
var name = "Jack";
const arrow = () => {
  console.log(this.name);
};
arrow();
// 출력: 'Jack', 화살표 함수가 선언된 Global 블록에서 this를 참조.
// Global환경에서 this는 window를 가르킨다.
```

```javascript
var name = "Jack";
const Obj = function () {
  this.name = "Koh";
};
Obj.prototype.getName = () => console.log(this.name);
new Obj().getName();
// 출력: 'Jack', 화살표 함수가 선언된 스코프는 Global 블록이다.
// 따라서 이 코드 또한 this는 window를 가르킨다.
```

```javascript
var name = "Jack";
const Obj = function () {
  this.name = "Koh";
};
Obj.prototype.getName = function () {
  const name = () => {
    console.log(this.name);
  };
  name();
};
new Obj().getName();
// 출력: 'Koh', 화살표 함수가 선언된 스코프는 getName의 Local 블록이다.
// 따라서 this는 getName을 실행시킨 Obj를 가르킨다.
```

```javascript
var name = "Jack";
const Obj = function () {
  this.name = "Koh";
  this.getName = () => {
    console.log(this.name);
  };
};
new Obj().getName();
// 출력: 'Koh', 화살표 함수가 선언된 스코프는 Obj Local 블록이다.
// 따라서 this는 Obj를 가르킨다.
```

- 화살표함수의 this는 어디에 선언이 되는지에따라 가르키는 this가 달라진다.
- 화살표 함수는 function 오브젝트가 생성될때 오브젝트가 스코프를 생성된 오브젝트에 바인딩을 한다.
- 한번 바인딩 된 화살표 함수의 this 는 다시 bind함수를 사용하여 래핑을 한다고 해서 this가 변경되지 않는다.
