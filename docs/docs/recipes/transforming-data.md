---
title: "Recipes: Transforming Data"
tableOfContentsDepth: 1
---

Gatsby에서 데이터 변환은 플러그인 중심입니다. Transformer 플러그인은 소스 플러그인을 사용하여 가져온 데이터를 더 유용한 것으로 가공합니다 (예: JSON을 JavaScript 객체 등으로).

## 마크다운을 HTML로 변환

`gatsby-transformer-remark` 플러그인은 마크다운 파일을 HTML로 변환 할 수 있습니다.

### 사전준비

- `gatsby-config.js` 및 `index.js` 페이지가 있는 Gatsby 사이트
- Gatsby 사이트 `src` 디렉토리에 저장된 마크다운 파일
- `gatsby-source-filesystem`과 같은 소스 플러그인 설치
- `gatsby-transformer-remark` 플러그인 설치

### 수행 절차

1. `gatsby-config.js`에 transformer 플러그인을 추가하세요:

```js:title=gatsby-config.js
plugins: [
  // not shown: gatsby-source-filesystem for creating nodes to transform
  `gatsby-transformer-remark`
],
```

2. `MarkdownRemark` 노드를 가져오기 위해 Gatsby 사이트의 `index.js` 파일에 GraphQL 쿼리를 추가하세요:

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

3. 개발 서버를 재시작하고 `http://localhost:8000/___graphql`에서 GraphiQL을 엽니다. `MarkdownRemark` 노드에서 사용 가능한 필드를 탐색하세요.

### 추가 정보

- `gatsby-transformer-remark`를 사용한 [마크다운을 HTML로 변환하는 방법에 대한 튜토리얼](/tutorial/part-six/#transformer-plugins)
- [Gatsby 플러그인 라이브러리](/plugins/?=transformer)에서 사용 가능한 transformer 플러그인 탐색하세요

## GraphQL을 사용하여 이미지를 그레이 스케일로 변환하기

### 사전준비

- `gatsby-config.js` 및 `index.js` 페이지가 있는 [Gatsby 사이트](/docs/quick-start)
- `gatsby-image`, `gatsby-transformer-sharp` 및 `gatsby-plugin-sharp` 패키지 설치
- `gatsby-source-filesystem`와 같은 소스 플러그인 설치
- `src/images` 폴더 안의 이미지(`.jpg`, `.png`, `.gif`, `.svg`, etc.)

### 수행 절차

1. `gatsby-config.js` 파일을 수정하여 이미지를 소싱하고 Gatsby의 GraphQL 데이터 레이어에 대한 플러그인을 구성하세요. 일반적인 접근법은 `gatsby-source-filesystem` 플러그인을 사용하여 이미지 디렉토리에서 직접 소스를 얻는 것입니다:

```javascript:title=gatsby-config.js

 plugins: [
   {
     resolve: `gatsby-source-filesystem`,
     options: {
       name: `images`,
       path: `${__dirname}/src/images`,
     },
   },
   `gatsby-transformer-sharp`,
   `gatsby-plugin-sharp`,
 ],
```

2. GraphQL을 사용하여 이미지를 쿼리하고 인라인으로 그레이 스케일 변환을 적용하세요. `relativePath`는`gatsby-source-filesystem`에서 설정한 경로의 상대 경로여야만 합니다.

```graphql
  query {
     file(relativePath: { eq: "corgi.jpg" }) {
       childImageSharp {
         // highlight-next-line
         fluid(grayscale: true) {
           ...GatsbyImageSharpFluid
         }
       }
     }
   }
```

참고: GraphQL playground인 `http://localhost:8000/___graphql`에서 이러한 매개 변수를 포함한 다른 매개 변수들을 찾을 수 있습니다.

3. 다음으로 "gatsby-image"로부터 `Img` 컴포넌트를 가져옵니다. JSX 내부에서 이를 사용하여 이미지를 표시합니다.

```jsx:title=src/pages/index.js
import React from "react"
import { useStaticQuery, graphql } from "gatsby"
import Layout from "../components/layout"
// highlight-next-line
import Img from "gatsby-image"

export default () => {
  const data = useStaticQuery(graphql`
    query {
     file(relativePath: { eq: "corgi.jpg" }) {
       childImageSharp {
         // highlight-next-line
         fluid(grayscale: true) {
           ...GatsbyImageSharpFluid
         }
       }
     }
   }
  `)
  return (
    <Layout>
      <h1>I love my corgi!</h1>
      // highlight-start
      <Img
        fluid={data.file.childImageSharp.fluid}
        alt="A corgi smiling happily"
      />
      // highlight-end
    </Layout>
  )
}
```

4. `gatsby develop`을 실행하여 개발 서버를 시작하세요.

5. 브라우저에서 이미지를 확인하세요: `http://localhost:8000/`

### 추가 정보

- [grayscale 과 duotone 쿼리 팁이 포함된 API 문서](/docs/gatsby-image/#shared-query-parameters)
- [Gatsby 이미지 문서](/docs/gatsby-image/)
- [이미지 프로세싱 예제](https://github.com/gatsbyjs/gatsby/tree/master/examples/image-processing)
