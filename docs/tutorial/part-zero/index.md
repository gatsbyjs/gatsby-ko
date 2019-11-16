---
title: 개발환경 설정하기
typora-copy-images-to: ./
disableTableOfContents: true
---

여러분의 첫 Gatsby 사이트를 만들기 시작하기 전에, 우선 주요 웹 기술들에 익숙해지고 필요한 소프트웨어 개발툴이 모두 있는지 확인해야 합니다.

## 커맨드라인에 익숙해지기

커맨드라인은 여러분의 컴퓨터에서 명령을 실행하는데 사용되는 텍스트 기반 인터페이스입니다. 터미널이라고 부르기도 하죠. 이 튜토리얼에선 두 용어가 같다고 생각하시면 됩니다. Mac에서 Finder를 사용하거나 Windows에서 탐색기를 사용하는 것과 비슷합니다. Finder와 탐색기는 그래픽 기반 인터페이스(Graphical user interfaces, GUI)의 예시이며, 커맨드라인은 강력한 텍스트 기반의 방법으로 여러분이 컴퓨터와 상호작용할 수 있게 해줍니다.

여러분의 컴퓨터에서 커맨드라인 인터페이스(Command line interface, CLI)를 찾아서 열어보세요. 여러분의 운영체제에 따라서 [**Mac 사용자를 위한 설명**](http://www.macworld.co.uk/feature/mac-software/how-use-terminal-on-mac-3608274/), [**Windows 사용자를 위한 설명**](https://www.quora.com/How-do-I-open-terminal-in-windows) 또는 [**Linux 사용자를 위한 설명**](https://www.howtogeek.com/140679/beginner-geek-how-to-start-using-the-linux-terminal/)을 참고해주세요.

## Homebrew 설치하기

Gatsby와 Node.js를 설치하기 위해선 [Homebrew](https://brew.sh/)를 사용하시는 것을 추천드립니다. 처음에 해 놓는 설정들이 나중에 여러분들에게 편하게 다가오니까요!

Homebrew를 설치하고 확인하는 방법은 다음과 같습니다:

1. 터미널을 실행하세요.
1. Homebrew가 설치되어 있는지 `brew -v`를 통해 확인해보세요. "Homebrew"와 버전 번호가 있다면 설치되어 있다는 뜻입니다.
1. 위와 같지 않다면, [Homebrew 설치하기](https://docs.brew.sh/Installation)를 통해 여러분의 운영체제(Mac, Linux 또는 Windows)에 맞게 설치해주세요.
1. Homebrew를 설치하셨다면, 2번으로 돌아가서 확인해주세요.

### Mac 사용자만: Xcode Command Line Tools 설치하기

1. 터미널을 실행하세요.
1. `xcode-select --install`를 실행해 Xcode Command line tools를 설치해주세요.
   1. 정상적으로 설치되지 않는다면 Apple 개발자 계정으로 가입 한 후[Apple 사이트에서 직접 다운로드](https://developer.apple.com/download/more/) 해주세요.
1. 설치가 진행된다는 알림이 뜬 이후 일부 개발자 도구 다운로드를 위해 소프트웨어 라이센스에 동의하는지에 대한 확인 알림이 뜰 것입니다.

## ⌚ Node.js와 npm 설치하기

Node.js는 웹 브라우저 밖에서 JavaScript를 실행할 수 있게 해주는 환경입니다. Gatsby는 Node.js 기반으로 만들어졌습니다. Gatsby를 설치하고 실행하기 위해서 여러분의 컴퓨터에는 Node.js의 최신 버전이 설치되어 있어야 합니다.

_참고: Gatsby는 최소 Node 8에서 작동하지만, 더 높은 버전을 사용하셔도 무방합니다._

1. 터미널을 실행하세요.
1. `brew update`를 통해 최신 Homebrew가 설치되어 있도록 해주세요.
1. `brew install node`를 통해 Node.js와 npm을 한 번에 설치하세요.

지금까지의 설치 과정을 모두 진행하셨다면, 이제 모든 것들이 잘 설치되어있는지 확인해볼까요:

### Node.js 설치 확인하기

1.  터미널을 실행하세요.
2.  `node --version` 커맨드를 실행하세요. (혹시 커맨드라인에 익숙치 않으시다면, “`커맨드`를 실행하라”는 말은 “`node --version` 를 치고, 엔터 키를 누르라”는 뜻입니다. “`커맨드`를 실행하라”는 말은 앞으로도 같은 의미로 쓰입니다).
3.  `npm --version`를 실행하세요.

각각의 커맨드에 따라 버전 번호가 출력될 것입니다. 아래 사진에서의 버전 번호와 같지 않을 수도 있습니다! 만일 커맨드를 실행해도 버전 번호가 출력되지 않는다면, 돌아가서 Node.js가 정상적으로 설치되었는지 다시 확인해주세요.

![터미널에서 Node와 npm 버전 확인하기](01-node-npm-versions.png)

## Git 설치하기

Git은 무료 오픈소스로 배포된 버전 관리 시스템으로 규모가 작거나 큰 모든 프로젝트들을 빠르고 효율적으로 관리할 수 있게 해줍니다. 여러분이 Gatsby "스타터" 사이트를 설치할 때, Gatsby는 실제로는 Git을 사용하여 여러분들의 사이트에 필요한 파일들을 다운로드하고 설치합니다. 여러분의 첫번째 Gatsby 사이트를 만들기 위해서는 Git이 설치되어있어야 합니다.

운영체제에 따라 Git을 설치하는 방법이 상이합니다. 여러분의 운영체제에 맞는 가이드를 따르세요:

- [macOS에서 Git 설치하기](https://www.atlassian.com/git/tutorials/install-git#mac-os-x)
- [Windows에서 Git 설치하기](https://www.atlassian.com/git/tutorials/install-git#windows)
- [Linux에서 Git 설치하기](https://www.atlassian.com/git/tutorials/install-git#linux)

## Gatsby CLI 사용하기

Gatsby CLI는 여러분들이 Gatsby로 작동되는 사이트들을 빠르게 만들 수 있도록 도와주며 개발 과정에 있어 특정 커맨드들을 사용가능하게 해줍니다. npm 패키지로 배포되어 있습니다.

Gatsby CLI는 npm을 통해 설치 가능하며 전역으로 설치하기 위해 `npm install -g gatsby-cli`를 실행해주세요.

_**참고**: Gatsby를 설치하고 처음 실행하게 되면 Gatsby 명령어들을 위한 익명 사용 데이터 수집에 관한 짧은 알림이 뜰 텐데, [telemetry 문서](/docs/telemetry)을 통해 이 데이터들이 어떻게 가져와지며 사용되는지 확인할 수 있습니다._

`gatsby --help`를 실행하시면 실행 가능한 모든 커맨드를 볼 수 있습니다.

![터미널에서 Gatsby 커맨드 확인하기](05-gatsby-help.png)

> 💡 만일 권한 문제로 Gatsby CLI를 설치하는 과정에서 문제가 생겼다면, [권한 수정에 관련된 npm 문서](https://docs.npmjs.com/getting-started/fixing-npm-permissions), 또는 [이 가이드](https://github.com/sindresorhus/guides/blob/master/npm-global-without-sudo.md)를 확인해주세요.

## Gatsby 사이트 만들기

이제 여러분은 Gatsby CLI를 이용해 여러분의 첫번째 Gatsby 사이트를 만들 준비가 되었습니다. 이를 이용해 특정 종류의 사이트들을 더 빠르게 만들기 위해 “스타터”(몇 가지 기본 설정과 함께 부분적으로 만들어진 사이트)들을 다운로드 할 수 있습니다. 이 튜토리얼에서는 Gatsby 사이트의 기본이 되는 요소들이 포함되어 있는 “Hello World” 스타터를 이용할 것입니다.

1.  터미널을 실행하세요.
2.  `gatsby new hello-world https://github.com/gatsbyjs/gatsby-starter-hello-world`를 실행하세요. (\_노트: 여러분의 다운로드 속도에 따라 소요 시간이 상이할 수 있습니다. 설명을 위해서 녹화된 아래의 gif 역시 실제 설치 시간보다 짧게 녹화된 것입니다_).
3.  `cd hello-world`를 실행하세요.
4.  `gatsby develop`를 실행하세요.

<video controls="controls" autoplay="true" loop="true">
  <source type="video/mp4" src="./03-create-site.mp4" />
  <p>죄송합니다, 브라우저에서 이 영상을 지원하지 않습니다.</p>
</video>

금방 무슨 일이 일어난 것일까요?

```shell
gatsby new hello-world https://github.com/gatsbyjs/gatsby-starter-hello-world
```

- `new` 는 새 Gatsby 프로젝트를 만들기 위한 커맨드입니다.
- 여기서 `hello-world`는 임의의 제목입니다 — 아무거나 쓰셔도 상관없습니다. CLI는 “hello-world”라는 폴더 안에 여러분의 새 사이트의 코드들을 넣을 것입니다.
- 마지막 GitHub 주소 부분은 여러분이 설치에 사용하고 있는 스타터의 특정 주소를 가리키고 있습니다.

```shell
cd hello-world
```

- 이것은 '지금 디렉토리의 하위 폴더인 “hello-world” 폴더로 이동 (`cd`) 하겠다는 뜻'입니다. 여러분들의 사이트에 대한 커맨드를 실행할 때마다, 그 사이트가 설치되어 있는 곳에서 실행해야 합니다 (무슨 말이냐면, 터미널에서 현재 위치가 여러분의 사이트들의 코드가 있는 디렉토리어야 한다는 뜻입니다).

```shell
gatsby develop
```

- 이 커맨드는 개발용 서버를 실행합니다. 여러분들은 로컬, 즉 개발 환경에서 여러분의 새 사이트를 보실 수 있습니다. (인터넷에 배포된 것이 아닌, 여러분의 컴퓨터에서만 볼 수 있습니다).

### 로컬 환경에서 여러분의 사이트 보기

브라우저의 새 탭을 열고 [**http://localhost:8000**](http://localhost:8000/)로 접속하시기 바랍니다.

![홈페이지 확인하기](04-home-page.png)

축하합니다! 여러분의 첫번째 Gatsby 사이트 개발을 드디어 시작했습니다! 🎉

개발용 서버가 켜져 있는 동안 로컬 환경에서 [**_http://localhost:8000_**](http://localhost:8000/)를 통해 사이트에 방문할 수 있습니다. 이는 `gatsby develop` 커맨드에 의해 실행되어 작동 중인 프로세스 입니다. 프로세스를 종료하기 위해선 (또는 “개발용 서버를 중단”하기 위해선), 터미널로 돌아가서 “control” 키를 누른 상태로, “c”를 누르세요. (ctrl-c). 다시 시작하기 위해선 `gatsby develop`를 실행하면 됩니다.

**참고:** 만일 `vagrant`와 같은 가상환경을 이용하고 있고 여러분의 로컬 IP 주소에서 접속하고 싶다면, `gatsby develop -- --host=0.0.0.0`를 실행하세요. 이 방법을 통해 개발용 서버는 'localhost'와 여러분의 로컬 IP 주소 양쪽 모두에 전달됩니다.

## 코드 에디터 설치하기

코드 에디터는 프로그래밍 소스코드를 수정하기 위한 용도로 만들어진 프로그램입니다. 다양하고 좋은 기능들을 갖춘 에디터들이 있습니다.

### VS Code 다운로드 하기

Gatsby 문서에는 VS Code에서 찍힌 스크린샷이 간간히 포함되어 있습니다. 그러므로 아직 딱히 선호하는 코드 에디터가 없으시다면 VS Code를 사용하시면 여러분의 스크린도 튜토리얼과 문서에 있는 화면과 똑같이 나올 겁니다. 만일 VS Code를 사용하기로 결정하셨으면, [VS Code 사이트](https://code.visualstudio.com/#alt-downloads)에 방문해 여러분의 운영체제에 맞는 버전을 다운로드 해주세요.

### Prettier 플러그인 설치하기

[Prettier](https://github.com/prettier/prettier)는 여러분의 코드 포맷을 도와주고 에러를 피할 수 있게 해주는 좋은 툴로, 사용하기를 추천합니다.

[Prettier VS Code plugin](https://github.com/prettier/prettier-vscode)를 통해 에디터에서 바로 사용할 수 있습니다:

1. VS Code의 플러그인 창을 열어주세요 (보기 => 확장).
2. "Prettier - Code formatter"를 검색해주세요.
3. "Install"을 눌러주세요. (설치 이후 플러그인을 적용하기 위해 VS Code를 재시작해야한다는 확인 알림이 뜰 것입니다. 최신 버전의 VS Code에서는 다운로드 이후 자동으로 플러그인을 적용할 것입니다.)

> 💡 만일 VS Code를 사용하지 않으신다면, Prettier 문서의 [설치 가이드](https://prettier.io/docs/en/install.html) 또는 [다른 에디터에 통합하기](https://prettier.io/docs/en/editors.html)를 참고해주세요.

## ➡️ 이제 무얼 할까요?

정리하자면, 이 파트에서 여러분은 다음을 진행하셨습니다:

- 커맨드라인에 관해 배우고 사용법을 익혔습니다.
- Node.js와 npm CLI, 버전 컨트롤을 위한 Git, 그리고 Gatsby CLI에 관해 알게되고 설치까지 끝마쳤습니다.
- Gatsby CLI를 이용해 새 Gatsby 사이트를 만들었습니다.
- Gatsby 개발용 서버를 실행하고 로컬 환경에서 여러분의 사이트에 접속해보았습니다.
- 코드 에디터를 다운로드 받았습니다.
- Prettier이라는 코드 포맷터를 설치했습니다.

이제 [**Gatsby 구성요소 알아보기**](/tutorial/part-one/)로 넘어가세요.

## 참고자료

### 핵심 기술들에 대해 알아보기

다음 기술들에 대해 벌써부터 전문적인 지식이 필요하진 않습니다. — 그러니까 혹시 잘 모르겠어도 걱정하지 마세요! 여러분은 이 튜토리얼 시리즈를 거치며 많은 것들을 배우게 될 것입니다. 다음은 여러분들이 Gatsby 사이트를 만들며 사용하게 될 몇 가지 웹 기술들입니다:

- **HTML**: 모든 웹 브라우저가 이해할 수 있는 마크업 언어입니다. HyperText Markup Language의 줄임말입니다. HTML은 여러분에게 제목, 문단 등과 같은 웹 콘텐츠의 보편적인 정보 구조를 짜는데 필요한 것들을 제공합니다.
- **CSS**: 웹 콘텐츠의 모양(글꼴, 색상, 레이아웃 등)을 스타일링하는 데 사용되는 언어로 Cascading Style Sheet의 약자입니다.
- **JavaScript**: 웹을 역동적이고 상호작용이 가능하도록 만들어주는 프로그래밍 언어입니다.
- **React**: 사용자 인터페이스를 구축하기 위한 코드 라이브러리로 (JavaScript로 만들어졌습니다) Gatsby를 이용해 페이지를 만들고 컨텐츠를 설계하는데 있어 사용됩니다.
- **GraphQL**: 웹 사이트로 데이터를 가져올 수 있는 쿼리 언어입니다. Gatsby를 이용해 사이트의 데이터를 관리하기 위해 사용하는 인터페이스입니다.

### 웹사이트란 무엇인가요?

HTML과 CSS 입문 강의를 포함한, 웹사이트가 무엇인지에 대한 소개글로는 “[**첫 웹사이트 만들어보기**](https://learn.shayhowe.com/html-css/building-your-first-web-page/)”를 참고하세요. 웹에 대해 공부하기 좋은 시작점이 될 것입니다. [**HTML**](https://www.codecademy.com/learn/learn-html), [**CSS**](https://www.codecademy.com/learn/learn-css), 그리고 [**JavaScript**](https://www.codecademy.com/learn/introduction-to-javascript)에 관한 강의는 Codecademy에서 볼 수 있습니다. [**React**](https://reactjs.org/tutorial/tutorial.html)와 [**GraphQL**](http://graphql.org/graphql-js/) 또한 각각의 홈페이지에서 튜토리얼을 찾아볼 수 있습니다.

### 커맨드 라인에 대해 더 알아보기

Mac과 Linux 사용자들에게는 [**Codecademy의 커맨드 라인 튜토리얼**](https://www.codecademy.com/courses/learn-the-command-line/lessons/navigation/exercises/your-first-command)이 좋은 입문용 자료가 될 것이고, Windows 사용자들은 [**이 튜토리얼**](https://www.computerhope.com/issues/chusedos.htm)을 참고해주세요. Windows 사용자들에게도 Codecademy 튜토리얼의 첫번째 페이지는 읽어보기 좋은 글입니다. 커맨드 라인을 사용하는 방법뿐 아니라 이것이 무엇인지에 대해서도 잘 설명되어 있습니다.

### npm에 대해 더 알아보기

npm은 JavaScript 패키지 매니저입니다. 패키지는 여러분들이 프로젝트에 포함할 지 선택할 수 있는 코드 모듈입니다. 만일 Node.js를 다운로드하고 설치하셨다면, npm 역시 함께 설치됩니다.

npm은 npm 웹사이트, npm 레지스트리, npm CLI로 이루어져있습니다.

- npm 웹사이트에서 여러분은 npm 레지스트리에서 어떤 JavaScript 패키지가 있는지 볼 수 있습니다.
- npm 레지스트리는 npm에 있는 JavaScript 패키지들에 대한 정보로 이루어진 거대한 데이터베이스입니다.
- 어떤 패키지를 사용할지 정했다면, npm CLI를 이용해 여러분의 프로젝트, 또는 전역(CLI 같은 경우)에 설치 할 수 있습니다. npm CLI는 레지스트리와 직접 상호작용하며 일반적으로 여러분은 npm 웹사이트 또는 npm CLI만을 사용하시게 됩니다.

> 💡 “[**npm이 무엇인가요?**](https://docs.npmjs.com/getting-started/what-is-npm)”에서 npm에 관한 소개를 찾을 수 있습니다.

### Git에 대해 더 알아보기

이 튜토리얼을 완료하기 위해 Git을 알아야 할 필요는 없지만, 매우 유용한 툴임에는 확실합니다. 버전 컨트롤, Git과 GitHub에 대해 자세히 알고 싶으시면 [Git 핸드북](https://guides.github.com/introduction/git-handbook/)을 참고하세요.
