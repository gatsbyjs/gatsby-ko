---
title: "Recipes: Styling with CSS"
tableOfContentsDepth: 1
---

웹 사이트에 스타일을 추가하는 정말 많은 방법이 있습니다. Gatsby는 공식 및 커뮤니티 플러그인을 통해 거의 모든 옵션을 지원합니다.

## Layout 컴포넌트 없이 전역 CSS 파일들 사용하기

### 사전준비

- index 페이지 컴포넌트를 가진 [Gatsby 사이트](/docs/quick-start/)
- `gatsby-browser.js` 파일

### 수행 절차

1. `src/styles/global.css` 파일 이름으로 전역 CSS 파일을 생성하고 아래 코드를 붙여 넣습니다:

```css:title=src/styles/global.css
html {
  background-color: lavenderblush;
}

p {
  color: maroon;
}
```

2. `gatsby-browser.js` 파일 안에서 아래와 같이 전역 CSS 파일을 임포트 합니다:

```javascript:title=gatsby-browser.js
import "./src/styles/global.css"
```

> **Note:** `require('./src/styles/global.css')`이런 문법을 사용하여 `gatsby-config.js` 파일 안에서 전역 CSS 파일을 임포트 할 수도 있습니다.

3. `gatsby develop`을 실행하여 사이트 곳곳에 적용된 전역 스타일을 확인해주세요.

> **참고:** 이 방법은 CSS-in-JS를 사용하여 스타일을 정의하는 경우 적합하지 않으며 이 경우 모든 공유 컴포넌트가 포함 된 layout 페이지를 사용해야합니다. 이것은 다음 레시피에서 다룹니다.

### 추가 정보

