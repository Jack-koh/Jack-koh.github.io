---
layout: post
title: "JavaScript 의 prototype"
summary: "객체를 바라보는 관점의 차이"
author: jack-koh
date: "2021-12-18 13:58:47 GMT+0900"
category: ["JavaScript"]
tags: JavaScript
thumbnail: /assets/img/posts/thumnail_js.png
keywords: Jack, developer, es6, javascript, 자바스크립트, frontend, 프론트엔드, class, prototype, 클래스, 프로토타입, OOP, 객체지향
usemathjax: false---
permalink: /blog/2021-12-17-prototype/
---

# Classes vs Prototypes

---

#### 1. 서론

자바스크립트를 처음 접했을때부터 지금까지 공부를 하다보면 뭔가 개념이 정립이 되지않고 겉도는듯한 느낌의 녀석들이 있습니다.  
바로 `prototype, this, lexical` 이녀석들인데요, 모두가 알다시피 이녀석들은 수학 공식을 외우듯이 사용법을 익히고 적응을 해야합니다.  
다양한 강의나 인터넷의 자료를 찾아봐도 스펙과 현상에 대해서 이야기를 하지만 원리나 이러한 방식을 채택하는 이유에 대해서는
명쾌한 해답들이 없었습니다. 그러다 우연한 기회에 이러한 현상들을 철학에 근거해 원론적인 의미를 설명하는 블로그를 찾을수있었고
블로그 글에 대한 흥미가 생겨 `prototype`에 대해서 더 깊이 알아보기 시작했습니다.

이 글을 통해서 prototype을 더이상 수학공식이 아닌 자바스크립트를 지탱하는 원리로 이해할수 있기를 기대합니다.

#### 플라톤과 idea, 아리스토텔레스와 classification

우선 prototype에 대해 이해하기에 앞서 prototype 의 대척점인 `Class의 기원`에 대해 이해를 해야합니다.

플라톤의 이데아론에 따르면 세계는 세계는 참된 실재들로만 구성되어 있는 `이데아계`, 우리가 현재 살고 있는 `현상계` 둘로 나뉘어 있습니다.  
그리고 이데아만이 참된 실재이기 때문에, 현상계에 있는 것들은 모두 거짓된 존재들이다 라고 설명합니다.

```
이데아 - 현상
영혼 - 육체
추상적 - 구체적
관념론 - 유물론
```

<p class="image__center">
  <img src="/assets/img/posts/javascript/prototype/플라톤.jpg" alt="플라톤" />
</p>

<p class="speak">"모든 서양철학의 전통은 플라톤에 대한 각주에 불과하다" - 화이트 헤드</p>

그렇다면 대체 이데아란 무엇일까요? 이를 알아보기 위해선 하나의 사례를 살펴볼 필요가 있습니다. 세상에는 많은 의자가 존재합니다.  
어떤 것은 나무로 된 것이며, 또 아떤 것은 푹신푹신한 쿠션으로 만들어져 있죠. 1인용도 있으며, 여러 사람이 함께 사용할 수 있는 의자도 있습니다.
하지만 이들은 모두 _'의자라고 불리는 무언가를 공통적으로 가지고 있습니다.'_ `이것들이 공유하는 것이 바로 이상적인 의자와의 관계, 즉 의자 '이데아' 와의 관계입니다.`
플라톤이 보기에 이데아는 실제로 존재하며, 그곳의 의자야 말로 '진정한' 의자라고 할수 있습니다. 현상계의 모든 의자들은 이데아의 복사물에 불과하죠.

```javascript
class 이데아 {...} // 이데아에 실재 존재하는 추상적인 물체(class)
const 현상 = new 이데아(); // 현상계에 구체화된 이데아의 복사물(instance)
```

후에 플라톤 이데아론은 그의 제자 아리스토텔레스에 의해 정립이 되는데 플라톤은 현실세계의 원리를 눈에 보이지 않는 관념적인 것으로
설명하려 했던 반면 아리스토 텔레스는 실제 감각되고 눈에 보이는 물질적인 것들로 현실 세계를 설명하려 했다. 이를 토대로
아리스토텔레스는 자연현상과 관련된부분에 집중을 했고 세상의 자연물들(동물, 식물 등)을 `동일한 속성을 갖는 개체라면 같은범주에 속하는 하나의 그룹(category)`으로 묶었고.
이를 `분류(classification)`이라고 합니다. class 의 어원처럼 보이네요.

