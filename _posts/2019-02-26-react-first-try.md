---
layout: post
title: "리액트(React) 입문자의 첫시도"
comments: true
author: feynubrick
date: 2019-02-24
tags: [JavaScript, Study, React, CodeStates]
---

# 무엇을 만들려고 하는가?

원래는 리액트의 기본부터 차근차근 공부해서 개념을 하나하나 정리하는 방식으로 포스트를 작성하려 했었다.
하지만 조금 공부해보니 그렇게 하기엔 시간과 능력 모두가 부족한 것 같았다.
그래서 방향을 바꿔 리액트로 어떤 앱을 만들면서 그 과정을 기록하기로 했다.
그래서 두서없을 수는 있으나 이 포스트를 읽고 나면, 내가 만들었던 것과 같은 조잡한 앱을 만들 수는 있을 것이다.
그 과정에서 리액트의 전체적인 흐름과 내용을 알게됐으면 좋겠다.

만드려고 하는 앱은 내가 방문하고 싶은 곳을 리스트로 보여주고,
추가도 할 수 있는 간단한 앱이다.

# 리액트 앱 만들기

설치환경은 macOS 10.14.3 이고 사용한 node는 nvm으로 설치했으며, 버전은 11.8이다.

```
$ npx create-react-app todolist
$ cd todolist
$ yarn start
```

yarn이 설치가 되어있지 않아서, 다음과 같이 설치했다.

```
$ brew install yarn --ignore-dependencies
```

뒤에 붙은 `--ignore-dependencies` 옵션은 node를 글로벌 공간에 새로 설치하지 않으려고 붙인 것이다.
nvm으로 설치한 node는 로컬에만 설치되어있으므로 이 옵션이 없으면 node가 없는 것으로 생각해 글로벌에 node를 설치하게된다.

아무튼 `$ yarn start` 이후에 브라우저가 자동으로 열리면 작업을 시작할 준비가 된 것이다.

`src` 디렉토리를 확인해보면 여러 파일이 있는데, 나머지는 우리 앱을 만드는데 필요없으므로 `src` 디렉토리에서 `index.js`만 남겨두고, 나머지는 다 삭제한다.

`index.js` 파일도 열어서 다음 내용만 남겨두고 모두 삭제한다.

```react
import React from 'react';
import ReactDOM from 'react-dom';
```

# 리액트의 기본 개념

## JSX

더 진행하기 전에 자바스크립트에서 본적 없는 요상한 문법을 익혀야한다.
모양을 보면 알겠지만, HTML 태그와 상당히 유사하다.
다음 예를 보자.

```react
const elementJSX = <h1>Hello, world!</h1>;
const element = React.createElement('h1', null, 'Hello, world!');
```

첫번째 줄의 `=`의 오른쪽은 완전히 HTML과 같다.
이 이상한 혼종(?) 태그 문법은 문자열도 아니고 HTML도 아니다.
이 문법은 그 다음 줄과 완전히 동일하다.
그러니까 리엑트에서 HTML을 쉽게 다룰 수 있도록 만든 문법인 것이다.
결국 `createElement`를 하고 HTML 문서에 뿌리면 위와 같은 모양일 될테니까 말이다.

그러니 JSX가 리엑트의 엘리먼트(element)를 만든다고 "받아들이고" 넘어가도록 하자.
로마에 왔으면 로마법을 따라야 하니까.

## 엘리먼트(Element)

리엑트 앱의 기본 구성 단위는 이렇게 만들어진 리엑트 엘리먼트다.
위에서 봤듯이 리엑트 엘리먼트는 JSX를 갖고 만든다.

이 엘리먼트를 HTML 파일에 그리기 위해서는 다음과 같이 해줘야한다.

```react
import React from 'react';
import ReactDOM from 'react-dom';

const element = <h1>Hello, world</h1>;
ReactDOM.render(element, document.getElementById('root'));
```

물론 `document.getElementById('root')`에서 보듯이 HTML 파일에 `root`라는 아이디를 갖는 HTML 엘리먼트가 있어야한다.
이 앱으로 만든 모든 변화는 `root`라는 아이디를 갖는 요소의 하위에 그려지게 된다.

이 리엑트 엘리먼트는 한번 만들어지고 나면, 그 내용을 바꿀 수 없다.
리엑트 엘리먼트는 어떤 시점에서의 UI를 나타내는 것이라고 받아들이면 이해가 쉽다.
즉, 보여지는 시점에서는 이 내용을 바꿀 방법이 없다는 것이다.
그러나 상황이 달라져 다시 엘리먼트를 만들어 그리게 될 때 내용이 달라질 수는 있다.

## 컴포넌트(Component)와 props

엘리먼트 여러개를 묶어 하나로 만들 수 있는데, 이게 바로 컴포넌트다.
이 컴포넌트는 마치 HTML 태그를 입맛에 맞게 바꾸어 만든 것과 같다.

