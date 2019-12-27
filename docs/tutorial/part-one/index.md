---
title: Get to Know Gatsby Building Blocks
typora-copy-images-to: ./
disableTableOfContents: true
---

[**지난 섹션**](/tutorial/part-zero/)에서, 필수 소프트웨어를 설치하고 [**“hello world” 스타터**](https://github.com/gatsbyjs/gatsby-starter-hello-world)를 사용하여 처음으로 Gatsby 사이트를 만들어보면서 여러분은 local 개발 환경을 준비해보았습니다. 이제 그 스타터에 의해 만들어진 코드를 더 자세히 살펴봅시다. 

## Gatsby 스타터를 이용하기

[**튜토리얼 파트 zero**](/tutorial/part-zero/)에서, 여러분은 “hello world” 스타터를 기반한 사이트를 다음과 같은 명령어를 사용하여 만들어 보았습니다:

```shell
gatsby new hello-world https://github.com/gatsbyjs/gatsby-starter-hello-world
```

새로운 Gatsby 사이트를 만들 때, 여러분은 다음 명령어 스트럭쳐를 사용해서 존재하는 Gatsby 스타터를 기반한 사이트를 만들 수 있습니다:

```shell
gatsby new [사이트_디렉토리_이름] [스타터_깃허브_리포_URL]
```

만약 마지막의 URL을 적지않는다면, Gatsby는 자동으로 [**default 스타터**](https://github.com/gatsbyjs/gatsby-starter-default)를 이용하여 만듭니다. 이전 튜토리얼 파트 0에서 만든 “Hello World” 사이트를 이번 튜토리얼에서도 사용하겠습니다.

### ✋ 코드 열어보기

여러분의 코드 에디터에서, “Hello World” 사이트를 위해 만들어진 코드를 열어보고 ‘hello-world’ 디렉토리 안에 있는 디렉토리와 파일들의 한번 살펴봅시다. 다음과 같을 것입니다:

![VS Code에서의 Hello World 프로젝트](01-hello-world-vscode.png)

_Note: 다시 말하지만, 비주얼 스튜디오 코드입니다. 다른 에디터를 사용하고 있다면 약간 다르게 보일 것입니다._

이제 홈페이지를 구동하는 코드를 살펴봅시다.

> 💡 이전 섹션에서 `gatsby develop` 이후에 개발 서버를 멈췄다면, 다시 작동 시켜주세요 — hello-world 사이트를 변경할 시간입니다!

## Gatsby 페이지에 익숙해지기

여러분의 코드 에디터에서 `/src` 디렉토리를 열어주세요. 그 안에 `/pages` 디렉토리가 있습니다.

`src/pages/index.js` 파일을 열어주세요. div와 “Hello world!”로 구성된 컴포넌트를 만드는 코드가 있습니다.

### ✋ “Hello World” 홈페이지를 변화시켜보기

1.  “Hello World!”를 “Hello Gatsby!”로 바꾸고 파일을 저장해봅시다. 여러분의 윈도우 창이 나란히 있다면, 여러분이 코드와 컨텐츠를 바꾸고 파일을 저장하면 곧바로 이가 반영되는 것을 볼 수 있을 것입니다.

<video controls="controls" autoplay="true" loop="true">
  <source type="video/mp4" src="./02-demo-hot-reloading.mp4"></source>
  <p>Sorry! Your browser doesn't support this video.</p>
</video>

> 💡 Gatsby는 여러분의 개발 속도를 높이기 위해 **hot reloading**를 사용합니다. 기본적으로, 여러분이 Gatsby 개발 서버를 사용하는 동안, Gatsby 사이트의 파일들은 백그라운드에서 “감시”되어집니다 — 여러분이 파일을 저장하면 바뀐 것들이 곧바로 브라우저에 반영이 됩니다. 개발 서버를 새로 실행하거나 페이지를 새로고침을 할 필요가 없습니다 — 바뀐 것들이 그냥 반영됩니다.

2.  조금 더 눈에 띄게 바꿔봅시다. `src/pages/index.js` 의 코드를 아래 코드로 바꿔봅시다. 글자 색상이 보라색이고 글자 크기도 조금 더 커져있는 것을 볼수 있을 것 입니다.

```jsx:title=src/pages/index.js
import React from "react"

export default () => (
  <div style={{ color: `purple`, fontSize: `72px` }}>Hello Gatsby!</div>
)
```

> 💡 튜토리얼 [**파트 2**](/tutorial/part-two/)에서 Gatsby 상에서의 스타일에 대해서 좀더 자세히 알아볼 것입니다.

3.  글자 크기 스타일을 지우고 “Hello Gatsby!” 글자를 1 레벨 헤더로 바꾸고 헤더 아래에 문장을 추가해봅시다.

```jsx:title=src/pages/index.js
import React from "react"

export default () => (
  {/* highlight-start */}
  <div style={{ color: `purple` }}>
    <h1>Hello Gatsby!</h1>
    <p>What a world.</p>
  {/* highlight-end */}
  </div>
)
```

![hot reloading과 변화](03-more-hot-reloading.png)

4.  이미치 추가하기. (Unsplash에서 랜덤 이미지).

```jsx:title=src/pages/index.js
import React from "react"

export default () => (
  <div style={{ color: `purple` }}>
    <h1>Hello Gatsby!</h1>
    <p>What a world.</p>
    {/* highlight-next-line */}
    <img src="https://source.unsplash.com/random/400x200" alt="" />
  </div>
)
```

![이미지 추가](04-add-image.png)

### 잠시… 자바스크립트 안에서 HTML?

_만약 여러분이 React와 JSX에 이미 익숙하다면, 이 섹션을 건너뛰어도 됩니다._ React 프레임워크를 이전에 사용해보지 않았다면, 자바스크립트 함수에서 HTML이 있는 것에 대해서 의아해할 것입니다. 혹은 첫번째 줄에 있는 `react`를 불러오지만 어디에서도 사용하지 않는지 의아해 할 것입니다. 이 두 요소가 합쳐진 “HTML-in-JS”은 React를 위해 자바스크립트의 문법을 확장한 것으로 JSX라고 불립니다. React에 대한 경험이 없더라도 튜토리얼을 진행할 수 있지만, 궁금해하는 분을 위해 간단히 소개합니다…

`src/pages/index.js` 파일의 원본 내용을 참고합시다:

```jsx:title=src/pages/index.js
import React from "react"

export default () => <div>Hello world!</div>
```

순수한 자바스크립트에서는 아래와 같습니다:

```javascript:title=src/pages/index.js
import React from "react"

export default () => React.createElement("div", null, "Hello world!")
```
이제 `'react'`를 불러오고 사용하는 것을 볼 수 있습니다! 잠깐. 여러분은 JSX를 작성하고 있지, 순수한 HTML과 자바스크립트를 작성하지 않았습니다. 어떻게 브라우저는 그것들을 읽고 있는걸까요? 간단히 답변하자면: 브라우저는 이를 읽지 않습니다. Gatsby 사이트는 도구들과 함께 설치되며, 이것들은 여러분의 소스 코드를 브라우저가 읽을 수 있게 변화시켜주게 셋팅 되어있습니다.

## 컴포넌트 작성하기

여러분이 방금 편집하던 홈페이지는 페이지 컴포넌트를 정의함으로써 만들어졌습니다. “component”는 무엇일까요?

광범위하게 정의된, 컴포넌트는 여러분의 사이트의 빌딩 블록입니다; UI(유저 인터페이스)의 한 부분을 표현하는 코드 그 자체입니다.

Gatsby는 React를 기반으로 합니다. 우리가 **컴포넌트**의 사용법과 정의에 대한 이야기를 할 때, 이는 **React 컴포넌트**를 이야기합니다 — 이는 입력을 허용하고 UI 한 부분을 표현하는 React 엘리먼트를 반환하는 코드(보통 JSX로 작성됨)입니다.

(만약 여러분이 이미 개발자라면) 컴포넌트를 빌드하기 시작할 때에 큰 정신적 변화 중의 하나는, 여러분의 CSS, HTML, 자바스크립트가 긴밀하게 엮여서 종종 한 파일안에서 이들이 존재하는 것입니다.

간단해 보이는 변화지만, 이는 사이트를 만드는 것에 대해 여러분이 어떻게 생각하는지에 대한 깊은 의미를 가지고 있습니다.

커스텀 버튼을 만드는 예제를 봅시다. 이전에, 여러분은 CSS 클래스(아마 `.primary-button`)와 커스텀 스타일을 만들었고 이 스타일을 적용하고 싶을 때 사용했습니다. 예를 들어:
 
```html
<button class="primary-button">Click me</button>
```

컴포넌트 세계에서는, 여러분은 `PrimaryButton` 컴포넌트에 버튼 스타일을 같이 만들며 사이트에서는 다음과 같이 사용합니다:

<!-- prettier-ignore -->
```jsx
<PrimaryButton>Click me</PrimaryButton>
```

컴포넌트는 여러분의 사이트를 구성하는 빌딩 블록이 됩니다. 브라우저가 제공하는, 예를들어 `<button />`, 빌딩 블록에 국한되지않고, 프로젝트의 요구를 우아하게 충족하는 새로운 빌딩 블록을 쉽게 만들수 있습니다.

### ✋ 페이지 컴포넌트 사용하기

`src/pages/*.js`에 정의된 React 컴포넌트는 자동으로 페이지가 됩니다. 한번 해봅시다.

여러분은 “Hello World” 스타터에 포함된 `src/pages/index.js`를 이미 가지고 있습니다. about 페이지를 만들어 봅시다.

1. `src/pages/about.js`에 새로운 파일을 만들어주고, 아래 코드를 복사하고 저장해주세요.

```jsx:title=src/pages/about.js
import React from "react"

export default () => (
  <div style={{ color: `teal` }}>
    <h1>About Gatsby</h1>
    <p>Such wow. Very React.</p>
  </div>
)
```

2.  http://localhost:8000/about/ 로 이동하기

![새로운 about 페이지](05-about-page.png)

React 컴포넌트를 `src/pages/about.js` 파일에 놓는 것만으로도, `/about`로 접근 가능한 페이지가 생성되었습니다.

### ✋ 서브 컴포넌트 사용하기

홈페이지와 about 페이지가 있고 이 두 페이지 다 꽤 커졌고 여러분은 많은 것들을 다시 작성하고 있다고 가정해 봅시다. 여러분은 서브 컴포넌트를 사용하여 UI를 재사용 가능한 조각으로 나눌수 있습니다. 두 페이지 모두 `<h1>` 헤더를 가지고 있으니 — `Header`를 표현하는 컴포넌트를 만듭시다.

1.  `src/components` 디렉토리를 만들고 그 안에 `heaer.js` 파일을 만드세요.
2.  `src/components/header.js` 파일에 아래 코드를 추가하세요.

```jsx:title=src/components/header.js
import React from "react"

export default () => <h1>This is a header.</h1>
```

3.  `about.js` 파일에서 `Header` 컴포넌트를 불러오게 수정하세요. `h1` 마크업을 `<Header />`로 바꾸세요:

```jsx:title=src/pages/about.js
import React from "react"
import Header from "../components/header" // highlight-line

export default () => (
  <div style={{ color: `teal` }}>
    <Header /> {/* highlight-line */}
    <p>Such wow. Very React.</p>
  </div>
)
```

![헤더 컴포넌트 추가하기](06-header-component.png)

브라우저에서, “About Gatsby” 헤더 글이 “This is a header.”로 바뀌었습니다. 하지만 “About” 페이지에서 “This is a header.”라고 보여주고 싶지않습니다. “About Gatsby”라고 보여주고 싶습니다.

4.  `src/components/header.js`로 돌아가서 아래와 같이 바꾸세요:

```jsx:title=src/components/header.js
import React from "react"

export default props => <h1>{props.headerText}</h1> {/* highlight-line */}
```

5.  `src/pages/about.js`로 돌아가서 아래와 같이 바꾸세요:

```jsx:title=src/pages/about.js
import React from "react"
import Header from "../components/header"

export default () => (
  <div style={{ color: `teal` }}>
    <Header headerText="About Gatsby" /> {/* highlight-line */}
    <p>Such wow. Very React.</p>
  </div>
)
```

![헤더에 데이터 전달하기](07-pass-data-header.png)

이제 “About Gatsby” 헤더 글을 다시 보게되었습니다!

### “props”이란?

이전에 여러분은 React 컴포넌트를 UI를 표현하는 재사용 가능한 코드라고 정의했습니다. 이 재사용 가능한 코드를 동적으로 만들기 위해서 여러분은 각기 다른 데이터를 제공할 수 있어야 합니다. “props"이라고 불리는 입력을 통해서 제공할 수 있습니다. Props는 React 컴포넌트로 공급되는 (적절한) 속성입니다.

`about.js`에서 여러분은 `headerText` prop와 그 값 `"About Gatsby"`를 임포트된 `Header` 서브 컴포넌트에 전달했습니다:

```jsx:title=src/pages/about.js
<Header headerText="About Gatsby" />
```

헤더 컴포넌트는 `headerText` prop을 받을 것을 예상합니다 (왜냐면 여러분이 그렇게 되게끔 작성했거든요). 그래서 이렇게 접근할 수 있습니다:

```jsx:title=src/components/header.js
<h1>{props.headerText}</h1>
```

> 💡 JSX 상에서, 여러분은 어떠한 자바스크립트 표현식도 `{}`로 감싸서 포함시킬수 있습니다. 이 방법으로 여러분은 “props” 오브젝트의 `headerText` 특성(또는 “prop!”)에 접근할 수 있습니다.

만약 여러분이 `<Header />` 컴포넌트에 다음과 같이 다른 prop을 전달한다면...

```jsx:title=src/pages/about.js
<Header headerText="About Gatsby" arbitraryPhrase="is arbitrary" />
```

...여러분은 이또한 `arbitraryPhrase` prop에 접근할 수 있을 것입니다: `{props.arbitraryPhrase}`.

6.  여러분의 컴포넌트를 재사용 가능하게 만드는 것을 강조하자면, 추가로 `<Header />` 컴포넌트를 about 페이지에 추가하고, `src/pages/about.js` 파일에 다음의 코드를 추가한 후 저장하세요.

```jsx:title=src/pages/about.js
import React from "react"
import Header from "../components/header"

export default () => (
  <div style={{ color: `teal` }}>
    <Header headerText="About Gatsby" />
    <Header headerText="It's pretty cool" /> {/* highlight-line */}
    <p>Such wow. Very React.</p>
  </div>
)
```

![재사용성을 보여주기 위해 헤더 복제하기](08-duplicate-header.png)

그게 다입니다; 두번째 헤더 — 어떤 코드도 재작성하지 않았고 — props를 사용해서 다른 데이터를 전달했습니다.

### 레이아웃 컴포넌트 사용하기

레이아웃 컴포넌트는 여러 페이지에서 공유할 섹션을 위한 것입니다. 예를들어, Gatsby 사이트는 보통 공유하는 헤더와 풋터가 있는 레이아웃 컴포넌트 가지고 있습니다. 레이아웃에 추가하는 다른 일반적인 것들은 사이드바와 네비게이션 메뉴가 있습니다.

레이아웃 컴포넌트는 [**part three**](/tutorial/part-three/)에서 볼 수 있습니다.

## 페이지 간의 링크

여러분은 페이지와 페이지 간에 링크를 해주고 싶은 때가 자주 있습니다  — Gatsby 사이트에서의 라우팅에 대해서 봅시다.

### ✋ `<Link />` 컴포넌트 사용하기

1.  index 페이지(`src/pages/index.js`)의 컴포넌트를 열고, Gatsby로부터 `<Link />` 컴포넌트를 불러오고, `<Link />` 컴포넌트를 헤더 위에 추가하고,  `to` 속성에 `"/contact/"` 값을 줍니다:

```jsx:title=src/pages/index.js
import React from "react"
import { Link } from "gatsby" // highlight-line
import Header from "../components/header"

export default () => (
  <div style={{ color: `purple` }}>
    <Link to="/contact/">Contact</Link> {/* highlight-line */}
    <Header headerText="Hello Gatsby!" />
    <p>What a world.</p>
    <img src="https://source.unsplash.com/random/400x200" alt="" />
  </div>
)
```

홈페이지에 있는 "Contact" 링크를 클릭하면, 여러분은 다음과 같은 것들을 볼 것입니다...

![Gatsby dev 404 페이지](09-dev-404.png)

...Gatsby dev 404 페이지. 왜? 아직 존재하지 않은 페이지에 링크를 걸려고 하고 있기 때문이죠.

2.  이제 여러분은 `src/pages/contact.js`에 위치한 "Contact" 페이지를 위한 페이지 컴포넌트를 만들고 홈페이지로 돌아갈수 있는 링크를 추가해줘야 합니다:

```jsx:title=src/pages/contact.js
import React from "react"
import { Link } from "gatsby"
import Header from "../components/header"

export default () => (
  <div style={{ color: `teal` }}>
    <Link to="/">Home</Link>
    <Header headerText="Contact" />
    <p>Send us a message!</p>
  </div>
)
```

이 파일을 저장한 후에, 여러분은 contact 페이지가 보여야하고 이 페이지와 홈페이지간의 링크도 가능해야 합니다.

<video controls="controls" loop="true">
  <source type="video/mp4" src="./10-linking-between-pages.mp4"></source>
  <p>Sorry! You browser doesn't support this video.</p>
</video>

Gatsby `<Link />` 컴포넌트는 여러분의 사이트안에 있는 페이지간의 링크를 위한 것입니다. 여러분의 Gatsby 사이트에 의해 관리되는 링크가 아닌 외부 링크는 일반적인 HTML `<a>` 태그를 사용하세요.

## Gatsby 사이트 배포하기

Gatsby.js는 _모던 사이트 생성기_ 이고, 이는 배포를 위한 복잡한 데이터베이스 또는 서버 설정이 없다는 걸 의미합니다. 대신에 Gatsby `build` 명령어가 정적 사이트 호스팅 서비스로 배포할 수 있는 정적인 HTML과 자바스크립트 파일들의 디렉토리를 만들어줍니다.

여러분의 첫번째 Gatsby 웹사이트를 배포할 때에 [Surge](http://surge.sh/)를 시도해보세요. Surge는 "정적인 사이트 호스트" 중의 하나로 Gatsby 사이트도 배포할 수 있습니다.

만약 이전에 Surge 설치와 셋업을 하지 않았다면, 새로운 터미널 윈도우를 열고 Surge의 커맨드라인 툴을 설치하세요:

```shell
npm install --global surge

# 이후 (무료) 계정을 만드세요
surge login
```

그 다음, 여러분의 사이트의 루트에서 다음에 나오는 명령어를 실행하여 사이트를 빌드하세요 (팁: 꼭 사이트의 루트에서 이 명령어를 실행하세요. 이번 경우에는 hello-world 폴더이며 `gatsby develop` 를 실행했던 곳에서 새로운 탭을 열고 확인할 수 있습니다):

```shell
gatsby build
```

빌드는 15-30초 가량 소요됩니다. 빌드가 완료된 후, `gatsby build` 명령어가 배포를 위해 방금 준비한 파일들을 살펴보는 것도 흥미롭습니다.

여러분의 사이트의 루트에서 다음에 나오는 터미널 명령어를 입력하여, `public` 폴더에 위치한 생성된 파일들을 살펴보세요.

```shell
ls public
```

그리고 드디어 surge.sh에 생성된 파일들을 옮겨서 사이트를 배포합니다.

```shell
surge public/
```

작동 중인 것이 멈추면, 터미널에서 다음과 같은 것들을 보게될 것입니다:

![Surge에 Gatsby사이트 배포하기 스크린샷](surge-deployment.png)


아래 줄에 위치한(위 경우 `lowly-pain.surge.sh`) 웹 주소를 열어보면 새로 배포한 사이트를 볼 수 있을 것입니다! 수고하셨습니다!

## ➡️ 다음에 할 것?

이번 섹션에서 여러분이 한 것들:

- Gatsby 스타터와, 이를 사용한 새로운 프로젝트를 만드는 법에 대한 학습
- JSX 문법에 대한 학습
- 컴포넌트에 대한 학습
- Gatsby 페이지 컴포넌트와 서브 컴포넌트에 대한 학습
- React “props”와 React 컴포넌트의 재사용에 대한 학습

이제 [**사이트에 스타일 추가하기**](/tutorial/part-two/)로 이동합시다!
