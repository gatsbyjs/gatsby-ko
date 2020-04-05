---
title: "Recipes: Querying Data"
tableOfContentsDepth: 1
---

Gatsby에서는 GraphQL 인터페이스만으로 모든 소스들의 데이터에 접근 할 수 있습니다.

## 페이지 쿼리로 데이터 쿼리하기

`graphql` 태그를 사용하여 Gatsby 사이트의 페이지에서 데이터를 쿼리 할 수 있습니다. 이를 통해 사이트 메타 데이터, 소스 플러그인, 이미지 등 Gatsby의 데이터 계층에 포함 된 모든 항목에 접근 할 수 있습니다.

### 수행 절차

1. `gatsby`로부터 `graphql`을 임포트 하세요.

2. 역따옴표(`\``)를 사용하여 쿼리가있는 `graphql` 템플릿을 작성하고 `query`라는 상수 이름으로 내보내세요(export).

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

### 추가 정보

- [GraphQL과 Gatsby](/docs/graphql/): 데이터의 예상 형태 이해하기
- [GraphQL로 페이지에서 데이터 쿼리하기에 대한 추가 정보](/docs/page-query/)
- GraphQL에서 사용되는 [Tagged Template Literals에 대한 MDN문서](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Template_literals)

## StaticQuery 컴포넌트를 사용하여 데이터 쿼리하기

`StaticQuery`는 헤더, 내비게이션 또는 기타 자식 컴포넌트와 같은 [페이지가 아닌 컴포넌트](/docs/static-query/)에서 Gatsby의 데이터 계층으로부터 데이터를 가져오기 위한 컴포넌트입니다.

### 수행 절차

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

## useStaticQuery 훅을 사용하여 데이터 쿼리하기

Gatsby v2.1.0 부터 `useStaticQuery` 훅을 사용하여 컴포넌트 대신 JavaScript 함수로 데이터를 쿼리 할 수 있습니다. 이 구문은 모든 것을 감싸는(wrap) 방식으로 사용해야 했던 `<StaticQuery>` 컴포넌트를 제거하며, 더 간결한 코드 작성을 할 수 있습니다.

`useStaticQuery` 훅은 GraphQL 쿼리를 가져와 요청 된 데이터를 반환합니다. 이는 변수에 저장되어 나중에 JSX 템플릿에서 사용할 수 있습니다.

### 사전준비

