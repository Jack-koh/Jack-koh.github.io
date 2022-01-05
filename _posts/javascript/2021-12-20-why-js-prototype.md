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
  <img class="w-80" src="/assets/img/posts/javascript/prototype/플라톤.jpg" alt="플라톤" />
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
  <img class="w-80" src="/assets/img/posts/javascript/prototype/비트겐슈타인.jpg" alt="비트겐슈타인" />
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
  <img class="w-80" src="/assets/img/posts/javascript/prototype/새.jpg" alt="새" />
</p>

‘새'를 예로 들어볼까요?
‘참새'는 새의 범주를 대표할 만한 가장 전형적인 녀석입니다. 이 녀석을 ‘원형(prototype)'으로 간주하겠습니다.

‘타조'는 전통적인 분류에선 같은 새가 되지만 프로토타입 이론에서는 ‘원형'에서 가장 멀리 떨어진, 즉 ‘비전형적인' 녀석이 됩니다. 범주의 가장 끄트머리에 있는 녀석이 되는 거죠.

즉, 객체는 ‘정의’로부터 분류되는 것이 아니라 가장 좋은 보기(prototype)로부터 범주화된다고 합니다.

이러한 분류 체계는 매우 경제적입니다. 만약 우리가 새로운 대상을 접해서 분류해야 할 때, 우리는 새로운 대상의 몇 가지 특징만 원형(prototype)과 비교확인만 하면 됩니다. 특징이 다를수록 원형에서 멀~리 떨어진 범주가 되는 거죠.

이 이론에는 또 한 가지 중요한 관점이 있습니다.

같은 단어라 할지라도 누가 `어떤 상황(context)`에서 접했나에 따라 의미가 달라진다는 것입니다. (비트겐슈타인의 의미사용이론)

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

<blockquote class="dev-tool-scope">
  <dl>
    <dt class="down">Script</dt>
    <dd>
      <dl>
        <dt class="down">
          <b class="sky">arr</b>
          <span>Array(3)</span>
        </dt>
        <dd>
          <b>0</b>
          <span class="purple">1</span>
        </dd>
        <dd>
          <b>1</b>
          <span class="purple">2</span>
        </dd>
        <dd>
          <b>2</b>
          <span class="purple">3</span>
        </dd>
        <dd>
          <b class="dark-sky">length</b>
          <span class="purple">3</span>
        </dd>
        <dd>
          <dl>
            <dt class="down">
              <b>[[prototype]]</b>
              <span>Array(0)</span>
            </dt>
            <dd>
              <b class="dark-sky">map</b>
              <span>
                <span class="orange">f</span> <span>map()</span>
              </span>
            </dd>
            <dd>
              <b class="dark-sky">pop</b>
              <span>
                <span class="orange">f</span> <span>pop()</span>
              </span>
            </dd>
            <dd>
              <b class="dark-sky">push</b>
              <span>
                <span class="orange">f</span> <span>push()</span>
              </span>
            </dd>
            <dd>
              <b class="dark-sky">reduce</b>
              <span>
                <span class="orange">f</span> <span>reduce()</span>
              </span>
            </dd>
            <dd>
              <dl>
                <dt>
                  <b>[[prototype]]</b>
                  <span>Object</span>
                </dt>
              </dl>
            </dd>
          </dl>
        </dd>
      </dl>
    </dd>
    <dd>
      <dl>
        <dt class="down">
          <b class="sky">obj</b>
          <span></span>
        </dt>
        <dd>
          <b>name</b>
          <span>"Jack</span>
        </dd>
        <dd>
          <dl>
            <dt class="down">
              <b>[[prototype]]</b>
              <span>Object</span>
            </dt>
            <dd>
              <dl>
                <dt>
                  <b class="dark-sky">constructor</b>
                  <span>
                    <span class="orange">f</span> <span class="gray">Object()</span>
                  </span>
                </dt>
              </dl>
            </dd>
            <dd>
              <dl>
                <dt>
                  <b class="dark-sky">hasOwnProperty</b>
                  <span>
                    <span class="orange">f</span> <span class="gray">hasOwnProperty()</span>
                  </span>
                </dt>
              </dl>
            </dd>
            <dd>
              <dl>
                <dt>
                  <b class="dark-sky">isPrototypeOf</b>
                  <span>
                    <span class="orange">f</span> <span class="gray">isPrototypeOf()</span>
                  </span>
                </dt>
              </dl>
            </dd>
            <dd>
              <dl>
                <dt>
                  <b class="dark-sky">propertyIsEnumerable</b>
                  <span>
                    <span class="orange">f</span> <span class="gray">propertyIsEnumerable()</span>
                  </span>
                </dt>
              </dl>
            </dd>
            <dd>
              <dl>
                <dt>
                  <b class="dark-sky">toLocaleString</b>
                  <span>
                    <span class="orange">f</span> <span class="gray">toLocaleString()</span>
                  </span>
                </dt>
              </dl>
            </dd>
            <dd>
              <dl>
                <dt>
                  <b class="dark-sky">toString</b>
                  <span>
                    <span class="orange">f</span> <span class="gray">toString()</span>
                  </span>
                </dt>
              </dl>
            </dd>
            <dd>
              <dl>
                <dt>
                  <b class="dark-sky">valueOf</b>
                  <span>
                    <span class="orange">f</span> <span class="gray">valueOf()</span>
                  </span>
                </dt>
              </dl>
            </dd>
            <dd>
              <dl>
                <dt>
                  <b class="dark-sky">__proto__</b>
                  <span>(...)</span>
                </dt>
              </dl>
            </dd>
          </dl>
        </dd>
      </dl>
    </dd>
  </dl>
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
const 새 = function () {
  this.등급 = 1;
  this.깃털 = true;
  this.부리 = true;
  this.날개 = true;
  this.날수있나 = true;
};

