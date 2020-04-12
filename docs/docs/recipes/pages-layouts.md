---
title: "Recipes: Pages and Layouts"
tableOfContentsDepth: 1
---

Add pages to your Gatsby site, and use layouts to manage common page elements.

## Project structure

Gatsby 프로젝트로 들어가면, 아래와 같은 폴더와 파일들을 볼 수 있습니다:

```
|-- /.cache
|-- /plugins
|-- /public
|-- /src
    |-- /pages
    |-- /templates
    |-- html.js
|-- /static
|-- gatsby-config.js
|-- gatsby-node.js
|-- gatsby-ssr.js
|-- gatsby-browser.js
```

몇 가지 중요한 파일들에 대한 설명입니다:

- `gatsby-config.js` — 프로젝트 제목, 설명, 플러그들을 위한 메타데이터와 Gatsby 사이트의 옵션을 구성합니다.
- `gatsby-node.js` — 빌드 과정에 영향을 주는 Gatsby의 Node.js API 들의 기본 설정을 커스터마이징하고 확장합니다.
- `gatsby-node.js` — implement Gatsby’s Node.js APIs to customize and extend default settings affecting the build process
- `gatsby-browser.js` — 브라우저에 영향을 주는 Gatsby의 browser API 들의 기본 설정을 커스터마이징하고 확장합니다.
- `gatsby-ssr.js` — 서버 사이드 렌더링에 영향을 주는 Gatsby의 server-side rendering API 들의 기본 설정을 커스터마이징하고 확장합니다.

### 추가 정보

- 모든 일반 폴더들과 파일들에 대해 알아보려면, [Gatsby의 프로젝트 구조](/docs/gatsby-project-structure/) 문서를 읽으세요.
- 일반 명령어들을 알아보려면, [Gatsby CLI 문서](/docs/gatsby-cli)를 확인하세요.
- 한눈에 보기 좋은 다운로드 가능한 [Gatsby 치트 시트](/docs/cheat-sheet/)를 확인하세요.

## 자동으로 페이지 생성하기

Gatsby 코어는 `src/pages` 내부의 React 컴포넌트들을 URL이 있는 페이지들로 자동으로 바꿉니다.
예를 들어, `src/pages/index.js`와 `src/pages/about.js` 의 컴포넌트들은 파일 이름들로부터 사이트의 index 페이지와 `/about` 페이지를 자동으로 생성합니다.

### 사전준비

- A [Gatsby site](/docs/quick-start)
- The [Gatsby CLI](/docs/gatsby-cli) installed

### 수행 절차

1. 프로젝트에 `src/pages` 디렉토리가 없다면 생성해 주세요.
2. pages 디렉토리에 컴포넌트 파일을 추가해 주세요:

```jsx:title=src/pages/about.js
import React from "react"

const AboutPage = () => (
  <main>
    <h1>About the Author</h1>
    <p>Welcome to my Gatsby site.</p>
  </main>
)

export default AboutPage
```

3. 개발 서버를 시작하기 위해 `gatsby develop` 명령을 실행하세요.
4. 브라우저로 새 페이지에 방문하세요: `http://localhost:8000/about`

### 추가 정보

- [페이지 생성 및 수정하기](/docs/creating-and-modifying-pages/)

## 페이지 간 연결하기

Gatsby에서 라우팅은 `<Link />` 컴포넌트에 의존합니다.

### 사전준비

- `index.js` 와 `contact.js` 두 페이지 컴포넌트가 있는 Gatsby 사이트
- Gatsby `<Link />` 컴포넌트
- `gatsby develop`을 실행하기 위한 [Gatsby CLI](/docs/gatsby-cli/)

### 수행 절차

1. index 페이지 컴포넌트(`src/pages/index.js`)를 열고, Gatsby로부터 `<Link />` 컴포넌트를 임포트 한 다음, 헤더 위에 `<Link />` 컴포넌트를 추가하고, `to` 프로퍼티에 `"/contact/"` 라는 경로명을 지정해줍니다:

```jsx:title=src/pages/index.js
import React from "react"
import { Link } from "gatsby"

export default () => (
  <div style={{ color: `purple` }}>
    <Link to="/contact/">Contact</Link>
    <p>What a world.</p>
  </div>
)
```

2. Run `gatsby develop` and navigate to the index page. You should have a link that takes you to the contact page when clicked!
2. `gatsby develop`을 실행하고 index 페이지로 이동하세요. 클릭시 contact 페이지로 연결되는 링크가 있어야 합니다!