```
아리스토 텔레스의 자연관

그의 스승인 플라톤과 같이, 아리스토텔레스의 철학은 보편성을 향해 있다.
하지만 아리스토텔레스는 그의 스승 플라톤이 보편성은 특정한 것에서 멀리 떨어져 존재하며,
이와 같은 보편성은 그들의 원형(原型) 또는 전형으로써 주장하였던 것과 대조적으로 특정한 것에서 보편성을 발견하였고
그것을 사물들의 본질(essence of things)이라 칭하였다.
따라서 아리스토텔레스에게 철학적인 방법이란 특정한 현상에 대한 연구로부터 본질에 관한 지식에 이르기까지의 과정을 포괄하며,
이는 플라톤의 이데아 또는 형상(form)이라 불린 것에 관한 지식으로부터 이들과 유사한 형태로의 관상(contemplation),
다시 말해 인식하는 양식에 이르기까지의 하향적인 과정을 취한 플라톤의 방법과 차이를 보인다.
아리스토텔레스에게 있어 '형상(form)'은 여전히 현상에 대한 무조건적인 기초를 가리키지만 이와 동시에 특정한 사물에 '구체화'되어 있다.
플라톤의 방법이 선험적인 원칙으로부터의 연역 추론에 기반해 있는 것이라면, 그의 방법은 귀납적이면서 연역적이라 할 수 있다.
```

