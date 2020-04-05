---
title: "Recipes: Sourcing Data"
tableOfContentsDepth: 1
---

Gatsby의 데이터 소싱은 플러그인 주도로 이뤄집니다. Source 플러그인은 소스에서 데이터를 가져옵니다. (예: `gatsby-source-filesystem` 플러그인은 파일 시스템에서 데이터를 가져오고 `gatsby-source-wordpress` 플러그인은 WordPress API에서 데이터를 가져옵니다). 직접 데이터를 제공 할 수도 있습니다.

## GraphQL에 데이터 추가하기

Gatsby의 [GraphQL 데이터 계층](/docs/querying-with-graphql/)은 노드를 사용하여 데이터 청크를 모델링합니다. Gatsby 소스 플러그인은 쿼리 할 수 있는 소스 노드를 추가하지만 직접 소스 노드를 작성할 수도 있습니다. 사용자 정의 데이터를 GraphQL 데이터 레이어에 직접 추가하기 위해 Gatsby는 당신이 활용할 수 있는 방법을 제공합니다.

이 레시피는`createNode()`를 사용하여 커스텀 데이터를 추가하는 방법을 보여줍니다.

### 수행 절차

1. `gatsby-node.js`에서 `sourceNodes()` 및 `actions.createNode()`를 사용하여 데이터를 쿼리 할 수 있는 노드를 생성하고 내보내세요(export).

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

3. Query the data (in GraphiQL or in your components).

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

### 부가 자료

