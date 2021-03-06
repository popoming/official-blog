---
title: 'CSS Selector Part.02'
date: 2017-11-16 12:00:00
categories:
- css
tags:
- css
- selector
thumbnail: https://www.w3.org/TR/css3-selectors/
author : 이영범
---



[지난 시간](http://tech.javacafe.io/css/2017/11/09/css_selector_part01/)에 CSS selector를 사용할 수 있는 라이브러리/메서드에 대해서 알아봤습니다<br>
이번 시간부터는 실전편으로 실제로 사용되는 CSS Selector들에 대해서 알아보겠습니다

# Quiz 
그냥 하면 재미가 없으니까 간단한 퀴즈로 시작해 보겠습니다<br>
<a href="/html/posts/2017-11-16-css_selector_part02_001.html" target="_blank">Selector Quiz</a>를 클릭해서 현재 수준을 진단해 보세요

## Selector 메서드 정의
설명의 편의를 위해 먼저 지난 시간에 만든 Selector에 사용할 메서드부터 정의하겠습니다<br>
(앞으로 나오는 selector를 실시간으로 실습해 보기 원하시면 <a href="/html/posts/2017-11-16-css_selector_part02_001.html" target="_blank">Selector Quiz</a>의 "Selector 확인"을 활용하세요)
```javascript
const $$ = document.querySelectorAll.bind(document);     // select 하고자 하는 대상이 복수
const $ = document.querySelector.bind(document);     // select 하고자 하는 대상이 단수
```

# 문제풀이와 설명

## 기본 Selector
제일 먼저 매우 간단하고 가장 많이 사용되는 id와 class selector를 살펴보겠습니다
```javascript
$('#idName')    // id로 조회
$$('.class-name')    // class로 조회
```

여기에 더해서 element의 이름을 명시해서 검색범위를 좁힐수도 있습니다
```javascript
$$('div.class-name')    // div element이면서 class이름이 class-name인 element 조회
```

## Attribute selector
조금 더 심화시켜서 attribute를 조회하는 selector를 보겠습니다<br>
아래와 같이 작성하면 element 내부에 attribute가 선언되어 있는 (attribute의 값의 유무, 일치여부와 상관없이) 모든 element의 조회가 가능합니다
```javascript
$$('[id]')    // 내부에 id attribute가 선언된 모든 element 조회
$$('[class]')    // 내부에 class attribute가 선언된 모든 element 조회
$$('[data-x]')    // 내부에 data-x attribute가 선언된 모든 element 조회
```

역시나 동일하게 element의 이름을 명시해서 검색범위를 좁할수도 있습니다
```javascript
$$('div[id]')    // div element 중 내부에 id attribute가 선언된 모든 element 조회
$$('li[class]')    // ul element 중 내부에 class attribute가 선언된 모든 element 조회
$$('a[data-x]')    // a element 중 내부에 data-x attribute가 선언된 모든 element 조회
```

attribute의 값이 일치하는 대상도 찾을수 있습니다<br>
다만, 이때 attribute의 값이 하나만 지정된 경우(예: data-x="x")와 복수(예: data-x="x y")로 지정된 경우에 따라 사용할 수 있는 selector가 달라집니다<br>
먼저 attribute의 값이 하나만 지정된 경우에 사용할 수 있는 = 연산자입니다<br>
해당 연산자를 제외한 이하의 연산자는 단/복수인 경우 모두 사용 가능합니다
```javascript
$$('data-x=x-suf')    // attribute로 "data-x"를 가지고, 그 값이 하나이면서 "x-suf"인 elements
```

## Pattern matching
한 번 더 심화시켜서 attribute의 값의 일치여부에 정규식과 유사한 로직을 넣을수도 있습니다<br>
^, $는 정규식에서 각각 시작값/끝값의 일치여부를 조사합니다. 이와 동일한 로직을 selector에서도 사용할 수 있습니다<br>
```javascript
$$('data-x^=pre')    // attribute로 "data-x"를 가지고, 그 값의 시작값이 "pre"인 elements
$$('data-x$=suf')    // attribute로 "data-x"를 가지고, 그 값의 끝값이 "suf"인 elements
```

*(와일드카드) 또한 사용 가능합니다. 값중의 일부라도 일치하는 모든 대상을 조회할 수 있습니다
```javascript
$$('data-x*=re')    // attribute로 "data-x"를 가지고, 그 값중에 "re"라는 글자가 포함된 elements
```

## etc.
~을 사용하면 해당하는 값이 복수로 선언되어 있을 때(예: data-x="a b c"), 그중에 일치하는 대상이 있는 경우를 조회할 수 있습니다<br>
"~=" 연산자와 "=" 연산자의 차이점을 주목하시기 바랍니다. "="연산자는 atrribute의 값이 하나인 경우에만 사용할 수 있지만 "~=" 연산자는 값이 여럿인 경우에도 사용이 가능합니다
```javascript
$$('data-x~=xx')    // attribute로 "data-x"를 가지고, 그 값이 여럿이고 값 중 하나가 "xx"인 elements
```

추가적으로 실용성은 떨어지지만 값을 하이픈으로 분류했을 때 (예: data-x="pre-x xx" -> pre만 일치, xx는 하이픈이 없어서 제외) 앞의 문자와 정확히 일치하는지 여부를 확인하는 "|=" 연산자도 있습니다<br>
해당 연산자는 과거에 lang="en-us"같이 attribute에 언어 정보(en)를 조회할 때 사용되었지만 현재는 복잡성 때문에 실용성이 많이 떨어집니다 
```javascript
$$('[lang|=en]')    // data-x attribute를 가지고 있고, 그 각각의 값을 하이픈으로 분리했을 때 분류된 첫 단어가 'en'과 정확히 일치하는 모든 element 조회
```
### The next step
이상으로 기본적인 Selector에 대해서 알아보았습니다<br>
다음 시간에는 내용을 조금 더 심화시켜서 selector를 조합하는 방법과 형제, 자식, 자손 등을 조회하는 방법에 대해서 알아보겠습니다