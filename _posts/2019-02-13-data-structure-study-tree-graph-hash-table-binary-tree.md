---
layout: post
title: "자료 구조 공부 / Tree, Graph, Hash Table, Binary Tree"
comments: true
author: feynubrick
date: 2019-02-13
tags: [JavaScript, Study, DataStructure, CodeStates]
---

# Tree

## 그림으로 표현

<figure>
  <img src="/assets/figures/tree-structure.png" alt="tree-structure"/>
</figure>

## Pseudocode

- 각 노드가 parent와 child 정보를 갖고 있게 한다.
- 이 정보는 모두 다른 노드를 가리키는 정보다.
- 하지만, 같은 parent를 공유하는 노드(siblings)끼리는 연결될 수 없다.
- 모든 노드는 단 하나의 부모만을 갖는다.

## 용어

- node
- edge
- root
- leaf
- parent
- child
- height
- depth

# Graph

노드와 엣지로 이루어진 비선형 데이터 구조.
유한한 갯수의 노드들과 이들 한쌍을 연결하는 엣지들로 구성되어있다.

<figure>
  <img src="/assets/figures/graph.png" alt="graph"/>
</figure>

## Pseudocode

- 모든 노드(vertices)는 자신과 연결된 노드에 대한 정보를 가질 수 있다.
- 그 정보의 크기는 자기자신을 제외한 나머지 노드의 수인 `N - 1`이다.

# Hash Table

<figure>
  <img src="/assets/figures/hash-table.png" alt="hash-table"/>
</figure>

## Pseudocode

- 주어진 정보를 요약해 자료구조상의 자료 하나를 가리키는 정보로 바꾸는 Hash function을 만든다.
- Hash function에 정보를 넣고, 결과로 나오는 주소를 찾아갈 수 있게 한다.

# Binary Tree

각 노드가 많아봤자 2개의 자식 노드를 갖는 구조다.