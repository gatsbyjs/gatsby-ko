---
title: 데이터를 이용해 프로그래밍식으로 페이지 만들기
typora-copy-images-to: ./
disableTableOfContents: true
---

이 튜토리얼은 Gatsby 데이터 레이어 시리즈의 일부입니다. 계속하기 전에 [파트 4](/tutorial/part-four/), [파트 5](/tutorial/part-five/), and [파트 6](/tutorial/part-six/)를 진행했는지 확인하세요.

## 이번 튜토리얼에서 다룰 것

여러분은 이전 튜토리얼에서, 마크다운 파일을 쿼리하고 블로그 포스트 제목 및 발췌의 리스트를 생성하는 멋진 인덱스 페이지를 만들었습니다. 그러나 여러분은 발췌 내용만이 아닌, 실제 페이지를 원할 것 입니다.

여러분은 계속해서 `src/pages`에 React 컴포넌트를 배치하여 페이지를 생성할 수 있습니다.
그러나, 여러분은 이제 _데이터_ 에서 페이지를 _프로그래밍적_ 으로 작성하는 방법을 배웁니다. Gatsby는 많은 정적 사이트 생성기처럼 파일을 페이지로 만드는 것에 제한되지 않습니다. Gatsby에서는 빌드 중에는 언제나 GraphQL을 사용하여 _데이터_ 를 쿼리하고 쿼리 결과를 _페이지_ 에 _매핑_ 할 수 있습니다. 이것은 정말 강력한 아이디어입니다. 여러분은 앞으로 이 튜토리얼에서 이것의 의미와 사용법을 살펴 볼 것입니다.

시작해 봅시다.

## 페이지용 slug 만들기

새 페이지를 만들 때는 두 단계가 있습니다:

1. 페이지의 "path" 또는 "slug"를 생성합니다.
2. 페이지를 만듭니다.

_**노트**: 종종 데이터 소스는 컨텐츠에 대한 slug나 path이름을 직접 제공합니다 - 이러한 시스템 중 하나(예: CMS)를 사용하는 경우, 마크다운 파일을 사용하여 slug를 직접 만들 필요가 없습니다._

