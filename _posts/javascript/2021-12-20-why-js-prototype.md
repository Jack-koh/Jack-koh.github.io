---
layout: post
title: 'JavaScript 의 prototype'
summary: '객체를 바라보는 관점의 차이'
author: jack-koh
date: '2021-12-18 13:58:47 GMT+0900'
category: ['JavaScript']
tags: JavaScript
thumbnail: /assets/img/posts/thumnail_js.png
keywords: Jack, developer, es6, javascript, 자바스크립트, frontend, 프론트엔드, class, prototype, 클래스, 프로토타입, OOP, 객체지향
usemathjax: false---
permalink: /blog/2021-12-17-iterable/
---

# Classes vs Prototypes

---

#### 1. 참조

[자바스크립트는 왜 프로토타입을 선택했을까](https://medium.com/@limsungmook/%EC%9E%90%EB%B0%94%EC%8A%A4%ED%81%AC%EB%A6%BD%ED%8A%B8%EB%8A%94-%EC%99%9C-%ED%94%84%EB%A1%9C%ED%86%A0%ED%83%80%EC%9E%85%EC%9D%84-%EC%84%A0%ED%83%9D%ED%96%88%EC%9D%84%EA%B9%8C-997f985adb42).  
[Classes vs Prototypes](http://citeseerx.ist.psu.edu/viewdoc/download?doi=10.1.1.56.4713&rep=rep1&type=pdf).

서구에서 바라보는 이분법적 사고의 기원, 플라톤과 아리스토텔레스의 분류개념을 설명하면서
class 관점에서 바라보는 객체에 대한 관념 그리고 prototype의 등장배경까지 이야기 형식으로 잘 설명해놓은 글입니다.
위의 글에서 설명한 방식대로 간단하게 class 과 prototype의 관점에 대해서 이야기를 해보겠습니다.

#### 2. Classes and classification
