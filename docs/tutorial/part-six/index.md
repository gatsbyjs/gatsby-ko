---
title: Transformer plugins
typora-copy-images-to: ./
disableTableOfContents: true
---

> 이 튜토리얼은 Gatsby 데이터 레이어 시리즈 중 하나입니다. 이 단계를 진행하기 전에 [파트 4](/tutorial/part-four/)와 [파트 5](/tutorial/part-five/)를 완료했는지 확인해주세요.

## 이번 튜토리얼에서는 무엇을 다루나요?

이전 튜토리얼에서는 source 플러그인이 데이터를 Gatsby의 데이터 시스템 내부로 넣는지 보여주었습니다. 이 튜토리얼에서는, 여러분은 transformer 플러그인이 source 플러그인으로부터 가져온 가공되지 않은 내용물을 변환시키는지 배울 것입니다. source 플러그인과 transformer 플러그인의 결합은 Gatsby 사이트를 만들 때 필요할지 모를 모든 데이터소싱과 데이터 변환을 지원합니다.

## Transformer 플러그인

종종, 여러분이 source 플러그인으로부터 받는 데이터의 형식은 여러분이 자신의 웹사이트를 만드는데 필요한 것이 아닙니다. filesystem source 플러그인은 여러분이 파일에 대한 데이터에 대해 의문을 제기할 수 있게 해주지만 하지만 파일 안에 있는 데이터는 어떻게 해야 할까요?

이것을 가능하게 하기 위해서, Gatsby는 source 플러그인으로부터 가공되지 않은 컨텐트를 가져와 더 유용한 것으로 변환시키는 transformer 플러그인을 지원합니다.

예를 들어 마크다운 파일이 있습니다. 마크다운은 작성하기에는 좋지만 페이지를 만들 때에는 마크다운이 HTML 형식이어야 합니다. 

여러분의 사이트 `src/pages/sweet-pandas-eating-sweets.md`에 마크다운 파일을 추가하세요. (이것은 여러분의 첫 마크다운 블로그 포스트가 될 것입니다) 그리고 transformer 플러그인과 GraphQL로 이를 HTML로 바꾸는 방법을 배워보세요.

```markdown:title=src/pages/sweet-pandas-eating-sweets.md
---
title: "Sweet Pandas Eating Sweets"
date: "2017-08-10"
---

Pandas are really sweet.

Here's a video of a panda eating sweets.

<iframe width="560" height="315" src="https://www.youtube.com/embed/4n0xNbfJLR8" frameborder="0" allowfullscreen></iframe>
```

파일을 저장한 후, `/my-files/`를 다시 보세요—새로운 마크다운 파일이 테이블 안에 있습니다. 이는 Gatsby의 매우 강력한 기능입니다. 전에 보셨던 `siteMetadata` 예시와 같이 source 플러그인은 실시간으로 데이터를 다시 채워넣을 수 있습니다. `gatsby-source-filesystem`은 항상 추가될 수 있는 새 파일을 스캔하고 있으며 여러분의 쿼리를 반복합니다.

마크다운 파일을 변환할 수 있는 transformer 플러그인을 추가하세요:

```shell
npm install --save gatsby-transformer-remark
```
그리고 이것을 평소대로 `gatsby-config.js`에 추가하세요:

```javascript:title=gatsby-config.js
module.exports = {
  siteMetadata: {
    title: `Pandas Eating Lots`,
  },
  plugins: [
    {
      resolve: `gatsby-source-filesystem`,
      options: {
        name: `src`,
        path: `${__dirname}/src/`,
      },
    },
    `gatsby-transformer-remark`, // highlight-line
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

개발 서버를 재시작하고, GraphiQL을 새로고침(혹은 다시 시작)하고 자동완성을 주목하세요:

![markdown-autocomplete](markdown-autocomplete.png)

`allMarkdownRemark`를 다시 선택하고 `allFile`과 같이 실행하세요. 거기서 최근에 추가한 마크다운 파일을 볼 수 있을 것입니다. `MarkdownRemark` 노드에서 사용가능한 분야를 탐험해보세요.

![markdown-query](markdown-query.png)

좋습니다! 기본적인 것들은 잘 맞아들어가고 있어요. source 플러그인들은 Gatsby의 데이터 시스템 안으로 데이터를 가져오고 transformer 플러그인들은 source 플러그인이 가져온 가공되지 않은 컨텐츠를 변환합니다. 이 패턴은 여러분이 Gatsby 사이트를 만들때 필요할 모든 데이터 소싱과 데이터 변환을 처리할 수 있습니다. 

## `src/pages/index.js`안에 사이트의 마크다운 파일 리스트 만들기

이제 여러분은 첫 페이지에 당신의 마크다운 파일의 리스트를 만들어야 합니다. 많은 블로그들처럼, 여러분은 첫 페이지에 각각의 포스트로 향하는 일련의 링크를 걸어두고 싶을 것입니다. GraphQL이 있다면 당신은 현재의 마크다운 블로그 포스트의 링크를 요청함으로서 리스트를 수동으로 관리할 필요가 없습니다. 

`src / pages / my-files.js` 페이지와 마찬가지로 `src / pages / index.js`를 다음과 같이 변경하여 초기 HTML 및 스타일링을 포함한 GraphQL 쿼리를 추가하십시오.

```jsx:title=src/pages/index.js
import React from "react"
import { graphql } from "gatsby"
import { css } from "@emotion/core"
import { rhythm } from "../utils/typography"
import Layout from "../components/layout"

