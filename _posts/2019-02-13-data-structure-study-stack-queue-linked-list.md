---
layout: post
title: "자료 구조 공부 / Stack, Queue, Linked List"
comments: true
author: feynubrick
date: 2019-02-13
tags: [JavaScript, Study, DataStructure, CodeStates]
---

# Stack

<figure>
  <img src="/assets/stack.jpg" alt="stack"/>
  <figcaption>Stack 이 무엇인지 어떤 일을 어떻게 할 수 있는지 보여주는 그림.</figcaption>
</figure>

## 요약
LIFO(Last In First Out). 쌓아놓고 위부터 뽑아감

## 구현을 위해 필요한 기능
- push: stack의 가장 위에 쌓기
- pop: stack의 가장 위의 항목을 빼내기
- peek or top: stack의 가장 윗 항목이 무엇인지 알아내기
- isEmpty: stack이 비었는지 확인하기

## Stack으로 할 수 있는 일

### balancing of Symbol
`[`, `]`, `{`, `}`, `(`, `)`, `"`, 같이 양쪽이 항상 닫혀 있어야하는 기호가 제대로 쓰여져 있는지 확인할 때, stack을 사용한다.

### undo-redo
대부분의 편집기에서 `Ctrl` + `Z` 키를 사용하면, 
편집 내용을 뒤로 돌리고, 그 뒤로 돌린 내용을 

### forward-backward in browser

# Queue

<figure>
  <img src="/assets/queue.png" alt="queue"/>
  <figcaption>Queue가 무엇인지 어떤 일을 어떻게 할 수 있는지 보여주는 그림.</figcaption>
</figure>

## 요약

FIFO(First in First Out). 대기줄을 생각하면 이해할 수 있다. 나중에 온 사람을 먼저 들여보내면 사람들이 화를 많이 낼 것 아닌가.

들어오는 곳과 나가는 곳이 따로 있음

## 구현을 위해 필요한 기능

- enqueue
- dequeue
- front
- rear

## 할 수 있는 일

즉시 처리되지 않아도 되지만, 처음 들어온 것이 처음 나가게 하는 순서로 처리되어야 하는 일들에 사용한다.

복수의 소비자가 같은 자원을 사용할 때

비동기로 데이터를 전송할 때

# Linked List

<figure>
  <img src="/assets/linked-list.png" alt="linked-list"/>
  <figcaption>Linked List가 무엇인지 어떤 일을 어떻게 할 수 있는지 보여주는 그림.</figcaption>
</figure>

## 요약
데이터에 꼬리표 같이 다음 항목을 가리키는 내용을 덧붙인 형태.

## 왜 필요한가

배열의 단점을 극복하기 위해서다.

배열의 크기는 고정되어 있어서, 사용하려면 처음 얼마나 많이 넣을 것인지 알고 정해놔야한다.
이런 이유로 꽉찬 배열에 추가로 새로운 항목을 넣으려면 많은 일을 해야한다. 그러니까 새로운 항목 하나를 넣기 위해 전체 배열을 다시 할당해야하는 것이다.

예를 들어, 정렬된 `[1,2,4,5,6,7,8]` 배열의 중간에 새로 항목 `3`을 추가하려면 `1,2` 이후의 모든 항목을 하나씩 뒤로 밀어야한다. 하지만 linked list를 사용하면, `2`에 붙어있는 꼬리표 내용을 `4`에서 `3`으로만 바꿔주면 해결된다.

삭제도 간단한데, 배열에서는 삭제한 이후 모든 그 뒷 항목들을 전부 한칸씩 당겨야하는데, linked list를 사용하면 역시 삭제한 항목의 바로 전 항목의 꼬리표 내용을 바꿔주면 된다.

물론 여기서 얘기한 배열의 단점은 자바스크립트의 배열에서는 나타나지 않는다. 그 이유는 자바스크립트 배열은 linked list를 사용해 구현되고 있기 때문이다.

## 문제점

무작위 접근이 허용되지 않는다. 이게 무슨 소리냐면, 항상 처음 항목부터 순서대로 따라서 접근해야한다.

## 구현을 위해 필요한 기능

- 항목 삽입하기
- 항목 삭제하기