마크다운 페이지를 만들기 위해, 다음의 두가지 Gatsby API 사용하는 방법을 배웁니다:
[`onCreateNode`](/docs/node-apis/#onCreateNode)와
[`createPages`](/docs/node-apis/#createPages)입니다. 이 두가지는 많은 사이트와 플러그인에서 사용되는 주요 API입니다.

우리는 Gatsby API를 쉽게 구현할 수 있도록 최선을 다하고 있습니다.
API를 구현하려면, `gatsby-node.js`에서 API 이름으로 함수를 export합니다.

그러므로 여러분도 그렇게 할 것입니다. 사이트의 루트에서 `gatsby-node.js`라는 파일을 만듭니다. 그리고 다음을 추가하세요.

```javascript:title=gatsby-node.js
exports.onCreateNode = ({ node }) => {
  console.log(node.internal.type)
}
```

이 `onCreateNode` 함수는 새로운 노드가 생성(또는 업데이트) 될 때마다 Gatsby에 의해 호출됩니다.

개발서버를 중지하고 다시 시작하세요. 여러분이 한 것처럼, 새로 생성된 노드가 터미널 콘솔에 출력되는 것을 볼 수 있습니다.

이 API를 사용하여 마크다운 페이지의 slug를 `MarkdownRemark` 노드에 추가하세요.

이제 MarkdownRemark 노드만 출력하도록 함수를 변경하십시오.

```javascript:title=gatsby-node.js
exports.onCreateNode = ({ node }) => {
  // highlight-start
  if (node.internal.type === `MarkdownRemark`) {
    console.log(node.internal.type)
  }
  // highlight-end
}
```

여러분은 각 마크다운 파일명을 페이지 slug로 만들고 싶을 것입니다. 따라서 `pandas-and-bananas.md`는 `/pandas-and-bananas/`이 됩니다. 하지만 `MarkdownRemark` 노드에서 어떻게 파일명을 얻을까요? `File` 노드는 디스크의 파일에 대해 필요한 데이터를 포함하므로, 파일명을 얻기 위해서는 "node graph"를 _상위_ `File` 노드로 _이동_ 해야 합니다. 그러려면, 함수를 다시 수정하세요:

```javascript:title=gatsby-node.js
// highlight-next-line
exports.onCreateNode = ({ node, getNode }) => {
  if (node.internal.type === `MarkdownRemark`) {
    // highlight-start
    const fileNode = getNode(node.parent)
    console.log(`\n`, fileNode.relativePath)
    // highlight-end
  }
}
```

개발 서버를 다시 시작하면, 두 마크다운 파일의 상대 경로가 터미널 화면에 출력되는 것을 볼 수 있습니다.

![markdown-relative-path](markdown-relative-path.png)

이제 여러분은 slug를 만들어야 합니다. 파일 이름에서 slug를 만드는 것이 까다로울 수 있으므로, `gatsby-source-filesystem` 플러그인은 slug 생성 함수를 제공합니다. 그것을 써봅시다.

```javascript:title=gatsby-node.js
const { createFilePath } = require(`gatsby-source-filesystem`) // highlight-line

exports.onCreateNode = ({ node, getNode }) => {
  if (node.internal.type === `MarkdownRemark`) {
    console.log(createFilePath({ node, getNode, basePath: `pages` })) // highlight-line
  }
}
```

이 함수는 slug 생성과 함께 부모 `File` 노드를 찾아줍니다. 개발 서버를 다시 실행하면 각 마크다운 파일마다 하나씩, 두 개의 슬러그가 터미널에 출력 된 것을 볼 수 있습니다.

이제 새 slug를 `MarkdownRemark` 노드에 직접 추가 할 수 있습니다. 이것은 노드에 추가 한 모든 데이터를 나중에 GraphQL로 쿼리 할 수 있으므로 강력합니다. 따라서 페이지를 만들 때 slug를 쉽게 얻을 수 있습니다.

이를 위해 API 구현으로 전달 받은 [`createNodeField`](/docs/actions/#createNodeField) 함수를 사용합니다. 이 함수를 사용하면 다른 플러그인으로 작성된 노드에 추가적으로 필드를 만들 수 있습니다. 노드의 원래 작성자만 노드를 직접 수정할 수 있습니다. 다른 모든 플러그인(`gatsby-node.js` 포함)은 이 함수를 사용하여 추가 필드를 작성해야합니다.

```javascript:title=gatsby-node.js
const { createFilePath } = require(`gatsby-source-filesystem`)
// highlight-next-line
exports.onCreateNode = ({ node, getNode, actions }) => {
  const { createNodeField } = actions // highlight-line
  if (node.internal.type === `MarkdownRemark`) {
    // highlight-start
    const slug = createFilePath({ node, getNode, basePath: `pages` })
    createNodeField({
      node,
      name: `slug`,
      value: slug,
    })
    // highlight-end
  }
}
```

개발 서버를 재시작하고 GraphiQL을 열거나 새로고침합니다. 그런 다음 이 GraphQL 쿼리를 실행하여 새 slug를 확인하세요.

```graphql
{
  allMarkdownRemark {
    edges {
      node {
        fields {
          slug
        }
      }
    }
  }
}
```

이제 slug가 생성되었으므로, 페이지를 만들 수 있습니다.

## 페이지 만들기

같은 `gatsby-node.js` 파일에서 다음을 추가하세요.

```javascript:title=gatsby-node.js
const { createFilePath } = require(`gatsby-source-filesystem`)

exports.onCreateNode = ({ node, getNode, actions }) => {
  const { createNodeField } = actions
  if (node.internal.type === `MarkdownRemark`) {
    const slug = createFilePath({ node, getNode, basePath: `pages` })
    createNodeField({
      node,
      name: `slug`,
      value: slug,
    })
  }
}

// highlight-start
exports.createPages = async ({ graphql, actions }) => {
  // **Note:** The graphql function call returns a Promise
  // see: https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Promise for more info
  const result = await graphql(`
    query {
      allMarkdownRemark {
        edges {
          node {
            fields {
              slug
            }
          }
        }
      }
    }
  `)

  console.log(JSON.stringify(result, null, 4))
}
// highlight-end
```

여러분은 플러그인이 페이지를 추가 할 수 있도록 Gatsby가 호출하는 [`createPages`](/docs/node-apis/#createPages) API의 구현을 추가했습니다.

이 튜토리얼 파트 소개에서 언급했듯이, 프로그래밍식으로 페이지를 작성하는 단계는 다음과 같습니다:

1.  GraphQL로 데이터 쿼리
2.  쿼리 결과를 페이지에 매핑

The above code is the first step for creating pages from your markdown as you're
using the supplied `graphql` function to query the markdown slugs you created.
그러면 쿼리 결과가 다음과 같이 출력됩니다:

![query-markdown-slugs](query-markdown-slugs.png)

여러분이 페이지를 만들기 위해서는 slug 외에 추가적으로 페이지 템플릿 컴포넌트가 필요합니다. Gatsby의 모든 것과 마찬가지로 프로그래밍식 페이지는 React 컴포넌트에 의해 구동됩니다. 페이지를 만들 때, 사용할 컴포넌트를 지정해야합니다.

`src/templates` 디렉토리를 만들고, `src/templates/blog-post.js`라는 파일에 다음을 추가합니다.

```jsx:title=src/templates/blog-post.js
import React from "react"
import Layout from "../components/layout"

export default () => {
  return (
    <Layout>
      <div>Hello blog post</div>
    </Layout>
  )
}
```

그런 다음 'gatsby-node.js'를 업데이트하세요.

```javascript:title=gatsby-node.js
const path = require(`path`) // highlight-line
const { createFilePath } = require(`gatsby-source-filesystem`)

exports.onCreateNode = ({ node, getNode, actions }) => {
  const { createNodeField } = actions
  if (node.internal.type === `MarkdownRemark`) {
    const slug = createFilePath({ node, getNode, basePath: `pages` })
    createNodeField({
      node,
      name: `slug`,
      value: slug,
    })
  }
}

exports.createPages = async ({ graphql, actions }) => {
  const { createPage } = actions // highlight-line
  const result = await graphql(`
    query {
      allMarkdownRemark {
        edges {
          node {
            fields {
              slug
            }
          }
        }
      }
    }
  `)

  // highlight-start
  result.data.allMarkdownRemark.edges.forEach(({ node }) => {
    createPage({
      path: node.fields.slug,
      component: path.resolve(`./src/templates/blog-post.js`),
      context: {
        // Data passed to context is available
        // in page queries as GraphQL variables.
        slug: node.fields.slug,
      },
    })
  })
  // highlight-end
}
```

개발 서버를 다시 시작하면 페이지가 생성됩니다! 개발 중에 생성 한 새 페이지를 쉽게 찾을 수있는 방법은 Gatsby가 유용하게 사이트의 페이지 목록을 보여주는 아무 경로로 이동하는 것입니다. <http://localhost:8000/sdf>로 이동하면 여러분이 만든 새 페이지들이 표시됩니다.

![new-pages](new-pages.png)

그 중 하나에 들어가면 다음을 볼 수 있어요:

![hello-world-blog-post](hello-world-blog-post.png)

Which is a bit boring and not what you want. Now you can pull in data from your markdown post. Change
`src/templates/blog-post.js` to:
이건 조금 따분하고 여러분이 원하는게 아니에요. 여러분은 이제 마크다운 포스트에서 데이터를 가져올 수 있을겁니다. 바꿉시다.

```jsx:title=src/templates/blog-post.js
import React from "react"
import { graphql } from "gatsby" // highlight-line
import Layout from "../components/layout"

// highlight-start
export default ({ data }) => {
  const post = data.markdownRemark
  // highlight-end
  return (
    <Layout>
      {/* highlight-start */}
      <div>
        <h1>{post.frontmatter.title}</h1>
        <div dangerouslySetInnerHTML={{ __html: post.html }} />
      </div>
      {/* highlight-end */}
    </Layout>
  )
}

// highlight-start
export const query = graphql`
  query($slug: String!) {
    markdownRemark(fields: { slug: { eq: $slug } }) {
      html
      frontmatter {
        title
      }
    }
  }
`
// highlight-end
```

그리고...

![blog-post](blog-post.png)

좋아요!

마지막 단계는 인덱스 페이지에서 새 페이지들에 연결하는 것입니다.

`src/pages/index.js`로 돌아가, 마크다운 slug를 쿼리하고, 링크를 만듭니다.

```jsx:title=src/pages/index.js
import React from "react"
import { css } from "@emotion/core"
import { Link, graphql } from "gatsby" // highlight-line
import { rhythm } from "../utils/typography"
import Layout from "../components/layout"

export default ({ data }) => {
  return (
    <Layout>
      <div>
        <h1
          css={css`
            display: inline-block;
            border-bottom: 1px solid;
          `}
        >
          Amazing Pandas Eating Things
        </h1>
        <h4>{data.allMarkdownRemark.totalCount} Posts</h4>
        {data.allMarkdownRemark.edges.map(({ node }) => (
          <div key={node.id}>
            {/* highlight-start */}
            <Link
              to={node.fields.slug}
              css={css`
                text-decoration: none;
                color: inherit;
              `}
            >
              {/* highlight-end */}
              <h3
                css={css`
                  margin-bottom: ${rhythm(1 / 4)};
                `}
              >
                {node.frontmatter.title}{" "}
                <span
                  css={css`
                    color: #555;
                  `}
                >
                  — {node.frontmatter.date}
                </span>
              </h3>
              <p>{node.excerpt}</p>
            </Link> {/* highlight-line */}
          </div>
        ))}
      </div>
    </Layout>
  )
}

export const query = graphql`
  query {
    allMarkdownRemark(sort: { fields: [frontmatter___date], order: DESC }) {
      totalCount
      edges {
        node {
          id
          frontmatter {
            title
            date(formatString: "DD MMMM, YYYY")
          }
          // highlight-start
          fields {
            slug
          }
          // highlight-end
          excerpt
        }
      }
    }
  }
`
```

됐습니다! 작지만 작동하는 블로그!

## 도전

사이트를 더 많이 가지고 놀아보세요. 마크다운 파일을 더 추가해보세요. `MarkdownRemark` 노드에서 다른 데이터를 쿼리하여 첫 페이지 또는 블로그 포스트 페이지에 추가하세요.

이번 파트에서는, Gatsby의 데이터 레이어로 구축하는 기초를 배웠습니다. 플러그인을 사용하여 데이터를 _소싱_ 및 _변환_ 하는 방법, GraphQL을 사용하여 데이터를 페이지에 _매핑_ 하는 방법 및 각 페이지의 데이터를 조회하는 _페이지 템플릿 컴포넌트_ 를 구축하는 방법에 대해 배웠습니다.

## 다음에 할 것은?

Gatsby 사이트를 만들었으니 다음에는 무엇을 할까요?

- 트위터에서 Gatsby 사이트를 공유하고 #gatsbytutorial를 검색하여 다른 사람들이 만든 것을 살펴보세요! 트윗에 @gatsbyjs를 멘션하고 해시태그 #gatsbytutorial를 포함하는 걸 잊지마세요 :)
- 여러분은 일부 [예제 사이트](https://github.com/gatsbyjs/gatsby/tree/master/examples#gatsby-example-websites)를 볼 수도 있습니다.
- 더 많은 [플러그인](/docs/plugins/) 알아보기
- [다른 사람들이 Gatsby로 어떤 것을 빌드하는지](/showcase/) 알아보기
- [Gatsby's APIs](/docs/api-specification/), [nodes](/docs/node-interface/), or [GraphQL](/docs/graphql-reference/)의 설명서를 확인해보기
