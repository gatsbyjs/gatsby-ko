---
title: Gatsby에서 데이터 다루기
typora-copy-images-to: ./
disableTableOfContents: true
---

파트 4에 오신 것을 환영합니다. 튜토리얼의 절반이나 지나왔네요! 이제 점점 익숙해져가고 있기를 바랍니다 😀

## 튜토리얼 전반부 요점 정리

지금까지 여러분은 React.js를 사용하는 방법을 배웠습니다. 웹사이트의 커스텀 빌딩 블럭 역할을 하는 컴포넌트가 얼마나 강력한지 알 수 있었습니다.

CSS 모듈을 사용하여 컴포넌트를 스타일링하는 것도 해봤습니다.

## 이번 튜토리얼에서 다룰 것

이번 튜토리알을 포함한 다음 4 파트의 튜토리얼에서, 마크다운, 워드프레스, headless CMS, 그 외의 다양한 데이터 소스로부터 여러분의 사이트를 손쉽게 만들수 있게 해주는 Gatsby의 강력한 특성인 Gatsby 데이터 레이어에 대해서 알아볼 것입니다.

**NOTE:** Gatsby 데이터 레이어는 GraphQL에 의해 작동합니다. GraphQL에 더 자세한 내용을 알고 싶다면, [GraphQL의 사용법](https://www.howtographql.com/)을 보기를 권장합니다.

## Gatsby에서의 데이터

웹사이트는 4 파트로 구성됩니다: HTML, CSS, JS, 데이터. 튜토리얼 전반부에서는 앞의 3개에 집중했었습니다. 이제 Gatsby 사이트에서 데이터를 어떻게 사용하는지 배워봅시다.

**데이터란?**

컴퓨터 사이언스 측면에서 데이터란 `"문자열"`, 정수 (`42`), 객체 (`{ pizza: true `) 같은 것들입니다.

하지만 Gatsby를 사용하기위한 측면에서 바라본 더욱 유용한 답변은 "React 컴포넌트 밖에 사는 모든 것" 입니다.

지금까지 여러분은 _직접_ 컴포넌트 안에 글을 적고 이미지를 추가하였습니다.
사이트를 만드는데 _훌륭한_ 방법입니다. 하지만 데이터를 컴포넌트 _밖_에 저장한 후 컴포넌트 _안_에 불러와야하는 경우가 자주 있습니다.

만약 여러분이 사이트를 워드프레스(컨텐츠를 추가 및 유지보수하기 좋은 좋은 인터페이스를 기여자들이 사용할 수 있겠습니다.) 와 Gatsby를 사용해서 만든다면, 워드프레스에 사이트의 _데이터_  가 있고 이 데이터를 여러분의 컴포넌트안에 _가져올 것_ 입니다.

데이터는 Markdown, CSV와 같은 파일 형식과 모든 종류의 데이터베이스와 API에 존재할 수 있습니다.

**Gatsby의 데이터 레이어는 여러분이 데이터를 직접적으로 컴포넌트에 여러분이 원하는 형태로 가져올 수 있게 해줍니다.

## 구조화 되지 않은 데이터 사용하기 vs GraphQL 사용하기

### Gatsby 사이트는 데이터를 가져오기 위해서 GraphQL과 source 플러그인을 꼭 사용해야하나요?

절대로 그렇지 않습니다! GraphQL 데이터 레이어를 통하지 않고, `createPages` API를 사용하여 구조화되지 않은 데이터를 Gatsby 페이지에 직접 통합할 수 있습니다. 소규모의 사이트에는 좋은 선택이며, 더 복잡한 사이트를 만드는데 GraphQL과 소스 플러그인을 사용하면 시간을 절약할 수 있습니다.


[GraphQL 없이 Gatsby 사용하기](/docs/using-gatsby-without-graphql/) 가이드에서 `createPages` API를 사용해서 어떻게 Gatsby 사이트에 데이터를 통합하는지도 배울수 있고, 예제 사이트도 볼수 있습니다!

### 구조화 되지 않은 데이터를 사용할 때와 GraphQL을 사용할 때

만약 소규모 사이트를 만든다면, 이번 가이드에서 설명했듯이 `createPages` API를 사용하여 구조화되지 않은 데이터를 가져오는 것이 효과적인 방법입니다. 그런 다음 나중에 사이트가 더 복잡해지면 더 복잡한 사이트를 만들거나 다음의 단계를 따라 여러분의 데이터를 변환하세요:

1.  [플러그인 라이브러리](/plugins/)에서 소스 플러그인 및 변환 플러그인이 있는지 확인하세요.
2.  만약 존재하지 않다면,  [플러그인 작성](/docs/creating-plugins/) 가이드를 읽고 여러분의 플러그인을 만드는 것을 고려해보세요!

### 어떻게 Gatsby 데이터 레이어가 GraphQL을 사용해서 컴포넌트에 데이터를 가져오는가

React 컴포넌트에 데이터를 불러오는 방법은 여러가지가 있습니다. 그 중에 가장 인기있고 강력한 것은 [GraphQL](http://graphql.org/)입니다.

GraphQL은 Facebook에서 제품 엔지니어들이 필요한 데이터를 컴포넌트에 불러오는데에 도움을 주기위해서 개발되었습니다.

GraphQL은 **쿼**리 **언**어(이름의 _QL_부분) 입니다. SQL에 익숙하다면, 매우 유사한 방식으로 작동함을 알 수 있습니다. 특별한 문법을 사용하여 여러분의 컴포넌트에 필요한 데이터를 기술하면 그대로 데이터가 제공됩니다.

Gatsby는 GraphQL을 사용하여 컴포넌트가 필요한 데이터를 선언할 수 있게 합니다.

## 새 예제 사이트 만들기

이번 튜토리얼을 위해서 새로운 사이트를 만드세요. "Pandas Eating Lots"라는 마크다운을 사용하는 블로그를 만들 것입니다. 팬더가 많이 먹는 모습이 담긴 사진과 비디오를 보여주기 위해 만들어진 사이트입니다. 만들면서 GraphQL과 Gatsby의 마크타운 지원에 살짝 발을 담궈볼 것입니다.

새로운 터미널 윈도우를 열고 다음 명령어를 입력해서 Gatsby 사이트를 `tutorial-part-four` 폴더에 생성한 후에 이동합시다:

```shell
gatsby new tutorial-part-four https://github.com/gatsbyjs/gatsby-starter-hello-world
cd tutorial-part-four
```

프로젝트 루트에 필요한 라이브러리를 설치합시다. Typhography 테마인 "Kirkham"와 CSS-in-JS 라이브러리인 ["Emotion"](https://emotion.sh/)을 사용할 것입니다:

```shell
npm install --save gatsby-plugin-typography typography react-typography typography-theme-kirkham gatsby-plugin-emotion @emotion/core
```

[파트 3](/tutorial/part-three)에서 했던 것과 비슷한 사이트 설정을 합니다. 이 사이트는 1개의 레이아웃 컴포넌트와 2개의 페이지 컴포넌트를 가질 예정입니다:

```jsx:title=src/components/layout.js
import React from "react"
import { css } from "@emotion/core"
import { Link } from "gatsby"

import { rhythm } from "../utils/typography"

export default ({ children }) => (
  <div
    css={css`
      margin: 0 auto;
      max-width: 700px;
      padding: ${rhythm(2)};
      padding-top: ${rhythm(1.5)};
    `}
  >
    <Link to={`/`}>
      <h3
        css={css`
          margin-bottom: ${rhythm(2)};
          display: inline-block;
          font-style: normal;
        `}
      >
        Pandas Eating Lots
      </h3>
    </Link>
    <Link
      to={`/about/`}
      css={css`
        float: right;
      `}
    >
      About
    </Link>
    {children}
  </div>
)
```

```jsx:title=src/pages/index.js
import React from "react"
import Layout from "../components/layout"

export default () => (
  <Layout>
    <h1>Amazing Pandas Eating Things</h1>
    <div>
      <img
        src="https://2.bp.blogspot.com/-BMP2l6Hwvp4/TiAxeGx4CTI/AAAAAAAAD_M/XlC_mY3SoEw/s1600/panda-group-eating-bamboo.jpg"
        alt="Group of pandas eating bamboo"
      />
    </div>
  </Layout>
)
```

```jsx:title=src/pages/about.js
import React from "react"
import Layout from "../components/layout"

export default () => (
  <Layout>
    <h1>About Pandas Eating Lots</h1>
    <p>
      We're the only site running on your computer dedicated to showing the best
      photos and videos of pandas eating lots of food.
    </p>
  </Layout>
)
```

```javascript:title=src/utils/typography.js
import Typography from "typography"
import kirkhamTheme from "typography-theme-kirkham"

const typography = new Typography(kirkhamTheme)

export default typography
export const rhythm = typography.rhythm
```

`gatsby-config.js` (src 디렉토리가 아니라 꼭 프로젝트 루트 디렉토리여야 합니다.)

```javascript:title=gatsby-config.js
module.exports = {
  plugins: [
    `gatsby-plugin-emotion`,
    {
      resolve: `gatsby-plugin-typography`,
      options: {
        pathToConfigModule: `src/utils/typography`,
      },
    },
  ],
}
```

위의 파일을 추가하고, `gatsby develop`를 실행하면 다음과 같은 것을 볼 수 있습니다:

![start](start.png)

이제 여러분은 1개의 레이아웃과 2개의 페이지를 가진 소규모 사이트를 만들었습니다.

이제 쿼리를 시작해봅시다 😋

## GraphQL 쿼리 첫걸음

여러분이 사이트를 만들 때, _사이트 제목_ 같은 일반적인 데이터를 재사용할 수 있습니다. `/about/` 페이지를 봅시다. `about.js` 페이지의 페이지 헤더에서 `<h1 />`과 레이아웃 컴포넌트에서 사이트 제목 `Pandas Eating Lots`가 사용되는 것을 알 수 있습니다.

만약 사이트 제목을 바꾸는 일이 생긴다면 어떻게 해야할까요? 컴포넌트를 일일이 찾아서 하나하나 바꿔줘야할 것입니다. 번거롭고 에러를 유발할 수있으며 대규모 사이트거나 구성이 복잡한 사이트라면 특히나 더 문제가 될 것입니다. 그러지말고, 제목을 한 곳에 두고 다른 파일에서 이를 참조하면, 하나만 바꾸면 Gatsby가 업데이트된 제목을 참조한 파일로 _가져옵니다._

공용 데이터를 위한 장소는 `gatsby-config.js`파일의 `siteMetadata` 객체입니다. `gatsby-config.js`에 여러분의 사이트 제목을 추가하세요:

```javascript:title=gatsby-config.js
module.exports = {
  // highlight-start
  siteMetadata: {
    title: `Title from siteMetadata`,
  },
  // highlight-end
  plugins: [
    `gatsby-plugin-emotion`,
    {
      resolve: `gatsby-plugin-typography`,
      options: {
        pathToConfigModule: `src/utils/typography`,
      },
    },
  ],
}
```

개발 서버를 재시작하세요.

### 페이지 쿼리 다루기

이제 사이트 제목이 쿼리로 사용 가능합니다; [페이지 쿼리](/docs/page-query)를 사용해서 `about.js` 파일에 제목을 추가하세요:

```jsx:title=src/pages/about.js
import React from "react"
import { graphql } from "gatsby" // highlight-line
import Layout from "../components/layout"

// highlight-next-line
export default ({ data }) => (
  <Layout>
    <h1>About {data.site.siteMetadata.title}</h1> {/* highlight-line */}
    <p>
      We're the only site running on your computer dedicated to showing the best
      photos and videos of pandas eating lots of food.
    </p>
  </Layout>
)

// highlight-start
export const query = graphql`
  query {
    site {
      siteMetadata {
        title
      }
    }
  }
`
// highlight-end
```

잘 작동하네요! 🎉

![siteMetadata에서 페이지 제목 가져오기](site-metadata-title.png)

`about.js`에 `title`을 불러오는 GraphQL은 다음과 같습니다:

```graphql:title=src/pages/about.js
{
  site {
    siteMetadata {
      title
    }
  }
}
```

> 💡 [파트 5](/tutorial/part-five/#introducing-graphiql)에서 여러분은 GraphQL을 통해서 가져올 수 있는 데이터를 대화형으로 탐색하고 위와 같은 쿼리를 작성할 수 있는 툴을 만날 것입니다.

페이지 쿼리는 컴포넌트 정의 밖에서만 존재할 수 있으며 -- 보통 페이지 컴포넌트의 끝에 두는게 관례 -- 페이지 컴포넌트에서만 가능합니다.

### StaticQuery 다루기

[StaticQuery](/docs/static-query/)는 Gatsby v2에서 소개된 새로운 API로서 페이지가 아닌 컴포넌트에서 (예를들어 `layout.js` 컴포넌트) GraphQL 쿼리를 통해 데이터를 검색하게 해줍니다.
새롭게 소개된 훅 버전을 사용해봅시다. — [`useStaticQuery`](/docs/use-static-query/).

`src/components/layout.js` 파일을 수정해서 `useStaticQuery` 훅과 이 데이터를 사용하는 `{data.site.siteMetadata.title}` 참조를 사용합시다. 작업을 마치면 파일은 다음과 같을 것입니다:

```jsx:title=src/components/layout.js
import React from "react"
import { css } from "@emotion/core"
// highlight-next-line
import { useStaticQuery, Link, graphql } from "gatsby"

import { rhythm } from "../utils/typography"
// highlight-start
export default ({ children }) => {
  const data = useStaticQuery(
    graphql`
      query {
        site {
          siteMetadata {
            title
          }
        }
      }
    `
  )
  return (
    // highlight-end
    <div
      css={css`
        margin: 0 auto;
        max-width: 700px;
        padding: ${rhythm(2)};
        padding-top: ${rhythm(1.5)};
      `}
    >
      <Link to={`/`}>
        <h3
          css={css`
            margin-bottom: ${rhythm(2)};
            display: inline-block;
            font-style: normal;
          `}
        >
          {data.site.siteMetadata.title} {/* highlight-line */}
        </h3>
      </Link>
      <Link
        to={`/about/`}
        css={css`
          float: right;
        `}
      >
        About
      </Link>
      {children}
    </div>
    // highlight-start
  )
}
// highlight-end
```

또 해냈네요! 🎉

![siteMetadata로부터 페이지 제목과 레이아웃 제목을 가져오기](site-metadata-two-titles.png)

왜 두개의 다른 쿼리를 사용했을까요? 이 예제를 통해 쿼리 유형과 포멧 방법 그리고 어디서 사용되는지에 대한 간단히 소개했습니다. 우선은 페이지만 페이지 쿼리를 만들수 있다고 기억하세요. 레이아웃과 같은 페이지가 아닌 컴포넌트는 StaticQuery를 사용할 수 있고요. [파트 7](/tutorial/part-seven/)에서 더 자세히 설명할 것입니다.

이제 진짜 제목을 복원해봅시다.
But let's restore the real title.

Gatsby의 핵심 원칙 중의 하나는 _만드는 사람은 만드는 것에 즉각적으로 연결되어야 한다는 것입니다_ ([hat tip to Bret Victor](http://blog.ezyang.com/2012/02/transcript-of-inventing-on-principle/)). 즉, 여러분의 코드가 바뀌면 여러분은 바로 바뀐 것이 어떤 영향을 주었는지 볼수 있어야 한다는 이야기입니다. Gatsby에 입력하는 것들을 조작하면, 화면에 새로운 출력이 나타납니다.

거의 대부분의 장소에서, 변경 사항은 즉시 적용됩니다. `gatsby-config.js`파일 안의 `title`을 "Panda Eating Lots"로 되돌리세요. 사이트에서 바로 바뀐 것을 볼 수 있을것입니다.

![두 제목 모두 Pandas Eating Lots라고 표시](pandas-eating-lots-titles.png)

## 다음에 할 것은?

[파트 5](/tutorial/part-five/)에서는 GraphQL과 소스 플러그인을 사용해서 여러분의 Gatsby 사이트에 데이터를 불러오는 방법에 대해서 배웁니다.
