---
layout: post
title: 'VS Code에서 ESlint와 Prettier 함께 사용하기'
comments: true
author: feynubrick
date: 2019-05-20
update: 2020-02-02
tags: [Lint, Code Formatter, VS code, Javascript]
---

혼자서만 코드를 짜다가, 여러 사람과 프로젝트를 하다보면 여러 문제를 겪습니다.
Git을 잘못 써서 사람들과 충돌이 생기기도 하고,
나와는 다른 방식으로 작성된 코드 때문에 두통이 생기기도 하죠.
주니어 개발자라 아직 심각한 상황은 겪지 못했지만, 아름답게 정렬되지 않은 코드를 보면 욱하는 마음이 듭니다.
제대로된 개발자가 되어가고 있다는 뜻이겠죠...?

프로젝트 시작 전에 코드 스타일을 통일하면, 적어도 코드 스타일 때문에 받는 스트레스는 줄어들 것이라는 것은 경험이 없는 저도 충분히 예상할 수 있는 결과입니다.

그런데 우리들이 말로 약속을 한다고 지킬 사람들이 아니죠.
약속 자체를 까먹거나 성향이 바뀌거나 바쁘거나 하면 코드에 난리가 날 겁니다.

그래서 ESlint와 Prettier라는 도구를 사용합니다.
이 도구들을 사용하면, "강제"로 내가 짠 코드를 정해진 스타일로 바꿔주니까요.
과장해서 말하면, 한 사람이 짠 코드처럼 보이게 만들어줍니다.

자, 그럼 ESlint와 Prettier라는 도구가 어떤 것이고 어떻게 사용할 수 있는지 알아보도록 합시다.

# ESlint와 Prettier가 뭔가요?

## ESlint

linter는 소스코드를 분석해서 문법 에러, 버그를 찾고 보고해주는 도구를 말합니다.
아무래도 코드를 짤 때 버그를 줄여줄 수 있는 도구이니 당연히 거의 모든 언어들이 각자의 linter를 갖고 있습니다.

