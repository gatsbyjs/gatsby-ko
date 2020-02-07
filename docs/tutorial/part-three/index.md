---
title: 중첩된 레이아웃 컴포넌트 만들기
typora-copy-images-to: ./
disableTableOfContents: true
---

파트 3에 오신 것을 환영합니다!

## 이번 튜토리얼에서 다룰 것

이번 파트에서 여러분은 Gatsby 플러그인과 "레이아웃" 컴포넌트를 만드는 방법을 배울 것입니다.

Gatsby 플러그인은 Gatsby 사이트에 기능을 추가하는 데 도움이 되는 JavaScript 패키지입니다. Gatsby는 확장 가능하도록 설계되었습니다. 즉, 플러그인이 Gatsby의 모든 기능을 확장하고 수정할 수 있습니다.

레이아웃 구성 요소는 여러 페이지에서 공유할 사이트 섹션을 위한 것입니다. 예를 들어 일반적으로 사이트에는 공유 헤더와 풋터가 있는 레이아웃 구성 요소가 있습니다. 레이아웃에 추가할 다른 일반적인 것들은 사이드바와 네비게이션 메뉴가 있습니다. 예를 들어 이 페이지의 위에 있는 헤더는 gatsbyjs.org의 레이아웃 컴포넌트의 일부입니다.

이제 파트 3를 본격적으로 시작합시다.

## 플러그인 사용하기

여러분은 아마 플러그인에 대해서 익히 들어봤을 것입니다. 많은 소프트웨어 시스템이 커스텀 플러그인을 추가해서 새로운 기능을 추가하거나 소프트웨어 코어의 작동 방식을 변경할 수 있습니다. Gatsby 플러그인도 동일한 방식으로 작동합니다.

여러분과 같은 커뮤니티 멤버들도 다른 사람들이 Gatsby 사이트를 만들때 사용가능한 플러그인(적은 분량의 자바스크립트 코드)에 기여할 수 있습니다.


> 이미 수백개의 플러그인이 있어요! Gatsby [플러그인 라이브러리](/plugins/)를 둘러보세요.

우리의 플러그인에 대한 목표는 플러그인의 설치 및 사용하기 쉽게 만드는 것입니다. 아마도 여러분이 만들 대부분의 Gatsby 사이트는 플러그인이 사용될 것입니다. 남은 튜토리얼을 진행하면서 여러분은 플러그인의 설치 및 사용 방법을 여러번 연습하게 될 것입니다.

이번 플러그인 사용에 대한 초기 소개를 위해 Typography.js용 Gatsby 플러그인을 설치하고 구현하겠습니다.

