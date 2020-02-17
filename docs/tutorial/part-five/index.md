---
title: 소스 플러그인
typora-copy-images-to: ./
disableTableOfContents: true
---

> 이 튜토리얼은 Gatsby 데이터 레이어 시리즈의 일부입니다. 계속하기 전에 [파트 4](/tutorial/part-four/)를 진행했는지 확인하세요.

## 이번 튜토리얼에서 다룰 것

이번 튜토리얼에서는, GraphQL과 소스 플러그인을 사용하여 Gatsby 사이트로 데이터를 가져오는 방법을 배울 것입니다. 그러나 그 플러그인들을 배우기에 앞서, 여러분은 쿼리를 바르게 구성하도록 도와주는 GraphiQL이라는 툴을 어떻게 사용하는지 알고싶을 것입니다.

## GraphiQL 소개

GraphiQL은 GraphQL 통합 개발 환경(IDE)입니다. 이것은 Gatsby 웹사이트를 구축할 때 자주 사용하는 강력한(그리고 놀랍도록 만능의) 툴입니다.

여러분의 사이트의 개발 서버가 정상 작동 중이라면 다음의 주소를 통해 GraphiQL에 접속할 수 있습니다.
<http://localhost:8000/___graphql>

<video controls="controls" autoplay="true" loop="true">
  <source type="video/mp4" src="/graphiql-explore.mp4"></source>
  <p>브라우저가 동영상을 지원하지 않습니다.</p>
</video>

내장된 `site` "type"을 살펴보고 -이전에 쿼리했던 `siteMetadata` 오브젝트를 포함하여- 사용가능한 필드를 알아봅니다. GraphiQL을 열고 여러분의 데이터를 갖고 놀아보세요! <kbd>Ctrl + Space</kbd>(또는 대체 단축키인 <kbd>Shift + Space</kbd>)를 눌러 자동완성 창을 불러오고 <kbd>Ctrl + Enter</kbd>로 GraphQL 쿼리를 실행하십시오. 여러분은 남은 튜토리얼 동안 더 많이 GraphiQL을 사용하게 될 것입니다.

## GraphiQL Explorer 사용하기

GraphiQL Explorer는 여러분이 손으로 쿼리들을 입력하는 반복적인 작업 없이 사용가능한 필드를 클릭하는 대화식으로 전체의 쿼리 작성을 가능하게 해줍니다.

<EggheadEmbed
  lessonLink="https://egghead.io/lessons/gatsby-build-a-graphql-query-using-gatsby-s-graphiql-explorer"
  lessonTitle="Build a GraphQL Query using Gatsby’s GraphiQL Explorer"
/>

## 소스 플러그인

Gatsby 사이트의 데이터는 API, 데이터베이스, CMS, 로컬 파일 등 어디에서든 가져올 수 있습니다.

소스 플러그인은 소스에서 데이터를 가져옵니다. 예) filesystem 소스 플러그인은 파일 시스템에서 데이터를 가져올 수 있습니다. WordPress 플러그인은 WordPress API에서 데이터를 가져올 수 있습니다.

[`gatsby-source-filesystem`](/packages/gatsby-source-filesystem/)를 추가하고 어떻게 작동하는지 살펴봅시다.

먼저, 프로젝트의 루트에 플러그인을 설치합니다:

```shell
npm install --save gatsby-source-filesystem
```

그리고 여러분의 `gatsby-config.js`에 이것을 추가하세요:

