[TOC]

# 자주쓰이는 HTML 태그

| 태그            | 설명                            |
| --------------- | ------------------------------- |
| `<div>`         | Division: 한줄을 차지           |
| `<span>`        | span: 컨텐츠 크기만큼 공간 차지 |
| `<img>`         | 이미지                          |
| `<a>`           | 링크                            |
| `<ul>` , `<li>` | unordered list, list item       |
| `<input>`       | input (Text, Radio, Checkbox)   |
| `<textarea>`    | Multi-line Text Input           |
| `<button>`      | 버튼                            |

## div

한 줄을 차지한다.

```html
<div>div tag 1</div>
<div>div tag 2</div>
```

div tag 1
div tag 2

## span

컨텐츠 크기 만큼만 공간을 차지한다.

```html
<span>span tag 1</span>
<span>span tag 2</span>
```

span tag 1 span tag 2

## img

```html
<img src="https://i.imgur.com/JVAj4tO.jpg">
```
<img src="https://i.imgur.com/JVAj4tO.jpg">

## a

링크 삽입

```html
<a href="https://codestates.com" target="_blank">코드스테이츠</a>
```
<a href="https://codestates.com" target="_blank">코드스테이츠</a>



## ul, li

```html
<ul>
    <li>item 1</li>
    <li>item 2</li>
    <li>item 3 has nested list
    	<ul>
            <li>item 3-1</li>
        </ul>
    </li>
</ul>
```
<ul>
    <li>item 1</li>
    <li>item 2</li>
    <li>item 3 has nested list
    	<ul>
            <li>item 3-1</li>
        </ul>
    </li>
</ul>

## input, textarea
```html
<input type="text" placeholder="type here">
<div>
    <input type="radio" name="choice" value="a"> a
    <input type="radio" name="choice" value="b"> b
</div>
<textarea></textarea>
<div>
    <input type="checkbox" checked> checked
    <input type="checkbox"> unchecked
</div>
```
<input type="text" placeholder="type here">
<div>
    <input type="radio" name="choice" value="a"> a
    <input type="radio" name="choice" value="b"> b
</div>
<textarea></textarea>
<div>
    <input type="checkbox" checked> checked
    <input type="checkbox"> unchecked
</div>

## button

```html
<button>Submit</button>
```
<button>Submit</button>

# HTML을 자바스크립트에 집어 넣는 방법

## HTML 내부에 작성

`<script>` 태그를 사용한다.

example:

```html
<!DOCTYPE html>
<html>
    <head>
    </head>
    <body>
        <script type="text/javascript">
        	console.log('write your JS here.');
        </script>
    </body>
</html>
```

## HTML 외부에 작성

`<script>` 태그의 `src`  속성을 사용한다.

index.html:

```html
<script src="main.js"></script>
```

main.js:

```javascript
console.log('write your JS here.');
```

# [HTML 태그 요소](https://developer.mozilla.org/en-US/docs/Web/HTML/Element)
## [Main root](https://developer.mozilla.org/en-US/docs/Web/HTML/Element#Main_root)

| 요소     | 설명                                                         |
| -------- | ------------------------------------------------------------ |
| `<html>` | HTML 문서의 root(최상위 요소)를 나타내고, root 요소라고 부르기도 한다. 다른 모든 요소들은 이 요소의 후손(descendants)다. |

## [문서 메타데이터](https://developer.mozilla.org/en-US/docs/Web/HTML/Element#Document_metadata)

메타데이터(metadata)는 페이지에 대한 정보를 담고 있다. 이것은 스타일, 스크립트, 그리고 소프트웨어(검색 엔진, 브라우저 등)가 사용하고 페이지를 렌더링(render)하도록 도와주는 데이터에 대한 정보를 포함한다. 스타일과 스크립트에 대한 메타데이타는 페이지 안이나 해당 정보를 갖고 있는 연결된 다른 파일에 정의되질 수 있다.

| 요소      | 설명                                                         |
| --------- | ------------------------------------------------------------ |
| `<head>`  | 이  문서의 제목, 사용된 스크립트와 스타일 시트로의 연결을 포함하는 일반적인 정보(metadata)를 제공한다. |
| `<link>`  | HTML 외부 리소스 연결 요소. 현재 문서와 외부 리소스의 관계를 설명한다. 이 요소는 스타일시트를 연결하는데 가장 많이 사용된다. 하지만 이외에 것들 중에서 사이트 아이콘(favicon 스타일의 아이콘과 모바일 홈 화면/앱 아이콘 둘다)을 만드는데도 사용된다. |
| `<meta>`  | `<base>`, `<link>`, `<script>`, `<style>`, `<title>` 같은 다른 HTML 메타 관련 요소로 표현되지 않는 메타데이터를 나타내는데 사용된다. |
| `<style>` | 문서 하나 또는 문서의 일부의 스타일 정보를 포함한다.         |
| `<title>` | 브라우저의 제목 막대 또는 페이지의 탭에 보이는 문서의 제목을 정의한다. |

## [Sectioning root](https://developer.mozilla.org/en-US/docs/Web/HTML/Element#Sectioning_root)

| 요소     | 설명                                                         |
| -------- | ------------------------------------------------------------ |
| `<body>` | HTML 문서의 내용을 나타낸다. 문서에는 오직 하나의 `<body>`요소만 쓸 수 있다. |

## [내용 구분](https://developer.mozilla.org/en-US/docs/Web/HTML/Element#Content_sectioning)

