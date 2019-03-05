---
layout: post
title: "RESTful API"
comments: true
author: feynubrick
date: 2019-03-05
tags: [Study, CodeStates]
---

# REST?

**RE**presentational **S**tate **T**ransfer로 Roy Fielding 이라는 사람의 [2000년도 박사학위 논문](https://www.ics.uci.edu/~fielding/pubs/dissertation/rest_arch_style.htm)에서 처음 등장했다.
이 말을 대충 번역하면 "표현상태전달" 정도가 되는데, 무엇을 표현하는 상태를 전달한다는 것일까?
바로 자원(Resource)이다.
이름 지을 수 있는 어떤 정보도 자원이 될 수 있다.
문서, 이미지, 다른 자원들의 집합, 사람 같은 것들 모두 이런 의미에서 자원이라 부를 수 있다.

## 자원을 표현하기

이 자원은 시간에 따라 변할 수 있는데, 어떤 특정 시점의 자원의 상태를 바로 자원 표현(resource representation)이라고 한다.
이 표현은 다음과 같은 것들로 구성되어있다.

- 데이터
- 데이터를 묘사하는 메타데이터(metadata)
- 다른 상태를 향한 하이퍼미디어(hypermedia) 링크

어떤 표현의 데이터 형식(data format)은 미디어 유형(media type)으로 알려져있다.
이 미디어 유형은 이 표현을 처리하기 위해 어떤 "사용 설명서"를 사용해야하는지 알려준다.
이런 각 유형별 "사용 설명서"는 [이 링크](https://www.iana.org/assignments/media-types/media-types.xhtml)에서 확인할 수 있다.

진짜 RESTful한 API는 하이퍼텍스트(hypertext)처럼 보인다.
Roy Fielding은 하이퍼텍스트를 "정보와 컨트롤을 동시에 주는 것"이라고 했다.
이게 무슨 말인지는 HTML 페이지를 떠올리면 이해가 쉽다.
HTML 페이지는 정보를 주지만, 동시에 다른 정보를 얻을 수 있는 링크를 제공하고 있다.
그러니까 정보와 컨트롤을 유저에게 동시에 제공하고 있는 것이다.
그렇지만 브라우저에서 하이퍼텍스트가 HTML 또는 XML, JSON일 필요는 없다.
데이터 포맷(data format)과 관계 유형이 주어져 있기만 하면 된다.


## 자원 메서드(Resource Methods)


# Guiding Principles of REST

[restfulapi.net](https://restfulapi.net/)에서는 다음 6가지 원칙을 이야기하고 있다. 

## 1. Client–server

> By separating the user interface concerns from the data storage concerns, we improve the portability of the user interface across multiple platforms and improve scalability by simplifying the server components.

## 2. Stateless

> Each request from client to server must contain all of the information necessary to understand the request, and cannot take advantage of any stored context on the server. Session state is therefore kept entirely on the client.

## 3. Cacheable

> Cache constraints require that the data within a response to a request be implicitly or explicitly labeled as cacheable or non-cacheable. If a response is cacheable, then a client cache is given the right to reuse that response data for later, equivalent requests.

## 4. Uniform interface

> By applying the software engineering principle of generality to the component interface, the overall system architecture is simplified and the visibility of interactions is improved. In order to obtain a uniform interface, multiple architectural constraints are needed to guide the behavior of components. REST is defined by four interface constraints: identification of resources; manipulation of resources through representations; self-descriptive messages; and, hypermedia as the engine of application state.

## 5. Layered system

> The layered system style allows an architecture to be composed of hierarchical layers by constraining component behavior such that each component cannot “see” beyond the immediate layer with which they are interacting.

## 6. Code on demand (optional)

> REST allows client functionality to be extended by downloading and executing code in the form of applets or scripts. This simplifies clients by reducing the number of features required to be pre-implemented.


# 참고

- [Wikipedia - Representational state transfer](https://en.wikipedia.org/wiki/Representational_state_transfer)
- [restfulapi.net](https://restfulapi.net/)
- [REST란? REST API란? RESTful이란?](https://gmlwjd9405.github.io/2018/09/21/rest-and-restful.html)