```react
function Welcome(props) {
    return <h1>Hello, {props.name}</h1>
} 
```

이렇게 만들면 마치 `<Welcome>`이라는 태그가 있었던 것처럼 사용할 수 있다.
다음과 같이 말이다.

```react
ReactDOM.render(<Welcome name="철수"/>, document.getElementById('root'));
```

여기서 props라는 것이 나오는데, 이것은 properties의 준말로 함수의 인자 같은 역할을 한다.
위의 예에서는 props의 name이라는 속성에 담긴 값을 `<h1>Hello, {props.name}</h1>`으로 참조하고 있다.
여기서 `{`, `}` 는 JSX의 중간에 구멍을 뚫고 자바스크립트 문법을 사용한 내용을 적을 수 있는 표시라고 생각하면 된다.

컴포넌트 안에 들어간 이 props는 컴포넌트 안에서 절대 수정되지 않는다.
리액트에는 다음과 같은 엄격한 규칙이 있으니 숙지해야한다.

> All React components must act like pure functions with respect to their props.
> 모든 리엑트 컴포넌트는 들어가는 props에 대해 pure function처럼 작동해야한다.

여기서 pure function은 인풋을 바꾸지 않고, 같은 인풋에 같은 결과를 주는 함수를 말한다.
그럼 이 props를 가공해서 시간에 대해 output을 바꾸려면 어떻게 해야하는가?

## 상태와 생애주기(State and Life Cycle)

state는 props와 유사하다. 하지만, 밖에서 이 값을 건드릴 수 없고, state를 소유한 컴포넌트에 의해서 완전히 통제된다.
컴포넌트 안에서만 작동하는 local state를 사용하기 위해서는 컴포넌트를 class로 등록해야한다.

```react
class Clock extends React.Component {
  constructor(props) {
    super(props);
    this.state = {date: new Date()};
  }

  render() {
    return (
      <div>
        <h1>Hello, world!</h1>
        <h2>It is {this.state.date.toLocaleTimeString()}.</h2>
      </div>
    );
  }
}

ReactDOM.render(
  <Clock />,
  document.getElementById('root')
);
```

위의 예를 보면, 자바스크립트 ES6 문법의 `class`를 사용하고 있는 것을 볼 수 있다.
리엑트 컴포넌트를 `class`로 사용하려면, `React.Component`에서 상속을 받아와야한다.
그리고 생성자(`constructor`)에 `super(props)`와 `state`를 넣어줘야한다.
그리고 반드시 `render` 함수를 갖고 있어야하고, 이 함수는 반드시 리엑트 엘리먼트 또는 컴포넌트를 반환해야한다.

리엑트 앱은 생애주기(life cycle)라는 것을 갖는데, 이 주기마다 실행되는 것들이 있다.
다음 그림을 보면, 과정을 한눈에 볼 수 있다.

<figure>
  <img src="/assets/figures/react-lifecycle.png" alt="lifecycle"/>
  <figcaption>출처: http://projects.wojtekmaj.pl/react-lifecycle-methods-diagram/</figcaption>
</figure>

이 생애주기는 마운팅(mounting), 업데이팅(updating), 언마운팅(unmounting)으로 구성되는데,
이들 생애주기에 실행되는 함수는 각각 `componentDidMount`, `componentDidUpdate`, `componentWillUnmount`다.

추측하건데, 이 생애주기를 관통하는 파라미터가 바로 `state`인 것 같다.
마운팅 시에 state가 생성되고 업데이트 시에 state가 업데이트되고 이 업데이트된 값들로 다른 컴포넌트를 생성하기도 하고 업데이트하기도 하기 때문이다. 위 그림에 나와 있지만, 업데이트 시에 `setState` 함수가 써 있는데, state를 바꿀 때는 이 함수를 반드시 사용해야한다.
state를 직접 바꾸면 업데이트가 일어나지 않을 것이다.

state는 한 컴포넌트 내에서만 의미가 있고 다른 컴포넌트에서는 접근이 불가능하므로 local 또는 encapsulated 라고 한다.
상태는 항상 어떤 구체적인 컴포넌트가 소유하고, 이 state를 갖고 만들어진 데이터나 UI는 state가 속해있는 컴포넌트의 하위 컴포넌트에만 영향을 미친다.

state는 생애주기마다 변할 수 있으므로, 이 state를 소유한 컴포넌트(stateful component)는 시간에 따라 변하는 컴포넌트고, state를 갖지 않은 컴포넌트(stateless component)는 시간에 따라 변하지 않는 컴포넌트라고 이해할 수 있을 것 같다.

# 방문하고 싶은 곳 목록 만들기