- React 및 ReactDOM 16.8.0 이상이 필요 (Gatsby를 최신 버전으로 유지하면 해결됩니다)
- 추천 자료: [React Hooks 규칙](https://reactjs.org/docs/hooks-rules.html)

### 수행 절차

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

### 추가 정보

- [컴포넌트에서 데이터를 쿼리하기위한 Static Query에 대한 추가 정보](/docs/static-query/)
- [Static query 와 page query의 차이점](/docs/static-query/#how-staticquery-differs-from-page-query)
- [useStaticQuery 훅에 대한 추가 정보](/docs/use-static-query/)
- [GraphiQL로 데이터 시각화](/docs/introducing-graphiql/)

## GraphQL 결과 제한하기

GraphQL을 사용하여 데이터를 쿼리 할 때 숫자를 명시하여 반환되는 결과를 제한 할 수 있습니다. 이것은 몇 개의 데이터 만 필요하거나 [데이터를 페이지네이션](/docs/add-pagination/) 해야하는 경우에 유용합니다.

데이터를 제한하려면 GraphQL 데이터 레이어에 몇개의 노드가 있는 Gatsby 사이트가 필요합니다. 모든 사이트들은 `allSitePage` 및 `sitePage`와 같은 자동으로 생성된 노드들을 가지고 있으며, `gatsby-config.js`에 `gatsby-source-filesystem`과 같은 소스 플러그인을 설치하면 더 많은 노드를 추가 할 수 있습니다.

### 사전준비

- [Gatsby 사이트](/docs/quick-start/)

### 수행 절차

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

5. GraphiQL 페이지에서 play 버튼을 클릭하면 `edges` 필드의 데이터가 지정된 수로 제한됩니다.

### 추가 정보

- [Gaysby의 GraphQL 데이터 API의 노드](/docs/node-interface/)에 대해 배우세요
- [제한하기에 대한 Gatsby GraphQL 레퍼런스](/docs/graphql-reference/#limit)
- 라이브 예제:

<iframe
  title="Limiting returned data"
  src="https://711808k40x.sse.codesandbox.io/___graphql?query=%7B%0A%20%20allSitePage(limit%3A%203)%20%7B%0A%20%20%20%20edges%20%7B%0A%20%20%20%20%20%20node%20%7B%0A%20%20%20%20%20%20%20%20id%0A%20%20%20%20%20%20%20%20path%0A%20%20%20%20%20%20%7D%0A%20%20%20%20%7D%0A%20%20%7D%0A%7D%0A&explorerIsOpen=false"
  width="600"
  height="300"
/>

## GraphQL 결과 정렬하기

`sort` 인수를 사용하여 GraphQL의 결과를 순서를 정할 수 있습니다. 어떤 필드로 정렬할지, 어떤 순서로 정렬할지를 지정 할 수 있습니다.

이 레시피를 위해서, GraphQL 데이터 레이어에 정렬할 노드들이 있는 Gatsby 사이트가 필요합니다. 모든 사이트는 `allSitePage`와 같은 자동으로 생성된 노드들을 가지고 있으며, 소스 플러그인을 설치하면 더 많은 노드를 추가 할 수 있습니다.

### 사전준비

- [Gatsby 사이트](/docs/quick-start)
- 접두사 `all`로 시작하는 쿼리 가능한 필드. 예: `allSitePage`

### 수행 절차

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

### 추가 정보

- [정렬을 위한 Gatsby GraphQL 레퍼런스](/docs/graphql-reference/#sort)
- [Gaysby의 GraphQL 데이터 API의 노드](/docs/node-interface/)에 대한 학습
- 라이브 예제:

<iframe
  title="Sorting data"
  src="https://711808k40x.sse.codesandbox.io/___graphql?query=%7B%0A%20%20allSitePage(sort%3A%20%7Bfields%3A%20path%2C%20order%3A%20ASC%7D)%20%7B%0A%20%20%20%20edges%20%7B%0A%20%20%20%20%20%20node%20%7B%0A%20%20%20%20%20%20%20%20id%0A%20%20%20%20%20%20%20%20path%0A%20%20%20%20%20%20%7D%0A%20%20%20%20%7D%0A%20%20%7D%0A%7D%0A&explorerIsOpen=false"
  width="600"
  height="300"
/>

## GraphQL 결과 필터링하기

`eq` (equals), `ne` (not equals), `in` 및 `regex`와 같은 연산자를 특정 필드에 사용하여 쿼리 결과를 필터링 할 수 있습니다.

이 레시피를 위해서, GraphQL 데이터 레이어에 필터 할 노드들이 있는 Gatsby 사이트가 필요합니다. 모든 사이트들은 `allSitePage` 와 같은 자동으로 생성된 노드들을 가지고 있으며, `gatsby-config.js` 파일에 `gatsby-source-filesystem` 과 `gatsby-transformer-remark` 같은 source 및 transformer 플러그인들을 설치하여 `allMarkdownRemark`를 생성하면 더 많은 노드를 추가하 수 있습니다.

### 사전준비

- [Gatsby 사이트](/docs/quick-start)
- 접두사 `all`로 시작하는 쿼리 가능한 필드. 예: `allSitePage` 또는 `allMarkdownRemark`

### 수행 절차

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

4. `allMarkdownRemark` 필드에 `filter` 인수를 추가하고 필터링하려는 필드가 있는 객체로 제공하세요. 이 예에서 마크다운 컨텐츠는 frontmatter 메타 데이터의 `categories` 속성으로 필터링 됩니다. 다음 값은 연산자이며, 'magical creatures' 이라는 값을 가진 `eq` 연산자를 사용합니다.

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

5. GraphiQL 페이지에서 play 버튼을 클릭합니다. filter 매개변수에 매치되는 데이터만 반환되는데, 이 예에서는 'magical creatures' 라는 category 값으로 태깅된 마크다운 파일 소스들의 내용만 반환됩니다.

### 추가 정보

- [filtering에 대한 Gatsby GraphQL 레퍼런스](/docs/graphql-reference/#filter)
- [사용 가능한 연산자 전체 목록](/docs/graphql-reference/#complete-list-of-possible-operators)
- [Gaysby의 GraphQL 데이터 API의 노드](/docs/node-interface/)에 대해 배우세요
- 라이브 예제:

<iframe
  title="Filtering data"
  src="https://711808k40x.sse.codesandbox.io/___graphql?query=%7B%0A%20%20allMarkdownRemark(filter%3A%20%7Bfrontmatter%3A%20%7Bcategories%3A%20%7Beq%3A%20%22magical%20creatures%22%7D%7D%7D)%20%7B%0A%20%20%20%20edges%20%7B%0A%20%20%20%20%20%20node%20%7B%0A%20%20%20%20%20%20%20%20frontmatter%20%7B%0A%20%20%20%20%20%20%20%20%20%20title%0A%20%20%20%20%20%20%20%20%20%20categories%0A%20%20%20%20%20%20%20%20%7D%0A%20%20%20%20%20%20%7D%0A%20%20%20%20%7D%0A%20%20%7D%0A%7D%0A&explorerIsOpen=false"
  width="600"
  height="300"
/>

## GraphQL 쿼리 별칭

GraphQL 쿼리의 필드 이름을 별칭(alias)으로 바꿀 수 있습니다.

동일한 데이터 소스에서 두 개의 쿼리를 실행하려면 동일한 이름의 두 쿼리와 이름 충돌을 피하기 위해 별명을 사용할 수 있습니다.

### 수행 절차

1. `gatsby develop`을 실행하여 개발 서버를 시작하세요.
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

### 추가 정보

- [Gatsby GraphQL 별칭 레퍼런스](/docs/graphql-reference/#aliasing)
- 라이브 예제:

<iframe
  title="Using aliases"
  src="https://711808k40x.sse.codesandbox.io/___graphql?query=%7B%0A%20%20fileCount%3A%20allFile%20%7B%20%0A%20%20%20%20totalCount%0A%20%20%7D%0A%20%20filePageInfo%3A%20allFile%20%7B%0A%20%20%20%20pageInfo%20%7B%0A%20%20%20%20%20%20currentPage%0A%20%20%20%20%7D%0A%20%20%7D%0A%7D%0A&explorerIsOpen=false"
  width="600"
  height="300"
/>

## GraphQL 쿼리 프래그먼트

GraphQL 프래그먼트는 재사용 할 수 있는 공유 가능한 쿼리의 덩어리(chunks)입니다.

이것을 사용하여 쿼리간에 여러 필드를 공유하거나 데이터와 함께 구성 요소를 배치 할 수 있습니다.

### 수행 절차

1. 프래그먼트가 있는 `graphql` 템플릿 문자열을 선언합니다. 프래그먼트는 키워드 `fragment`, 이름, 그와 관련된 GraphQL 타입 (`Site` 타입의 경우 `on Site`로 명시) 및 프래그먼트를 구성하는 필드로 구성되어야 합니다.

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

2. 자 이제, 프래그먼트로 지정한 필드를 추가하기 위해 프래그먼트를 쿼리에 포함 시켜주세요. 모든 필드를 독립적으로 선언하지 않고도 해당 필드가 포함됩니다:

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

**주의**: Gatsby에서 프래그먼트를 임포트할 필요는 없습니다. 프래그먼트를 사용하여 쿼리를 내보내면(export) 해당 프래그먼트를 프로젝트의 _모든_ 쿼리에서 사용할 수 있습니다.

프래그먼트는 다른 프래그먼트 안에 중첩 될 수 있으며 동일한 쿼리에서 여러 프래그먼트를 사용할 수 있습니다.

### 추가 정보

- [프래그먼트를 사용하는 간단한 예제 저장소](https://github.com/gatsbyjs/gatsby/tree/master/examples/using-fragments)
- [Gatsby GraphQL 프래그먼트 레퍼런스](/docs/graphql-reference/#fragments)
- [Gatsby 이미지 쿼리 프래그먼트](/docs/gatsby-image/#image-query-fragments)
- [같은 장소의 데이터를 여러곳에서 사용하는 예제 저장소](https://github.com/gatsbyjs/gatsby/tree/master/examples/gatsbygram)

## fetch로 클라이언트 사이드에서 데이터 쿼리하기

빌드타임에만 데이터를 쿼리해서 정적으로만 다룰 필요는 없습니다. 일반 React 앱에서 데이터를 가져오는 방식으로 런타임에 데이터를 쿼리 할 수 있습니다.

### 사전준비

- [Gatsby 사이트](/docs/quick-start/)
- `index.js`와 같은 페이지 컴포넌트

### 수행 절차

1. `src/page`안의 정의된 페이지나 레이아웃 컴포넌트와 같이 React 컴포넌트가 정의된 파일에서 `useState`와 `useEffect`의 React 훅을 임포트하세요.

```jsx:title=src/pages/index.js
import React, { useState, useEffect } from "react"
```

2. 컴포넌트 내부에서, 컴포넌트가 클라이언트의 브라우저 안에 마운트 되었을 때 비동기적으로 데이터를 가져오는(fetch) 함수를 `useEffect` 훅으로 감싸세요(wrap). 그런 다음 `fetch` API 결과를 기다린(`await`) 후 응답 받은 결과를 상태 변수(`starsCount`)로 저장하기 위해 `useState` 훅을 통해 얻은 set 함수(예제에선 `setStarsCount`) 를 호출하세요.

```jsx:title=src/pages/index.js
import React, { useState, useEffect } from "react"

const IndexPage = () => {
  // highlight-start
  const [starsCount, setStarsCount] = useState(0)
  useEffect(() => {
    // get data from GitHub api
    fetch(`https://api.github.com/repos/gatsbyjs/gatsby`)
      .then(response => response.json()) // parse JSON from request
      .then(resultData => {
        setStarsCount(resultData.stargazers_count)
      }) // set data for the number of stars
  }, [])
  // highlight-end

  return (
    <section>
      // highlight-start
      <p>Runtime Data: Star count for the Gatsby repo {starsCount}</p>
      // highlight-end
    </section>
  )
}

export default IndexPage
```

### 추가 정보

- [Client-side에서 데이터 가져오기](/docs/data-fetching/) 가이드
- 이 예제를 사용하는 라이브 [예제 사이트](https://gatsby-data-fetching.netlify.com/)
