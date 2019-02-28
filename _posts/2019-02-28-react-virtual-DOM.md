---
layout: post
title: "Virtual DOM과 Reconciliation"
comments: true
author: feynubrick
date: 2019-02-28
tags: [JavaScript, Study, React, CodeStates]
---

# 가상(Virtual) DOM?

가상 DOM에 대해 찾아보면, 공식 문서에 다음과 같이 나와있다.

> The virtual DOM (VDOM) is a programming concept where an ideal, or “virtual”, representation of a UI is kept in memory and synced with the “real” DOM by a library such as ReactDOM.

요약하면, UI를 표현하는 것을 메모리에 두고 진짜 DOM과 동기화한 것을 말한다.
그러니까 가상 DOM은 DOM, 즉 Document Object Model과 같지는 않으나 같은 것을 표현하는 것을 메모리에 올려놓고, 뭔가가 변하면 DOM에 그것을 반영하는 것이다.

# Reconciliation

가상 DOM을 ReactDOM과 같은 라이브러리를 통해 진짜 DOM에 반영하는 과정을 "reconciliation"(화해, 중재, 조정이라는 뜻)이라고 한다.
이렇게 반영해서 DOM을 만들어내는 과정을 "그려낸다(render)"라고 하면,
가장 단순하게 생각하면, 모든 것을 다시 새로 그리면 된다.
하지만 모든 것을 새로 그리는 것은 자원의 낭비다.
만약 수천개의 엘리먼트를 새로 그려야된다면, 서버도 고통받고, 사용자도 고통받을 것이다.

리엑트에서는 다음의 두가지 가정을 바탕으로 이런 문제를 피해, 변한 내용만 다시 그리는 방법을 구현했다.

1. 서로다른 타입의 두개의 엘리먼트는 다른 트리를 만들어낸다.
2. 개발자가 `key` 속성을 사용해 어떤 자식 엘리먼트가 변하지 않는지 표시할 수 있다.

# Diffing Algorithm

"Diffing"은 현재 것과 옛것 두개를 비교해 무엇이 다른지 알아내는 것을 말한다.

<figure>
  <img src="/assets/figures/react-virtual-dom-reconciliation.png" alt="diffing"/>
  <figcaption>Diffing을 표현한 그림. 빨간색으로 표시된 부분이 업데이트된 부분이다. 출처: https://www.oreilly.com/library/view/learning-react-native/9781491929049/ch02.html</figcaption>
</figure>

리엑트는 변화가 생긴 DOM 노드를 버전관리처럼 이전과 비교해 어떤 것이 달라졌는지를 확인한다.
그리고 그것을 업데이트하고, 그 노드의 자식 노드들도 업데이트한다.
그리고 다시 render한다.

이 과정은 변화가 어떻게 일어났느냐에 따라 다르게 진행된다.

## 서로 다른 타입의 엘리먼트

만약에 원래 있던 엘리먼트가 `<div>` 엘리먼트였고, 새로 바뀌는 엘리먼트가 `<span>`이라면,
예전 엘리먼트의 트리(tree)를 부수고 새 트리를 처음부터 만든다.
부수려면 먼저 현재 가상 DOM에서 언마운트(unmount)해야하므로, 하위에 있는
컴포넌트 인스턴스는 `componentWillUnmount()`에 적힌 내용을 실행하게 된다. 

새 트리를 만들 때, 새 가상 DOM 노드가 가상 DOM에 마운트(mount)된다.
만들어지는 하위 컴포넌트 인스턴스들은 componentWillMount()에 있는 것을 실행하고 나서 `componentDidMount()`에 있는 것을 실행한다.
예전 트리와 관련되어 있던 state들은 모두 없어진다.

## 같은 유형의 가상 DOM 엘리먼트

같은 유형의 리엑트 DOM 엘리먼트 두개를 비교할 때, 리엑트는 두개 모두의 속성을 보고, 가상 DOM 노드는 그대로 두고 속성만 업데이트한다.
업데이트가 끝나면, 그 자식 노드들을 찾아가 같은 작업을 해준다.

## 같은 유형의 컴포넌트 엘리먼트

컴포넌트가 업데이트되면, 인스턴스는 그대로 유지된다. 그래서 state는 새로 그려도 유지된다.
리엑트는 하위 컴포넌트 인스턴스의 props를 새 엘리먼트와 맞게 업데이트하고,
`componentWillReceiveProps()`와 `componentWillUpdate()`를 호출한다.
그 다음으로 `render()` 메서드를 호출한다.

## 자식 노드로 리커전

가상 DOM 노드의 자식 노드로 리커전할 때, 리엑트는 자식 리스트에 대해 반복하고 차이가 있을 때마다 변화를 만든다.

```react
//이전
<ul>
  <li>first</li>
  <li>second</li>
</ul>

// 이후
<ul>
  <li>first</li>
  <li>second</li>
  <li>third</li>
</ul>
```

그러니까 여기서는 변화가 있는 유일한 부분인 `<li>third</li>`를 추가만 한다.
그런데 다음과 같은 경우

```react
// 이전
<ul>
  <li>second</li>
  <li>third</li>
</ul>

//이후
<ul>
  <li>first</li>
  <li>second</li>
  <li>third</li>
</ul>
```

실제로 변화가 일어난 것은 `<first>` 하나지만, 순서가 바뀌면서
세개를 다 업데이트하게 된다.

## 키(Keys)

이런 문제를 해결하기 위해 리엑트는 키(key) 속성을 지원한다.
위와 같은 문제가 생기면 단순히 키를 사용해 변화된 엘리먼트만 골라서 업데이트 할 수 있다.

키 값은 형제 엘리먼트(siblings)들 사이에서만 유일하면 되고, 전역적으로 유일할 필요는 없다. 키에 index를 사용하면 좋지 않은데, 자리를 바꾸면 이 인덱스와 키가 둘 다 바뀌기 때문에, 항상 업데이트 된다.
키의 덕을 많이 보려면, 안정적이고 유일한 것(예를들어 id)을 키로 쓰는 것이 좋다.

# 참고

- [리엑트의 공식문서 FAQ: Virtual DOM and Internals](https://reactjs.org/docs/faq-internals.html#what-is-the-virtual-dom)
- [React Virtual DOM Explained in Simple English](https://programmingwithmosh.com/react/react-virtual-dom-explained/)
- [How React Reconciliation works](https://css-tricks.com/how-react-reconciliation-works/)

