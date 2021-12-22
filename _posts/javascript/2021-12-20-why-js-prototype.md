---
layout: post
title: "자바스크립트는 왜 프로토타입을 선택했을까"
summary: "prototype에 대한 이해와 유래"
author: jack-koh
date: "2021-12-18 13:58:47 GMT+0900"
category: ["JavaScript"]
tags: JavaScript
thumbnail: /assets/img/posts/thumnail_js.png
keywords: Jack, developer, es6, javascript, 자바스크립트, frontend, 프론트엔드, class, prototype, 클래스, 프로토타입, OOP, 객체지향
usemathjax: false---
permalink: /blog/2021-12-17-iterable/
---

# Classes vs Prototypes

---

#### 1. prototype-based language

JavaScript는 흔히 `프로토타입 기반언어(prototype-based language)`라고 불립니다. 그렇다면 프로토타입이란 무엇일까요?  
자바스크립트 개발자라면 필수적으로 이해하고 넘어가야하는 부분이고 자바스크립트 철학의 근간을 이루는 부분이라고 생각하기때문에
이번 포스트는 프로토타입에 대한 생각을 정리해보려고 합니다.

우선 prototype 과 class 는 객체를 바라보는 개념자체가 상반된 방식입니다. 특히 문맥을 강조하는 prototype의 특성상
렉시컬 스코프에 의한 호이스팅과 실행문맥에 의한 복잡한 this가 필연적으로 발생할수 밖에 없었습니다.

이 글을 통해 보다 근본적인 이해를 더하고 javascript의 prototype 방식의 철학을 이해할수 있기를 기대합니다.

#### 2. Classes and classification