[Typography.js](https://kyleamathews.github.io/typography.js/)는 사이트의 타이포그래피의 전역 베이스 스타일을 생성하는 자바스크립트 라이브러리 입니다. 이 라이브러리는 Gatsby 사이트에서 간편하게 사용할 수 있는 [대응하는 Gatsby 플러그인](/packages/gatsby-plugin-typography/)이 있습니다.

### ✋ 새로운 Gatsby 사이트를 만들기

[파트 2](/tutorial/part-two/)에서 언급했듯이, 이제 이전 튜토리얼에서 사용한 터미널과 파일들을 끄고 여러분의 데스크탑 환경을 깨끗하게 만들어주는게 좋습니다. 그리고 새로운 터미널을 켜고 다음에 나열된 명령어를 이용해서 `tutorial-part-three` 디렉토리에 새로운 Gatsby 사이트를 만들어주세요:

```shell
gatsby new tutorial-part-three https://github.com/gatsbyjs/gatsby-starter-hello-world
cd tutorial-part-three
```

### ✋ `gatsby-plugin-typography`의 설치 및 설정

플러그인을 사용하기 위한 두 단계가 있습니다: 설치와 설정.

1. `gatsby-plugin-typography` NPM 패키지를 설치하기.

```shell
npm install --save gatsby-plugin-typography react-typography typography typography-theme-fairy-gates
```

> Note: Typography.js는 몇가지 추가적인 패키지가 필요하며, 이는 설명서에 포함되어 있습니다. 이러한 필수적인 것들은 각 플러그인의 "install" 지시에 나열 되어있습니다.
 
2. 프로젝트 루트에 위치한 `gatsby-config.js`파일을 다음과 같이 변경하세요:

```javascript:title=gatsby-config.js
module.exports = {
  plugins: [
    {
      resolve: `gatsby-plugin-typography`,
      options: {
        pathToConfigModule: `src/utils/typography`,
      },
    },
  ],
}
```

`gatsby-config.js`는 특별한 파일로 Gatsby가 자동으로 인식하는 파일입니다. 플러그인을 추가하고 다른 사이트의 설정을 하는 파일입니다.

> 만약 더 자세한 내용이 알고 싶으면 [doc에 있는 gatsby-config.js 문서](/docs/gatsby-config/)를 보세요.

3. Typography.js는 설정 파일이 필요합니다. `src` 디렉토리 안에 `utils` 디렉토리를 만들어주세요. 그리고 `utils` 안에 `typography.js` 파일을 만들어서 아래 내용을 복사해주세요:

```javascript:title=src/utils/typography.js
import Typography from "typography"
import fairyGateTheme from "typography-theme-fairy-gates"

const typography = new Typography(fairyGateTheme)

export const { scale, rhythm, options } = typography
export default typography
```

4. 개발 서버를 시작해주세요.

```shell
gatsby develop
```

사이트가 불러온 후, 크롬 개발자 툴을 사용해서 HTML을 살펴보면, 타이포그래피 플러그인이 `<head>` 엘리먼트 안에 `<style>` 엘리먼트와 CSS를 추가한 것을 볼 수 있습니다:

![typography-styles](typography-styles.png)

### ✋ 컨텐츠 만들기와 스타일 바꾸기

`src/pages/index.js`에 아래 내용을 붙여넣으면 Typography.js에 의해 만들어진 CSS의 영향을 더 잘 확인할 수 있습니다.

```jsx:title=src/pages/index.js
import React from "react"

export default () => (
  <div>
    <h1>Hi! I'm building a fake Gatsby site as part of a tutorial!</h1>
    <p>
      What do I like to do? Lots of course but definitely enjoy building
      websites.
    </p>
  </div>
)
```

이제 여러분의 사이트는 다음과 같을 것입니다:

![no-layout](no-layout.png)

이제 간단히 개선해봅시다. 많은 사이트들이 페이지 가운데에 글자가 중앙 정렬된 하나의 컬럼을 가지고 있는 것을 볼 수 있습니다. 이를 만들기 위해, `src/pages/index.js`에 있는 `<div>`에 다음 스타일을 추가하세요.

```jsx:title=src/pages/index.js
import React from "react"

export default () => (
  // highlight-next-line
  <div style={{ margin: `3rem auto`, maxWidth: 600 }}>
    <h1>Hi! I'm building a fake Gatsby site as part of a tutorial!</h1>
    <p>
      What do I like to do? Lots of course but definitely enjoy building
      websites.
    </p>
  </div>
)
```

![with-layout2](with-layout2.png)

잘했습니다. 여러분의 첫번째 Gatsby 플러그인의 설치와 설정을 마쳤습니다!

## 레이아웃 컴포넌트 만들기

이제 레이아웃 컴포넌트에 대해서 배워봅시다. 이를 위해, 프로젝트에 새로운 페이지를 몇개 추가해볼거에요: about 페이지와 contact 페이지.

```jsx:title=src/pages/about.js
import React from "react"

export default () => (
  <div>
    <h1>About me</h1>
    <p>I’m good enough, I’m smart enough, and gosh darn it, people like me!</p>
  </div>
)
```

```jsx:title=src/pages/contact.js
import React from "react"

export default () => (
  <div>
    <h1>I'd love to talk! Email me at the address below</h1>
    <p>
      <a href="mailto:me@example.com">me@example.com</a>
    </p>
  </div>
)
```

이제 about 페이지가 어떤지 한번 봅시다:

![about-uncentered](about-uncentered.png)

흠. 새로 추가한 두페이지의 컨텐츠도 index 페이지와 같이 가운데 정렬 되어있다면 더 보기 좋을것 같네요. 그리고 전역 네비게이션 같은 것이 있다면 방문자가 다른 페이지들을 방문하기 더 편할 것 같습니다.

여러분의 첫번째 레이아웃 컴포넌트를 만들어서 이러한 변경 사항들을 해결합시다.

### ✋ 첫번째 레이아웃 컴포넌트 만들기

1. `src/components`에 디렉토티를 추가하세요.

2. `src/components/layout.js`에 가장 기본이 되는 레이아웃 컴포넌트를 만들어주세요:

```jsx:title=src/components/layout.js
import React from "react"

export default ({ children }) => (
  <div style={{ margin: `3rem auto`, maxWidth: 650, padding: `0 1rem` }}>
    {children}
  </div>
)
```

3. `src/pages/index.js` 페이지 컴포넌트 안에 새로 만든 레이아웃 컴포넌트를 불러오세요:

```jsx:title=src/pages/index.js
import React from "react"
import Layout from "../components/layout" // highlight-line

export default () => (
  <Layout> {/* highlight-line */}
    <h1>Hi! I'm building a fake Gatsby site as part of a tutorial!</h1>
    <p>
      What do I like to do? Lots of course but definitely enjoy building
      websites.
    </p>
  </Layout> {/* highlight-line */}
)
```

![with-layout2](with-layout2.png)

좋아요, 레이아웃이 작동하네요! index 페이지의 컨텐츠는 가운데 정렬 되어있습니다.

하지만 `/about/` 또는 `/contact/`에 가보면 아직 페이지의 컨텐츠가 가운데 정렬 되어 있지않습니다.

4. `about.js` and `contact.js`에 레이아웃 컴포넌트를 불러오세요. (방금 전에 `index.js`에 했던 것을 하시면 됩니다.)

공유하는 레이아웃 컴포넌트로 인해 세 페이지의 컨텐츠가 전부 다 가운데 정렬되어있습니다!

### ✋ 사이트 타이틀 추가하기

1. 여러분의 레이아웃 컴포넌트에 다음의 내용을 추가하세요:

```jsx:title=src/components/layout.js
import React from "react"

export default ({ children }) => (
  <div style={{ margin: `3rem auto`, maxWidth: 650, padding: `0 1rem` }}>
    <h3>MySweetSite</h3> {/* highlight-line */}
    {children}
  </div>
)
```

만약 세 페이지 중의 하나라도(예를들어 `/about` 페이지) 가보면, 같은 타이틀이 추가되어있는 것을 볼 수 있습니다. 

![with-title](with-title.png)

### ✋ 페이지 간에 네비게이션 링크 추가하기

1. 레이아웃 컴포넌트 파일에 다음의 내용을 복사해 넣으세요:

```jsx:title=src/components/layout.js
import React from "react"
// highlight-start
import { Link } from "gatsby"

const ListLink = props => (
  <li style={{ display: `inline-block`, marginRight: `1rem` }}>
    <Link to={props.to}>{props.children}</Link>
  </li>
)
// highlight-end

export default ({ children }) => (
  <div style={{ margin: `3rem auto`, maxWidth: 650, padding: `0 1rem` }}>
    {/* highlight-start */}
    <header style={{ marginBottom: `1.5rem` }}>
      <Link to="/" style={{ textShadow: `none`, backgroundImage: `none` }}>
        <h3 style={{ display: `inline` }}>MySweetSite</h3>
      </Link>
      <ul style={{ listStyle: `none`, float: `right` }}>
        <ListLink to="/">Home</ListLink>
        <ListLink to="/about/">About</ListLink>
        <ListLink to="/contact/">Contact</ListLink>
      </ul>
    </header>
    {/* highlight-end */}
    {children}
  </div>
)
```

![with-navigation2](with-navigation2.png)

짜잔! 기본 전역 네비게이션이 있는 세 페이지로 된 사이트가 만들어졌습니다.

_Challenge:_ 새로 익힌 "레이아웃 컴포넌트"를 사용해서, 헤더, 풋터, 전역 네비게이션, 사이드바 등을 여러분의 Gatsby 사이트에 추가해보세요!

## 다음에 할 것은?

[튜토리알 파트 4](/tutorial/part-four/)에서는 Gatsby 데이터 레이어와 프로그래밍 방식으로 페이지를 만드는 방법을 배워볼 것입니다
