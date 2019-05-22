---
layout: post
title: 'VS Code에서 ESlint와 Prettier 함께 사용하기'
comments: true
author: feynubrick
date: 2019-05-20
tags: [Study, Node.js]
---

혼자서만 코드를 짜다가, 여러 사람과 프로젝트를 하다보면 여러 문제를 겪습니다.
Git을 잘못 써서 사람들과 충돌이 생기기도 하고,
나와는 다른 방식으로 작성된 코드 때문에 두통이 생기기도 하죠.
주니어 개발자라 아직 심각한 상황은 겪지 못했지만, 아름답게 정렬되지 않은 코드를 보면 욱하는 마음이 듭니다.
제대로된 개발자가 되어가고 있다는 뜻이겠죠...?

프로젝트 시작 전에 코드 스타일을 통일하면, 적어도 코드 스타일 때문에 받는 스트레스는 줄어들 것이라는 것은 경험이 없는 저도 충분히 예상할 수 있는 결과입니다.

그런데 우리들이 말로 약속을 한다고 지킬 사람들이 아니죠.
개발하다보면 그런 약속 따위를 기억할 여유가 없잖아요.

그래서 ESLint와 Prettier라는 도구를 사용합니다.
이 도구를 사용하면, "강제"로 내가 짠 코드를 정해진 스타일로 바꿔주니까요.
과장해서 말하면, 한 사람이 짠 코드처럼 보이게 만들어줍니다.

자, 그럼 ESLint와 Prettier라는 도구가 어떤 것이고 어떻게 사용할 수 있는지 알아보도록 합시다.

# ESLint와 Prettier가 뭔가요?

## ESLint