> **주의**: Gatsby의 `<Link />`는 [`@reach/router`의 Link 컴포넌트](https://reach.tech/router/api/Link)의 래퍼(wrapper) 컴포넌트 입니다. Gatsby의 `<Link />` 컴포넌트에 대한 자세한 정보는 [`<Link />` API 레퍼런스](/docs/gatsby-link/) 문서를 참고하세요.

## 레이아웃 컴포넌트 생성하기

React layout 컴포넌트로 페이지들을 감싸는 것이 일반적이므로 여러 페이지 간 마크업, 스타일 및 기능을 공유 할 수 있습니다.

### 사전준비

- [Gatsby 사이트](/docs/quick-start/)

### 수행 절차

1. `src/components` 안에 자식 컴포넌트를 props로 전달받는 layout 컴포넌트를 생성합니다:

```jsx:title=src/components/layout.js
import React from "react"

export default ({ children }) => (
  <div style={{ margin: `0 auto`, maxWidth: 650, padding: `0 1rem` }}>
    {children}
  </div>
)
```

2. 페이지에서 layout 컴포넌트를 임포트하여 사용합니다:

```jsx:title=src/pages/index.js
import React from "react"
import { Link } from "gatsby"
import Layout from "../components/layout"

export default () => (
  <Layout>
    <Link to="/contact/">Contact</Link>
    <p>What a world.</p>
  </Layout>
)
```

### 추가 정보

- [튜토리얼 파트3](/tutorial/part-three/#your-first-layout-component)에서 layout 컴포넌트를 생성하세요
- [Layout 컴포넌트](/docs/layout-components/) 스타일링

## createPage를 사용하여 프로그래밍으로 페이지 생성하기

Gatsby가 제공하는 헬퍼 메소드를 사용하여 `gatsby-node.js` 파일에 페이지를 생성하는 코드를 작성 할 수 있습니다.

### 사전준비

- [Gatsby 사이트](/docs/quick-start)
- `gatsby-node.js` 파일

### 수행 절차

1. `gatsby-node.js` 파일 안에, `createPages` 추가하고 내보냅니다(export)

```javascript:title=gatsby-node.js
// highlight-start
exports.createPages = ({ actions }) => {
  // ...
}
// highlight-end
```

2. actions로부터 `createPage` 액션을 뽑아내어 자체적으로 호출하고, 데이터를 추가하거나 가져올 수 있습니다

```javascript:title=gatsby-node.js
exports.createPages = ({ actions }) => {
  // highlight-start
  const { createPage } = actions
  // pull in or use whatever data
  const dogData = [
    {
      name: "Fido",
      breed: "Sheltie",
    },
    {
      name: "Sparky",
      breed: "Corgi",
    },
  ]
  // highlight-end
}
```

3. `gatsby-node.js`의 데이터로 반복하고 매 반복 단계에서 `createPage`에 경로, 템플릿 및 컨텍스트 (props안의 pageContext로 전달 될 데이터)를 제공하세요

```javascript:title=gatsby-node.js
exports.createPages = ({ actions }) => {
  const { createPage } = actions

  const dogData = [
    {
      name: "Fido",
      breed: "Sheltie",
    },
    {
      name: "Sparky",
      breed: "Corgi",
    },
  ]
  // highlight-start
  dogData.forEach(dog => {
    createPage({
      path: `/${dog.name}`,
      component: require.resolve(`./src/templates/dog-template.js`),
      context: { dog },
    })
  })
  // highlight-end
}
```

4. `createPage`에 제공된 페이지의 템플릿으로 사용할 React 컴포넌트를 생성하세요

```jsx:title=src/templates/dog-template.js
import React from "react"

export default ({ pageContext: { dog } }) => (
  <section>
    {dog.name} - {dog.breed}
  </section>
)
```

5. `gatsby develop`을 실행하고 생성된 페이지 중 하나(예: `http://localhost:8000/Fido`)로 이동하여 페이지에 표시되는 데이터를 확인하세요

### 추가 정보

- [데이터로부터 페이지 생성하는 프로그램 작성](/tutorial/part-seven/)에 대한 튜토리얼 섹션
- [GraphQL 없이 Gatsby 사용하기](/docs/using-gatsby-without-graphql/) 레퍼런스 가이드
- 이 레시피의 [예제 저장소](https://github.com/gatsbyjs/gatsby/tree/master/examples/recipe-createPage)