export default ({ data }) => {
  console.log(data)
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
          </div>
        ))}
      </div>
    </Layout>
  )
}

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

이제 첫 페이지는 이렇게 보일 것입니다:

![frontpage](frontpage.png)

하지만 당신의 한 블로그 포스트는 좀 외로워 보이네요 그러니 `src/pages/pandas-and-bananas.md`에 하나를 추가해봅시다. 


```markdown:title=src/pages/pandas-and-bananas.md
---
title: "Pandas and Bananas"
date: "2017-08-21"
---

Do Pandas eat bananas? Check out this short video that shows that yes! pandas do
seem to really enjoy bananas!

<iframe width="560" height="315" src="https://www.youtube.com/embed/4SZl1r2O_bY" frameborder="0" allowfullscreen></iframe>
```

![two-posts](two-posts.png)

근사하네요! 하지만... 게시물의 순서가 잘못되었네요.

하지만 이것은 쉽게 고칠 수 있습니다. 어떠한 유형의 연결을 요청할 때는, 다양한 인자를 GraphQL 쿼리로 넘길 수 있습니다. 여러분은 노드를 `정렬`하고 `걸러낼 수` 있고, 몇개의 노드를 `건너뛸지` 설정할 수 있으며, 몇개의 노드를 검색할지 `한계`를 정할수도 있습니다. 이 강력한 연산자와 함께, 여러분은 여러분이 필요한 포맷으로 여러분이 필요한 데이터를 선택할 수 있습니다.

당신의 기본 페이지의 GraphQL 쿼리에서, `allMarkdownRemark` 를`allMarkdownRemark(sort: { fields: [frontmatter___date], order: DESC })`로 변경하세요. _참고:  frontmatter와 date 사이에는 3개의 언더바 (\_)가 있습니다._ 이를 저장하면 정렬 순서는 수정될 것입니다.

Try opening GraphiQL and playing with different sort options. You can sort the
`allFile` connection along with other connections.

쿼리 연산자에 대한 자세한 내용은 [GraphQL 참조 가이드](/docs/graphql-reference/)를 참고해주세요.

## 도전

블로그 포스트를 포함한 새로운 페이지를 만드는걸 시도해보고 홈페이지에 있는 블로그 포스트 리스트에 어떤 일이 일어나는지 보세요!

## 이 다음에는?

좋습니다! 여러분은 방금 여러분의 마크다운 파일을 요청하고 블로그 게시물 제목과 발췌문 목록를 만들어내는 훌륭한 기본 페이지를 만들어 냈습니다!
하지만 여러분은 여러분의 마크다운 파일들의 실제 페이지를 보고 싶지 그저 발췌된 부분만을 보고 싶지는 않을 것입니다. 

여러분은 `src/pages`에 리액트 컴포넌트를 위치시킴으로서 계속해서 페이지들을 만들어낼 수 있습니다. 하지만 여러분은 다음에 데이터로부터 프로그래밍적으로 페이지를 만드는 방법에 대해 배울 것입니다. Gatsby는 많은 정적 사이트 생성기처럼 파일로부터 페이지를 만드는 데 그치지 않습니다. Gatsby는 여러분이 여러분의 데이터를 요청하고 그 결과를 페이지로 전달하도록 하기 위해 GraphQL을 사용하도록 합니다-모든 빌드 시간동안. 이것은 매우 강력한 아이디어입니다. 여러분은 이것의 영향과 사용방법에 대해 [프로그래밍적으로 데이터로부터 페이지를 만드는 것](/tutorial/part-seven/)을 배울 다음 튜토리얼에서 배울 것입니다. 