내용 구분 요소(content sectioning elements)는 당신이 문서 내용을 논리적 조각들로 조직할 수 있게 해준다. 이 구분 요소를 사용해서 당신의 페이지 내용의 얼개를 만들어라. header와 footer navigation과 내용의 섹션을 identify하는 heading 요소를 포함해서 말이다.

| 요소                                       | 설명                                                         |
| ------------------------------------------ | ------------------------------------------------------------ |
| `<address>`                                | 이 태그가 둘러싸는 HTML이 사람 또는 단체의 연락처 정보라는 것을 나타낸다. |
| `<article>`                                | 문서, 페이지, 응용프로그램, 또는 사이트 에서 자기 충족적인 구성을 나타낸다. 자기 충족적인 것은 다른 것에 의존하지 않고 배포할 수 있고 재사용 가능한 것을 말한다. 예: 포럼의 포스트, 잡지나 신문 기사, 블로그 기사 |
| `<aside>`                                  | 문서의 주 내용에 간접적으로만 관련된 문서의 한 부분을 나타낸다. |
| `<footer>`                                 | 가장 가까운 sectioning content나 sectioning root 요소에 대한 각주를 나타낸다. 각주는 통상적으로 해당 섹션의 저자, 저작권 데이터 또는 관련된 문서들로의 링크에 대한 정보를 포함한다. |
| `<header>`                                 | 도입부 내용이나 통상적으로 한 묶음의 도입부 또는 전체 내용을 조망하는 것을 도와주는 것(navigational aids)을 나타낸다. 어떤 제목 요소들을 포함 할 수 있지만 로고나 검색 양식, 저자이름 같은 다른 요소들도 포함 할 수 있다. |
| `<h1>`, `<h2>`,`<h3>`,`<h4>`,`<h5>`,`<h6>` | 여섯개 수준의 제목을 나타낸다. `<h1>`이 제일 높은 수준, `<h6>`은 제일 낮은 수준의 제목이다. |
| `<hgroup>`                                 | 문서의 각 절에 대한 다수준 제목을 나타낸다. 이것은 `<h1>`부터 `<h6>`까지의 요소들을 한 묶음의 그룹으로 만든다. |
| `<main>`                                   | 문서의 `<body>`에서 가장 지배적인 내용을 나타낸다. 주 내용 구역은 문서의 중심 주제와 직접적으로 연관되거나 이를 확장하는 것 또는 응용프로그램의 중심 기능을 포함한다. |
| `<nav>`                                    | 현재 문서나 다른 문서의 여러 곳을 이동하는 데 필요한 링크를 제공하는 것을 목적으로 만들어진 페이지의 한 부분을 나타낸다. 이 네비게이션 섹션의 흔히 볼 수 있는 예는 메뉴, 목차, 색인이다. |
| `<section>`                                | 한 HTML 문서 내에 포함된 독립된 섹션(이를 나타낼 더 구체적인 의미 요소를 갖고있지 않는)을 나타낸다. |

## [Text content]()

| 요소           | 설명                                                         |
| -------------- | ------------------------------------------------------------ |
| `<blockquote>` | 이 태그 사이에 들어간 텍스트가 확장된 인용(quotation)이라는 것을 나타낸다. 보통, 들여쓰기로 화면에 표현된다. 인용의 출처에 대한 URL은 `cite` 속성을 사용해 표시할 수 있다.  한편, 출처는 `<cite>` 요소를 사용해 텍스트로 표현할 수 있다. |
| `<div>`        | flow 내용을 담는 범용 컨테이너를 나타내는 요소. CSS를 사용해 꾸며지기 전에는 내용물이나 레이아웃에 아무 영향을 주지 않는다. |
| `<dl>`         | 설명서 목록(description list)를 나타내는 요소. 이 요소는 용어와(`<dt>` 요소를 사용해 표시되는, description term) 설명(`<dd>` 요소로 제공되는, description details or definition)의 그룹으로 이루어진 리스트를 품는다. |
| `<figcaption>` | 이것의 부모인 `<figure>` 요소의 보조 설명에 사용된다.        |
| `<figure>`     | 이것 하나 만으로 이 안의 내용이 설명될 수 있는 내용을 나타낸다. 자주 캡션(`<figcaption>`)과 함께 사용되며 보통 하나의 단위로 참조된다. |
| `<hr>`         | 이야기에서 장면 전환이나, 절에서의 주제 전환 같은 것에서 구문을 구분하는 장식적인 선을 말한다. |
| `<li>`         | ordered list(`<ol>`)나 unordered list(`<ul>`) 또는 `<menu>`의 list item |
| `<main>`       | `<body>`에서 가장 지배적인 내용을 나타낸다.                  |
| `<ol>`         | ordered list                                                 |
| `<p>`          | paragraph                                                    |
| `<pre>`        | 구문  해석이 되지 않고 HTML에 써있는 그대로 표시되는 텍스트인 preformatted text를 나타낸다. |
| `<ul>`         | unordered list                                               |



## 컨텐츠 관련

### div, span
#### div
#### span
### a
### ul, li
#### ul
#### li
### img
### iframe
### br
### table, thread, tbody, tr, th, td
#### table
#### thread
#### tbody
#### tr
#### th
#### td
### code, pre
#### code
#### pre

## 폼 관련

### form
### input
type: text, checkbox, color, date, password, ...
### button
### textarea
### select, option
#### select
#### option

## 권장하지 않는 태그

### b
### font
### center