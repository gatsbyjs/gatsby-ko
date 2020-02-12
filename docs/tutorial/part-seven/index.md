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
[`createPages`](/docs/node-apis/#createPages)입니다. 이것은 많은 사이트와 플러그인에서 사용되는 두 가지 주요 API입니다.

We do our best to make Gatsby APIs simple to implement. To implement an API, you export a function
with the name of the API from `gatsby-node.js`.

So, here's where you'll do that. In the root of your site, create a file named
`gatsby-node.js`. Then add the following.

```javascript:title=gatsby-node.js
exports.onCreateNode = ({ node }) => {
  console.log(node.internal.type)
}
```

This `onCreateNode` function will be called by Gatsby whenever a new node is created (or updated).

Stop and restart the development server. As you do, you'll see quite a few newly
created nodes get logged to the terminal console.

Use this API to add the slugs for your markdown pages to `MarkdownRemark`
nodes.

Change your function so it now only logs `MarkdownRemark` nodes.

```javascript:title=gatsby-node.js
exports.onCreateNode = ({ node }) => {
  // highlight-start
  if (node.internal.type === `MarkdownRemark`) {
    console.log(node.internal.type)
  }
  // highlight-end
}
```

You want to use each markdown file name to create the page slug. So
`pandas-and-bananas.md` will become `/pandas-and-bananas/`. But how do you get
the file name from the `MarkdownRemark` node? To get it, you need to _traverse_
the "node graph" to its _parent_ `File` node, as `File` nodes contain data you
need about files on disk. To do that, modify your function again:

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

After restarting your development server, you should see the relative paths for your two markdown
files print to the terminal screen.

![markdown-relative-path](markdown-relative-path.png)

Now you'll have to create slugs. As the logic for creating slugs from file names can get
tricky, the `gatsby-source-filesystem` plugin ships with a function for creating
slugs. Let's use that.

```javascript:title=gatsby-node.js
const { createFilePath } = require(`gatsby-source-filesystem`) // highlight-line

exports.onCreateNode = ({ node, getNode }) => {
  if (node.internal.type === `MarkdownRemark`) {
    console.log(createFilePath({ node, getNode, basePath: `pages` })) // highlight-line
  }
}
```

The function handles finding the parent `File` node along with creating the
slug. Run the development server again and you should see logged to the terminal
two slugs, one for each markdown file.

Now you can add your new slugs directly onto the `MarkdownRemark` nodes. This is
powerful, as any data you add to nodes is available to query later with GraphQL.
So, it'll be easy to get the slug when it comes time to create the pages.

To do so, you'll use a function passed to your API implementation called
[`createNodeField`](/docs/actions/#createNodeField). This function
allows you to create additional fields on nodes created by other plugins. Only
the original creator of a node can directly modify the node—all other plugins
(including your `gatsby-node.js`) must use this function to create additional
fields.

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

Restart the development server and open or refresh GraphiQL. Then run this
GraphQL query to see your new slugs.

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

Now that the slugs are created, you can create the pages.

## Creating pages

In the same `gatsby-node.js` file, add the following.

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

You've added an implementation of the
[`createPages`](/docs/node-apis/#createPages) API which Gatsby calls so plugins can add
pages.

As mentioned in the intro to this part of the tutorial, the steps to programmatically creating pages are:

1.  Query data with GraphQL
2.  Map the query results to pages

The above code is the first step for creating pages from your markdown as you're
using the supplied `graphql` function to query the markdown slugs you created.
Then you're logging out the result of the query which should look like:

![query-markdown-slugs](query-markdown-slugs.png)

You need one additional thing beyond a slug to create pages: a page template
component. Like everything in Gatsby, programmatic pages are powered by React
components. When creating a page, you need to specify which component to use.

Create a directory at `src/templates`, and then add the following in a file named
`src/templates/blog-post.js`.

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

Then update `gatsby-node.js`

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

Restart the development server and your pages will be created! An easy way to
find new pages you create while developing is to go to a random path where
Gatsby will helpfully show you a list of pages on the site. If you go to
<http://localhost:8000/sdf>, you'll see the new pages you created.

![new-pages](new-pages.png)

Visit one of them and you see:

![hello-world-blog-post](hello-world-blog-post.png)

Which is a bit boring and not what you want. Now you can pull in data from your markdown post. Change
`src/templates/blog-post.js` to:

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

And…

![blog-post](blog-post.png)

Sweet!

The last step is to link to your new pages from the index page.

Return to `src/pages/index.js`, query for your markdown slugs, and create
links.

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
                    color: #bbb;
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

And there you go! A working, albeit small, blog!

## Challenge

Try playing more with the site. Try adding some more markdown files. Explore
querying other data from the `MarkdownRemark` nodes and adding them to the
frontpage or blog posts pages.

In this part of the tutorial, you've learned the foundations of building with
Gatsby's data layer. You've learned how to _source_ and _transform_ data using
plugins, how to use GraphQL to _map_ data to pages, and then how to build _page
template components_ where you query for data for each page.

## What's coming next?

Now that you've built a Gatsby site, where do you go next?

- Share your Gatsby site on Twitter and see what other people have created by searching for #gatsbytutorial! Make sure to mention @gatsbyjs in your Tweet and include the hashtag #gatsbytutorial :)
- You could take a look at some [example sites](https://github.com/gatsbyjs/gatsby/tree/master/examples#gatsby-example-websites)
- Explore more [plugins](/docs/plugins/)
- See what [other people are building with Gatsby](/showcase/)
- Check out the documentation on [Gatsby's APIs](/docs/api-specification/), [nodes](/docs/node-interface/), or [GraphQL](/docs/graphql-reference/)