이러한 아리스토텔레스의 분류개념은는 후에 서구와 많은 나라들의 공통된 생각으로 이어졌고 서구의 이분법적 사고에 큰 영향을 끼치게 되며, 실제로
[Fred Brooks](https://ko.wikipedia.org/wiki/%ED%94%84%EB%A0%88%EB%8D%94%EB%A6%AD_%EB%B8%8C%EB%A3%A9%EC%8A%A4). 을 비롯한 많은 연구가들에게 많은 영감을 줍니다.

```javascript
class 조류 {
  constructor() {
    this.날개 = true;
    this.부리 = true;
    this.깃털 = true;
  }
}

const 참새 = new 조류();
```

#### 분류(classification)에 대한 비판과 가족유사성(Family Resemblance)

아리스토텔레스의 분류(classification)는 오랫동안 잘 이해되고 문제가 없는것으로 간주되었습니다. 그러다 19세기에 들어서 유명한 영국 철학자 W. Whewell과 W.S. 제본스는
어떤 속성을 기반으로 분류를 해야할지에 대한 보편적인 규칙은 없다고 강조했습니다. 이러한 관점에 비추어 볼 때 일반화된 규칙으로 사물을 분류하는것을 오히려
더 복잡한 결과를 초래한다고 주장했습니다.

이러한 비판은 19세기 후반 매우 유명한 철학자인 [루트비히 비트겐슈타인(Ludwig Wittgenstein)](https://ko.wikipedia.org/wiki/%EB%A3%A8%ED%8A%B8%EB%B9%84%ED%9E%88_%EB%B9%84%ED%8A%B8%EA%B2%90%EC%8A%88%ED%83%80%EC%9D%B8).
에 의해 계속 되었습니다.

<p class="image__center">
  <img src="/assets/img/posts/javascript/prototype/비트겐슈타인.jpg" alt="비트겐슈타인" />
</p>

<p class="speak">공유 속성의 관점에서 정의하기 어려운 개념이 있다(사실상 올바른 분류란 없다) — 비트겐슈타인</p>

비트겐슈타인은 어떤 특성들이 분류를 하기위한 필수적인 속성인지 구분하는것은 어렵다고 말합니다. 그러면서 게임을 예로 들었는데요
게임은 단순한 개념을 가지고 있으면서 공유의 관점에서는 정의하기 어렵습니다.

서구의 전통놀이 ring-around-the-rosy처럼(승자와 패자가 없는 게임이라고 합니다.) 경쟁이 없는 게임이 있을수도있고,
주사위던지기처럼 운과 관련된 게임도 있습니다. 체스처럼 기술이 필요한 게임도 있고 포커처럼 운과 기술 모두가 필요한 게임도있습니다.

미술품 또한 공유속성의 관점에서 정의하기 어려운 개념중 하나입니다. 예술과 예술에 대한 명확한 경계를 정의 할수 있는사람은 아무도없고
공통의 속성을 가진 예술작품 또한 존재하지 않습니다. 예술에 대한 정의는 주관적이며 상황이나 관점에 크게 의존하기때문입니다.

이러한 분류모델에 대한 비판을 제시한 후 비트겐슈타인은 `가족유사성(Family Resemblance)`이라는 이론은 제시합니다.
가족 유사성은, 예를들어 당신과 당신의 형은 엄마의 눈을 닮았지만, 당신과 누나는 아빠의 높은 광대뼈를 닮았습니다.
이처럼 가족 구성원들은 서로 가족이라는 하나의 그룹으로 묶일때 가족 모두가 공유하는 단일 속성들이 필요하지 않습니다.
그러면서 대부분의 개념에서의 의미는 공유된 속성으로서의 정의가 아닌 가족에 의해 결정된다고 이야기 합니다.
이는 후에 프로토타입 이론의 기초가 됩니다.

#### 원형이론(prototype theory)의 등장

비트겐슈타인의 연구 결과는 1970년경 [엘리노어 로쉬(Eleanor Rosch)](https://ko.wikipedia.org/wiki/%EC%97%98%EB%A6%AC%EB%85%B8%EC%96%B4_%EB%A1%9C%EC%89%AC).에 의해 원형 이론에 대한 연구로 발전했습니다. Rosch는 범주에는 일반적으로 가장 좋은 예(원형, 프로토타입)가 있으며 범주화에는 인간의 모든 감각이 관여한다고 했습니다.

<p class="speak">공유 속성으로 범주를 정의 하는 경우 그 범주에 속한 어떠한 구성원도 범주의 가장 적절한 표본이 되지 않는다. - 로쉬</p>
로쉬의 원형 이론에 따르면 원형(prototype)은 범주의 가장 적합한 특징들을 가지고 있고, 우리가 형성하는 심리학적인 범주는 원형(prototype)을 중심으로 구성됩니다.
그리고 범주에 속한 객체들은 원형을 중심으로 등급이 매겨진다고 표현했습니다.

1975년 로쉬는 한가시 실험을 실시합니다.

- 실험 참가자들에게 여러 범주 구성원(사과, 코코넛, 오렌지)의 속성을 적어보라고 함
- 각 범주 구성원에 대해 범주의 다른 구성원과 공유하는 속성의 개수를 도출함
- 사과, 오렌지 : 2점(둥글다. 즙이 있다.)
- 코코넛 : 1점(둥글다)

점수가 높을수록 ‘가족 유사성'이 높다고 볼 수 있습니다. 전통적인 분류에선 모두 과일로 볼 수 있지만, 프로토타입 이론에서는 사과와 오렌지가 가장 전형적인 무언가라고 볼 수 있습니다. 반면에 코코넛은 저 중에서 가장 비전형적인 것으로 볼 수 있습니다.

예를들어 여러분의 생각속에 전형적인 새의 조건을 갖춘 새가 참새(prototype)라면 비둘기는 타조보다 더 등급이 높은 새로 분류됩니다.
즉, 객체는 ‘정의’로부터 분류되는 것이 아니라 가장 좋은 보기(prototype)로부터 범주화된다고 합니다.

이 실험을 통해 로쉬는 “인간은 ‘등급이 매겨진 (개념) 구조(graded structure)’를 가진다”라고 주장합니다.
인간은 사물을 분류할 때 자연스럽게 가장 유사성 높은 것 순서대로 등급을 매긴다는 의미로 볼 수 있습니다.
이렇게 분류했을 때 가장 높은 등급을 가진 녀석이 나올 텐데요, 이것이 바로 원형(Prototype)이다. 란 주장이 프로토타입 이론입니다.

<p class="image__center">
  <img src="/assets/img/posts/javascript/prototype/새.jpg" alt="새" />
</p>

‘새'를 예로 들어볼까요?
‘참새'는 새의 범주를 대표할 만한 가장 전형적인 녀석입니다. 이 녀석을 ‘원형(prototype)'으로 간주하겠습니다.

‘타조'는 전통적인 분류에선 같은 새가 되지만 프로토타입 이론에서는 ‘원형'에서 가장 멀리 떨어진, 즉 ‘비전형적인' 녀석이 됩니다. 범주의 가장 끄트머리에 있는 녀석이 되는 거죠.

즉, 객체는 ‘정의’로부터 분류되는 것이 아니라 가장 좋은 보기(prototype)로부터 범주화된다고 합니다.

이러한 분류 체계는 매우 경제적입니다. 만약 우리가 새로운 대상을 접해서 분류해야 할 때, 우리는 새로운 대상의 몇 가지 특징만 원형(prototype)과 비교확인만 하면 됩니다. 특징이 다를수록 원형에서 멀~리 떨어진 범주가 되는 거죠.

이 이론에 또 한 가지 중요한 것이 있습니다.

같은 단어라 할지라도 누가 `어떤 상황(context)`에서 접했나에 따라 의미가 달라진다는 것입니다. (의미사용이론)

예를 들면 아이가 생각하는 새의 범주에서 ‘참새’는 명확하게 새에 속하지만 ‘펭귄' 은 해당 범주에 속하지 못할 수도 있습니다.
아이가 생각할 땐 펭귄이 매우 비전형적이기 때문이죠. 하지만 조류학자가 생각할 때 ‘참새'와 ‘펭귄'은 명확하게 유사한 새의 범주에 속할 수 있습니다.
같은 단어여도 어떤 상황(누가, 어디서…)에서 접했나에 따라 범주는 크게 달라집니다.

- 현실에 존재하는 것 중 가장 좋은 본보기를 원형(prototype)으로 선택한다.
- 문맥(컨텍스트)에 따라 ‘범주’, 즉 ‘의미’가 달라진다.

#### prototype-based programming

이러한 프로토타입 이론은 그대로 프로토타입 기반 객체지향 프로그래밍 언어를 통해 구현되었습니다. 1980~90년대에 이 이론을 토대로 많은 프로토타입 기반 언어가 생겨났습니다. 대표적으로 Javascript의 모태인 Self 언어(Self 에서 로쉬의 프로토타입을 참고한 부분은 [Prototype-based programming](https://en.wikipedia.org/wiki/Prototype-based_programming). 문서 참고)와, 가족유사성(Family Resemblance)을 언어 차원에서 완벽하게 구현한 Kevo 가 있습니다.

프로토타입 기반 OOP 언어의 특징은 다음과 같습니다.

- 개별 객체(instance) 수준에서 메소드와 변수를 추가
- 객체 생성은 일반적으로 복사를 통해 이루어짐
- 확장(extends)은 클래스가 아니라 위임(delegation)
- 현재 객체가 메시지에 반응하지 못할 때 다른 객체로 메시지를 전달할 수 있게 하여 상속의 본질을 지원
- 개별 객체 수준에서 객체를 수정하고 발전시키는 능력은 선험적 분류의 필요성을 줄이고 반복적인 프로그래밍 및 디자인 스타일을 장려
- 프로토타입 프로그래밍은 일반적으로 분류하지 않고 유사성을 활용하도록 선택
- 결과적으로 설계는 맥락에 의해 평가

지금까지 지루한 철학적인부분에 대해서 대략적으로 살펴봤습니다. 사실 철학적인 모든부분을 이해할필요는 없습니다.(플라톤이 누구이며 아리스토텔레스는 누구이며..)
결국 우리는 prototype-based language 인 자바스크립트가 객체를 어떤 관점으로 바라보는지 궁굼했었고 이러한 관점들이 어떻게 코드에 녹아있는지가 궁금했습니다.
지금부터는 코드를 보며 앞서 설명한 컨셉들이 어떤식으로 적용이 되어있는지 살펴봅시다.

```javascript
const arr = []; // 배열
const obj = {}; // 오브젝트
debugger;
```

<blockquote class="dev-tool">
  <div class="head">
    <p class="title arrow-down">Scope</p>
  </div>
  <div class="body">
    <p class="title arrow-down">Script</p>
     <p class="sub-title arrow-down">
      <span class="key">arr</span>
      <span class="value">Array(0)</span>
    </p>
    <p class="property">
      <span class="key">0</span>
      <span class="value purple">1</span>
    </p>
    <p class="property">
      <span class="key">1</span>
      <span class="value purple">2</span>
    </p>
    <p class="property">
      <span class="key">2</span>
      <span class="value purple">3</span>
    </p>
    <p class="property">
      <span class="key dark-sky">length</span>
      <span class="value purple">3</span>
    </p>
    <div class="margin-l-10 margin-b-10">
      <p class="sub-title arrow-down">
        <span class="key gray">[[Prototype]]</span>
        <span class="value">Array(0)</span>
      </p>
      <div class="margin-l-10 margin-b-10">
        <p class="property">
          <span class="key dark-sky">map</span>
          <span class="value gray"><span class="orange">f</span> map()</span>
        </p>
        <p class="property">
          <span class="key dark-sky">pop</span>
          <span class="value gray"><span class="orange">f</span> pop()</span>
        </p>
        <p class="property">
          <span class="key dark-sky">push</span>
          <span class="value gray"><span class="orange">f</span> push()</span>
        </p>
        <p class="property">
          <span class="key dark-sky">reduce</span>
          <span class="value gray"><span class="orange">f</span> reduce()</span>
        </p>
        <p class="sub-title arrow-right">
          <span class="key gray">[[Prototype]]</span>
          <span class="value">Object</span>
        </p>
      </div>
    </div>
    <p class="sub-title arrow-down">
      <span class="key">obj</span>
      <span class="value"></span>
    </p>
    <p class="property">
      <span class="key">name</span>
      <span class="value">"Jack"</span>
    </p>
    <div class="margin-l-10">
      <p class="sub-title arrow-down">
        <span class="key gray">[[Prototype]]</span>
        <span class="value">Object</span>
      </p>
      <div class="margin-l-10 margin-b-10">
        <p class="sub-title arrow-right">
          <span class="key dark-sky">constructor</span>
          <span class="value"><span class="orange">f</span> Object()</span>
        </p>
        <p class="sub-title arrow-right">
          <span class="key dark-sky">hasOwnProperty</span>
          <span class="value"><span class="orange">f</span> hasOwnProperty()</span>
        </p>
        <p class="sub-title arrow-right">
          <span class="key dark-sky">isPrototypeOf</span>
          <span class="value"><span class="orange">f</span> isPrototypeOf()</span>
        </p>
        <p class="sub-title arrow-right">
          <span class="key dark-sky">propertyIsEnumerable</span>
          <span class="value"><span class="orange">f</span> propertyIsEnumerable()</span>
        </p>
        <p class="sub-title arrow-right">
          <span class="key dark-sky">toLocaleString</span>
          <span class="value"><span class="orange">f</span> toLocaleString()</span>
        </p>
        <p class="sub-title arrow-right">
          <span class="key dark-sky">toString</span>
          <span class="value"><span class="orange">f</span> toString()</span>
        </p>
        <p class="sub-title arrow-right">
          <span class="key dark-sky">valueOf</span>
          <span class="value"><span class="orange">f</span> valueOf()</span>
        </p>
        <p class="sub-title arrow-right">
          <span class="key dark-sky">__proto__</span>
          <span class="value">(...)</span>
        </p>
      </div>
    </div>
  </div>
</blockquote>

우리는 배열과 오브젝트의 구조를 살펴보았습니다. 위의 구조를 살펴보면 배열은 자기자신의 고유속성으로 인덱스와 인덱스에 해당하는 값을 가지고있고 length를 가지고 있습니다.
그리고 prototype이 Array인 객체를 참조하고 있고 우리가 만들어낸 arr 이라는 객체는 Array의 속성을 사용할수 있습니다. 그리고 Array 객체에 걸려있는 prototype을 보면
Array의 prototype은 Object로 설정되어 있습니다. 밑에 있는 obj의 구조를 살펴보면 obj의 고유 속성은 { name: 'Jack' } 이며 obj객체의 prototype은 Object입니다.

이를 통해 우리가 알수 있는 사실이 있다. 바로 자바스크립트에서 원형(가장 좋은 본보기)은 Object이며 모든 객체가 결국 Object를 prototype(원형)으로 참조하고있습니다.
이러한 현상은 앞서 설명한 로쉬의 `등급이 매겨진 (개념) 구조(graded structure)`를 따릅니다. 따라서 위의 코드에선 obj가 더 높은 가족 유사성이 높고 arr는 obj보단 원형으로 부터
거리가 있는 개체입니다.

이러한 구조를 바탕으로 직접 위에서 언급한 참새에 대한 구조를 코드로 구현해봅시다.

```javascript
// 원형(prototype) 객체
const 새 = new Object({
  등급: 1,
  깃털: true,
  부리: true,
  날개: true,
  날수있나: true,
});

// 날지 못하는 새들의 prototype
const 못나는새 = Object.create(새);
못나는새.날수있나 = false;
못나는새.등급 = 2;

const 참새 = Object.create(새); // 참새는 날수 있는 새이므로 원형의 속성을 위임받는다.
const 타조 = Object.create(못나는새); // 타조는 날수없는 새이므로 못나는새의 속성을 위임받는다.
```

<blockquote class="dev-tool">
  <div class="head">
    <p class="title arrow-down">Scope</p>
  </div>
  <div class="body">
    <p class="title arrow-down">Script</p>
     <p class="sub-title arrow-down">
      <span class="key">새</span>
      <span class="value"></span>
    </p>
    <div class="margin-l-10 margin-b-10">
      <p class="property">
        <span class="key">등급</span>
        <span class="value purple">1</span>
      </p>
      <p class="property">
        <span class="key">깃털</span>
        <span class="value purple">true</span>
      </p>
      <p class="property">
        <span class="key">날개</span>
        <span class="value purple">true</span>
      </p>
      <p class="property">
        <span class="key">날수있나</span>
        <span class="value purple">true</span>
      </p>
      <p class="property">
        <span class="key">부리</span>
        <span class="value purple">true</span>
      </p>
      <p class="sub-title arrow-right">
        <span class="key gray">[[Prototype]]</span>
        <span class="value">Object</span>
      </p>
    </div>
    <p class="sub-title arrow-down">
      <span class="key">못나는새</span>
      <span class="value"></span>
    </p>
    <div class="margin-l-10 margin-b-10">
      <p class="property">
        <span class="key">등급</span>
        <span class="value purple">2</span>
      </p>
      <p class="property">
        <span class="key">날수있나</span>
        <span class="value purple">false</span>
      </p>
      <p class="sub-title arrow-right">
        <span class="key gray">[[Prototype]]</span>
        <span class="value">새</span>
      </p>
    </div>
    <p class="sub-title arrow-down">
      <span class="key">참새</span>
      <span class="value"></span>
    </p>
    <div class="margin-l-10 margin-b-10">
      <p class="sub-title arrow-right">
        <span class="key gray">[[Prototype]]</span>
        <span class="value">새</span>
      </p>
    </div>
    <p class="sub-title arrow-down">
      <span class="key">타조</span>
      <span class="value"></span>
    </p>
    <div class="margin-l-10 margin-b-10">
      <p class="sub-title arrow-right">
        <span class="key gray">[[Prototype]]</span>
        <span class="value">못나는새</span>
      </p>
    </div>
  </div>
</blockquote>

객체들의 구조를 살펴보면 참새는 새로부터 속성을 위임받습니다. 원형에 가깝고 새의 표본이 될수 있는객체이므로 등급도 높습니다.  
타조는 못나는새로부터 속성을 위임받고 못나는새는 새로부터 속성을 위임받습니다. 원형과 거리가 멀고 등급도 낮으므로 다른 새들의 표본이 되지는 못합니다.

사실 위의 구조는 class를 통해서도 똑같이 구현이 가능합니다. 하지만 우리는 관점에 대해서 생각해봐야합니다.
못나는 새라는 범주가 정의되고 이러한 범주에 의해 타조의 속성이 생기는것이 아니라 타조의 속성이 있고 이를 기반으로 못나는 새라는 범주에 들어가는것 입니다.
이 부분이 prototype-base programming의 가장 핵심부분인것같습니다.

이후 설명할 this 및 context, lexical에서 이러한 관점은 아주 중요한 지표가 됩니다.

#### this, 실행 컨텍스트(excution context), lexical scope, hoisting

자 이제 앞서배운 prototype 원리에 기반하여 자바스크립트의 중요한 부분인 `this, lexical scope, hoisting` 에대해서 설명해보겠습니다.

#### 참조

[플라톤의 이데아론과 객체지향 패러다임 비교 연구](https://dcollection.yonsei.ac.kr/public_resource/pdf/000000128475_20211223120406.pdf).  
[프로토타입 의미론의 위상](http://daegu.dcollection.net/public_resource/pdf/000002419851_20211223121258.pdf).  
[Classes vs Prototypes](http://citeseerx.ist.psu.edu/viewdoc/download?doi=10.1.1.56.4713&rep=rep1&type=pdf).  
[Class-based programming](https://en.wikipedia.org/wiki/Class-based_programming).  
[아리스토텔레스](https://ko.wikipedia.org/wiki/%EC%95%84%EB%A6%AC%EC%8A%A4%ED%86%A0%ED%85%94%EB%A0%88%EC%8A%A4).
[자바스크립트는 왜 프로토타입을 선택하게 됬을까?](https://medium.com/@limsungmook/%EC%9E%90%EB%B0%94%EC%8A%A4%ED%81%AC%EB%A6%BD%ED%8A%B8%EB%8A%94-%EC%99%9C-%ED%94%84%EB%A1%9C%ED%86%A0%ED%83%80%EC%9E%85%EC%9D%84-%EC%84%A0%ED%83%9D%ED%96%88%EC%9D%84%EA%B9%8C-997f985adb42)