[ESlint](https://eslint.org/)는 자바스크립트용 linter입니다.
그럼 linter는 뭘까요?
linter는 소스코드를 분석해서

- 문법 에러
- 버그
- 정해진 스타일과 다른 스타일로 작성된 코드

등을 표시해주는 도구를 말합니다.

## Prettier

[Prettier](https://prettier.io/) 홈페이지에 가면 이게 뭔지 아주 간략히 설명해놓고 있습니다.

> An opinionated code formatter

직역하면, 독단적인 코드 포매터네요.
그러니까 정해진 스타일로 코드를 강제로 바꾸는 녀석이군요.

## 근데 하나만 쓰면 되지, 왜 둘을 같이 쓰나요?

근데 왜 이 둘을 같이 쓸까요?
Prettier는 코드의 스타일을 잡아냅니다.
그러니까,

- 한줄의 최대 길이
- tab과 space 섞어쓰기
- 인용 스타일(`'`인지 `"`인지)

같은 것만 신경씁니다.

반면에 ESLint는 코드의 질(quality)을 주로 신경 쓰는데요.
그러니까,

- 사용되지 않는 변수
- 전역변수 금지
- promise를 선호

와 같이 스타일과는 다른 코드의 문법적인 스타일도 원하는 대로 정할 수 있습니다.
물론, ESLint로는 스타일도 잡을 수가 있습니다.
하지만, Prettier는 파일을 분석한 뒤, 자기가 강제로 파일을 바꿔놓습니다.
(eslint도 `--fix` 옵션을 붙여 명령어를 실행하면 강제로 바꾸긴 합니다만, Prettier는 에디터의 확장프로그램과 함께 자동으로 바꿀 수 있습니다)

따라서 Prettier를 같이 쓰면, ESLint와 Prettier로 스타일, 문법 오류를 바로잡은 뒤 파일을 자동으로 그에 맞게 바꾸는 것이죠.

그러니까 ESLint와 Prettier를 같이 사용한다는 것은
서로의 부족한 점을 매꿔준다는 얘기입니다.
아름답네요!

# ESLint와 Prettier 설치하기

왜 이 둘을 같이 써야하는지, 이제 설명은 충분히 된 것 같습니다.

그럼 이제 우리가 설치해야하는 것이 무엇인지 알아보겠습니다.

- Visual Studio Code (VS Code) 확장 프로그램 설치
- Node.js 모듈 설치

제목에 써놨듯이 여기서는 VS Code를 사용한다고 생각하고 설명할 것입니다.
확장 프로그램은 ESLint와 Prettier를 VS Code와 연동하기 위해 설치하는 것입니다.
그러니까 단순 보조인 것이죠.

진짜는 Node.js 모듈입니다.
이 녀석이 없으면 작동을 하지 않습니다.

이제 설치해볼까요?

## Node module 설치하기

VS Code에 확장프로그램 설치했다고 끝이 아닙니다.
이 확장프로그램들은 그저 ESLint와 Prettier가 VS Code에서 잘 작동하도록 돕는 친구들일 뿐입니다.
본체는 Node에서 실행될 모듈들입니다!

이 본체들을 그럼 어디에 설치해야할까요?
당연히 우리가 공동으로 작업할 프로젝트 폴더에 설치해야겠죠.
이렇게 설치하고 GitHub에 올리기만 하면 모든 팀원들이 따로 설정할 필요없이 같은 설정을 적용할 수 있습니다.

작업할 프로젝트 폴더에 가서 필요한 모듈을 설치합시다.

### eslint 모듈 설치

프로젝트 폴더로 왔다면, 터미널을 켜서 다음 명령어를 입력합니다.

`yarn`을 사용하신다면 다음 명령어를,

```
$ yarn add eslint --dev
```

`npm`을 사용하신다면 다음 명령어를 사용하세요.

```
$ npm install eslint --save-dev
```

터미널이 생소하실 분들을 위해 이 위의 내용이 뭔지 설명드리면,
`$ 명령어`에서 `$`는 이 뒤에 오는 것이 명령어라는 뜻입니다.
더 정확하게는 Shell에서 실행하라는 말입니다.

자 이제 좀 기다리면 eslint 설치가 완료됩니다!

### prettier 모듈

같은 폴더에서 역시 터미널에 다음 명령어를 입력합니다.

```
$ yarn add prettier --dev --exact
```

또는

```
$ npm install prettier --save-dev --save-exact
```

한가지 주목할 점은 eslint 모듈을 설치할 때와는 다르게 `--exact`, `--save-exact` 옵션이 추가됐다는 것입니다.
Prettier에서는 이 옵션을 붙이는 것을 추천하는데요.
버전이 달라지면서 생길 스타일 변화를 막기 위해서라고 합니다.

### 필요한 모듈 추가로 설치하기

위 두 모듈을 사용할 때 설치하면 좋은(이라고 쓰고 "꼭 사용해야하는"이라고 읽는다) 모듈도 같이 설치해줍니다.

- eslint-config-prettier
- eslint-plugin-react

`eslint-config-prettier`는 eslint와 prettier를 함께 사용할 때 불필요하거나, 충돌할지 모르는 eslint 설정을 해제하는 모듈입니다.
혹시모를 충돌을 막고 싶다면 꼭 설치하는게 좋겠죠.

`eslint-plugin-react`는 리엑트를 개발할 때 사용하면 좋습니다.

## VS Code에 ESLint, Prettier 확장프로그램 설치하기

이제 Node 모듈은 다 설치했고, VS Code와 같이 사용할 때 필요한 모듈을 설치하고 설정을 바꿔줄 차례입니다!

VS Code의 "Extensions: Marketplace"에 들어가서 eslint와 prettier를 검색해 설치합니다.
다음 그림들을 참고해서 적절한 것을 설치하도록 합니다.

<figure>
  <img src="https://i.imgur.com/9mHyUjT.png" alt="eslint"/>
  <figcaption>ESLint extension in VS Code</figcaption>
</figure>

<figure>
  <img src="https://i.imgur.com/EoKb3uP.png" alt="prettier"/>
  <figcaption>Prettier extension in VS Code</figcaption>
</figure>

### VS Code 설정하기

VS Code에서 저장할 때마다 포맷을 하게 하고 싶다면, 설정을 좀 만져줘야합니다.
VS Code의 설정(윈도우, 리눅스에서는 `Ctrl` + `,`, 맥에서는 `Cmd` + `,`)으로 들어갑니다.
오른쪽 위 구석에 보면 `{ }` 모양의 아이콘이 있을 겁니다.
이걸 클릭하면 VS Code의 설정파일인 `settings.json`을 직접 편집할 수 있는데요,
여기에 다음의 키-값 쌍을 추가합니다.

```json
...,
"editor.formatOnSave": true,
"javascript.format.enable": false, // VS Code의 기본 포매터를 끕니다
"prettier.eslintIntegration": true,
...
```

# 스타일 규칙 설정하기

이제 준비는 다 됐습니다.
규칙만 정하면 되겠네요.

본격적으로 들어가기전에 드리고 싶은 말씀이 있습니다.
스타일은 주관적인 것이라 정답이 없다는 것입니다.
어떤 규칙을 사용하시더라도 이건 각자의 취향일 뿐이니 본인에게 맞는 것을 찾아서, 변경해서 쓰시면 됩니다!

## 어떤 걸 고를까?

규칙을 정하려면 어떤 규칙이 있는지 알아야겠죠?
[ESLint 규칙](https://eslint.org/docs/rules/)을 한번 살펴볼까요?

그런데 말입니다... 세상에, 규칙이 너무 많습니다.
어떤 걸 어떻게 골라야될지 모르겠네요.
모든 것을 완전히 다 바꾸기는 어렵기 때문에 여러가지 규칙을 미리 정해둔 모음들이 있는데요.

- prettier
- airbnb
- standard
- ...

찾아보시면 많은 규칙 모음을 확인하실 수 있을 것입니다.
어떤 규칙을 사용하셔도 상관 없습니다.
다시 말하지만, 각자에게 맞는 규칙을 사용하시면 됩니다.
여기서는 "Prettier"라는 규칙을 적용하는 방법과,
이 규칙을 토대로 원하는 대로 어떻게 바꿀 수 있을지도 알아보겠습니다.

## Prettier 규칙

### prettier 관련 모듈 설치

다음의 모듈을 설치해줍니다.

- eslint-plugin-prettier

이 모듈은 Prettier 규칙을 ESLint의 규칙으로 사용하기 위한 것입니다.

```
$ yarn add eslint-plugin-prettier --dev
```

또는

```
$ npm install eslint-plugin-prettier --save-dev
```

명령어를 이용합니다.

### 설정파일 만들기

프로젝트 폴더의 최상단에 `.eslintrc.json`를 만들고 다음과 같이 구성해줍니다.

```json
{
  "extends": [
    "eslint:recommended",
    "plugin:prettier/recommended",
    "plugin:react/recommended",
    "prettier",
    "prettier/react"
  ],
  "plugins": ["prettier", "react"],
  "parserOptions": {
    "ecmaVersion": 6,
    "sourceType": "module",
    "ecmaFeatures": {
      "jsx": true
    }
  },
  "env": {
    "es6": true,
    "browser": true,
    "node": true
  },
  "rules": {
    "prettier/prettier": [
      "error",
      {},
      {
        "usePrettierrc": false
      }
    ]
  }
}
```

이렇게 하면, Prettier의 규칙을 ESLint의 규칙으로 사용하게 됩니다.

조금 설명을 덧붙이면, `"extends"` 옵션에 들어가는 내용은 뒤에 나오는 규칙이 앞의 규칙을 덮어씌웁니다.
그러니까 겹치는 것이 있다면 뒤에 것이 설정되는 것이죠.
`"prettier"`는 `eslint-config-prettier`의 설정으로, 충돌이 날만한 것을 잡아 주기 때문에 뒤에 놓는 것이 좋습니다.
`"prettier/react"`는 같은 모듈의 react 설정으로 역시 충돌 날만한 것들을 잡아주게 됩니다.

`"rules"."prettier/prettier"` 옵션은 내부에 두종류 옵션을 넣을 수 있습니다.
여기 적혀있는 두번째 옵션에 들어있는 `"usePrettierrc": false`는 Prettier의 규칙 설정 파일인 `.prettierrc` 파일을 무시하겠다는 옵션입니다.
설정한 것 이외에 것이 적용되는 것을 방지하기 위해서 입니다.
첫번째 옵션에는 우리가 원하는 대로 Prettier의 규칙을 넣을 수 있습니다.
이는 바로 다음 절에서 확인해보도록 합시다.

그외 설명이 되지 않은 나머지는 각 모듈의 npm 페이지의 추천 방법대로 적용한 것입니다.
그리고 react를 사용할 때 발생할만한 에러를 막아주는 옵션도 몇개 추가했습니다.

- "parserOptions"."ecmaVersion": 6 => ES6 문법 사용가능
- "parserOptions"."sourceType": "module" => import/export 사용가능
- "env"."es6": true => ES6의 전역변수 사용가능
- "env"."browser": true => DOM 사용가능

각 설정에 관심있거나, 여기에 소개되지 않은 플러그인을 추가해서 적용하고 싶으신 분은 각 플러그인의 페이지에서 옵션을 확인하시면 될 것입니다.

## 내가 원하는 대로 바꾸기

그럼 내가 원하는 대로는 어떻게 바꿀까요?
이것도 어렵지는 않습니다.
ESLint 규칙은 `"rules"`에서 바꾸고, prettier 규칙은 `"rules"."prettier/prettier"`에서 바꾸면 되니까요.

다음 내용처럼 말이죠.

```json
...,
"rules": {
    "prettier/prettier": [
      "error",
      {
        "singleQuote": true,
        "printWidth": 120
      },
      {
        "usePrettierrc": false
      }
    ],
    "no-console": "warn",
    "semi": 2,
    "no-undef": "warn"
  }
```

필요한 규칙은 [ESLint 공식문서](https://eslint.org/docs/rules/)에서 찾아서 바꾸시면 될 겁니다.

# [추가] 제대로된 코드가 아니면 git commit 안되도록 막기 (Husky)

지금까지는 저장했을 때, 정해진 규칙에 맞게 코드를 바꾸는 방법에 대해서 알아봤습니다.
여기서는 `git commit`을 할 때, 코드가 제대로 고쳐졌는지 검사해서 자동으로 바꾸는 방법을 알아보겠습니다.
코드가 더럽혀지는걸 막기 위해서요.

## Husky

Husky는 git에 어떤 명령이 실행될 때 같이 실행하게 할 수 있는 명령을 관리하는 도구입니다.
우리가 하고 싶은 것은 커밋되기 전에 문법 검사를 해서 수정하는 것이므로 Husky를 이용하면 될 것 같네요!

```
$ yarn add husky --dev
```

또는

```
$ npm install husky --save-dev
```

로 모듈을 설치합니다.

커밋되기 전에 어떤 걸 해야할지 Husky에게 알려주려면 `pacakge.json`을 좀 만져야합니다.
다음 속성을 추가합시다.

```json
...,
"husky": {
    "hooks": {
      "pre-commit": "eslint src --fix"
    }
  },
...
```

이렇게 해놓으면, 커밋을 할 때 등록해둔 명령어를 실행하고 자동으로 고쳐지지 않으면 에러를 띄워 커밋이 되지 않게 만들 수 있습니다.

## Lint-staged

Husky가 다 좋은데, 문제는 모든 코드를 다 검사한다는 겁니다.
매번 이러면 낭비가 심하니, staged된 코드들만 검사하도록 합시다.
그러려면 `lint-staged`라는 모듈을 설치해야 합니다.

```
$ yarn add lint-staged --dev
```

또는

```
$ npm install lint-staged --save-dev
```

Husky처럼 `package.json`에 어떤 걸 해야할지 설명해줘야합니다.
`package.json`의 맨 뒤 내용을 다음과 같이 바꿔줍니다.

```json
...,
"lint-staged": {
  "*.{js,jsx}": [
    "eslint --fix",
    "git add"
  ]
},
"husky": {
  "hooks": {
    "pre-commit": "lint-staged"
  }
}
```

`lint-staged`를 커밋될 때 사용할 것이니까 Husky 설정도 바꿔줬습니다!

# 마치며

지금까지 ESLint와 Prettier, Husky와 Lint-staged를 사용해 코드의 스타일을 자동으로 검사하고 수정하는 방법에 대해 알아봤습니다.

저도 처음에는 그냥 아무 글을 보고 그대로 따라했습니다.
그런데 각 옵션이 무엇인지, 어떤 플러그인은 왜 설치하는 것인지 궁금해서 찾아보니 각각이 다 의도가 있는 것이었습니다.
시간이 좀 걸리더라도, 여기서 사용된 플러그인, 규칙들을 찾아보시고 본인의 프로젝트에 맞게 변경하시는 것을 추천드립니다!

# 참고

- https://hackernoon.com/write-beautiful-and-consistent-javascript-code-using-eslint-prettier-and-vscode-760837fdef89
- https://www.wisdomgeek.com/development/web-development/using-prettier-format-javascript-code/
- https://www.npmjs.com/package/eslint-config-prettier
- https://www.johnstewart.dev/eslint-prettier-vscode/
- https://www.orangejellyfish.com/blog/code-consistency-with-eslint-and-husky/
- https://medium.com/@sgroff04/configure-eslint-prettier-and-flow-in-vs-code-for-react-development-c9d95db07213
