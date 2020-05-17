---
title: "Recipes: Working with Images"
tableOfContentsDepth: 1
---

정적 리소스로써 이미지를 액세스하거나 강력한 플러그인을 통해 이미지 최적화 프로세스를 자동화 하세요.

## 웹팩을 사용하여 컴포넌트에서 이미지 임포트하기

웹팩을 사용하여 이미지를 JavaScript 모듈로 바로 임포트 할 수 있습니다. 이 프로세스는 이미지를 자동으로 축소하고 사이트의 `public` 폴더에 복사하여 일반 파일 경로와 같은 HTML `<img>` 요소에 전달할 수 있는 동적 이미지 URL을 제공합니다.

<EggheadEmbed
  lessonLink="https://egghead.io/lessons/gatsby-import-a-local-image-into-a-gatsby-component-with-webpack"
  lessonTitle="Import a Local Image into a Gatsby Component with webpack"
/>

### 사전준비

- React 컴포넌트를 내보내기(export) 하는 `.js` 파일을 가진 [Gatsby 사이트](/docs/quick-start)
- `src` 폴더안의 이미지 파일 (`.jpg`, `.png`, `.gif`, `.svg` 등)

### 수행 절차

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

### 추가 정보

- [webpack으로 이미지를 가져오는 예제 코드 저장소](https://github.com/gatsbyjs/gatsby/tree/master/examples/recipe-webpack-image)
- [Gatsby의 모든 이미지 사용 기법에 대한 추가 정보](/docs/images-and-files/)

## `static` 폴더의 이미지 참조하기

webpack을 통해 자원을 임포트하는 대신 빌드 할 때 자동으로 `public` 폴더에 복사되는 `static` 폴더의 컨텐츠를 사용 할 수 있습니다.

이는 [특정 사용 사례](/docs/static-folder/#when-to-use-the-static-folder)에 대한 **예외적인 방법**이며 Gatsby의 최적화를 활용하려면 [웹팩을 사용하여 컴포넌트에서 이미지 임포트하기](#import-an-image-into-a-component-with-webpack)와 같은 다른 방법을 사용하는 것이 좋습니다.

<EggheadEmbed
  lessonLink="https://egghead.io/lessons/gatsby-use-a-local-image-from-the-static-folder-in-a-gatsby-component"
  lessonTitle="Use a local image from the static folder in a Gatsby component"
/>

### 사전준비

- React 컴포넌트를 내보내기(export) 하는 `.js` 파일을 가진 [Gatsby 사이트](/docs/quick-start)
- `static` 폴더안의 이미지 파일 (`.jpg`, `.png`, `.gif`, `.svg` 등)

### 수행 절차

1. 이미지가 프로젝트 루트 밑 `static` 폴더에 있는지 확인하세요. 프로젝트 구조는 다음과 같습니다:

```text
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

### 추가 정보

- [static 폴더에서 이미지를 참조하는 예제 코드 저장소](https://github.com/gatsbyjs/gatsby/tree/master/examples/recipe-static-image)
- [static 폴더 사용하기](/docs/static-folder/)
- [Gatsby의 모든 이미지 사용 기법에 대한 추가 정보](/docs/images-and-files/)

## gatsby-image를 사용하여 로컬 이미지 최적화 및 쿼리하기

`gatsby-image` 플러그인은 사이트의 이미지 최적화와 관련된 많은 고통을 덜어줍니다.

Gatsby는 GraphQL로 쿼리하여 Gatsby의 image 컴포넌트로 전달할 수 있는 최적화 된 리소스를 생성합니다. 이렇게하면 여러 사이즈의 이미지 만들어 적시에 로딩시키는 것과 같은 어려운 작업을 처리 할 수 있습니다.

### 사전준비

- `gatsby-image`, `gatsby-transformer-sharp` 및 `gatsby-plugin-sharp` 패키지 설치 후 `gatsby-config`의 plugins 배열에 추가
- `gatsby-source-filesystem`과 같은 플러그인을 사용하여 `gatsby-config`를 통해 [소스로 제공되는 이미지](/packages/gatsby-image/#install)

### 수행 절차

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

### 추가 정보

- [여기의 예제를 설명하는 코드 저장소](https://github.com/gatsbyjs/gatsby/tree/master/examples/recipes-gatsby-image)
- [Gatsby Image API](/docs/gatsby-image/)
- [Gatsby Image 사용하기](/docs/using-gatsby-image)
- [Gatsby에서 이미지 사용에 대한 추가 정보](/docs/working-with-images/)
- [여기 내용을 단계별로 설명하는 무료 egghead.io 영상](https://egghead.io/playlists/using-gatsby-image-with-gatsby-ea85129e)

## gatsby-image를 사용하여 post frontmatter의 이미지 최적화 및 쿼리하기

블로그 게시물의 추천 이미지와 같은 사용 사례에서도 _여전히_ `gatsby-image`를 사용할 수 있습니다. `Img` 컴포넌트는 가공된 이미지 데이터가 필요한데, 이 이미지 데이터는 frontmatter에 이미지 URL을 가지고 있는 `.md` 또는 `.mdx` 로컬(또는 원격) 파일에서 가져올 수 있습니다.

마크다운의 인라인 이미지(`![]()` 문법을 사용하는)를 위해서는 [`gatsby-remark-images`](/packages/gatsby-remark-images/) 같은 플러그인 사용을 고려하세요

### 사전준비

- `gatsby-image`, `gatsby-transformer-sharp` 및 `gatsby-plugin-sharp` 패키지 설치 후 `gatsby-config`의 plugins 배열에 추가
- `gatsby-source-filesystem`과 같은 플러그인을 사용하여 `gatsby-config`를 통해 [소스로 제공되는 이미지](/packages/gatsby-image/#install)
- `gatsby-config`를 통해 소스로 제공되는 frontmatter에 image URL을 가진 마크다운 파일들
- [`createPages`](https://www.gatsbyjs.org/docs/node-apis/#createPages)를 사용하여 마크다운 파일로부터 생성된 페이지들

### 수행 절차

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

3. 이제 `gatsby-image`에서 `Img`를 `gatsby`에서 `graphql`을 템플릿 컴포넌트로 임포트 하고 전달된 `slug` 기반의 이미지 데이터를 얻기 위해 [pageQuery](/docs/page-query/)를 작성한 다음 그 데이터를 `<Img />` 컴포넌트에 전달하세요:

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

4. `gatsby develop`을 실행하면 소싱된 파일의 이미지들이 파일 시스템안에 생성됩니다.

### 추가 정보

- [이 레시피를 사용하는 예제 코드 저장소](https://github.com/gatsbyjs/gatsby/tree/master/examples/recipes-gatsby-image)
- [frontmatter에 피처드 이미지 사용](/docs/working-with-images-in-markdown/#featured-images-with-frontmatter-metadata)
- [Gatsby 이미지 API](/docs/gatsby-image/)
- [gatsby-image 사용하기](/docs/using-gatsby-image)
- [Gatsby의 이미지 작업에 대한 추가 정보](/docs/working-with-images/)