- [튜토리얼 파트5](/tutorial/part-five/#source-plugins)의 `gatsby-source-filesystem` 플러그인을 사용한 예제를 살펴보세요
- [Gatsby 라이브러리](/plugins/?=source)에서 사용 가능한 소스 플러그인을 검색하세요
-[Pixabay 소스 플러그인 튜토리얼](/docs/pixabay-source-plugin-tutorial/)을 따라해보고 소스 플러그인 이해하세요
- createNode 함수 [문서](/docs/actions/#createNode)

## GraphQL을 사용하여 블로그 게시물 및 페이지를 위한 마크다운 데이터 소싱하기

마크다운(Markdown) 데이터를 소싱하고 Gatsby의 [`createPages` API](/docs/actions/#createPage)를 사용하여 페이지를 동적으로 만들 수 있습니다.

이 레시피는 Gatsby의 GraphQL 데이터 레이어를 사용하여 로컬 파일 시스템의 마크다운 파일로부터 페이지를 작성하는 방법을 보여줍니다.

### 사전준비

- `gatsby-config.js` 파일이 있는 [Gatsby 사이트](/docs/quick-start)
- [Gatsby CLI](/docs/gatsby-cli) 설치
- [gatsby-source-filesystem 플러그인](/packages/gatsby-source-filesystem) 설치
- [gatsby-transformer-remark 플러그인](/packages/gatsby-transformer-remark) 설치
- `gatsby-node.js` 파일

### 수행 절차

1. `gatsby-config.js`에서, `gatsby-source-filesystem`과 함께 `gatsby-transformer-remark`를 구성하여 소스 폴더로부터 마크다운 파일을 가져오세요. 이것은 이미지 등을 위해 추가했던 이전 `gatsby-source-filesystem` 항목에 추가됩니다:

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

2. 제목, 날짜 및 경로 frontmatter 정보를 포함한 마크다운 게시물을 간단한 본문 내용과 함께 `src/content`에 추가해주세요:

```markdown:title=src/content/my-first-post.md
---
title: My First Post
date: 2019-07-10
path: /my-first-post
---

This is my first Gatsby post written in Markdown!
```

3. `gatsby develop`으로 개발 서버를 시작하고 `http://localhost:8000/___graphql`의 GraphiQL 탐색기로 이동한 후 모든 마크다운 데이터를 얻기 위한 쿼리를 작성하세요:

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

4. GraphQL 쿼리를 `gatsby-node.js`에 복사하고 결과를 순환하여 빌드 시간에 마크다운 게시물로부터 페이지를 생성하는 JavaScript 코드를 추가하세요:

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

5. 빌드 시간에 마크다운 콘텐츠에서 페이지를 동적으로 생성하기 위해 GraphQL 쿼리를 포함하는 post 템플릿을 `src/templates`에 추가하세요:

```jsx:title=src/templates/post.js
import React from "react"
import { graphql } from "gatsby"

export default function Template({ data }) {
  const { markdownRemark } = data // data.markdownRemark holds your post data
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

### 부가 자료

- [튜토리얼: 데이터로부터 프로그램 방식으로 여러 페이지 생성하기](/tutorial/part-seven/)
- [페이지 생성 및 수정하기](/docs/creating-and-modifying-pages/)
- [Markdown 페이지 추가하기](/docs/adding-markdown-pages/)
- [프로그래밍 방식으로 데이터로부터 페이지를 생성하는 안내서](/docs/programmatically-create-pages-from-data/)
- 이 레시피에 대한 [예제 저장소](https://github.com/gatsbyjs/gatsby/tree/master/examples/recipe-sourcing-markdown)

## WordPress로부터 소싱하기

### 사전준비

-`gatsby-config.js` 및`gatsby-node.js` 파일이 있는 기존 [Gatsby 사이트](/docs/quick-start/)
- 자체 호스팅 또는 Wordpress.com 의 WordPress 인스턴스

### 수행 절차

1. 다음 명령을 실행하여 `gatsby-source-wordpress` 플러그인을 설치하세요:

```shell
npm install gatsby-source-wordpress --save
```

2. 다음을 포함하도록 `gatsby-config.js` 파일을 수정하여 플러그인을 구성하세요:

```javascript:title=gatsby-config.js
module.exports = {
  ...
  plugins: [
    {
      resolve: `gatsby-source-wordpress`,
      options: {
        // baseUrl will need to be updated with your WordPress source
        baseUrl: `wpexample.com`,
        protocol: `https`,
        // is it hosted on wordpress.com, or self-hosted?
        hostingWPCOM: false,
        // does your site use the Advanced Custom Fields Plugin?
        useACF: false
      }
    },
  ]
}
```

> **참고:** 플러그인 구성에 대한 자세한 내용은 [`gatsby-source-wordpress` 플러그인 문서](/packages/gatsby-source-wordpress/?=wordpre#how-to-use)를 참조하세요.

3. 다음 코드가 포함 된 `src/templates/post.js`와 같은 템플릿 컴포넌트를 생성합니다.

```jsx:title=post.js
import React, { Component } from "react"
import { graphql } from "gatsby"
import PropTypes from "prop-types"

class Post extends Component {
  render() {
    const post = this.props.data.wordpressPost

    return (
      <>
        <h1>{post.title}</h1>
        <div dangerouslySetInnerHTML={{ __html: post.content }} />
      </>
    )
  }
}

Post.propTypes = {
  data: PropTypes.object.isRequired,
  edges: PropTypes.array,
}

export default Post

export const pageQuery = graphql`
  query($id: String!) {
    wordpressPost(id: { eq: $id }) {
      title
      content
    }
  }
`
```

4. `gatsby-node.js`에 다음 샘플 코드를 붙여 넣어 WordPress 게시물에 대한 동적 페이지를 생성합니다:

```javascript:title=gatsby-node.js
const path = require(`path`)
const { slash } = require(`gatsby-core-utils`)

exports.createPages = async ({ graphql, actions }) => {
  const { createPage } = actions

  // query content for WordPress posts
  const result = await graphql(`
    query {
      allWordpressPost {
        edges {
          node {
            id
            slug
          }
        }
      }
    }
  `)

  const postTemplate = path.resolve(`./src/templates/post.js`)
  result.data.allWordpressPost.edges.forEach(edge => {
    createPage({
      // `path` will be the url for the page
      path: edge.node.slug,
      // specify the component template of your choice
      component: slash(postTemplate),
      // In the ^template's GraphQL query, 'id' will be available
      // as a GraphQL variable to query for this posts's data.
      context: {
        id: edge.node.id,
      },
    })
  })
}
```

5. `gatsby-develop`를 실행하여 새로 생성 된 페이지를 확인하세요.

6. `http://localhost:8000/__graphql`에서 `GraphiQL IDE`를 열고 문서 또는 탐색기를 열어 `allWordpressPosts`의 쿼리 가능한 필드를 확인하세요.

위 `gatsby-node.js`에서 생성 된 동적 페이지에는 게시물을 위한 템플릿 컴포넌트  WordPress 게시물 콘텐츠를 제공하는 샘플 GraphQL 쿼리를 사용하는 특정 게시물로 이동하는 고유 한 경로가 있습니다.

### 부가 자료

- [WordPress와 Gatsby 시작하기](/blog/2019-04-26-how-to-build-a-blog-with-wordpress-and-gatsby-part-1/)
- [WordPress로부터 소싱하기](/docs/sourcing-from-wordpress/)에 대한 추가 정보
- [WordPress로부터 소싱하기에 대한 라이브 예](https://github.com/gatsbyjs/gatsby/tree/master/examples/using-wordpress)

## Contentful 데이터 소싱하기

### 사전준비

- [Gatsby 사이트](/docs/quick-start/)
- [Contentful 계정](https://www.contentful.com/)
- [Contentful CLI](https://www.npmjs.com/package/contentful-cli) 설치

### 수행 절차

1. CLI를 사용하여 Contentful에 로그인하고 다음 단계를 수행하세요. 계정이 없는 경우 계정을 만드는게 도움이 됩니다.

```shell
contentful login
```

2. Space가 없는 경우 새 space를 만듭니다. 명령이 끝날 때 제공되는 space ID를 저장하세요. 이미 Contentful space 및 space ID가 있는 경우 2, 3단계를 건너 뛸 수 있습니다.

주의: 새 계정의 경우 기본으로 주어지는 온 보딩 space를 덮어 쓸 수 있습니다. [계정에 포함 된 space](https://app.contentful.com/account/profile/space_memberships)를 확인하세요.

```shell
contentful space create --name 'Gatsby example'
```

3. `<space ID>` 대신에 이전 명령에서 반환 된 새로운 space ID를 사용하여 블로그 컨텐츠가 포함 된 새로운 공간을 시드하세요.

```shell
contentful space seed -s '<space ID>' -t blog
```

예를 들어: `contentful space seed -s '22fzx88spbp7' -t blog`

4. Space에 대한 새 액세스 토큰을 만듭니다. 6 단계에서 필요하므로 이 토큰을 기억하세요.

```shell
contentful space accesstoken create -s '<space ID>' --name 'Example token'
```

5. Gatsby 사이트에`gatsby-source-contentful` 플러그인을 설치하세요:

```shell
npm install --save gatsby-source-contentful
```

6. `gatsby-config.js` 파일을 수정하고 `gatsby-source-contentful`을 `plugins` 배열에 추가하여 플러그인을 활성화 하세요. 보안을 위해 space ID와 토큰을 보관할 때 [환경 변수](/docs/environment-variables/) 사용을 강력히 고려해야 합니다.

```javascript:title=gatsby-config.js
plugins: [
   // add to array along with any other installed plugins
   // highlight-start
   {


    resolve: `gatsby-source-contentful`,
    options: {
      spaceId: `<space ID>`, // or process.env.CONTENTFUL_SPACE_ID
      accessToken: `<access token>`, // or process.env.CONTENTFUL_TOKEN
    },
  },
  // highlight-end
],
```

7. `gatsby develop`을 실행하고 사이트가 성공적으로 컴파일 되었는지 확인하세요.

8. Query data with the [GraphiQL editor](/docs/introducing-graphiql/) at `http://localhost:8000/___graphql`. The Contentful plugin adds several new node types to your site, including every content type in your Contentful website. Your example space with a "Blog Post" content type produces a `allContentfulBlogPost` node type in GraphQL.

8. `http://localhost:8000/___graphql`에서 [GraphiQL 편집기](/docs/introducing-graphiql/)를 사용하여 데이터를 쿼리하세요. Contentful 플러그인은 Contentful 웹 사이트의 모든 컨텐츠 유형을 포함하는 몇 가지 새로운 노드 타입을 사이트에 추가합니다. "블로그 게시물" 컨텐츠 타입의 예제 space는 GraphQL에서 `allContentfulBlogPost` 노드 타입을 생성합니다.

![아래에 설명 된 샘플 쿼리가 있는 graphql 인터페이스](../images/recipe-sourcing-contentful-graphql.png)

Contentful에서 블로그 게시물 제목을 쿼리하려면 다음 GraphQL 쿼리를 사용하세요:

```graphql
{
  allContentfulBlogPost {
    edges {
      node {
        title
      }
    }
  }
}
```

Contentful 노드는 `createdAt` 또는`node_locale`과 같은 여러 메타 데이터 필드도 포함 합니다.

9. 블로그 게시물에 대한 링크 목록을 표시하려면`/src/pages/blog.js`에 새 파일을 만듭니다. 이 페이지에는 모든 게시물이 업데이트 날짜별로 정렬되어 표시됩니다.

```jsx:title=src/pages/blog.js
import React from "react"
import { graphql, Link } from "gatsby"

const BlogPage = ({ data }) => (
  <div>
    <h1>Blog</h1>
    <ul>
      {data.allContentfulBlogPost.edges.map(({ node, index }) => (
        <li key={index}>
          <Link to={`/blog/${node.slug}`}>{node.title}</Link>
        </li>
      ))}
    </ul>
  </div>
)

export default BlogPage

export const query = graphql`
  {
    allContentfulBlogPost(sort: { fields: [updatedAt] }) {
      edges {
        node {
          title
          slug
        }
      }
    }
  }
`
```

게시물 세부 사항 페이지를 포함하여 만족스러운 사이트를 계속 구축하려면 나머지 [Gatsby 문서](/docs/sourcing-from-contentful/) 및 부가 자료를 확인하세요.

### 부가 자료

- [React와 Contentful을 사용한 사이트 구축하기](/blog/2018-1-25-Building-a-site-with-react-and-contentful/)
- [Contentful 데이터 소싱에 대한 추가 정보](/docs/sourcing-from-contentful/)
- [Contentful 소스 플러그인](/packages/gatsby-source-contentful/)
- [객체로 반환되는 긴 텍스트 필드 타입](/packages/gatsby-source-contentful/#a-note-about-long-text-fields)
- [이 레시피의 예제 저장소](https://github.com/gatsbyjs/gatsby/tree/master/examples/recipe-sourcing-contentful)

## 외부 데이터 소싱으로 GraphQL없이 페이지 만들기

[GraphQL을 고려해야 하는 이유가 있지만](/docs/why-gatsby-uses-graphql/) 페이지에 데이터를 포함하기 위해 꼭 GraphQL 데이터 계층을 사용할 필요는 없습니다. 노드의 `createPages` API를 사용하여 GraphQL 및 소스 플러그인을 통하지 않고 구조화되지 않은 데이터를 개츠비 사이트로 직접 가져올 수 있습니다.

이 레시피에서는 [PokéAPI의 REST 엔드 포인트](https://www.pokeapi.co/)에서 가져온 데이터로 동적 페이지를 생성해볼 것입니다. [전체 예제](https://github.com/jlengstorf/gatsby-with-unstructured-data/)는 GitHub에서 찾을 수 있습니다.

### 사전준비

- `gatsby-node.js` 파일이 있는 Gatsby 사이트
- [Gatsby CLI](/docs/gatsby-cli) 설치
- npm 으로부터 [axios](https://www.npmjs.com/package/axios) 패키지 설치

### 수행 절차

1. In `gatsby-node.js`, add the JavaScript code to fetch data from the PokéAPI and programmatically create an index page:
1. `gatsby-node.js`안에 PokéAPI에서 데이터를 가져오고 index 페이지를 생성하는 JavaScript 코드를 작성하세요:

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

  // Create a page that lists Pokémon.
  createPage({
    path: `/`,
    component: require.resolve("./src/templates/all-pokemon.js"),
    context: { allPokemon },
  })
}
```

2. 웹 사이트에 Pokémon을 표시 할 템플릿을 만듭니다:

```jsx:title=src/templates/all-pokemon.js
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

### 부가 자료

- [전체 Pokemon 데이터 저장소](https://github.com/jlengstorf/gatsby-with-unstructured-data/)
- [GraphQL 없이 Gatsby 사용하기](/docs/using-gatsby-without-graphql/)에서 구조화되지 않은 데이터 사용에 대한 추가정보
- When and how to [query data with GraphQL](/docs/querying-with-graphql/) for more complex Gatsby sites
- 더 복잡한 Gatsby 사이트에서 언제 어떻게 [GraphQL로 데이터를 쿼리 해야하나요](/docs/querying-with-graphql/)

## Drupal로 부터 콘텐츠 소싱하기

### 사전준비

- [Gatsby 사이트](/docs/quick-start)
- [Drupal](http://drupal.org) 사이트
- Drupal 사이트의 [JSON:API module](https://www.drupal.org/project/jsonapi) 설치 및 활성화

### 수행 절차

1. `gatsby-source-drupal` 플러그인을 설치하세요.

```shell
npm install --save gatsby-source-drupal
```

2. `gatsby-config.js` 파일을 수정하여 이 플러그인을 추가 해주세요.

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

3. `gatsby develop`으로 개발 서버를 시작하고, `http://localhost:8000/___graphql`에서 GraphiQL 탐색기를 엽니다. 탐색기 탭에서 Drupal 블록들을 위한 `allBlockBlock`과 같은 새로운 노드 타입과 Drupal 사이트의 모든 컨텐츠 타입에 대한 노드가 표시됩니다. 예를 들어, "Page" 컨텐츠 타입이 있으면 `allNodePage`로 사용할 수 있습니다. 모든 "Page" 노드의 제목과 본문을 쿼리하려면 다음과 같은 쿼리를 사용하십시오:

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

### 추가 정보

- [Drupal과 Gatsby를 독립적으로 사용하기](/blog/2018-08-13-using-decoupled-drupal-with-gatsby/)
- [Drupal로부터 소싱하기에 대한 자세한 내용](/docs/sourcing-from-drupal)
- [튜토리얼: 데이터로부터 프로그램 방식으로 여러 페이지 생성하기](/tutorial/part-seven/)