const 참새 = new 새();
// 못나는새 객체
const 못나는새 = function () {
  this.날수있나 = false;
  this.등급 = 2;
};
못나는새.prototype = 참새; // 원형의 속성을 위임받는다.
const 타조 = new 못나는새();
debugger;
```

<blockquote class="dev-tool-scope">
  <dl>
    <dt class="down">Script</dt>
    <dd>
      <dl>
        <dt>
          <b class="sky">새</b>
          <span><span class="orange">f</span> ()</span>
        </dt>
      </dl>
      <dl>
        <dt>
          <b class="sky">못나는새</b>
          <span><span class="orange">f</span> ()</span>
        </dt>
      </dl>
      <dl>
        <dt class="down">
          <b class="sky">참새</b>
          <span>새</span>
        </dt>
        <dd>
          <b>등급</b>
          <span class="purple">1</span>
        </dd>
        <dd>
          <b>깃털</b>
          <span class="purple">true</span>
        </dd>
        <dd>
          <b>날개</b>
          <span class="purple">true</span>
        </dd>
        <dd>
          <b>날수있나</b>
          <span class="purple">true</span>
        </dd>
        <dd>
          <b>부리</b>
          <span class="purple">true</span>
        </dd>
        <dd>
          <dl>
            <dt class="down">
              <b>[[prototype]]</b>
              <span>Object</span>
            </dt>
          </dl>
        </dd>
      </dl>
      <dl>
        <dt class="down">
          <b class="sky">타조</b>
          <span>못나는새</span>
        </dt>
        <dd>
          <b>등급</b>
          <span class="purple">2</span>
        </dd>
        <dd>
          <b>날수있나</b>
          <span class="purple">false</span>
        </dd>
        <dd>
          <dl>
            <dt class="down">
              <b>[[prototype]]</b>
              <span>새</span>
            </dt>
            <dd>
              <b>등급</b>
              <span class="purple">1</span>
            </dd>
            <dd>
              <b>깃털</b>
              <span class="purple">true</span>
            </dd>
            <dd>
              <b>날개</b>
              <span class="purple">true</span>
            </dd>
            <dd>
              <b>날수있나</b>
              <span class="purple">true</span>
            </dd>
            <dd>
              <b>부리</b>
              <span class="purple">true</span>
            </dd>
            <dd>
              <dl>
                <dt>
                  <b>[[prototype]]</b>
                  <span>Object</span>
                </dt>
              </dl>
            </dd>
          </dl>
        </dd>
      </dl>
    </dd>
  </dl>
</blockquote>

객체들의 구조를 살펴보면 참새는 새로부터 속성을 위임받습니다. 원형에 가깝고 새의 표본이 될수 있는객체이므로 등급도 높습니다.  
타조는 못나는새로부터 속성을 위임받고 못나는새는 새로부터 속성을 위임받습니다. 원형과 거리가 멀고 등급도 낮으므로 다른 새들의 표본이 되지는 못합니다.

다음은 class의 상속을 이용해서 위와같은 구조를 만들어보겠습니다.

```javascript
class 새 {
  constructor() {
    this.등급 = 1;
    this.깃털 = true;
    this.날개 = true;
    this.날수있나 = true;
    this.부리 = true;
  }
}

