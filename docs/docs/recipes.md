---
title: 레시피
tableOfContentsDepth: 2
---

<!-- Basic template for a Gatsby recipe:

## Task to accomplish.
1-2 sentences about it. The more concise and focused, the better!

### 사전 준비
- System/version requirements
- Everything necessary to set up the task
- Including setting up accounts at other sites, like Netlify
- See [docs templates](/docs/docs-templates/) for formatting tips

### 지시 사항
Step-by-step directions. Each step should be repeatable and to-the-point. Anything not critical to the task should be omitted.

#### Live example (optional)
A live example may not be possible depending on the nature of the recipe, in which case it is fine to omit.

### 추가 정보
- Tutorials
- Docs pages
- Plugin READMEs
- etc.

See [docs templates](/docs/docs-templates/) in the contributing docs for more help.
-->

[튜토리얼](/tutorial/)과 [문서](/docs) 사이의 무언가를 원하시나요? 여기 Gatsby 스타일의 무언가를 만들 수 있도록 가이드 해주는 레시피가 있습니다.

## 1. Pages 와 Layouts

### 프로젝트 구조

Gatsby 프로젝트에서, 아래와 같은 폴더와 파일들을 볼 수 있을 겁니다.

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

몇가지 중요한 파일들과 그 설명입니다:

- `gatsby-config.js` — 프로젝트 제목, 설명, 플러그들을 위한 메타데이터와 Gatsby 사이트의 옵션을 구성합니다.
- `gatsby-node.js` — 빌드 과정에 영향을 주는 Gatsby의 Node.js API 들의 기본 설정을 커스터마이징하고 확장합니다.
- `gatsby-node.js` — implement Gatsby’s Node.js APIs to customize and extend default settings affecting the build process
- `gatsby-browser.js` — 브라우저에 영향을 주는 Gatsby의 browser API 들의 기본 설정을 커스터마이징하고 확장합니다.
- `gatsby-ssr.js` — 서버 사이드 렌더링에 영향을 주는 Gatsby의 server-side rendering API 들의 기본 설정을 커스터마이징하고 확장합니다.

#### 추가 정보