- [Layout 컴포넌트 없이 전역 스타일 추가하기](/docs/global-css/#adding-global-styles-without-a-layout-component)에 대한 추가 정보

## Layout 컴포넌트에서 전역 스타일 사용하기

### 사전준비

- index 페이지 컴포넌트를 가진 [Gatsby 사이트](/docs/quick-start/)

### 수행 절차

[공유되는 layout 컴포넌트](/tutorial/part-three/#your-first-layout-component)에 전역 스타일을 추가 할 수 있습니다. 이 컴포넌트는 헤더 또는 풋터 글과 같이 사이트 전체의 공통적인 항목에 사용됩니다.

1. `/src/components` 디렉토리가 없다면 디렉토리를 생성해주세요.

2. components 디렉토리 안에 `layout.css`와 `layout.js` 두개의 파일을 생성해주세요.

3. `layout.css`에 다음을 추가해주세요:

```css:title=/src/components/layout.css
body {
  background: red;
}
```

4. CSS 파일을 임포트 하고 layout 마크업을 익스포트 하도록 `layout.js`를 수정합니다:

```jsx:title=/src/components/layout.js
import React from "react"
import "./layout.css"

export default ({ children }) => <div>{children}</div>
```

5. 이제 사이트의 홈페이지인 `/src/pages/index.js`를 수정하여 새로운 layout 컴포넌트를 사용합니다:

```jsx:title=/src/pages/index.js
import React from "react"
import Layout from "../components/layout"

export default () => <Layout>Hello world!</Layout>
```

### 추가 정보

- [전역 CSS 파일을 사용한 표준 스타일링](/docs/global-css/)
- [layout 컴포넌트에 대한 추가 정보](/tutorial/part-three)

## Styled Components 사용하기

### 사전준비

- index 페이지 컴포넌트를 가진 [Gatsby 사이트](/docs/quick-start/)
- `package.json`안에 [gatsby-plugin-styled-components, styled-components, and babel-plugin-styled-components](/packages/gatsby-plugin-styled-components/) 설치

### 수행 절차

1. `gatsby-config.js` 파일 안에서 `gatsby-plugin-styled-components`를 추가하세요

```javascript:title=gatsby-config.js
module.exports = {
  plugins: [`gatsby-plugin-styled-components`],
}
```

2. index 페이지 컴포넌트(`src/pages/index.js`)를 열고 `styled-components` 패키지를 임포트 하세요

3. 각 구성 요소에 대한 스타일 블록을 작성하여 컴포넌트를 스타일링 하세요

4. JSX 안에 스타일링된 컴포넌트를 포함시켜 페이지에 적용하세요

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

4. `gatsby develop`을 실행하여 변경을 확인하세요

### 추가 정보

- [styled-components 사용에 대한 추가 정보](/docs/styled-components/)
- [Egghead 수업](https://egghead.io/lessons/gatsby-style-gatsby-sites-with-styled-components)

## CSS Modules 사용하기

### 사전준비

- index 페이지 컴포넌트를 가진 [Gatsby 사이트](/docs/quick-start/)

### 수행 절차

1. `src/pages/index.module.css` 파일 이름으로 CSS module 을 생성하고 다음을 모두 모듈에 붙여 넣습니다:

```css:title=src/pages/index.module.css
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

**주의:**
파일 확장자가 `.css` 가 아닌 `.module.css`인것에 유의해야 하는데, 이는 Gatsby에게 이것이 CSS module임을 알려줍니다.

### 추가 정보

- [CSS Modules 사용하기](/tutorial/part-two/#css-modules)에 대한 추가 정보
- [CSS modules 사용하기 라이브 예제](https://github.com/gatsbyjs/gatsby/blob/master/examples/using-css-modules)

## Sass/SCSS 사용하기

Sass는 CSS의 확장으로 중첩 규칙, 변수, 믹스인 등과 같은 고급 기능을 제공합니다.

Sass에는 2 가지 구문이 있습니다. 가장 일반적으로 사용되는 구문은 "SCSS"이며, CSS의 상위 집합입니다. 즉, 모든 유효한 CSS 구문은 유효한 SCSS 구문입니다. SCSS 파일은 .scss 확장자를 사용합니다.

Sass는 .scss 및 .sass 파일을 .css 파일로 컴파일하므로, 이런 고급 기능들을 사용하여 스타일 시트를 작성할 수 있습니다.

### 사전준비

- [Gatsby 사이트](/docs/quick-start/).

### 수행 절차

1. [gatsby-plugin-sass](https://www.gatsbyjs.org/packages/gatsby-plugin-sass/) Gatsby 플러그인과 `node-sass` 설치하세요.

`npm install --save node-sass gatsby-plugin-sass`

2. `gatsby-config.js` 파일에 이 플러그인을 추가하세요.

```javascript:title=gatsby-config.js
plugins: [`gatsby-plugin-sass`],
```

3. 스타일 시트를 `.sass` 또는 `.scss` 파일로 작성하고 임포트 합니다. 스타일을 가져 오는 방법을 모르는 경우 [CSS로 스타일링](/docs/recipes/#2-styling-with-css)을 살펴보세요

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

_주의: 이전 CSS module 레시피에서 언급했듯이 Sass/SCSS 파일도 module로 사용할 수 있습니다. 차이점은 확장자가 `.css`가 아닌 `.scss` 또는 `.sass` 여야합니다._

### 추가 정보

- [.sass와 .scss의 차이점](https://responsivedesign.is/articles/difference-between-sass-and-scss/)
- [공식 Sass 웹사이트의 Sass 가이드](https://sass-lang.com/guide)
- [더 많은 설명과 더 많은 리소스를 제공하는 더 완전한 Sass 설치 튜토리얼](https://www.gatsbyjs.org/docs/sass/)

## 로컬 폰트 파일 적용하기

### 사전준비

- [Gatsby 사이트](/docs/quick-start/)
- `.woff2`, `.ttf` 등의 폰트 파일

### 수행 절차

1. 폰트 파일을 Gatsby 프로젝트에 `src/fonts/fontname.woff2`와 같은 경로에 복사하세요.

```text
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

HTML `body` 요소를 대상으로하면 글꼴이 페이지의 대부분의 텍스트에 적용됩니다. 추가 CSS는 `button` 또는 `textarea`와 같은 다른 요소를 대상으로 할 수 있습니다.

위의 단계에 따라 폰트가 업데이트되지 않으면 관련 CSS에서 기존 font-family가 교체 되었는지 확인하세요.

### 추가 정보

- [파일로 에셋 임포트 하기](/docs/importing-assets-into-files/)에 대한 추가 정보

## Emotion 사용하기

[Emotion](https://emotion.sh)은 인라인 CSS 스타일과 styled components 를 모두 지원하는 강력한 CSS-in-JS 라이브러리입니다. 각 스타일링 기능을 개별적으로 또는 동일한 파일에서 함께 사용할 수 있습니다.

### 사전준비

- [Gatsby 사이트](/docs/quick-start)

### 수행 절차

1. [Gatsby Emotion 플러그인](/packages/gatsby-plugin-emotion/)과 Emotion 패키지 설치.

```shell
npm install --save gatsby-plugin-emotion @emotion/core @emotion/styled
```

2. `gatsby-plugin-emotion` 을 `gatsby-config.js` 파일에 추가하세요:

```javascript:title=gatsby-config.js
module.exports = {
  plugins: [`gatsby-plugin-emotion`],
}
```

3. Gatsby 사이트의 페이지가 아직 없다면 `src/pages/emotion-sample.js`로 페이지를 하나 만드세요.

Emotion의 `css` core 패키지를 임포트 합니다. 그런 다음 `css` prop을 사용하여 컴포넌트 내의 모든 요소에 [Emotion object styles](https://emotion.sh/docs/object-styles)를 추가 할 수 있습니다:

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

4. Emotion의 [styled components](https://emotion.sh/docs/styled)를 사용하려면 패키지를 임포트하고 `styled` 함수를 사용하여 정의하세요.

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

### 추가 정보

- [Gatsby에서 Emotion 사용하기](/docs/emotion/)
- [Emotion 웹사이트](https://emotion.sh)
- [Emotion과 Gatsby 시작하기](https://egghead.io/lessons/gatsby-getting-started-with-emotion-and-gatsby)

## Google Fonts 사용하기

프로젝트 번들에 자신만의 [Google Fonts](https://fonts.google.com/)를 포함시켜 호스팅하면 사이트가 로드 될 때 네트워크를 통해 추가적으로 가져 오지 않아도되므로 데스크톱에서 최대 300ms, 3G에서 1초 이상 사이트 로딩 속도가 개선됩니다. 성능을 위해 이런 사용자 정의 글꼴 사용은 오직 필수적인 것으로만 제한하는 것이 좋습니다.

### 사전준비

- [Gatsby 사이트](/docs/quick-start)
- [Gatsby CLI](/docs/gatsby-cli/) 설치
- [typefaces 프로젝트](https://github.com/KyleAMathews/typefaces)에서 폰트 패키지 선택하기

### 수행 절차

1. `npm install --save typeface-your-chosen-font`에서, `your-chosen-font` 부분을 [typefaces 프로젝트](https://github.com/KyleAMathews/typefaces)에서 찾은 원하는 폰트이름으로 변경하여 실행하세요.

예를들어 인기있는 'Source Sans Pro' 폰트를 로드하려면: `npm install --save typeface-source-sans-pro`.

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

### 추가 정보

- [Typography.js](/docs/typography-js/) - Gatsby 사이트에서 Google 폰트를 사용하는 또 다른 옵션
- [Typefaces 프로젝트 문서](https://github.com/KyleAMathews/typefaces/blob/master/README.md)
- [Live Kyle Mathews의 블로그의 라이브 예제](https://www.bricolage.io/typefaces-easiest-way-to-self-host-fonts/)