class 못나는새 extends 새 {
  constructor() {
    super();
    this.등급 = 2;
    this.날수있나 = false;
  }
}

const 참새 = new 새();
const 타조 = new 못나는새();
```

문법적으로는 더 깔끔해보입니다. 한번 내부의 속성들도 분석을 해볼까요?

<blockquote class="dev-tool-scope">
  <dl>
    <dt class="down">Script</dt>
    <dd>
      <dl>
        <dt>
          <b class="sky">새</b>
          <span><span class="orange">f</span> ()</span>
        </dt>
      </dl>
      <dl>
        <dt>
          <b class="sky">못나는새</b>
          <span><span class="orange">f</span> ()</span>
        </dt>
      </dl>
      <dl>
        <dt class="down">
          <b class="sky">참새</b>
          <span>새</span>
        </dt>
        <dd>
          <b>등급</b>
          <span class="purple">1</span>
        </dd>
        <dd>
          <b>깃털</b>
          <span class="purple">true</span>
        </dd>
        <dd>
          <b>날개</b>
          <span class="purple">true</span>
        </dd>
        <dd>
          <b>날수있나</b>
          <span class="purple">true</span>
        </dd>
        <dd>
          <b>부리</b>
          <span class="purple">true</span>
        </dd>
        <dd>
          <dl>
            <dt class="down">
              <b>[[prototype]]</b>
              <span>Object</span>
            </dt>
          </dl>
        </dd>
      </dl>
      <dl>
        <dt class="down">
          <b class="sky">타조</b>
          <span>못나는새</span>
        </dt>
        <dd>
          <b>등급</b>
          <span class="purple">2</span>
        </dd>
        <dd>
          <b>깃털</b>
          <span class="purple">true</span>
        </dd>
        <dd>
          <b>날개</b>
          <span class="purple">true</span>
        </dd>
        <dd>
          <b>날수있나</b>
          <span class="purple">false</span>
        </dd>
        <dd>
          <b>부리</b>
          <span class="purple">true</span>
        </dd>
        <dd>
          <dl>
            <dt class="down">
              <b>[[prototype]]</b>
              <span>새</span>
            </dt>
          </dl>
        </dd>
      </dl>
    </dd>
  </dl>
</blockquote>

위의 구조를 살펴볼때 원형을 참조하던 prototype의 방식과는 다른 구조로 속성이 생겨납니다.  
class는 범주화된 그룹으로부터 속성들을 부여받고 이를 상속이라고 부릅니다.
현상계는 이데아계의 복사본이라는 플라톤의 이데아론의 관점과 비슷하죠?

우리는 이를 통해 다시한번 관점에 대해서 생각해봐야합니다. prototype의 객체를 바라보는 관점은
못나는 새라는 범주가 정의되고 이러한 범주에 의해 타조의 속성이 정의 되는것이 아니라
타조 그 자체로서의 특징이 스스로에게 있고 이를 기반으로 못나는 새라는 범주에 들어가는것 입니다.
쉽게말해, 객체는 객체 그자체로 특성이 정해진것이고 객체 스스로가 의미가 정해지는 주체라는 것 입니다.
이 부분이 prototype-base programming의 가장 핵심부분인것같습니다.

이후 설명할 this 및 context, lexical에서 이러한 관점은 아주 중요한 지표가 됩니다.

#### 의미사용이론

자바스크립트의 중요한 개념들인 `this, lexical scope, hoisting` 비트겐슈타인의 또다른 이론인 의미사용이론의 관점을 따릅니다.
아래의 예시를 한번 살펴보겠습니다.

- 상황A : 미장이가 벽돌을 쌓다가 조수에게 “벽돌!” 하고 외칩니다.
- 상황B : 만약 공사장 아래를 지나가는 사람에게 “벽돌!”하고 외칩니다.

위의 상황에서의 벽돌은 같은 단어를 사용했지만 벽돌이라는 단어가 사용된 `환경에 따라서 그 의미가 달라집니다.`
A라는 상황에서는 벽돌을 가져다 달라는 의미로 사용된것처럼 보이고 B라는 상황에서는 벽돌이 떨어지니까 피하라는 경고의 메세지로 사용되는것처럼 보입니다.  
하나의 예시를 더 살펴볼까요?

적도 근처에 파푸아뉴기니라는 섬에 살고있는 Dani라는 원주민들은 색을 구분하는 기준이 밝고 따듯한 색,
어둡고 차가운색 이렇게 두가지로밖에 분류하지 않는다고 합니다. 반면 문명화된 사회에서는 빨강, 파랑, 노랑등 다양한 색을 특성별로 구분해놓았습니다.
만약 Dani족 사람들에게 빨강, 노랑, 주황등의 색을 보여준다면 이들은 밝고 따듯한색으로만 인식하지만 우리는 빨강, 노랑 주황 세개의 각기 다른색으로
인식합니다. 같은 색이라도 `어떠한 대상이 바라보느냐에 따라 그 의미도 달라지게 된것 입니다.`

위의 예시에서 보듯이 의미사용이론은 `환경이나 대상에 따라 그 의미가 달라진다`라는 관점이 있습니다. 이부분이 바로 javascript 원형이론의 또하나의
핵심개념입니다.

#### 어휘적 범위(lexical scope)

의미사용이론에 따르면 단어의 의미는 그 어휘적인, 근처 환경에서의 의미가 됩니다. 이는 Javascript에 다음처럼 적용됩니다.

<p class="speak">변수의 의미는 그 어휘적인(Lexical), 실행 문맥(Execution Context)에서의 의미가 된다</p>

그렇기 때문에 동일 범위(실행 문맥)의 모든 선언을 참고(호이스팅)해 의미를 정의합니다. 아래의 예시를 볼까요?

```javascript
// global을 어휘적 범위(lexical scope)로 가지는 선언부: print 함수 호이스팅
print();
function print() {
  // print를 어휘적 범위(lexical scope)로 가지는 선언부: var name = 'Koh', printInner 함수 호이스팅
  var name = "Jack";
  printInner();
  function printInner() {
    console.log(name);
  }
}
```

위의 코드를 실행시켜보면 print함수가 실행되는 순간이 print 함수가 선언되어있는부분보다 위에 작성이 되었지만 에러가 발생하지않고
잘 작동합니다. 이런 현상이 바로 호이스팅(hoisting)이라는 현상인데요. 자바스크립트의 선언부들이 자신들이 속한 lexical 환경에서의
의미를 부여받기 위해 lexical scope내에서 최상단에 위치하게 되는것 입니다.

사실 저는 원형이론, 의미사용이론 다좋다고 하더라도 호이스팅의 이러한 현상은 코드를 작성하는데 좋지 못한 현상이라고 생각합니다. ㅎㅎㅎㅎ 다행이 let과 ,const 등이 나오면서 호이스팅의 단점들이 조금은 보완된것같습니다.

#### 실행 컨텍스트(excution context)

비트겐슈타인은 그의 대표적인 저서 ‘철학적 탐구'에서 `단어의 쓰임새가 곧 의미`라는 점을 강조했습니다(의미사용이론).

그는 이를 ‘발화’ 라고 얘기했는데요, 위에서 예시 들었던 것 처럼 ‘벽돌!’이라고 크게 외칠 때, 그것이 어디서 ‘발화’되느냐에 따라서 단어의 의미가 달라집니다. 좀더 쉽게 얘기하면 받아들이는 ‘대상' 에 따라서 같은 단어도 의미가 달라진다는 얘기입니다.

이것이 바로 프로토타입과 클래스의 대표적인 차이라고 볼 수 있습니다. 전혀 다르게 단어를 보는 방식이고 중요한 세계관의 차이입니다. 미리 분류하고 정의한 클래스를 가장 중요하게 여기는 전통적인 방식과는 달리, 프로토타입에서는 받아들이는 주체와 문맥이 가장 중요한 것이죠. 프로그래밍으로 보자면 실행(invoke)하는 ‘객체' 가 중요하다는 의미입니다.

이것이 바로 프로토타입 기반 언어인 자바스크립트에서 this 가 클래스 기반 언어들과 다르게 동작하는 이유입니다.

프로토타입 기반 언어에서는 this 가 정의된 함수가 어떻게 발화(invoke) 되었는지에 따라 가리키는 값이 달라집니다. 정확히는 받아들이는 대상의 컨텍스트를 가리킵니다.

이를 이해하려면 먼저 메소드에 대해서 명확하게 알아야합니다.

- 메소드: 객체의 함수

```javascript
const obj = {
  method() {
    console.log("메소드 입니다");
  },
};
//실행형태: obj.method()
const func = function () {
  console.log("함수 입니다");
};
//실행형태: 함수()
```

위의 예시를 보면 method라는 함수는 obj의 속성으로 사용되고있고 func라는 함수는 일반함수로 선언만 되어있습니다. 우리는 이러한형태를 메소드와 일반 함수로 구분할수있지만 실제로는 obj객체와 더불어 func또한 Script(전역)오브젝트의 속성으로 등록되어 있습니다.

<blockquote class="dev-tool-scope">
  <dl>
    <dt class="down">Script</dt>
    <dd>
      <dl>
        <dt class="down">
          <b class="sky">obj</b>
          <span></span>
        </dt>
        <dd>
          <dl>
            <dt>
              <b class="sky">method</b>
              <span><span class="orange">f</span> method()</span>
            </dt>
          </dl>
        </dd>
        <dd>
          <dl>
            <dt>
              <b>[[prototype]]</b>
              <span>Object</span>
            </dt>
          </dl>
        </dd>
      </dl>
    </dd>
    <dd>
      <dl>
        <dt>
          <b class="sky">함수</b>
          <span><span class="orange">f</span> ()</span>
        </dt>
      </dl>
    </dd>
  </dl>
</blockquote>

그러므로 일반적으로 함수가 실행될때 Script 오브젝트가 실행주체가 되는것이고 this는 자연스럽게 전역객체를 지칭하게 됩니다.
이것이 일반적으로 this가 window를 가르키는 이유입니다. 하지만 무조건적으로 this가 window를 가르키는것은 아닙니다. 아래의 예시를 살펴볼까요?

```javascript
const 건설현장 = {
  인력: "20명",
  진행현황: "80%",
  알림: "벽돌! 피해!!",
  alert() {
    console.log(this.알림);
  },
};

