---
layout: post
title: "자바스크립트 공부 // 8. DOM(Document Object Model)"
comments: true
author: feynubrick
date: 2019-01-18
tags: [JavaScript, Study]
---

# DOM(Document Object Model)

웹 브라우저의 자바스크립트 런타임 환경(JavaScript Runtime Environment)이 제공하는 API(Application Programing Interface)다.
풀어서 얘기하면, DOM(Document Object Model)이란 웹 문서에 접근해 프로그래밍해서 웹문서를 바꿀 수 있도록 웹 브라우저가 제공하는 도구다.

DOM은 트리구조를 갖고 있는데, 이 트리구조의 가지들은 노드에 연결되어있다.
각 노드는 HTML 문서의 각 엘리먼트(element)들을 나타내는 객체들로 이뤄져 있다.
객체에 있는 여러 메서드와 속성을 이용하면, 자바스크립트를 통해 이 트리에 접근해 HTML 엘리먼트에 변화를 줄 수 있다.

# DOM 인터페이스

## 엘리먼트 가져오기

- `getElementsByTagName`
- `getElementById`
- `getElementsByClassName`
- `querySelector`
- `querySelectorAll`

## 이벤트 핸들러 추가하기

- on*EventName*: 예를 들어, `onClick`
- 예시:

index.html

```html
...
<body>
    <input id="queryString" type="text" />
    <button id="search">Search</button>

    <script type="text/javascript" src="script.js"></script>
</body>
```

script.js

```javascript
var elInput = document.getElementById('queryString');
var elSearch = document.getElementById('search');

elSearch.onClick = function() {
    alert(elInput.value);
}
```

## 엘리먼트 생성하기

```javascript
function createButton() {
    var btn = document.createElement("BUTTON");
    var t = document.createTextNode("Click Me!");
    btn.appendChild(t);
    document.body.appendChild(btn);
}
```

[my-post-async]: {{ site.baseurl }}{% post_url 2019-01-16-javascript-study-7-asynchronous-call %}
[javascript-runtime-env]: http://dolszewski.com/javascript/javascript-runtime-environment