자바스크립트에서 쓸 수 있는 linter는 JS Lint, JS Hint, ESlint 등입니다.
이중 가장 인기가 많은 것이 [ESlint](https://eslint.org/)입니다.

ESlint는 보고만 해주는 것이 아니라, 고쳐주기까지 하는데요.
규칙을 자신의 상황에 맞게 바꿀 수도 있어 좋습니다.

## Prettier

그럼 [Prettier](https://prettier.io/)는 뭘까요?

Prettier는 코드에 적용되어 있던 원래 스타일을 깡그리 무시하고, 줄 길이를 고려해서 정해진 규칙대로 다시 써서 내보냅니다.
따라서 일관된 코드 스타일을 전체 코드 베이스에 강제합니다.
그러니까 아름답고 일관적으로 작성된 코드건 작성한 사람도 못 알아보는 엉망으로 작성된 코드건 하나의 스타일로 바뀌는 거죠.

독재가 따로 없네요!
그래서 Prettier는 자신을 이렇게 소개합니다.

> opinionated code formatter

그러니까 독단적인 코드 포매터입니다.

개발할 때 코드 스타일의 "독재자"를 필요로하는 언어가 엄청 많은 것 같습니다.
다음의 언어들이 모두 지원되는 것을 보면요.

- JavaScript, including ES2017
- JSX
- Angular
- Vue
- Flow
- TypeScript
- CSS, Less, and SCSS
- HTML
- JSON
- GraphQL
- Markdown, including GFM and MDX
- YAML

## 근데 하나만 쓰면 되지, 왜 둘을 같이 쓰나요?

조금 검색을 해보시면 아시겠지만, 코드 스타일을 맞추기 위한 방법을 알려주는 많은 블로그 글들이 ESlint만 사용하지는 않고, Prettier를 같이 사용하도록 소개하고 있습니다.

ESlint는 문법 에러를 잡아내고 특정 문법 요소를 쓰도록 만드는 등 코드 퀄리티와 관련된 것을 고치기 위해 사용하고,
Prettier는 한 줄의 최대 길이나, tab을 쓸 것인지 space를 쓸 것인지, 인용스타일은 `'`로 할 것인지 `"`로 할 것인지 같이 문법적으로는 문제가 안되지만 미학적(?)으로 문제가 되는 것들을 잡아서 고쳐내는 데 사용합니다.

따라서 Prettier를 같이 쓰면, ESLint와 Prettier로 스타일, 코드 퀄리티를 바로 잡은 뒤 파일을 자동으로 그에 맞게 바꾸는 것이죠.

말을 들어서다음과 같이 엉망으로 작성된 코드와 함께, 직접 눈으로 보면서 확인해봅시다.
이 코드는 Express의 HelloWorld 예제를 최대한 못나보이게 바꿔본 것입니다.

```javascript
const express=require('express');
var a
const app=express(    )
const port= 3000;app.get("/", (  req,res)=> {res.send("Hello World!")})
app.listen(
    port, function ( ){
        console.log(`Example app listening on port ${port}!`)
})
```

휴, 일부러 잘못 짜기도 참 어렵군요.

아무튼, 이 코드를 ESlint를 써서 고치면 다음과 같이 됩니다.

```javascript
const express = require('express');

let a;
const app = express();
const port = 3000; app.get('/', (req, res) => { res.send('Hello World!'); });
app.listen(
  port, () => {
    console.log(`Example app listening on port ${port}!`);
  },
);
```

한결 낫군요. 다음과 같이 간단한 스타일부터

- semi colon(`;`)을 강제
- 띄어쓰기는 한칸만
- 인용기호는 `'`만

다음과 같은 코드 퀄리티와 관련된 내용을 자동으로 고쳐줬습니다.

- `var` 금지
- 콜백함수로 애로우 함수를 사용

자동으로 고쳐지지 않는 문법 오류가 있으면 다음과 같이 알려주기도 하죠!

```
  3:5  error    'a' is defined but never used  no-unused-vars
  8:5  warning  Unexpected console statement   no-console

✖ 2 problems (1 error, 1 warning)
```

일단 알려준 오류를 해결해보겠습니다.
`let a;`를 지우고 봤지만 여전히 맘에 안드는 것들이 있습니다.
줄 바꿈이나 코딩스타일이 저랑은 영 안맞네요.

이때 Prettier를 쓰면 다음과 같이 흡족하게 바뀝니다.

```javascript
const express = require("express");

const app = express();
const port = 3000;
app.get("/", (req, res) => {
  res.send("Hello World!");
});
app.listen(port, () => {
  console.log(`Example app listening on port ${port}!`);
});
```

이제 ESlint와 Prettier 각각이 어떤 일을 하는지는 이해하셨을 것입니다.

# ESLint와 Prettier 설치하기

왜 이 둘을 같이 써야하는지, 이제 설명은 충분히 된 것 같습니다.

그럼 이제 우리가 설치해야하는 것이 무엇인지 알아보겠습니다.

- Node.js 모듈 설치와 설정
- Visual Studio Code (VS Code) 확장 프로그램 설치와 설정

제목에 써놨듯이 여기서는 VS Code를 사용한다고 생각하고 설명할 것입니다.
확장 프로그램은 ESLint와 Prettier를 VS Code와 연동하기 위해 설치하는 것입니다.
그러니까 단순 보조인 것이죠.

진짜는 Node.js 모듈입니다.
이 녀석이 없으면 작동을 하지 않습니다.

이제 설치해볼까요?

## Node module 설치하기

이 본체들을 그럼 어디에 설치해야할까요?
당연히 우리가 공동으로 작업할 프로젝트 디렉토리에 설치해야겠죠.
이렇게 설치하고 GitHub에 올리기만 하면 모든 팀원들이 따로 설정할 필요없이 같은 설정을 적용할 수 있습니다.

작업할 프로젝트 디렉토리에 가서 필요한 모듈을 설치합시다.

### eslint 모듈

프로젝트 디렉토리로 왔다면, 터미널을 켜서 다음 명령어를 입력합니다.

```
$ npm install eslint --save-dev
```

자 이제 좀 기다리면 eslint 설치가 완료됩니다!

### prettier 모듈

같은 디렉토리에서 역시 터미널에 다음 명령어를 입력합니다.

```
$ npm install prettier --save-dev --save-exact
```

한가지 주목할 점은 eslint 모듈을 설치할 때와는 다르게 `--save-exact` 옵션이 추가됐다는 것입니다.
Prettier에서는 이 옵션을 붙이는 것을 추천하는데요.
버전이 달라지면서 생길 스타일 변화를 막기 위해서라고 합니다.

### 필요한 추가 모듈들

이 둘을 함께 쓰려면 추가로 여러 모듈을 설치해야 합니다.

- `eslint-config-prettier`: Prettier와 충돌할 설정들을 비활성화합니다.
- `eslint-plugin-prettier`: 코드 코맷할 때 Prettier를 사용하게 만드는 규칙을 추가합니다.


다음 명령어로 위에서 언급한 모듈을 설치합니다.

```
$ npm install eslint-plugin-prettier eslint-config-prettier --save-dev
```

그리고 프로젝트의 최상위 위치에 .eslinrc.json을 만들고 안에 다음의 내용을 추가합니다.

```json
{
  "plugins": ["prettier"],
  "extends": ["eslint:recommended", "plugin:prettier/recommended"],
  "rules": {
    "prettier/prettier": "error"
  }
}
```

## VS Code에 ESLint, Prettier 확장프로그램 설치하기

이제 Node 모듈은 다 설치하고 설정했으니, VS Code와 같이 사용할 때 필요한 모듈을 설치하고 설정을 바꿔줄 차례입니다!

VS Code의 "Extensions: Marketplace"에 들어가서 eslint와 prettier를 검색해 설치합니다.
다음 그림들을 참고해서 적절한 것을 설치하도록 합니다.

<figure>
  <img src="https://i.imgur.com/rTU6jno.png" alt="eslint"/>
  <figcaption>ESLint extension in VS Code</figcaption>
</figure>

<figure>
  <img src="https://i.imgur.com/3rpFVi7.png" alt="prettier"/>
  <figcaption>Prettier extension in VS Code</figcaption>
</figure>

### VS Code 설정하기

VS Code에서 파일을 저장할 때마다 자동으로 고쳐지게 하고 싶다면, 설정을 좀 만져줘야합니다.
VS Code의 설정(윈도우, 리눅스에서는 `Ctrl` + `,`, 맥에서는 `Cmd` + `,`)으로 들어갑니다.

<figure>
  <img src="https://i.imgur.com/dlJdnRr.png" alt="vscode-setting"/>
  <figcaption>VS Code 설정</figcaption>
</figure>

위 그림에서 왼쪽 위에 "Workspace"라고 글씨가 써져있는데요.
이걸 누르면 현재 작업공간에만 변경된 설정이 적용되는데요.
현재 디렉토리의 `.vscode/settings.json`에 설정이 저장됩니다.

linter와 prettier 같은 설정은 프로젝트 별로 설정이 다른 경우가 많기 때문에 개인적으로는 작업공간마다 설정파일을 따로 관리하는 것을 선호합니다. 하지만 전체 디렉토리에 같은 설정이 기본으로 설정되더라도, 그 작업공간에 설정을 별도로 두면 그 설정이 선택되기 때문에 전체 작업공간에 적용하는 것도 괜찮습니다. 필요와 취향에 맞게 설정하면 될 것 같습니다.

아무튼 오른쪽 위 구석에 위 스크린샷에서 가리키는 아이콘이 있을 겁니다.
이걸 클릭하면 VS Code의 설정파일인 `settings.json`을 직접 편집할 수 있는데요,
여기에 다음의 키-값 쌍들을 추가합니다.

```json
{
    // Set the default
    "editor.formatOnSave": false,
    // Enable per-language
    "[javascript]": {
        "editor.formatOnSave": true
    },
    "editor.codeActionsOnSave": {
        // For ESLint
        "source.fixAll.eslint": true
    }
}
```

# 스타일 규칙 설정하기

사실 이제 모든 설정이 끝났으니 여기서 멈추고 프로젝트를 바로 시작하시면 됩니다.
문제는 사용하시다보면 맘에 안드시는 것들을 만날 것이라는 사실인데요.
만나실 때마다 바꿀 수도 있습니다.
하지만 미리 규칙을 정해놓고 간다면 깔끔하겠죠?

[ESLint 규칙](https://eslint.org/docs/rules/)과 [Prettier의 옵션](https://prettier.io/docs/en/options.html)을 살펴봅시다.

그런데 말입니다... 세상에, 규칙이 너무 많습니다.
어떤 걸 어떻게 골라야될지 모르겠네요.
모든 것을 완전히 다 바꾸기는 어렵기 때문에 여러가지 규칙을 미리 정해둔 모음들이 있는데요.

- airbnb
- standard
- google
- ...

찾아보시면 많은 규칙 모음을 확인하실 수 있을 것입니다.
이중 하나를 골라서 사용하셔도 되고, 어떤 규칙을 사용하셔도 상관 없습니다.

스타일은 주관적인 것이라 정답이 없기 때문입니다.
어떤 규칙을 사용하시더라도 이건 각자의 취향일 뿐이니 본인에게 맞는 것을 찾아서, 변경해서 쓰시면 됩니다!

위의 규칙 모음의 모듈의 doc을 살펴보시면 자세한 지침이 나와있으니, 위의 규칙모음을 사용하신다면 그것을 참조하시는 것이 좋겠습니다.

여기서는 어떤 규칙 모음을 사용하던지, 내가 원하는 규칙을 어떻게 적용해야하는지 알아보도록 하겠습니다.

## ESlint 설정 파일

위의 내용을 잘 따라오시고, 스타일 모음을 별도로 설정하지 않으셨다면, `.eslintrc.json`은 이런 모양일 것입니다.

```json
{
    "plugins": ["prettier"],
    "extends": ["eslint:recommended", "plugin:prettier/recommended"],
    "rules": {
        "prettier/prettier": "error"
    }
}
```

이 파일이 바로 ESlint를 설정할 수 있는 파일입니다.
여기에 필요한 옵션들을 집어 넣으면 됩니다.

[ESlint 공식 홈페이지](https://eslint.org/docs/user-guide/configuring)를 읽어보시면 필요한 설정을 할 수 있으실 것입니다.
하지만 장소만 알려주고 어떻게 하는지는 설명을 안하는 것은 너무 무책임한 것 같으니 제가 어떤 설정을 선택했고, 어떻게 적용했는지 설명드리겠습니다.

### 예) Express.js를 위한 설정

우선 설정을 보시죠.

```json
{
    "plugins": ["prettier"],
    "extends": ["eslint:recommended", "plugin:prettier/recommended"],
    "parserOptions": {
        "ecmaVersion": 7,
        "sourceType": "script",
        "ecmaFeatures": {
            "jsx": false
        }
    },
    "env": {
        "browser": false,
        "node": true
    },
    "ignorePatterns": ["node_modules/"],
    "rules": {
        "prettier/prettier": "error"
    },
}
```

여기서 설정한 내용은 [ESlint 문서](https://eslint.org/docs/user-guide/configuring)에서 나온 설명에서 Express.js 프로젝트를 위해 저에게 필요한 것들만 뽑아낸 것입니다.

이 설정을 보시면 ESlint에는 크게 세가지 종류의 설정이 있는 것을 알 수 있습니다.

- `parserOptions`
- `env`: 미리 정의된 전역 변수에 뭐가 있는지 명시
- `rules`: 적용할 규칙들


`parserOptions`는 자바스크립트를 해석하는 파서의 설정에 관한 것입니다.
여기에는 제가 넣은 설정에 대한 설명은 각각 다음과 같습니다.

- ecmaVersion: 자바스크립트의 버전 7 (es7 / ECMA 2016) 선택
- sourceType: import/export 사용 안함
- ecmaFeatures: jsx를 허용안함(디폴트가 false이지만, 글쓰는 목적으로 false로 명시함)

`env`는 linter가 파일을 분석하려 시도할 때 알아야하는, 미리 정의된 전역변수에 무엇이 있는지 명시하는 속성입니다.

- browser: 브라우저에서 사용하는 document 같은 객체를 사용할 수 있게 합니다.
- node: node에서 console 같은 전역변수를 사용할 수 있게 합니다.

`ignorePatterns`에는 어떤 파일, 디렉토리는 무시할 것인지 설정할 수 있는 곳입니다.
이걸 gitignore처럼 별도에 파일에 적어도 되는데요.
`.eslintignore`파일을 만들어 넣어주면됩니다.

`rules`에는 ESlint에서 설정할 수 있는 규칙들을 적는 곳입니다.
[ESLint 규칙](https://eslint.org/docs/rules/)을 참조해서 원하는 규칙을 여기에 넣어주면 됩니다.
이 예시에 추가되어있는 `"prettier/prettier": "error"`는 Prettier와의 충돌을 막기 위해 Prettier에서 지적하는 내용을 error로 취급하겠다는 것입니다.


## Prettier 설정

prettier 규칙은 `.prettierrc.json`파일을 `.eslintrc.json`과 같은 위치에 만들고, [Prettier의 옵션 문서](https://prettier.io/docs/en/options.html)에서 필요한 설정을 골라 안에 채워 넣으면 됩니다.
다음 내용처럼 말이죠.

```json
{
    "trailingComma": "es5",
    "tabWidth": 2,
    "semi": true,
    "singleQuote": true
}
```

# [추가] 제대로된 코드가 아니면 git commit 안되도록 막기 (Husky)

지금까지는 저장했을 때, 정해진 규칙에 맞게 코드를 바꾸는 방법에 대해서 알아봤습니다.
여기서는 `git commit`을 할 때, 코드가 제대로 고쳐졌는지 검사해서 자동으로 바꾸는 방법을 알아보겠습니다.
코드가 더럽혀지는걸 막기 위해서요.

## Husky

Husky는 git에 어떤 명령이 실행될 때 같이 실행하게 할 수 있는 명령을 관리하는 도구입니다.
우리가 하고 싶은 것은 커밋되기 전에 문법 검사를 해서 수정하는 것이므로 Husky를 이용하면 될 것 같네요!

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
      "pre-commit": "eslint . --fix && prettier --write"
    }
  },
...
```

이렇게 해놓으면, 커밋을 할 때 등록해둔 명령어를 실행하고 자동으로 고쳐지지 않으면 에러를 띄워 커밋이 되지 않게 만들 수 있습니다.

여기서 `eslint . --fix`로는 현재 디렉토리의 모든 코드를 eslint를 사용해 자동으로 고친다는 의미입니다.
이 작업이 끝나면 `prettier --write`를 실행해 prettier 규칙을 적용한 뒤 자동으로 고칩니다.

## Lint-staged

Husky가 다 좋은데, 문제는 모든 코드를 다 검사한다는 겁니다.
매번 이러면 낭비가 심하니, staged된 코드들만 검사하도록 합시다.
그러려면 `lint-staged`라는 모듈을 설치해야 합니다.

```
$ npm install lint-staged --save-dev
```

Husky처럼 `package.json`에 어떤 걸 해야할지 설명해줘야합니다.
`package.json`의 맨 뒤 내용을 다음과 같이 바꿔줍니다.

```json
...,
"lint-staged": {
  "*.js": ["eslint . --fix", "prettier --write"]
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

- <https://codeburst.io/javascript-linting-tools-comparison-ebcb4aa23c49>
- <https://hackernoon.com/write-beautiful-and-consistent-javascript-code-using-eslint-prettier-and-vscode-760837fdef89>
- <https://www.wisdomgeek.com/development/web-development/using-prettier-format-javascript-code/>
- <https://www.npmjs.com/package/eslint-config-prettier>
- <https://www.johnstewart.dev/eslint-prettier-vscode/>
- <https://www.orangejellyfish.com/blog/code-consistency-with-eslint-and-husky/>
- <https://medium.com/@sgroff04/configure-eslint-prettier-and-flow-in-vs-code-for-react-development-c9d95db07213>