const 미장쟁이 = {
  알림: "벽돌! 필요해!!",
  alert: 건설현장.alert,
};

건설현장.alert(); // 결과: 벽돌! 피해!!
미장쟁이.alert(); // 결과: 벽돌! 필요해!!
```

위의 코드의 예시처럼 메소드가 선언된 가장 가까운 어휘적 범위(lexical scope)를 this로 가르키게 되고 이러한 원리로 인해 this의 대상이 달라지게됩니다. 이를 `실행 컨텍스트`라고 합니다.

지금까지 자바스크립트의 원리와철학에 대해 알아보았습니다. 이론적인 부분과 관념에 대해서 이야기를 하다 보니 문법과 현상에 대한 더 자세한 설명을 같이 하기에는 조금 무리가 있는것같아 최대한 간단한 예시들만 사용하느라 설명에 부족한점이 많이 있는것같습니다.

이 포스트를 통해서는 자바스크립트가 객체를 어떤 관점에서 바라보고있는지만 이해하는것에 초점을 맞췄기때문에 prototype 사용법, hoisting, 실행컨텍스트, lexical cope등은 다른 포스트에서 더 자세히 다룰수 있도록 해보겠습니다.

#### 참조

[플라톤의 이데아론과 객체지향 패러다임 비교 연구](https://dcollection.yonsei.ac.kr/public_resource/pdf/000000128475_20211223120406.pdf).  
[프로토타입 의미론의 위상](http://daegu.dcollection.net/public_resource/pdf/000002419851_20211223121258.pdf).  
[Classes vs Prototypes](http://citeseerx.ist.psu.edu/viewdoc/download?doi=10.1.1.56.4713&rep=rep1&type=pdf).  
[Class-based programming](https://en.wikipedia.org/wiki/Class-based_programming).  
[아리스토텔레스](https://ko.wikipedia.org/wiki/%EC%95%84%EB%A6%AC%EC%8A%A4%ED%86%A0%ED%85%94%EB%A0%88%EC%8A%A4).
[자바스크립트는 왜 프로토타입을 선택하게 됬을까?](https://medium.com/@limsungmook/%EC%9E%90%EB%B0%94%EC%8A%A4%ED%81%AC%EB%A6%BD%ED%8A%B8%EB%8A%94-%EC%99%9C-%ED%94%84%EB%A1%9C%ED%86%A0%ED%83%80%EC%9E%85%EC%9D%84-%EC%84%A0%ED%83%9D%ED%96%88%EC%9D%84%EA%B9%8C-997f985adb42)