- For a tour of all the common folders and files, read the docs on [Gatsby's Project Structure](/docs/gatsby-project-structure/)
- 일반 폴더들과 파일들에 대해 알아보려면, [Gatsby의 프로젝트 구조](/docs/gatsby-project-structure/) 문서를 읽으세요.
- For common commands, check out the [Gatsby CLI docs](/docs/gatsby-cli)
- 일반 명령어들을 알아보려면, [Gatsby CLI 문서](/docs/gatsby-cli)를 확인하세요.
- Check out the [Gatsby Cheat Sheet](/docs/cheat-sheet/) for downloadable info at a glance
- 한눈에 보기 좋은 다운로드 가능한 [Gatsby 치트 시트](/docs/cheat-sheet/)를 확인하세요.

### 페이지들을 자동으로 생성하기

Gatsby 코어는 `src/pages` 내부의 React 컴포넌트들을 URL이 있는 페이지들로 자동으로 바꿉니다.
예를 들어, `src/pages/index.js`와 `src/pages/about.js` 의 컴포넌트들은 파일 이름들로부터 사이트의 index 페이지와 `/about` 페이지를 자동으로 생성합니다.

#### 사전준비

- [Gatsby site](/docs/quick-start) 프로젝트
- [Gatsby CLI](/docs/gatsby-cli) 의 설치상태

#### 지시사항

1. 프로젝트에 `src/pages` 디렉토리가 없다면 생성해 주세요.
2. pages 디렉토리에 컴포넌토 파일을 추가해 주세요:

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

3. 개발 서버를 실행시키기 위해 `gatsby develop` 명령을 실행하세요.
4. 브라우저로 새 페이지에 방문하세요: `http://localhost:8000/about`

#### 추가 정보

- [페이지 생성 및 수정](/docs/creating-and-modifying-pages/)

### 페이지 간 연결하기

Gatsby 에서 라우팅은 `<Link />` 컴포넌트에 의존합니다.

#### 사전 준비

- `index.js` 와 `contact.js` 라는 두 페이지 컴포넌트가 있는 Gatsby 사이트
- Gatsby `<Link />` 컴포넌트
- `gatsby develop`을 실행하기 위한 [Gatsby CLI](/docs/gatsby-cli/)


#### 지시 사항

1. index 페이지 컴포넌트(`src/pages/index.js`)를 열고, Gatsby 로부터 `<Link />` 컴포넌트를 임포트 한 다음, 헤더 위에 `<Link />` 컴포넌트를 추가하고, `to` 프로퍼티에 `"/contact/"` 라는 경로명을 지정해줍니다.

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

2. `gatsby develop`을 실행하고 index 페이지로 이동하세요. 클릭시 contact 페이지로 연결되는 링크가 있어야 합니다!

> **주의**: Gatsby의 `<Link />` 컴포넌트는 [`@reach/router`의 Link 컴포넌트](https://reach.tech/router/api/Link)를 감싸는 래퍼 입니다. Gatsby의 `<Link />` 컴포넌트에 대한 자세한 정보는 [`<Link />` API 레퍼런스](/docs/gatsby-link/) 문서를 참고하세요.

### layout 컴포넌트 생성하기

React layout 컴포넌트로 페이지들을 감싸는 것이 일반적이므로 여러 페이지 간 마크업, 스타일 및 기능을 공유 할 수 있습니다.

#### 사전 준비

- Gatsby 사이트

#### 지시 사항

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
import Layout from "../components/layout"

export default () => (
  <Layout>
    <Link to="/contact/">Contact</Link>
    <p>What a world.</p>
  </Layout>
)
```

#### 추가 정보

- [튜토리얼 파트3](/tutorial/part-three/#your-first-layout-component)에서 layout 컴포넌트를 생성하세요
- [Layout 컴포넌트](/docs/layout-components/) 스타일링

### createPage를 사용하여 프로그래밍 방식으로 여러 페이지 생성하기

Gatsby가 제공하는 헬퍼 메소드를 사용하여 `gatsby-node.js` 파일에서 프로그래밍 방식으로 페이지를 생성할 수 있습니다.

#### 사전 준비

- [Gatsby 사이트](/docs/quick-start)
- `gatsby-node.js` 파일

#### 지시 사항

1. `gatsby-node.js` 파일 안에, `createPages` 추가하고 내보냅니다(export).

```javascript:title=gatsby-node.js
// highlight-start
exports.createPages = ({ actions }) => {
  // ...
}
// highlight-end
```

2. 사용 가능한 actions 로 부터 `createPage` 액션을 뽑아내어 자체적으로 호출하고, 데이터를 추가하거나 가져올 수 있습니다.

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

3. `gatsby-node.js`의 데이터로 반복하고 매 반복 단계에서 `createPage`에 경로, 템플릿 및 컨텍스트 (props안의 pageContext로 전달 될 데이터)를 제공하세요.

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

4. `createPage`에 제공된 페이지의 템플릿으로 사용할 React 컴포넌트를 생성하세요.

```jsx:title=src/templates/dog-template.js
import React from "react"

export default ({ pageContext: { dog } }) => (
  <section>
    {dog.name} - {dog.breed}
  </section>
)
```

5. `gatsby develop`을 실행하고 생성된 페이지 중 하나(예: `http://localhost:8000/Fido`)로 이동하여 페이지에 표시되는 데이터를 확인하세요.

#### 추가 정보

- [데이터로부터 프로그래밍 방식으로 여러 페이지 생성하기](/tutorial/part-seven/)에 대한 튜토리얼 섹션
- [GraphQL 없이 Gatsby 사용하기](/docs/using-gatsby-without-graphql/) 레퍼런스 가이드
- 이 레시피의 [예제 저장소](https://github.com/gatsbyjs/gatsby/tree/master/examples/recipe-createPage)

## 2. CSS로 스타일링

웹 사이트에 스타일을 추가하는 방법에는 정말 많은 방법이 있습니다. Gatsby는 공식 및 커뮤니티 플러그인을 통해 거의 모든 옵션을 지원합니다.

### Layout 컴포넌트 없이 전역 CSS 파일들 사용하기

#### 사전 준비

- index 페이지 컴포넌트를 가진 [Gatsby 사이트](/docs/quick-start/)
- `gatsby-browser.js` 파일

#### 지시 사항

1. `src/styles/global.css` 파일 이름으로 전역 CSS 파일을 생성하고 아래 코드를 붙여 넣습니다.

```css:title=src/styles/styles/global.css
html {
  background-color: lavenderblush;
}

p {
  color: maroon;
}
```

2. `gatsby-browser.js` 파일 안에서 아래와 같이 전역 CSS 파일을 임포트 합니다.

```javascript:gatsby-browser.js
import "./src/styles/global.css"
```

> **Note:** `require('./src/styles/global.css')`이런 문법을 사용하여 `gatsby-config.js` 파일 안에서 전역 CSS 파일을 임포트 할 수도 있습니다.

3. `gatsby develop`을 실행하여 사이트 곳곳에 적용된 전역 스타일을 확인해주세요.

> **Note:** This approach is not the best fit if you are using CSS-in-JS for styling your site, in which case a layout page with all the shared components should be used. This is covered in the next recipe.
> **참고:** 이 방법은 CSS-in-JS를 사용하여 스타일을 정의하는 경우 적합하지 않으며 이 경우 모든 공유 컴포넌트가 포함 된 layout 페이지를 사용해야합니다. 이것은 다음 레시피에서 다룹니다.


#### 추가 정보

- [Layout 컴포넌트 없이 전역 스타일 추가하기](/docs/global-css/#adding-global-styles-without-a-layout-component)에 대한 추가 정보

### Layout 컴포넌트에서 전역 스타일 사용하기

#### 사전 준비

- index 페이지 컴포넌트를 가진 [Gatsby 사이트](/docs/quick-start/)

#### 지시 사항

[공유 layout 컴포넌트](/tutorial/part-three/#your-first-layout-component)에 전역 스타일을 추가 할 수 있습니다. 이 컴포넌트는 헤더 또는 풋터 글과 같이 사이트 전체의 공통적인 항목에 사용됩니다.

1. `/src/components` 디렉토리가 없다면 디렉토리를 생성해주세요.

2. components 디렉토리 안에 `layout.css` 와 `layout.js` 두개의 파일을 생성해주세요.

3. `layout.css`에 다음을 추가해주세요:

```css:title=/src/components/layout.css
body {
  background: red;
}
```

4. `layout.js`를 편집하여 CSS 파일을 임포트하고 layout 마크 업을 내보냅니다:

```jsx:title=/src/components/layout.js
import React from "react"
import "./layout.css"

export default ({ children }) => <div>{children}</div>
```

5. 이제 사이트의 홈페이지인 `/src/pages/index.js`를 편집하여 새로운 layout 컴포넌트를 사용합니다:

```jsx:title=/src/pages/index.js
import React from "react"
import Layout from "../components/layout"

export default () => <Layout>Hello world!</Layout>
```

#### 추가 정보

- [전역 CSS 파일을 사용한 표준 스타일링](/docs/global-css/)
- [layout 컴포넌트에 대한 추가 정보](/tutorial/part-three)

### Styled Components 사용하기

#### 사전 준비

- index 페이지 컴포넌트를 가진 [Gatsby 사이트](/docs/quick-start/)
- `package.json`안에 [gatsby-plugin-styled-components, styled-components, and babel-plugin-styled-components](/packages/gatsby-plugin-styled-components/) 설치

#### 지시 사항

1. `gatsby-config.js` 파일 안에서 `gatsby-plugin-styled-components`를 추가하세요.

```javascript:title=gatsby-config.js
module.exports = {
  plugins: [`gatsby-plugin-styled-components`],
}
```

2. index 페이지 컴포넌트(`src/pages/index.js`)를 열고 `styled-components` 패키지를 임포트 하세요.

3. 각 element 타입에 대한 스타일 블록을 작성하여 컴포넌트를 스타일링 하세요.

4. JSX 안에 스타일링된 컴포넌트를 포함시켜 페이지에 적용하세요.

```jsx:title=src/pages/index.js
import React from "react"
import styled from "styled-components" //highlight-line

const Container = styled.div`
  margin: 3rem auto;
  max-width: 600px;
  display: flex;
  flex-direction: column;
  align-items: center;
  justify-content: center;
`

const Avatar = styled.img`
  flex: 0 0 96px;
  width: 96px;
  height: 96px;
  margin: 0;
`

const Username = styled.h2`
  margin: 0 0 12px 0;
  padding: 0;
`

const User = props => (
  <>
    <Avatar src={props.avatar} alt={props.username} />
    <Username>{props.username}</Username>
  </>
)

export default () => (
  <Container>
    <h1>About Styled Components</h1>
    <p>Styled Components is cool</p>
    <User
      username="Jane Doe"
      avatar="https://s3.amazonaws.com/uifaces/faces/twitter/adellecharles/128.jpg"
    />
    <User
      username="Bob Smith"
      avatar="https://s3.amazonaws.com/uifaces/faces/twitter/vladarbatov/128.jpg"
    />
  </Container>
)
```

4. `gatsby develop`을 실행하여 변경을 확인하세요.

#### 추가 정보

- [styled-components 사용에 대한 추가 정보](/docs/styled-components/)
- [Egghead 수업](https://egghead.io/lessons/gatsby-style-gatsby-sites-with-styled-components)

### CSS Modules 사용하기

#### 사전 준비

- index 페이지 컴포넌트를 가진 [Gatsby 사이트](/docs/quick-start/)

#### 지시 사항

1. `src/pages/index.module.css` 파일 이름으로 CSS module 을 생성하고 다음을 모두 모듈에 붙여 넣습니다:

```css:title=src/components/index.module.css
.feature {
  margin: 2rem auto;
  max-width: 500px;
}
```

2. `index.js` 파일 안에서 CSS module을 `style` 객체로 임포트 하여 다음과 같이 페이지를 수정합니다:

```jsx:title=src/pages/index.js
import React from "react"

// highlight-start
import style from "./index.module.css"

export default () => (
  <section className={style.feature}>
    <h1>Using CSS Modules</h1>
  </section>
)
// highlight-end
```

3. `gatsby develop`을 실행하여 변경을 확인하세요.

**Note:**
Notice that the file extension is `.module.css` instead of `.css`, which tells Gatsby that this is a CSS module.

**주의:**
파일 확장자가 `.css` 가 아닌 `.module.css`인것에 유의해야 하는데, 이는 Gatsby에게 이것이 CSS module임을 알려줍니다.


#### 추가 정보

- [CSS Modules 사용하기](/tutorial/part-two/#css-modules)에 대한 추가 정보
- [CSS modules 사용하기 라이브 예제](https://github.com/gatsbyjs/gatsby/blob/master/examples/using-css-modules)

### Sass/SCSS 사용하기

Sass는 CSS의 확장으로 중첩 규칙, 변수, 믹스인 등과 같은 고급 기능을 제공합니다.

Sass에는 2 가지 구문이 있습니다. 가장 일반적으로 사용되는 구문은 "SCSS"이며, CSS의 상위 집합입니다. 즉, 모든 유효한 CSS 구문은 유효한 SCSS 구문입니다. SCSS 파일은 .scss 확장자를 사용합니다.

Sass는 .scss 및 .sass 파일을 .css 파일로 컴파일하므로, 이런 고급 기능들을 사용하여 스타일 시트를 작성할 수 있습니다.

#### 사전 준비

- [Gatsby 사이트](/docs/quick-start/).

#### 지시 사항

1. [gatsby-plugin-sass](https://www.gatsbyjs.org/packages/gatsby-plugin-sass/) Gatsby 플러그인과 `node-sass` 설치하세요.

`npm install --save node-sass gatsby-plugin-sass`

2. `gatsby-config.js` 파일에 이 플러그인을 추가하세요.

```javascript:title=gatsby-config.js
plugins: [`gatsby-plugin-sass`],
```

3.  Write your stylesheets as `.sass` or `.scss` files and import them. If you don't know how to import styles, take a look at [Styling with CSS](/docs/recipes/#2-styling-with-css)
3. 스타일 시트를 `.sass` 또는 `.scss` 파일로 작성하고 임포트 합니다. 스타일을 가져 오는 방법을 모르는 경우 [CSS로 스타일링](/docs/recipes/#2-styling-with-css)을 살펴보세요.

```css:title=styles.scss
$font-stack: Helvetica, sans-serif;
$primary-color: #333;

body {
  font: 100% $font-stack;
  color: $primary-color;
}
```

```css:title=styles.sass
$font-stack:    Helvetica, sans-serif
$primary-color: #333

body
  font: 100% $font-stack
  color: $primary-color
```

```javascript
import "./styles.scss"
import "./styles.sass"
```

_주의: 이전 CSS module 레시피에서 언급했듯이 Sass/SCSS 파일도 module로 사용할 수 있습니다. 차이점은 확장자가 .css가 아닌 .scss 또는 .sass여야합니다._


#### 추가 정보

- [.sass와 .scss의 차이점](https://responsivedesign.is/articles/difference-between-sass-and-scss/)
- [공식 Sass 웹사이트의 Sass 가이드](https://sass-lang.com/guide)
- [A more complete installation tutorial on Sass with some more explanations and more resources](https://www.gatsbyjs.org/docs/sass/)
- [더 많은 설명과 더 많은 리소스를 제공하는 더 완벽한 Sass 설치 튜토리얼](https://www.gatsbyjs.org/docs/sass/)

### 파일로 폰트 추가하기

#### 사전 준비

- [Gatsby 사이트](/docs/quick-start/)
- `.woff2`, `.ttf` 등의 폰트 파일

#### 지시 사항

1. 폰트 파일을 Gatsby 프로젝트에 `src/fonts/fontname.woff2`와 같은 경로에 복사하세요.

```
src/fonts/fontname.woff2
```

2. Gatsby 사이트의 번들로 포함시키기 위해 CSS 파일에서 폰트를 임포트 하세요:

```css:title=src/css/typography.css
@font-face {
  font-family: "Font Name";
  src: url("../fonts/fontname.woff2");
}
```

**주의:** "Font Name" 이 관련 CSS에서 참조되는지 확인하세요. 예:

```css:title=src/components/layout.css
body {
  font-family: "Font Name", sans-serif;
}
```

By targeting the HTML `body` element, your font will apply to most text on the page. Additional CSS can target other elements, such as `button` or `textarea`.

If fonts are not updating following steps above, make sure to replace the existing font-family in relevant CSS.

HTML `body` 요소를 대상으로하면 글꼴이 페이지의 대부분의 텍스트에 적용됩니다. 추가 CSS는 `button` 또는 `textarea`와 같은 다른 요소를 대상으로 할 수 있습니다.

위의 단계에 따라 폰트가 업데이트되지 않으면 관련 CSS에서 기존 font-family를 교체하십시오.

#### 추가 정보

- [파일로 에셋 임포트 하기](/docs/importing-assets-into-files/)에 대한 추가 정보

### Emotion 사용하기

[Emotion](https://emotion.sh)은 인라인 CSS 스타일과 styled components 를 모두 지원하는 강력한 CSS-in-JS 라이브러리입니다. 각 스타일링 기능을 개별적으로 또는 동일한 파일에서 함께 사용할 수 있습니다.

#### 사전 준비

- [Gatsby 사이트](/docs/quick-start)

#### 지시 사항

1. [Gatsby Emotion 플러그인](/packages/gatsby-plugin-emotion/)과 Emotion 패키지들 설치.

```shell
npm install --save gatsby-plugin-emotion @emotion/core @emotion/styled
```

2. `gatsby-plugin-emotion` 을 `gatsby-config.js` 파일에 추가하세요.

```javascript:title=gatsby-config.js
module.exports = {
  plugins: [`gatsby-plugin-emotion`],
}
```

3. Gatsby 사이트의 페이지가 아직 없다면 `src/pages/emotion-sample.js`로 페이지를 하나 만드세요.

Import Emotion's `css` core package. You can then use the `css` prop to add [Emotion object styles](https://emotion.sh/docs/object-styles) to any element inside a component:
Emotion의 `css` core 패키지를 가져옵니다. 그런 다음 `css` prop을 사용하여 컴포넌트 내의 모든 엘리먼트에 [Emotion object styles](https://emotion.sh/docs/object-styles)를 추가 할 수 있습니다.

```jsx:title=src/pages/emotion-sample.js
import React from "react"
import { css } from "@emotion/core"

export default () => (
  <div>
    <p
      css={{
        background: "pink",
        color: "blue",
      }}
    >
      This page is using Emotion.
    </p>
  </div>
)
```

4. Emotion의 [styled components](https://emotion.sh/docs/styled)를 사용하려면 패키지를 임포트하고 `styled` 함수를 사용하여 정의하십시오.

```jsx:title=src/pages/emotion-sample.js
import React from "react"
import styled from "@emotion/styled"

const Content = styled.div`
  text-align: center;
  margin-top: 10px;
  p {
    font-weight: bold;
  }
`

export default () => (
  <Content>
    <p>This page is using Emotion.</p>
  </Content>
)
```

#### 추가 정보

- [Gatsby 에서 Emotion 사용하기](/docs/emotion/)
- [Emotion 웹사이트](https://emotion.sh)
- [Emotion 과 Gatsby 시작하기](https://egghead.io/lessons/gatsby-getting-started-with-emotion-and-gatsby)

### Google Fonts 사용하기

프로젝트 번들에 자신만의 [Google Fonts](https://fonts.google.com/)를 포함시켜 호스팅하면 사이트가 로드 될 때 네트워크를 통해 추가적으로 가져 오지 않아도되므로 데스크톱에서 최대 300ms, 3G에서 1초 이상 사이트 로딩 속도가 개선됩니다. 성능을 위해 이런 사용자 정의 글꼴 사용은 오직 필수적인 것으로만 제한하는 것이 좋습니다.

#### 사전 준비

- [Gatsby 사이트](/docs/quick-start)
- [Gatsby CLI](/docs/gatsby-cli/) 설치
- [typefaces 프로젝트](https://github.com/KyleAMathews/typefaces)에서 폰트 패키지 선택하기

#### 지시 사항

1. `npm install --save typeface-your-chosen-font`에서, `your-chosen-font` 부분을 [typefaces 프로젝트](https://github.com/KyleAMathews/typefaces)에서 찾은 원하는 폰트이름으로 변경하여 실행하세요.

예를들어 인기있는 'Source Sans Pro' 글꼴을 로드하려면: `npm install --save typeface-source-sans-pro`.

2. layout 템플릿, 페이지 컴포넌트 또는 `gatsby-browser.js` 에 `import "typeface-your-chosen-font"` 를 추가하세요.

```jsx:title=src/components/layout.js
import "typeface-your-chosen-font"
```

3. 임포트 한 후에, CSS stylesheet, CSS Module 또는 CSS-in-JS에서 폰트 이름을 사용 할 수 있습니다.


```css:title=src/components/layout.css
body {
  font-family: "Your Chosen Font";
}
```

_참고: 위의 예에서 관련 CSS 선언은 `font-family: 'Source Sans Pro';` 입니다._

#### 추가 정보

- [Typography.js](/docs/typography-js/) - Gatsby 사이트에서 Google 폰트를 사용하는 또 다른 옵션
- [Typefaces 프로젝트 문서](https://github.com/KyleAMathews/typefaces/blob/master/README.md)
- [Live Kyle Mathews의 블로그의 라이브 예제](https://www.bricolage.io/typefaces-easiest-way-to-self-host-fonts/)

## 3. Starter로 작업하기
[Starters](/docs/starters/)는 공식적으로 또는 커뮤니티에 의해 유지보수되는 개츠비 사이트의 보일러 플레이트입니다.

### Starter 사용하기

#### 사전 준비

- [Gatsby CLI](/docs/gatsby-cli) 설치

#### 지시 사항

1. 사용하고 싶은 starter를 찾으세요. (_[Starter 라이브러리](/starters/?v=2)에서 찾기 좋습니다!_)

2. Starter를 기반으로 새 사이트를 생성하기위해 터미널에서 다음을 실행하세요:

```shell
gatsby new {프로젝트-이름} {starter의-링크}
```

> _위의 명령을 그대로 실행하면 안됩니다. {프로젝트-이름}과 {starter의-링크}를 변경해야하는 것을 잊지마세요!_

3. 새 사이트를 실행하세요:

```shell
cd {프로젝트-이름}
gatsby develop
```

#### 추가 정보

- Gatsby starter 사용에 대한 [더 자세한 가이드](/docs/starters/)를 확인하세요.
- [튜토리얼 파트1](/tutorial/part-one/#using-gatsby-starters)에서 starters 사용을 위한 [Gatsby CLI](/docs/gatsby-cli) 사용법을 습득하세요.
- [Starter 라이브러리](/starters/?v=2)를 탐색해 보세요.
- Gatsby의 [공식 기본 starter](https://github.com/gatsbyjs/gatsby-starter-default)를 확인하세요.

## 4. 테마 작업하기

Gatsby 테마는 Gatsby 구성(공유되는 기능, 데이터 소싱, 디자인)을 설치 가능한 패키지로 추상화합니다. 이는 구성 및 기능이 프로젝트에 직접 작성되지 않고 버전닝, 중앙 관리 되며 종속성으로 설치됨을 의미합니다. 테마를 매끄럽게 업데이트하고, 여러 테마들을 조립하며, 호환되는 테마를 다른 테마로 교체 할 수도 있습니다.

- [Gatsby 테마는 무엇입니까?](/docs/themes/what-are-gatsby-themes)를 더 읽어보세요.

### 테마 starter를 사용하여 새 사이트 생성하기

테마를 구성하는 스타터를 기반으로 사이트를 작성하는 것은 테마를 **구성하지 않은** 스타터를 기반으로 사이트를 작성하는 것과 동일한 프로세스를 따릅니다. 이 예에서는 [새 사이트를 만들기위한 공식 Gatsby 블로그 starter](https://github.com/gatsbyjs/gatsby-starter-blog-theme)를 사용할 수 있습니다.

#### 사전 준비

- [Gatsby CLI](/docs/gatsby-cli) 설치

#### 지시 사항

1. 블로그 테마 starter를 기반으로 새 사이트를 생성하세요:

```shell
gatsby new {프로젝트-이름} https://github.com/gatsbyjs/gatsby-starter-blog-theme
```

2. 새 사이트를 실행하세요:

```shell
cd {프로젝트-이름}
gatsby develop
```

#### 추가 정보

- [짧은 개념 가이드](/docs/themes/using-a-gatsby-theme) 또는 보다 상세한 [단계별 튜토리얼](/tutorial/using-a-theme)에서 기존 Gatsby 테마를 사용하는 방법에 대해 알아 봅니다.

### 새 테마 만들기

<EggheadEmbed
  lessonLink="https://egghead.io/lessons/gatsby-use-the-gatsby-theme-workspace-starter-to-begin-building-a-new-theme"
  lessonTitle="Use the Gatsby Theme Workspace Starter to Begin Building a New Theme"
/>

#### 사전 준비

- [Gatsby CLI](/docs/gatsby-cli) 설치

* [Yarn](https://yarnpkg.com/lang/en/docs/install/#mac-stable) 설치

#### 지시 사항

1. [Gatsby 테마 workspace starter](https://github.com/gatsbyjs/gatsby-starter-theme-workspace)를 사용하여 새 테마 workspace를 생성하세요:

```shell
gatsby new {프로젝트-이름} https://github.com/gatsbyjs/gatsby-starter-theme-workspace
```

2. Workspace 안의 example 사이트를 실행하세요:

```shell
yarn workspace example develop
```

#### 추가 정보

- Gatsby 테마 workspace starter에 대한 [더 자세한 가이드](/docs/themes/building-themes/)를 확인하세요.
-[Egghead의 Gatsby 테마 작성 영상 강좌](https://egghead.io/courses/gatsby-theme-authoring) 또는 [영상 강좌를 보완하는 튜토리얼 문서](/tutorial/building-a-theme)에서 자신만의 테마를 만드는 방법을 배웁니다.

## 5. 데이터 소싱하기

Gatsby의 데이터 소싱은 플러그인 주도로 이뤄집니다. Source 플러그인은 소스에서 데이터를 가져옵니다. (예: `gatsby-source-filesystem` 플러그인은 파일 시스템에서 데이터를 가져오고 `gatsby-source-wordpress` 플러그인은 WordPress API에서 데이터를 가져옵니다). 직접 데이터를 제공 할 수도 있습니다.

### GraphQL에 데이터 추가하기

Gatsby의 [GraphQL 데이터 계층](/docs/querying-with-graphql/)은 노드를 사용하여 데이터 청크를 모델링합니다. Gatsby source plugin은 쿼리 할 수 있는 소스 노드를 추가하지만 직접 소스 노드를 작성할 수도 있습니다. 사용자 정의 데이터를 GraphQL 데이터 레이어에 직접 추가하기 위해 Gatsby는 활용할 수 있는 방법을 제공합니다.

이 레시피는`createNode()`를 사용하여 커스텀 데이터를 추가하는 방법을 보여줍니다.

#### 지시 사항

1. `gatsby-node.js`에서 `sourceNodes()` 및 `actions.createNode()`를 사용하여 데이터를 쿼리 할 수 있는 노드를 생성하고 내보내세요.

```javascript:title=gatsby-node.js
exports.sourceNodes = ({ actions, createNodeId, createContentDigest }) => {
  const pokemons = [
    { name: "Pikachu", type: "electric" },
    { name: "Squirtle", type: "water" },
  ]

  pokemons.forEach(pokemon => {
    const node = {
      name: pokemon.name,
      type: pokemon.type,
      id: createNodeId(`Pokemon-${pokemon.name}`),
      internal: {
        type: "Pokemon",
        contentDigest: createContentDigest(pokemon),
      },
    }
    actions.createNode(node)
  })
}
```

2. `gatsby develop`를 실행하세요.

   > _참고: `gatsby-node.js`를 변경 한 후 변경 사항을 적용하려면 `gatsby develop`을 다시 실행해야합니다._

3. GraphQL 또는 컴포넌트에서 데이터를 쿼리하세요.

```graphql
query MyPokemonQuery {
  allPokemon {
    nodes {
      name
      type
      id
    }
  }
}
```

#### 추가 정보

- [튜토리얼 파트5](/tutorial/part-five/#source-plugins)의 `gatsby-source-filesystem` 플러그인을 사용한 예제를 살펴보기
- [Gatsby 라이브러리](/plugins/?=source)에서 사용 가능한 소스 플러그인을 검색해보기
-[Pixabay 소스 플러그인 튜토리얼](/docs/pixabay-source-plugin-tutorial/)을 따라해보고 소스 플러그인 이해하기
- createNode 함수 [문서](/docs/actions/#createNode)

### GraphQL을 사용하여 블로그 게시물 및 페이지를 위한 Markdown 데이터 소싱하기

Markdown 데이터를 소싱하고 Gatsby의 [`createPages` API](/docs/actions/#createPage)를 사용하여 페이지를 동적으로 만들 수 있습니다.

#### 사전 준비

- `gatsby-config.js` 파일이 있는 [Gatsby 사이트](/docs/quick-start)
- [Gatsby CLI](/docs/gatsby-cli) 설치
- [gatsby-source-filesystem 플러그인](/packages/gatsby-source-filesystem) 설치
- [gatsby-transformer-remark 플러그인](/packages/gatsby-transformer-remark) 설치
- `gatsby-node.js` 파일

#### 지시 사항

1. `gatsby-config.js`에서, `gatsby-source-filesystem`과 함께 `gatsby-transformer-remark`를 구성하여 소스 폴더로부터 Markdown 파일을 가져 오세요. 이것은 이미지 등을 위해 추가했던 이전 `gatsby-source-filesystem` 항목에 추가됩니다.


```js:title=gatsby-config.js
module.exports = {
  plugins: [
    `gatsby-transformer-remark`,
    {
      resolve: `gatsby-source-filesystem`,
      options: {
        name: `content`,
        path: `${__dirname}/src/content`,
      },
    },
  ]
```

2. 제목, 날짜 및 경로 frontmatter 정보를 포함한 Markdown 게시물을 간단한 본문 내용과 함께 `src/content`에 추가해주세요:

```markdown:title=src/content/my-first-post.md
---
title: My First Post
date: 2019-07-10
path: /my-first-post
---

This is my first Gatsby post written in Markdown!
```

3. `gatsby develop`으로 개발 서버를 시작하고 `http://localhost:8000/___graphql`의 GraphiQL 탐색기로 이동한 후 모든 Markdown 데이터를 얻기 위한 쿼리를 작성하세요.

```graphql
{
  allMarkdownRemark {
    edges {
      node {
        frontmatter {
          path
        }
      }
    }
  }
}
```

<iframe
  title="Query for all markdown"
  src="https://q4xpb.sse.codesandbox.io/___graphql?explorerIsOpen=false&query=%7B%0A%20%20allMarkdownRemark%20%7B%0A%20%20%20%20edges%20%7B%0A%20%20%20%20%20%20node%20%7B%0A%20%20%20%20%20%20%20%20frontmatter%20%7B%0A%20%20%20%20%20%20%20%20%20%20path%0A%20%20%20%20%20%20%20%20%7D%0A%20%20%20%20%20%20%7D%0A%20%20%20%20%7D%0A%20%20%7D%0A%7D"
  width="600"
  height="300"
/>

4. GraphQL 쿼리를 `gatsby-node.js`에 복사하고 결과를 순환하여 빌드 시간에 Markdown 게시물로 부터 페이지를 생성하는 JavaScript 코드를 추가하세요:

```js:title=gatsby-node.js
const path = require(`path`)

exports.createPages = async ({ actions, graphql }) => {
  const { createPage } = actions

  const result = await graphql(`
    {
      allMarkdownRemark {
        edges {
          node {
            frontmatter {
              path
            }
          }
        }
      }
    }
  `)
  if (result.errors) {
    console.error(result.errors)
  }

  result.data.allMarkdownRemark.edges.forEach(({ node }) => {
    createPage({
      path: node.frontmatter.path,
      component: path.resolve(`src/templates/post.js`),
    })
  })
}
```

5. 빌드 시간에 Markdown 콘텐츠에서 페이지를 동적으로 생성하기 위해 GraphQL 쿼리를 포함하는 post 템플릿을 `src/templates`에 추가하세요:

```jsx:title=src/templates/post.js
import React from "react"
import { graphql } from "gatsby"

export default function Template({ data }) {
  const { markdownRemark } = data // data.markdownRemark에 post data가 들어있음
  const { frontmatter, html } = markdownRemark
  return (
    <div className="blog-post">
      <h1>{frontmatter.title}</h1>
      <h2>{frontmatter.date}</h2>
      <div
        className="blog-post-content"
        dangerouslySetInnerHTML={{ __html: html }}
      />
    </div>
  )
}

export const pageQuery = graphql`
  query($path: String!) {
    markdownRemark(frontmatter: { path: { eq: $path } }) {
      html
      frontmatter {
        date(formatString: "MMMM DD, YYYY")
        path
        title
      }
    }
  }
`
```

6. `gatsby develop`을 실행하여 개발 서버를 재시작하세요. 브라우저에서 게시물을 확인하세요: `http://localhost:8000/my-first-post`

#### 추가 정보

- [튜토리얼: 데이터로부터 프로그램 방식으로 여러 페이지 생성하기](/tutorial/part-seven/)
- [페이지 생성 및 수정하기](/docs/creating-and-modifying-pages/)
- [Markdown 페이지 추가하기](/docs/adding-markdown-pages/)
- [프로그래밍 방식으로 데이터로부터 페이지를 생성하는 안내서](/docs/programmatically-create-pages-from-data/)
- [이 레시피에 대한 예제 저장소](https://github.com/gatsbyjs/gatsby/tree/master/examples/recipe-sourcing-markdown)

### 외부 소스에서 데이터를 가져 와서 GraphQL없이 페이지 만들기

[GraphQL을 고려해야하는 이유가 있지만](/docs/why-gatsby-uses-graphql/) 페이지에 데이터를 포함하기 위해 꼭 GraphQL 데이터 계층을 사용할 필요는 없습니다. 노드의 `createPages` API를 사용하여 GraphQL 및 소스 플러그인을 통하지 않고 구조화되지 않은 데이터를 개츠비 사이트로 직접 가져올 수 있습니다.

이 레시피에서는 [PokéAPI의 REST 엔드 포인트](https://www.pokeapi.co/)에서 가져온 데이터로 동적 페이지를 생성해볼 것입니다. [전체 예제](https://github.com/jlengstorf/gatsby-with-unstructured-data/)는 GitHub에서 찾을 수 있습니다.

#### 사전 준비

- `gatsby-node.js` 파일이 있는 Gatsby 사이트
- [Gatsby CLI](/docs/gatsby-cli) 설치
- npm 으로부터 [axios](https://www.npmjs.com/package/axios) 패키지 설치

#### 지시 사항

1. `gatsby-node.js`안에서, PokéAPI에서 데이터를 가져오고 프로그래밍 방식으로 index 페이지를 생성하는 JavaScript 코드를 추가하세요:

```js:title=gatsby-node.js
const axios = require("axios")

const get = endpoint => axios.get(`https://pokeapi.co/api/v2${endpoint}`)

const getPokemonData = names =>
  Promise.all(
    names.map(async name => {
      const { data: pokemon } = await get(`/pokemon/${name}`)
      return { ...pokemon }
    })
  )
exports.createPages = async ({ actions: { createPage } }) => {
  const allPokemon = await getPokemonData(["pikachu", "charizard", "squirtle"])

  // Pokémon들을 나열하는 페이지를 생성합니다.
  createPage({
    path: `/`,
    component: require.resolve("./src/templates/all-pokemon.js"),
    context: { allPokemon },
  })
}
```

2. 웹페이지에 Pokémon을 표시 할 템플릿을 만듭니다:

```js:title=src/templates/all-pokemon.js
import React from "react"

export default ({ pageContext: { allPokemon } }) => (
  <div>
    <h1>Behold, the Pokémon!</h1>
    <ul>
      {allPokemon.map(pokemon => (
        <li key={pokemon.id}>
          <img src={pokemon.sprites.front_default} alt={pokemon.name} />
          <p>{pokemon.name}</p>
        </li>
      ))}
    </ul>
  </div>
)
```

3. 데이터를 가져오고, 페이지를 빌드하고, 개발 서버를 시작하기위해 `gatsby develop`을 실행하세요.
4. 브라우저에서 웹페이지를 확인하세요: `http://localhost:8000`

#### 추가 정보

- [전체 Pokemon 데이터 저장소](https://github.com/jlengstorf/gatsby-with-unstructured-data/)
- [GraphQL 없이 Gatsby 사용하기](/docs/using-gatsby-without-graphql/)에서 구조화되지 않은 데이터 사용에 대한 추가정보
- When and how to [query data with GraphQL](/docs/querying-with-graphql/) for more complex Gatsby sites
- 더 복잡한 Gatsby 사이트에서 언제 어떻게 [GraphQL로 데이터를 쿼리 해야하나요](/docs/querying-with-graphql/)

### Drupal의 콘텐츠 가져오기

#### 사전 준비

- [Gatsby 사이트](/docs/quick-start)
- [Drupal](http://drupal.org) 사이트
- Drupal 사이트의 [JSON:API module](https://www.drupal.org/project/jsonapi) 설치 및 활성화

#### 지시 사항

1. `gatsby-source-drupal` 플러그인을 설치하세요.

```
npm install --save gatsby-source-drupal
```

2. `gatsby-config.js` 파일을 수정하여 이 플러그인을 활성화 해주세요.

```javascript:title=gatsby-config.js
module.exports = {
  plugins: [
    {
      resolve: `gatsby-source-drupal`,
      options: {
        baseUrl: `https://your-website/`,
        apiBase: `api`, // optional, defaults to `jsonapi`
      },
    },
  ],
}
```

3. `gatsby develop`으로 개발 서버를 시작하고, `http://localhost:8000/___graphql`에서 GraphiQL 탐색기를 엽니다. 탐색기 탭에서 Drupal 블록들을 위한 `allBlockBlock`과 같은 새로운 노드 타입과 Drupal 사이트의 모든 컨텐츠 타입에 대한 노드가 표시됩니다. 예를 들어, "Page" 컨텐츠 타입이 있으면 `allNodePage`로 사용할 수 있습니다. 모든 "Page" 노드의 제목과 본문을 쿼리하려면 다음과 같은 쿼리를 사용하십시오.

```graphql
{
  allNodePage {
    edges {
      node {
        title
        body {
          value
        }
      }
    }
  }
}
```

4. Gatsby 사이트에서 Drupal 데이터를 사용하려면, `src/page/drupal.js`이라는 새 페이지를 만들어주세요. 이 페이지에 모든 Drupal "Page" 노드를 나열할 것입니다.

_**주의:** 정확한 GraphQL 스키마는 Drupal 인스턴스의 구조에 따라 달라집니다._

```jsx:title=src/pages/drupal.js
import React from "react"
import { graphql } from "gatsby"

const DrupalPage = ({ data }) => (
  <div>
    <h1>Drupal pages</h1>
    <ul>
    {data.allNodePage.edges.map(({ node, index }) => (
      <li key={index}>
        <h2>{node.title}</h2>
        <div>
          {node.body.value}
        </div>
      </li>
    ))}
   </ul>
  </div>
)

export default DrupalPage

export const query = graphql`
  {
  allNodePage {
    edges {
      node {
        title
        body {
          value
        }
      }
    }
  }
}
```

5. 실행 중인 개발 서버에서, `http://localhost:8000/drupal`을 방문하여 새 페이지를 볼 수 있습니다.

#### 추가 정보

- [독립적인 Drupal과 Gatsby 사용하기](/blog/2018-08-13-using-decoupled-drupal-with-gatsby/)
- [Drupal로부터 가져오기에 대한 자세한 내용](/docs/sourcing-from-drupal)
- [튜토리얼: 데이터로부터 프로그램 방식으로 여러 페이지 생성하기](/tutorial/part-seven/)

## 6. 데이터 쿼리하기

### 페이지 쿼리로 데이터 쿼리하기

`graphql` 태그를 사용하여 Gatsby 사이트의 페이지에서 데이터를 쿼리 할 수 있습니다. 이를 통해 사이트 메타 데이터, 소스 플러그인, 이미지 등 Gatsby의 데이터 계층에 포함 된 모든 항목에 접근 할 수 있습니다.

#### 지시 사항

1. `gatsby`로부터 `graphql`을 임포트 하세요.

2. 역따옴표를 사용하여 쿼리가있는 `graphql` 템플릿을 작성하고 `query`라는 상수 이름으로 export 하세요.

3. 컴포넌트에 `data`를 prop으로 전달하세요.

4. `data` 변수는 쿼리의 결과를 가지고 있으며 JSX에서 참조되어 HTML을 생산할 수 있습니다.

```jsx:title=src/pages/index.js
import React from "react"
// highlight-next-line
import { graphql } from "gatsby"

import Layout from "../components/layout"

// highlight-start
export const query = graphql`
  query HomePageQuery {
    site {
      siteMetadata {
        title
      }
    }
  }
`
// highlight-end

// highlight-next-line
const IndexPage = ({ data }) => (
  <Layout>
    // highlight-next-line
    <h1>{data.site.siteMetadata.title}</h1>
  </Layout>
)

export default IndexPage
```

#### 추가 정보

- [GraphQL과 Gatsby](/docs/graphql/): 데이터의 예상 형태 이해하기
- [GraphQL로 페이지에서 데이터 쿼리하기에 대한 추가 정보](/docs/page-query/)
- GraphQL에서 사용했던 [Tagged Template Literals에 대한 MDN문서](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Template_literals)

### StaticQuery 컴포넌트를 사용하여 데이터 쿼리하기

`StaticQuery`는 헤더, 내비게이션 또는 기타 자식 컴포넌트와 같은 [페이지가 아닌 컴포넌트](/docs/static-query/)에서 Gatsby의 데이터 계층으로부터 데이터를 가져오기위한 컴포넌트입니다.

#### 지시 사항

1. `StaticQuery` 컴포넌트에는 두 개의 필수 props인 `query`와 `render`가 필요합니다.

```jsx:title=src/components/NonPageComponent.js
import React from "react"
import { StaticQuery, graphql } from "gatsby"

const NonPageComponent = () => (
  <StaticQuery
    query={graphql` // highlight-line
      query NonPageQuery {
        site {
          siteMetadata {
            title
          }
        }
      }
    `}
    render={(
      data // highlight-line
    ) => (
      <h1>
        Querying title from NonPageComponent with StaticQuery:
        {data.site.siteMetadata.title}
      </h1>
    )}
  />
)

export default NonPageComponent
```

2. 이제 이 컴포넌트를 [다른 컴포넌트](/docs/building-with-components#non-page-components) 같이 더 큰 페이지의 JSX 컴포넌트 및 HTML 마크 업으로 가져 와서 사용할 수 있습니다.

### `useStaticQuery` 훅을 사용하여 데이터 쿼리하기

Since Gatsby v2.1.0, you can use the `useStaticQuery` hook to query data with a JavaScript function instead of a component. The syntax removes the need for a `<StaticQuery>` component to wrap everything, which some people find simpler to write.

Gatsby v2.1.0 부터 `useStaticQuery` 훅을 사용하여 컴포넌트 대신 JavaScript 함수로 데이터를 쿼리 할 수 있습니다. 이 구문은 모든 것을 감싸기위한 필요했던 `<StaticQuery>` 컴포넌트를 제거하며, 코드 작성을 더 간단하게 할 수 있습니다.

`useStaticQuery` 훅은 GraphQL 쿼리를 가져와 요청 된 데이터를 반환합니다. 이는 변수에 저장되어 나중에 JSX 템플릿에서 사용할 수 있습니다.

#### 사전 준비

- React 및 ReactDOM 16.8.0 이상이 필요 (Gatsby를 최신 버전으로 유지하면 해결됩니다)
- 추천 자료: [React Hooks 규칙](https://reactjs.org/docs/hooks-rules.html)

#### 지시 사항

1. 데이터를 쿼리하는 훅을 사용하기 위해 `gatsby`에서 `useStaticQuery`와 `graphql`을 임포트합니다.

2. Stateless function 컴포넌트를 시작할 때, `graphql` 쿼리를 `useStaticQuery` 의 인수로 전달하여 사용하고 변수에 할당합니다.

3. 컴포넌트에서 리턴되는 JSX 코드에서, 반환된 데이터를 사용하기 위해 해당 변수를 참조 할 수 있습니다.

```jsx:title=src/components/NonPageComponent.js
import React from "react"
import { useStaticQuery, graphql } from "gatsby" //highlight-line

const NonPageComponent = () => {
  // highlight-start
  const data = useStaticQuery(graphql`
    query NonPageQuery {
      site {
        siteMetadata {
          title
        }
      }
    }
  `)
  // highlight-end
  return (
    <h1>
      Querying title from NonPageComponent: {data.site.siteMetadata.title}{" "}
      //highlight-line
    </h1>
  )
}

export default NonPageComponent
```

#### 추가 정보

- [컴포넌트에서 데이터를 쿼리하기위한 Static Query에 대한 추가 정보](/docs/static-query/)
- [Static query 와 page query의 차이점](/docs/static-query/#how-staticquery-differs-from-page-query)
- [useStaticQuery 훅에 대한 추가 정보](/docs/use-static-query/)
- [GraphiQL로 데이터 시각화](/docs/introducing-graphiql/)

### GraphQL 결과 제한하기

GraphQL을 사용하여 데이터를 쿼리 할 때 숫자를 명시하여 반환되는 결과를 제한 할 수 있습니다. 이것은 몇 개의 데이터 만 필요하거나 [데이터를 페이지네이션](/docs/add-pagination/) 해야하는 경우에 유용합니다.

데이터를 제한하려면 GraphQL 데이터 레이어에 몇개의 노드가 있는 Gatsby 사이트가 필요합니다. 모든 사이트들은 `allSitePage` 및 `sitePage`와 같은 자동으로 생성된 노드들을 가지고 있으며, `gatsby-config.js`에 `gatsby-source-filesystem`과 같은 소스 플러그인을 설치하면 더 많은 노드를 추가 할 수 있습니다.


#### 사전 준비

- [Gatsby 사이트](/docs/quick-start/)

#### 지시 사항

1. `gatsby develop`을 실행하여 개발 서버를 시작하세요.
2. 브라우저 탭을 열고 `http://localhost:8000/___graphql`에 접속하세요.
3. 에디터에서 아래 필드들을 가진 `allSitePage` 쿼리를 추가하세요:

```graphql
{
  allSitePage {
    edges {
      node {
        id
        path
      }
    }
  }
}
```

4. allSitePage 필드에 정수 값 `3` 으로 `limit` 인수를 추가하세요.

```graphql
{
  allSitePage(limit: 3) { // highlight-line
    edges {
      node {
        id
        path
      }
    }
  }
}
```

5. GraphiQL 페이지에서 play 버튼을 클릭하면 `edges` 필드의 데이터가 명시한 수로 제한됩니다.

#### 추가 정보

- [Gaysby의 GraphQL 데이터 API의 노드](/docs/node-interface/)에 대한 학습
- [제한하기에 대한 Gatsby GraphQL 레퍼런스](/docs/graphql-reference/#limit)
- 라이브 예제:

<iframe
  title="Limiting returned data"
  src="https://711808k40x.sse.codesandbox.io/___graphql?query=%7B%0A%20%20allSitePage(limit%3A%203)%20%7B%0A%20%20%20%20edges%20%7B%0A%20%20%20%20%20%20node%20%7B%0A%20%20%20%20%20%20%20%20id%0A%20%20%20%20%20%20%20%20path%0A%20%20%20%20%20%20%7D%0A%20%20%20%20%7D%0A%20%20%7D%0A%7D%0A&explorerIsOpen=false"
  width="600"
  height="300"
/>

### GraphQL 결과 정렬하기

`sort` 인수를 명시하여 GraphQL의 결과를 순서를 정할 수 있습니다. 어떤 필드로 정렬 할지, 어떤 순서로 정렬 할 지를 명시 할 수 있습니다.

이 레시피를 위해서, GraphQL 데이터 레이어에 정렬 할 노드들이 있는 Gatsby 사이트가 필요합니다. 모든 사이트들은 `allSitePage` 와 같은 자동으로 생성된 노드들을 가지고 있으며, 소스 플러그인을 설치하면 더 많은 노드를 추가 할 수 있습니다.

#### 사전 준비

- [Gatsby 사이트](/docs/quick-start)
- 접두사 `all`이 있는 쿼리 가능한 필드. 예: `allSitePage`

#### 지시 사항

1. `gatsby develop`을 실행하여 개발 서버를 시작하세요.
2. 브라우저 탭을 열고 `http://localhost:8000/___graphql`에 접속하세요.
3. 에디터에서 아래 필드들을 가진 `allSitePage` 쿼리를 추가하세요:

```graphql
{
  allSitePage {
    edges {
      node {
        id
        path
      }
    }
  }
}
```

4. `allSitePage` 필드에 `sort` 인수를 추가하고 `fields` 및 `order` 속성을 가진 객체를 제공해주세요. `fields`의 값은 정렬 할 필드 또는 필드의 배열 일 수 있으며 (이 예에서는 `path` 필드를 사용함) `order` 는 오름차순 및 내림차순으로 각각 `ASC` 또는 `DESC`가 될 수 있습니다.

```graphql
{
  allSitePage(sort: {fields: path, order: ASC}) { // highlight-line
    edges {
      node {
        id
        path
      }
    }
  }
}

```

5. GraphiQL 페이지에서 play 버튼을 클릭하면 `path` 필드에 따라 오름차순으로 정렬되어있는 데이터를 반환 받게됩니다.

#### 추가 정보

- [정렬을 위한 Gatsby GraphQL 레퍼런스](/docs/graphql-reference/#sort)
- [Gaysby의 GraphQL 데이터 API의 노드](/docs/node-interface/)에 대한 학습
- 라이브 예제:

<iframe
  title="Sorting data"
  src="https://711808k40x.sse.codesandbox.io/___graphql?query=%7B%0A%20%20allSitePage(sort%3A%20%7Bfields%3A%20path%2C%20order%3A%20ASC%7D)%20%7B%0A%20%20%20%20edges%20%7B%0A%20%20%20%20%20%20node%20%7B%0A%20%20%20%20%20%20%20%20id%0A%20%20%20%20%20%20%20%20path%0A%20%20%20%20%20%20%7D%0A%20%20%20%20%7D%0A%20%20%7D%0A%7D%0A&explorerIsOpen=false"
  width="600"
  height="300"
/>

### GraphQL 결과 필터링하기

`eq` (equals), `ne` (not equals), `in` 및 `regex`와 같은 연산자를 특정 필드에 사용하여 쿼리 결과를 필터링 할 수 있습니다.

이 레시피를 위해서, GraphQL 데이터 레이어에 필터 할 노드들이 있는 Gatsby 사이트가 필요합니다. 모든 사이트들은 `allSitePage` 와 같은 자동으로 생성된 노드들을 가지고 있으며, `gatsby-config.js` 파일에 `gatsby-source-filesystem` 과 `gatsby-transformer-remark` 같은 source 및 transformer 플러그인들을 설치하여 `allMarkdownRemark`를 생성하면 더 많은 노드를 추가하 수 있습니다.

#### 사전 준비

- [Gatsby 사이트](/docs/quick-start)
- 접두사 `all`이 있는 쿼리 가능한 필드. 예: `allSitePage` 또는 `allMarkdownRemark`

#### 지시 사항

1. `gatsby develop`을 실행하여 개발 서버를 시작하세요.
2. 브라우저 탭을 열고 `http://localhost:8000/___graphql`에 접속하세요.
3. `allMarkdownRemark`와 같이 'all' 접두사가 붙은 필드를 사용하여 에디터에 쿼리를 추가하세요. (노드 목록을 반환됩니다)

```graphql
{
  allMarkdownRemark {
    edges {
      node {
        frontmatter {
          title
          categories
        }
      }
    }
  }
}
```

4. `allMarkdownRemark` 필드에 `filter` 인수를 추가하고 필터링하려는 필드가있는 객체를 제공하세요. 이 예에서 Markdown 컨텐츠는 frontmatter 메타 데이터의 `categories` 속성으로 필터링합니다. 다음 값은 연산자이며, 'magical creatures' 이라는 값을 가진 `eq` 연산자를 사용합니다.

```graphql
{
  allMarkdownRemark(filter: {frontmatter: {categories: {eq: "magical creatures"}}}) { // highlight-line
    edges {
      node {
        frontmatter {
          title
          categories
        }
      }
    }
  }
}
```

5. GraphiQL 페이지에서 play 버튼을 클릭합니다. filter 매개변수에 매치되는 데이터만 반환되는데, 이 예에서는 'magical creatures' 라는 category 값으로 태깅된 Markdown 파일 소스들의 내용만 반환됩니다.

#### 추가 정보

- [filtering에 대한 Gatsby GraphQL 레퍼런스](/docs/graphql-reference/#filter)
- [사용 가능한 연산자 전체 목록](/docs/graphql-reference/#complete-list-of-possible-operators)
- [Gaysby의 GraphQL 데이터 API의 노드](/docs/node-interface/)에 대한 학습
- 라이브 예제:

<iframe
  title="Filtering data"
  src="https://711808k40x.sse.codesandbox.io/___graphql?query=%7B%0A%20%20allMarkdownRemark(filter%3A%20%7Bfrontmatter%3A%20%7Bcategories%3A%20%7Beq%3A%20%22magical%20creatures%22%7D%7D%7D)%20%7B%0A%20%20%20%20edges%20%7B%0A%20%20%20%20%20%20node%20%7B%0A%20%20%20%20%20%20%20%20frontmatter%20%7B%0A%20%20%20%20%20%20%20%20%20%20title%0A%20%20%20%20%20%20%20%20%20%20categories%0A%20%20%20%20%20%20%20%20%7D%0A%20%20%20%20%20%20%7D%0A%20%20%20%20%7D%0A%20%20%7D%0A%7D%0A&explorerIsOpen=false"
  width="600"
  height="300"
/>

### GraphQL 쿼리 별칭

GraphQL 쿼리의 필드 이름을 별칭으로 바꿀 수 있습니다.

동일한 데이터 소스에서 두 개의 쿼리를 실행하려면 동일한 이름의 두 쿼리와 이름 충돌을 피하기 위해 별명을 사용할 수 있습니다.


#### 지시 사항

1. `gatsby develop`을 실행하여 개발 서버를 시작하세요.
2. Open the GraphiQL explorer in a browser tab at: `http://localhost:8000/___graphql`
2. 브라우저 탭을 열고 `http://localhost:8000/___graphql`에 접속하여 GraphiQL에 접속하세요.
3. 에디터에서 `allFile` 과 같이 같은 이름을 가진 두개의 필드를 추가하세요.

```graphql
{
  allFile {
    totalCount
  }
  allFile {
    pageInfo {
      currentPage
    }
  }
}
```

4. GraphQL 스키마에서 필드 이름 앞에 사용할 별칭 이름을 콜론으로 구분하여 추가해주세요. (예. `[alias-name]: [original-name]`)

```graphql
{
  fileCount: allFile { // highlight-line
    totalCount
  }
  filePageInfo: allFile { // highlight-line
    pageInfo {
      currentPage
    }
  }
}
```

5. GraphiQL 페이지에서 play 버튼을 클릭하면 제공 한 별칭 이름을 가진 2개의 객체가 출력됩니다.

#### 추가 정보

- [Gatsby GraphQL 별칭 레퍼런스](/docs/graphql-reference/#aliasing)
- 라이브 예제:

<iframe
  title="Using aliases"
  src="https://711808k40x.sse.codesandbox.io/___graphql?query=%7B%0A%20%20fileCount%3A%20allFile%20%7B%20%0A%20%20%20%20totalCount%0A%20%20%7D%0A%20%20filePageInfo%3A%20allFile%20%7B%0A%20%20%20%20pageInfo%20%7B%0A%20%20%20%20%20%20currentPage%0A%20%20%20%20%7D%0A%20%20%7D%0A%7D%0A&explorerIsOpen=false"
  width="600"
  height="300"
/>

### GraphQL 쿼리 프래그먼트

GraphQL 프래그먼트는 재사용 가능한 쿼리의 공유 가능한 청크입니다.

이것을 사용하여 쿼리간에 여러 필드를 공유하거나 데이터와 함께 구성 요소를 배치 할 수 있습니다.

#### 지시 사항

1. Declare a `graphql` template string with a Fragment in it. The fragment should be made up of the keyword `fragment`, a name, the GraphQL type it is associated with (in this case of type `Site`, as demonstrated by `on Site`), and the fields that make up the fragment:

1. 프래그먼트가 있는 `graphql` 템플릿 문자열을 선언합니다. 프래그먼트는 키워드 `fragment`, 이름, 그와 관련된 GraphQL 타입 (`Site` 타입의 경우 `on Site` 로 명시) 및 프래그먼트를 구성하는 필드로 구성되어야합니다.

```jsx
export const query = graphql`
  // highlight-start
  fragment SiteInformation on Site {
    title
    description
  }
  // highlight-end
`
```

2. 자 이제, 프래그먼트로 명시한 타입의 필드 쿼리에 프래그먼트을 포함 시켜주세요. 모든 필드를 독립적으로 선언하지 않고도 해당 필드가 포함됩니다:

```diff
export const pageQuery = graphql`
  query SiteQuery {
    site {
-     title
-     description
+   ...SiteInformation
    }
  }
`
```

**주의**: Gatsby에서 프래그먼트를 임포트할 필요는 없습니다. 프래그먼트를 사용하여 쿼리를 내보내면(exporting) 해당 프래그먼트를 프로젝트의 _모든_ 쿼리에서 사용할 수 있습니다.

프래그먼트는 다른 프래그먼트 안에 중첩 될 수 있으며 동일한 쿼리에서 여러 프래그먼트를 사용할 수 있습니다.

#### 추가 정보

- [프래그먼트를 사용하는 간단한 예제 저장소](https://github.com/gatsbyjs/gatsby/tree/master/examples/using-fragments)
- [Gatsby GraphQL 프래그먼트 레퍼런스](/docs/graphql-reference/#fragments)
- [Gatsby 이미지 쿼리 프래그먼트](/docs/gatsby-image/#image-query-fragments)
- [Example repo with co-located data](https://github.com/gatsbyjs/gatsby/tree/master/examples/gatsbygram)
- [같은 장소의 데이터를 여러곳에서 사용하는 예제 저장소](https://github.com/gatsbyjs/gatsby/tree/master/examples/gatsbygram)

## 7. 이미지 사용하기

### 웹팩으로 이미지 가져오기

웹팩을 사용하여 이미지를 JavaScript 모듈로 바로 가져올 수 있습니다. 이 프로세스는 이미지를 자동으로 축소하고 사이트의 `public` 폴더에 복사하여 일반 파일 경로와 같은 HTML `<img>` 요소에 전달할 수있는 동적 이미지 URL을 제공합니다.

<EggheadEmbed
  lessonLink="https://egghead.io/lessons/gatsby-import-a-local-image-into-a-gatsby-component-with-webpack"
  lessonTitle="Import a Local Image into a Gatsby Component with webpack"
/>

#### 사전 준비

- React 컴포넌트를 내보내기 하는 `.js` 파일을 가진 [Gatsby 사이트](/docs/quick-start)
- `src` 폴더안의 이미지 파일 (`.jpg`, `.png`, `.gif`, `.svg` 등)

#### 지시 사항

1. `src` 폴더의 경로에서 파일을 가져옵니다:

```jsx:title=src/pages/index.js
import React from "react"
// Tell webpack this JS file uses this image
import FiestaImg from "../assets/fiesta.jpg" // highlight-line
```

2. `index.js` 안에서, `src` 속성이 있는 `<img>` 태그를 webpack으로부터 가져온 변수로 (예제에선 `FiestaImg`) 추가하고 `alt` 속성을 추가합니다. [이미지 태그 설명 참고](https://webaim.org/techniques/alttext/):

```jsx:title=src/pages/index.js
import React from "react"
import FiestaImg from "../assets/fiesta.jpg"

export default () => (
  // The import result is the URL of your image
  <img src={FiestaImg} alt="A dog smiling in a party hat" /> // highlight-line
)
```

3. `gatsby develop`을 실행하여 개발 서버를 시작하세요.
4. 브라우저에서 이미지를 확인하세요: `http://localhost:8000/`

#### 추가 정보

- [webpack으로 이미지를 가져오는 예제 코드 저장소](https://github.com/gatsbyjs/gatsby/tree/master/examples/recipe-webpack-image)
- [Gatsby의 모든 이미지 사용 기법에 대한 추가 정보](/docs/images-and-files/)

### 'static' 폴더의 이미지 참조하기

webpack을 통해 자원을 임포트하는 대신 빌드 할 때 자동으로 `public` 폴더에 복사되는 `static` 폴더의 컨텐츠를 사용 할 수 있습니다.

이는 [특정 사용 사례](/docs/static-folder/#when-to-use-the-static-folder)에 대한 **예외적인 방법**이며 Gatsby의 최적화를 활용하려면 [웹팩으로 이미지 가져오기](#import-an-image-into-a-component-with-webpack)와 같은 다른 방법을 사용하는 것이 좋습니다.

<EggheadEmbed
  lessonLink="https://egghead.io/lessons/gatsby-use-a-local-image-from-the-static-folder-in-a-gatsby-component"
  lessonTitle="Use a local image from the static folder in a Gatsby component"
/>

#### 사전 준비

- React 컴포넌트를 내보내기 하는 `.js` 파일을 가진 [Gatsby 사이트](/docs/quick-start)
- `static` 폴더안의 이미지 파일 (`.jpg`, `.png`, `.gif`, `.svg` 등)

#### 지시 사항

1. 이미지가 프로젝트 루트 밑 `static` 폴더에 있는지 확인하세요. 프로젝트 구조는 다음과 같습니다:

```
├── gatsby-config.js
├── src
│   └── pages
│       └── index.js
├── static
│       └── fiesta.jpg
```

2. `index.js` 안에서, `src` 속성이 있는 `<img>` 태그를 `static` 폴더로 부터의 상태경로로 명시하고, `alt` 속성을 추가합니다. [이미지 태그 설명 참고](https://webaim.org/techniques/alttext/):

```jsx:title=src/pages/index.js
import React from "react"

export default () => (
  <img src={`fiesta.jpg`} alt="A dog smiling in a party hat" /> // highlight-line
)
```

3. `gatsby develop`을 실행하여 개발 서버를 시작하세요.
4. 브라우저에서 이미지를 확인하세요: `http://localhost:8000/`

#### 추가 정보

- [static 폴더에서 이미지를 참조하는 예제 코드 저장소](https://github.com/gatsbyjs/gatsby/tree/master/examples/recipe-static-image)
- [static 폴더 사용하기](/docs/static-folder/)
- [Gatsby의 모든 이미지 사용 기법에 대한 추가 정보](/docs/images-and-files/)

### gatsby-image를 사용하여 로컬 이미지 최적화 및 쿼리하기

`gatsby-image` 플러그인은 사이트의 이미지 최적화와 관련된 많은 고통을 덜어줍니다.

Gatsby는 GraphQL로 쿼리하여 Gatsby의 image 컴포넌트로 전달할 수 있는 최적화 된 리소스를 생성합니다. 이렇게하면 여러 사이즈의 이미지 만들어 적시에 로딩시키는 것과 같은 어려운 작업을 처리 할 수 있습니다.

#### 사전 준비

- `gatsby-image`, `gatsby-transformer-sharp`, 및 `gatsby-plugin-sharp` 패키지 설치 및  `gatsby-config`의 plugins 배열에 추가
- `gatsby-source-filesystem`과 같은 플러그인을 사용하여 `gatsby-config`에서 [소스로 제공되는 이미지](/packages/gatsby-image/#install)

#### 지시 사항

1. 먼저 `gatsby-image`에서 `Img`와, `gatsby`에서 `graphql` 및 `useStaticQuery`를 가져옵니다.

```jsx
import { useStaticQuery, graphql } from "gatsby" // to query for image data
import Img from "gatsby-image" // to take image data and render it
```

2. 이미지 데이터를 얻기위한 쿼리를 작성하고 데이터를 `<Img />` 구성 요소에 전달합니다:

다음 옵션 중 하나 또는 여러개의 조합을 선택하세요.

a. 파일 [경로](/docs/content-and-data/)로 쿼리 된 단일 이미지 (예: `images/corgi.jpg`)

```jsx
const data = useStaticQuery(graphql`
  query {
    file(relativePath: { eq: "corgi.jpg" }) { // highlight-line
      childImageSharp {
        fluid {
          base64
          aspectRatio
          src
          srcSet
          sizes
        }
      }
    }
  }
`)

return (
  <Img fluid={data.file.childImageSharp.fluid} alt="A corgi smiling happily" />
)
```

b. [GraphQL fragment](/docs/using-fragments/)를 사용하여 필요한 필드를 더 간결하게 쿼리

```jsx
const data = useStaticQuery(graphql`
  query {
    file(relativePath: { eq: "corgi.jpg" }) {
      childImageSharp {
        fluid {
          ...GatsbyImageSharpFluid // highlight-line
        }
      }
    }
  }
`)

return (
  <Img fluid={data.file.childImageSharp.fluid} alt="A corgi smiling happily" />
)
```

c. 디렉토리(예: `images/dogs`)로 부터 여러 이미지를 `extension` 및 `relativeDirectory` 필드로 [필터링](/docs/graphql-reference/#filter) 한 다음 `Img` 구성 요소에 매핑


```jsx
const data = useStaticQuery(graphql`
  query {
    allFile(
      // highlight-start
      filter: {
        extension: { regex: "/(jpg)|(png)|(jpeg)/" }
        relativeDirectory: { eq: "dogs" }
      }
      // highlight-end
    ) {
      edges {
        node {
          base
          childImageSharp {
            fluid {
              ...GatsbyImageSharpFluid
            }
          }
        }
      }
    }
  }
`)

return (
  <div>
    // highlight-start
    {data.allFile.edges.map(image => (
      <Img
        fluid={image.node.childImageSharp.fluid}
        alt={image.node.base.split(".")[0]} // only use section of the file extension with the filename
      />
    ))}
    // highlight-end
  </div>
)
```

**주의**: 이 방법은 접근성을 위한 `alt` 텍스트를 이미지와 일치시키기 어려울 수 있습니다. 이 예제에서는 `hat in a party hat.jpg`와 같이 파일 이름에 `alt` 텍스트가 포함 된 이미지를 사용합니다.

d. `fluid` 대신 `fixed` 필드를 사용한 고정 크기의 이미지

```jsx
const data = useStaticQuery(graphql`
  query {
    file(relativePath: { eq: "corgi.jpg" }) {
      childImageSharp {
        fixed(width: 250, height: 250) { // highlight-line
          ...GatsbyImageSharpFixed
        }
      }
    }
  }
`)
return (
  <Img fixed={data.file.childImageSharp.fixed} alt="A corgi smiling happily" />
)
```

e. `maxWidth`를 사용한 고정 크기의 이미지

```jsx
const data = useStaticQuery(graphql`
  query {
    file(relativePath: { eq: "corgi.jpg" }) {
      childImageSharp {
        fixed(maxWidth: 250) { // highlight-line
          ...GatsbyImageSharpFixed
        }
      }
    }
  }
`)
return (
  <Img fixed={data.file.childImageSharp.fixed} alt="A corgi smiling happily" /> // highlight-line
)
```

f. maxWidth(픽셀)와 quality(기본값은 50, 즉 50%)를 가진 컨테이너를 채우는 이미지 

```jsx
const data = useStaticQuery(graphql`
  query {
    file(relativePath: { eq: "corgi.jpg" }) {
      childImageSharp {
        fluid(maxWidth: 800, quality: 75) { // highlight-line
          ...GatsbyImageSharpFluid
        }
      }
    }
  }
`)

return (
  <Img fluid={data.file.childImageSharp.fluid} alt="A corgi smiling happily" />
)
```

3. (선택사항) 다른 컴포넌트와 마찬가지로 `<Img />`에 인라인 스타일을 추가합니다

```jsx
<Img
  fluid={data.file.childImageSharp.fluid}
  alt="A corgi smiling happily"
  style={{ border: "2px solid rebeccapurple", borderRadius: 5, height: 250 }} // highlight-line
/>
```

4. (선택사항) GraphQL 쿼리가 반환 한 `aspectRatio` 필드를 `<Img />` 컴포넌트에 전달하기 전에 이미지를 원하는 종횡비로 강제 재정의 합니다

```jsx
<Img
  fluid={{
    ...data.file.childImageSharp.fluid,
    aspectRatio: 1.6, // 1280 / 800 = 1.6
  }}
  alt="A corgi smiling happily"
/>
```

5. `gatsby develop`을 실행하여 파일 시스템으로부터 (아직 수행하지 않은 경우)이미지를 생성하고 생성된 이미지를 캐시하세요

#### 추가 정보

- [여기의 예제를 보여주는 코드 저장소](https://github.com/gatsbyjs/gatsby/tree/master/examples/recipes-gatsby-image)
- [Gatsby Image API](/docs/gatsby-image/)
- [Gatsby Image 사용하기](/docs/using-gatsby-image)
- [Gatsby에서 이미지 사용에 대한 추가 정보](/docs/working-with-images/)
- [여기 내용을 단계별로 설명하는 무료 egghead.io 영상](https://egghead.io/playlists/using-gatsby-image-with-gatsby-ea85129e)

### gatsby-image를 사용하여 post frontmatter에서 이미지 최적화 및 쿼리
블로그 게시물의 추천 이미지와 같은 사용 사례에서도 `gatsby-image`를 사용할 수 있습니다. `Img` 컴포넌트는 가공된 이미지 데이터가 필요한데, 이 이미지 데이터는 frontmatter에 URL을 가지고 있는 `.md` 또는 `.mdx` 파일로부터 로컬(또는 원격) 파일에서 가져올 수 있습니다.

마크다운의 인라인 이미지(`![]()` 문법을 사용하는)를 위해서는 [`gatsby-remark-images`](/packages/gatsby-remark-images/) 같은 플러그인 사용을 고려하세요.

#### 사전 준비
- `gatsby-image`, `gatsby-transformer-sharp` 및 `gatsby-plugin-sharp` 패키지가 설치 및 `gatsby-config`의 plugins 배열에 추가
- `gatsby-source-filesystem`과 같은 플러그인을 사용하여 `gatsby-config`에서 [소스로 제공되는 이미지](/packages/gatsby-image/#install)
- `gatsby-config`에서 소스로 제공되는 frontmatter에 image URL들을 가진 Markdown 파일들
- [`createPages`](https://www.gatsbyjs.org/docs/node-apis/#createPages)를 사용하여 마크다운 파일로부터 생성된 페이지들

#### 지시 사항

1. 프로젝트의 마크다운 파일이 유효한 경로의 이미지 URL을 가지고 있는지 확인하세요

```mdx:title=post.mdx
---
title: My First Post
featuredImage: ./corgi.png // highlight-line
---

Post content...
```

2. `gatsby-node.js` 에서`createPages`가 호출 될 때 컨텍스트에 고유 식별자(이 예제에서는 slug)가 전달되는지 확인하세요. 이 식별자는 나중에 Layout 컴포넌트의 GraphQL 쿼리로 전달됩니다.

```js:title=gatsby-node.js
exports.createPages = async ({ graphql, actions }) => {
  const { createPage } = actions

  // query for all markdown

  result.data.allMdx.edges.forEach(({ node }) => {
    createPage({
      path: node.fields.slug,
      component: path.resolve(`./src/components/markdown-layout.js`),
      // highlight-start
      context: {
        slug: node.fields.slug,
      },
      // highlight-end
    })
  })
}
```

3. 이제 `gatsby-image`에서 `Img`를 `gatsby`에서 `graphql`을 템플릿 컴포넌트로 가져와서 
전달된 `slug` 기반의 이미지 데이터를 얻기 위해 [pageQuery](/docs/page-query/)를 작성한 다음 그 데이터를 `<Img />`컴포넌트에 전달하세요:

```jsx:title=markdown-layout.jsx
import React from "react"
import { graphql } from "gatsby" // highlight-line
import Img from "gatsby-image" // highlight-line

export default ({ children, data }) => (
  <main>
    // highlight-start
    <Img
      fluid={data.markdown.frontmatter.image.childImageSharp.fluid}
      alt="A corgi smiling happily"
    />
    // highlight-end
    {children}
  </main>
)

// highlight-start
export const pageQuery = graphql`
  query PostQuery($slug: String) {
    markdown: mdx(fields: { slug: { eq: $slug } }) {
      id
      frontmatter {
        image {
          childImageSharp {
            fluid {
              ...GatsbyImageSharpFluid
            }
          }
        }
      }
    }
  }
`
// highlight-end
```

4. `gatsby develop`을 실행하면 파일 시스템안의 소스 파일의 이미지가 생성됩니다.

#### 추가 정보

- [이 레시피를 사용하는 예제 코드 저장소](https://github.com/gatsbyjs/gatsby/tree/master/examples/recipes-gatsby-image)
- [frontmatter에 추천 이미지 사용](/docs/working-with-images-in-markdown/#featured-images-with-frontmatter-metadata)
- [Gatsby 이미지 API](/docs/gatsby-image/)
- [gatsby-image 사용하기](/docs/using-gatsby-image)
- [Gatsby의 이미지 작업에 대한 추가 정보](/docs/working-with-images/)

## 8. 데이터 변환
Gatsby에서 데이터 변환은 플러그인 중심입니다. Transformer 플러그인은 소스 플러그인을 사용하여 가져온 데이터를 더 유용한 것으로 가공합니다 (예: JSON을 JavaScript 객체 등으로).

### 마크다운을 HTML로 변환

`gatsby-transformer-remark` 플러그인은 마크다운 파일을 HTML로 변환 할 수 있습니다.

#### 사전 준비

- `gatsby-config.js` 및 `index.js` 페이지가있는 Gatsby 사이트
- Gatsby 사이트 `src` 디렉토리에 저장된 마크 다운 파일
- `gatsby-source-filesystem`과 같은 소스 플러그인 설치
- `gatsby-transformer-remark` 플러그인 설치

#### 지시 사항

1. `gatsby-config.js`에 transformer 플러그인을 추가하세요:

```js:title=gatsby-config.js
plugins: [
  // not shown: gatsby-source-filesystem for creating nodes to transform
  `gatsby-transformer-remark`
],
```

2. `MarkdownRemark` 노드를 가져오기 위해 Gatsby 사이트의 `index.js` 파일에 GraphQL 쿼리를 추가합니다:

```jsx:title=src/pages/index.js
export const query = graphql`
  query {
    allMarkdownRemark {
      totalCount
      edges {
        node {
          id
          frontmatter {
            title
            date(formatString: "DD MMMM, YYYY")
          }
          excerpt
        }
      }
    }
  }
`
```

3. 개발 서버를 재시작하고 `http://localhost:8000/___graphql`에서 GraphiQL을 엽니다. `MarkdownRemark` 노드에서 사용 가능한 필드를 탐색합니다.

#### 추가 정보

- `gatsby-transformer-remark`를 사용한 [마크다운을 HTML로 변환하는 방법에 대한 튜토리얼](/tutorial/part-six/#transformer-plugins)
- [Gatsby 플러그인 라이브러리](/plugins/?=transformer)에서 사용 가능한 transformer 플러그인 탐색

## 9. 사이트 배포

쇼타임! 사이트가 마음에 드시나요? 배포 할 준비가 되었습니다!

### 배포 준비

#### 사전 준비

- [Gatsby 사이트](/docs/quick-start)
- [Gatsby CLI](/docs/gatsby-cli) 설치

#### 지시 사항

1. 개발 서버가 실행 중이면 중지하세요 (대부분의 경우 명령줄에서 `Ctrl + C`)

2. 루트 디렉토리 (`/`)인 표준 사이트 경로에 대해서는 명령줄에서 Gatsby CLI를 사용하여 `gatsby build`를 실행하세요. 빌드 된 파일은 `public` 폴더에 생성됩니다.

```shell
gatsby build
```

3. `/` 이외의 사이트 경로 (예 :`/site-name/`)를 포함 시키려면 `gatsby-config.js`에 다음을 추가하고 `yourpathprefix`를 원하는 경로로 바꿔주세요:

```js:title=gatsby-config.js
module.exports = {
  pathPrefix: `/yourpathprefix`,
}
```

이렇게 하는데에는 몇 가지 이유가 있습니다. 예를 들어 Gatsby로 만들어지지 않은 다른 사이트 도메인 위에 Gatsby로 구축 된 블로그를 호스팅 할 수 있습니다. 기본 사이트는 `example.com`으로 연결되며 Gatsby 사이트는 `example.com/blog` 와 같은 경로 접두사를 사용할 수 있습니다.

4. `gatsby-config.js`에 경로 접두사가 설정되어 있으면 `--prefix-paths` 플래그와 함께 `gatsby build`를 실행하여 모든 Gatsby 사이트 URL과 `<Link>` 태그의 시작 부분에 접두사를 자동으로 추가하세요.

```shell
gatsby build --prefix-paths
```

5. `gatsby build`를 실행할 때 `gatsby develop`때와 사이트가 동일하게 보이는지 확인하세요. 사이트를 빌드 할 때 `gatsby serve`를 실행하면 완성 된 제품을 실제로 배포하기 전에 테스트 (그리고 필요한 경우 디버그) 할 수 있습니다.

```shell
gatsby build && gatsby serve
```

#### 추가 정보

- [튜토리얼 파트1](/tutorial/part-one/#deploying-a-gatsby-site) 에서 예제 사이트 구축 및 배포를 안내합니다
- [성능 최정화](/docs/performance/)에 대해 알아보기
- [다른 배포 관련 주제](/docs/preparing-for-deployment/)에 대해 읽어보기
- 특정 호스팅 플랫폼 및 배포 방법에 대해서는 [배포 문서](/docs/deploying-and-hosting/)를 확인하세요

### Netlify에 배포

[`netlify-cli`](https://www.netlify.com/docs/cli/)를 사용하여 명령줄 인터페이스를 벗어나지 않고 Gatsby 애플리케이션을 배포하세요.

#### 사전 준비

- 단일 `index.js` 컴포넌트가 있는 [Gatsby 사이트](/docs/quick-start)
- [netlify-cli](https://www.npmjs.com/package/netlify-cli) 패키지 설치
- [Gatsby CLI](/docs/gatsby-cli) 설치

#### 지시 사항

1. `gatsby build`를 사용하여 gatsby 애플리케이션을 빌드하세요.

2. `netlify login`을 사용하여 Netlify에 로그인하세요.

3. `netlify init` 명령을 실행하세요. "신규 사이트 생성 및 구성"옵션을 선택하세요.

4. 원하는 경우 사용자 정의 웹 사이트 이름을 선택하거나 Enter 키를 눌러 임의의 웹 사이트 이름을 받으세요.

5. [팀](https://www.netlify.com/docs/teams/)을 선택하세요.

6. 배포 경로를 `public/`으로 변경하세요.

7. `netlify deploy --prod`를 사용하여 프로덕션에 배포하기 전에 모든 것이 잘 보이는지 확인하세요.

#### 추가 정보

- [Netlify에서 호스팅](/docs/hosting-on-netlify)
- [gatsby-plugin-netlify](/packages/gatsby-plugin-netlify)

### ZEIT Now에 배포

[Now CLI](https://zeit.co/download)를 사용하여 명령줄 인터페이스를 벗어나지 않고 Gatsby 애플리케이션을 배포하세요.

#### 사전 준비

- [ZEIT Now](https://zeit.co/signup) 계정
- 단일 `index.js` 컴포넌트가 있는 [Gatsby 사이트](/docs/quick-start)
- [Now CLI](https://zeit.co/download) 패키지 설치
- [Gatsby CLI](/docs/gatsby-cli) 설치

#### 지시 사항

1. `now login`을 사용하여 Now CLI에 로그인

2. 터미널에서 Gatsby.js 애플리케이션 디렉토리로 이동하세요.

3. `now`를 실행하여 배포하세요

#### 추가 정보

- [ZEIT Now에 배포하기](/docs/deploying-to-zeit-now/)