```javascript:title=gatsby-config.js
module.exports = {
  siteMetadata: {
    title: `Pandas Eating Lots`,
  },
  plugins: [
    // highlight-start
    {
      resolve: `gatsby-source-filesystem`,
      options: {
        name: `src`,
        path: `${__dirname}/src/`,
      },
    },
    // highlight-end
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

그것을 저장하고 gatsby 개발 서버를 재시작합니다. 그런 다음 GraphiQL을 다시 여세요.

여러분은 이제 탐색창에서 `allFile`과 `file`을 볼 수 있고 선택도 가능할 것입니다:

![graphiql-filesystem](graphiql-filesystem.png)

`allFile` 드롭다운을 클릭하세요. 쿼리 영역에서 `allFile` 뒤에 커서를 놓고 <kbd>Ctrl + Enter</kbd>를 누릅니다. 이것은 각 파일의 `id`에 대한 쿼리 미리채우기를 합니다. "Play"를 눌러 쿼리를 실행하세요:

![filesystem-query](filesystem-query.png)

Explorer에서 `id`가 자동으로 선택되었습니다. 필드의 해당 체크박스에 체크해서 더 많은 필드를 선택합니다. 새 필드를 사용하여 쿼리를 다시 실행하려면 "Play"를 누르세요:

![filesystem-explorer-options](filesystem-explorer-options.png)

또는, 자동완성 단축키(<kbd>Ctrl + Space</kbd>)를 사용하여 필드를 추가할 수도 있습니다. 이것은 `File` 노드에서 쿼리 가능한 필드를 보여줍니다.

![filesystem-autocomplete](filesystem-autocomplete.png)

쿼리를 재실행 할 때마다 <kbd>Ctrl + Enter</kbd>를 눌러, 여러분의 쿼리에 여러 필드를 추가해 보세요. 여러분은 업데이트된 쿼리 결과들을 볼 수 있을 겁니다:

![allfile-query](allfile-query.png)

결과는 `File` "nodes"의 배열입니다(노드는 "graph"의 오브젝트에 붙인 이름입니다). 각 `File` 노드 오브젝트에는 쿼리한 필드가 있습니다.

## GraphQL 쿼리를 이용한 페이지 빌드

Gatsby를 사용하여 새 페이지를 만드는 것은 보통 GraphiQL에서 시작됩니다. 먼저 여러분은 GraphiQL에서 데이터 쿼리를 스케치 하고 그것을 React 페이지 컴포넌트에 복사하여 UI 빌드를 시작합니다.

해봅시다.

`src/pages/my-files.js`에 새 파일을 만들고 아래의 `allFile` GraphQL 쿼리를 사용해봅시다:

```jsx:title=src/pages/my-files.js
import React from "react"
import { graphql } from "gatsby"
import Layout from "../components/layout"

export default ({ data }) => {
  console.log(data) // highlight-line
  return (
    <Layout>
      <div>Hello world</div>
    </Layout>
  )
}

export const query = graphql`
  query {
    allFile {
      edges {
        node {
          relativePath
          prettySize
          extension
          birthTime(fromNow: true)
        }
      }
    }
  }
`
```

`console.log(data)`가 중요한 라인입니다. 여러분이 UI를 구축하는 동안 브라우저의 콘솔에서 데이터를 탐색할 수 있도록 GraphQL 쿼리로 얻은 데이터를 콘솔에 출력하는 새 컴포넌트를 만들때 이것은 자주 도움이 됩니다.

`/my-files/`의 새 페이지에 들어가 브라우저의 콘솔을 열면 다음과 같은 내용을 볼 수 있습니다:

![data-in-console](data-in-console.png)

데이터의 형태는 GraphQL 쿼리의 형태와 일치합니다.

여러분의 컴포넌트에 코드를 추가해서 파일 데이터를 출력하세요.

```jsx:title=src/pages/my-files.js
import React from "react"
import { graphql } from "gatsby"
import Layout from "../components/layout"

export default ({ data }) => {
  console.log(data)
  return (
    <Layout>
      {/* highlight-start */}
      <div>
        <h1>My Site's Files</h1>
        <table>
          <thead>
            <tr>
              <th>relativePath</th>
              <th>prettySize</th>
              <th>extension</th>
              <th>birthTime</th>
            </tr>
          </thead>
          <tbody>
            {data.allFile.edges.map(({ node }, index) => (
              <tr key={index}>
                <td>{node.relativePath}</td>
                <td>{node.prettySize}</td>
                <td>{node.extension}</td>
                <td>{node.birthTime}</td>
              </tr>
            ))}
          </tbody>
        </table>
      </div>
      {/* highlight-end */}
    </Layout>
  )
}

export const query = graphql`
  query {
    allFile {
      edges {
        node {
          relativePath
          prettySize
          extension
          birthTime(fromNow: true)
        }
      }
    }
  }
`
```

그리고 이제 [http://localhost:8000/my-files](http://localhost:8000/my-files)에 들어가보세요… 😲

![my-files-page](my-files-page.png)

## 다음에 할 것은?

이제 여러분은 소스 플러그인이 Gatsby의 _데이터 시스템으로_ 데이터를 가져오는 방법을 배웠습니다. 다음 튜토리얼에서는 트랜스포머 플러그인이 소스 플러그인에서 가져온 raw 데이터를 _변환_ 하는 방법을 배울 것입니다. 소스 플러그인과 트랜스포머 플러그인의 조합은 Gatsby 사이트를 구축할 때 필요한 모든 데이터 소싱과 데이터 변환을 처리할 수 있습니다. [튜토리얼의 파트 6](/tutorial/part-six/)에서 트랜스포머 플러그인에 대해 알아보세요.