만들고 싶은 것은 미리 적어둔 목록을 처음에 보여주고, 사용자의 입력을 받아서 목록에 추가해서 보여주는 기능을 하는 웹페이지다.
여기에 몇가지를 추가하고 싶은데, 방문했던 곳을 클릭하면 취소선이 그 위에 그려지고, 목록 위에 마우스를 올리면 더 잘 볼 수 있도록 텍스트가 변하는 기능이다.

## 앱의 흐름

먼저 목록의 항목이 될 컴포넌트를 만들고, 전체 페이지를 보여줄 컴포넌트를 만들어야한다.
그 뒤에 사용자의 입력을 받아서 내용을 업데이트해야 한다.

여기서 생각해봐야할 것은 시간에 따라 변해야할 내용이 state에 들어있어야하고, 이벤트가 일어날 때 setState로 이 state의 변화를 줘야한다는 점이다.
이렇게 해야 생애주기가 제대로 작동해 원하는대로 변화한 내용을 그려낼(render) 수 있다.
즉, setState로 변화한 상태로 인해 render가 일어나게 된다.
위에서 봤던 생애주기 그림을 떠올리면 이해가 쉬울 것이다.

그러니까 변화할 내용은 state를 통해 전달하고 변화된 내용은 setState를 통해 생애주기를 작동시켜 render를 다시 해야하는 것이다.

사용자의 입력을 받아들여 목록을 업데이트하는 것도 마찬가지다.
제출하는 이벤트가 생기면, state에 항목 추가에 관한 정보를 넣고, setState로 다시 목록을 rendering 하면 된다.

이런 것들을 떠올리고 다음 내용을 보면 이해가 좀 더 쉬울 것이다.

## 완성된 코드

```react
import React from 'react';
import ReactDOM from 'react-dom';

class PlaceItem extends React.Component {
    constructor(props) {
        console.log('called constructor() with props: ', props);
        super(props);

        this.state = {
            done: false,
            mouseOver: false,
        }

    }

    onItemClick() {
        console.log('called onItemClick()');
        this.setState({
            done: !this.state.done
        });
    }

    onItemHover() {
        console.log('called onItemHover()');
        this.setState({
            mouseOver: !this.state.mouseOver
        });
    }

    render() {
        console.log('called render()');
        const style = {
            textDecoration: this.state.done ? 'line-through' : 'none',
            fontWeight: this.state.mouseOver ? 'bold' : 'normal',
            color: this.state.mouseOver ? 'blue' : 'black',
            fontFamily: "Arial"
        };

        return (
            <li style={style}
                onClick={this.onItemClick.bind(this)}
                onMouseOver={this.onItemHover.bind(this)}
                onMouseLeave={this.onItemHover.bind(this)}>
                {this.props.place}
            </li>
        );
    }
}

class PlacesToVisit extends React.Component {
    constructor(props) {
        super(props);
        this.state = {userInput: '', places: props.places};

        this.handleChange = this.handleChange.bind(this);
        this.handleSubmit = this.handleSubmit.bind(this);
    }
    
    handleChange(event) {
        this.setState({userInput: event.target.value});
        console.log(this.state.userInput);
    }

    handleSubmit(event) {
        this.setState({places: this.state.places.concat(this.state.userInput)});
        console.log(this.state.places);
        event.preventDefault();
    }

    render(props) {
        return (
            <div>
                <form onSubmit={this.handleSubmit}>
                    <label>
                        Place:
                        <input type="text" value={this.state.userInput} onChange={this.handleChange} />
                    </label>
                    <input type="submit" value="Submit" />
                </form>
    
                <h2>Places I want to visit!</h2>
                <ul>
                    {this.state.places.map(place =>
                        <PlaceItem place={place} />
                    )}
                </ul>
            </div>
        );
    }
}


var element = <PlacesToVisit places={['Vatican', 'Florence', 'Berlin', 'London']}/>;
ReactDOM.render(element, document.getElementById("root"));
```

# 마치면서

리엑트를 처음 접해봤는데, 처음에는 왜 이렇게 하는 건가 싶었다.
JSX 문법은 혼란스러웠다.
하지만 리엑트 홈페이지의 "Main Concepts" 문서를 읽고 나니 리엑트가 어떤 방식으로 웹페이지를 그려내고 있는지 알 수 있게 됐다.

가장 중요한 개념은 생애주기인 것 같다.
state라는 파라미터를 중심으로 모든 리엑트 컴포넌트가 만들어지고, 업데이트 되고, 사라지는 것을 알고 나니 이해가 조금 더 되었다.
세부적인 것들은 더 공부를 해봐야하겠지만,
지금까지 본 것들로 봤을 때 상당히 직관적인 개념으로 만들어져있는 것 같았다. 더 공부해보면 재미있는 기능들이 아주 많을 것 같다.

# 참고

- 코드스테이츠의 Immersive 강의
- [React Docs Main Concepts](https://reactjs.org/docs)