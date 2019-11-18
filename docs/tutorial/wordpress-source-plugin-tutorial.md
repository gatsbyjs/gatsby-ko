---
title: "WordPress Source Plugin Tutorial"
---

## 워드프레스로부터 데이터를 가져와서 사이트를 만드는 방법

### 이번 튜토리알이 다루는 점들:

이번 튜토리알에서, 설치된 워드프레스로부터 블로그와 이미지 데이터를 Gatsby 사이트로 가져와서 데이터를 출력하기 위한 `gatsby-source-wordpress` 플러그인을 설치할 것입니다. [Gatsby + WordPress demo site](https://github.com/gatsbyjs/gatsby/tree/master/examples/using-wordpress) 는 이번 튜토리알에서 만들 것과 유사한 예제 사이트의 소스 코드를 보여줍니다. 이번 튜토리알의 다음 파트, [Adding Images to a WordPress Site](/tutorial/wordpress-image-tutorial/), 에서 다룰 예정인 멋진 이미지들이 빠져있지만이요. :D

#### GraphQL을 선호한다면?

GraphQL을 사용하는 것을 선호하는 분을 위한, 워드프레스의 default 와 custom 데이터를 간단히 공개하는 [wp-graphql](https://github.com/wp-graphql/wp-graphql) 플러그인이 있습니다.

WP-API에 의해 지원되는 것과 동일한 인증 스키마가 wp-graphql에서 지원되며, [gatsby-source-graphql](/packages/gatsby-source-graphql/) 플러그인과 함께 사용될 수 있습니다.

## 왜 이 튜토리알을 살펴봐야할까요?

각각의 source 플러그인이 작동하는 방법은 다를 수 있지만, 여러분이 앞으로 만들게 될 거의 대부분의 Gtasby 사이트에서 source 플러그인을 사용하게 될 것이기 때문에 이번 튜토리알을 살펴볼 가치가 있습니다. 이번 튜토리알에서 여러분은 Gtasby 사이트와 CMS의 기본적인 연결과, 데이터를 가져오고, 그 데이터를 React를 사용해서 여러분의 사이트에 우아한 방법으로 출력할 것입니다.

계속 추가되고 있는 사용가능한 source 플러그인을 보고 싶다면, [Gatsby plugin library](/plugins/?=source) 에서 “source” 로 검색 해보세요.

### `gatsby-source-wordpress` 플러그인을 사용한 사이트 제작

Gatsby 프로젝트를 만들고 방금 만든 프로젝트 디렉토리로 이동하세요:

```shell
 gatsby new wordpress-tutorial-site
cd wordpress-tutorial-site
```

`gatsby-source-wordpress` 플러그인을 설치하세요. 이번 튜토리알에 포함되어있지 않은 이 플러그인의 특징과 GraphQL 쿼리 예제에 대한 추가적인 정보가 보고 싶다면 [`gatsby-source-wordpress` plugin’s README file](/packages/gatsby-source-wordpress/?=wordpress)를 봐주세요.

```shell
npm install --save gatsby-source-wordpress
```

`gatsby-config.js`에 다음 코드를 사용하여 `gatsby-source-wordpress` 플러그인을 추가해주세요. 이 역시 [demo site’s source code](https://github.com/gatsbyjs/gatsby/blob/master/examples/using-wordpress/gatsby-config.js)에서 찾을 수 있습니다.

```js:title=gatsby-config.js
module.exports = {
  siteMetadata: {
    title: "Gatsby WordPress Tutorial"
  },
  plugins: [
    // https://public-api.wordpress.com/wp/v2/sites/gatsbyjsexamplewordpress.wordpress.com/pages/
    /*
     * Gatsby's data processing layer begins with “source”
     * plugins. Here the site sources its data from WordPress.
     */
    // highlight-start
    {
      resolve: `gatsby-source-wordpress`,
      options: {
        /*
         * The base URL of the WordPress site without the trailingslash and the protocol. This is required.
         * Example : 'dev-gatbsyjswp.pantheonsite.io' or 'www.example-site.com'
         */
        baseUrl: `dev-gatbsyjswp.pantheonsite.io`,
        // The protocol. This can be http or https.
        protocol: `http`,
        // Indicates whether the site is hosted on wordpress.com.
        // If false, then the assumption is made that the site is self hosted.
        // If true, then the plugin will source its content on wordpress.com using the JSON REST API V2.
        // If your site is hosted on wordpress.org, then set this to false.
        hostingWPCOM: false,
        // If useACF is true, then the source plugin will try to import the WordPress ACF Plugin contents.
        // This feature is untested for sites hosted on WordPress.com
        useACF: true
      }
    }
    // highlight-end
  ]
};
```

### Wordpress로부터 데이터를 가져오기 위한 GraphQL 쿼리 작성

이제 여러분은 Wordpress 사이트로부터 데이터를 가져오기 위한 GraphQL 쿼리를 작성할 준비가 되었습니다. 여러분은 블로그 포스트들의 제목과 작성일, 그리고 블로그 포스트 컨텐츠를 가져올 쿼리를 작성할 거에요.

실행:

```shell
gatsby develop
```

브라우저 상에서 localhost:8000 에 접속해서 사이트를 보고, GraphQL 쿼리를 작성할 수 있는 localhost:8000/\_\_\_graphql 도 접속하세요.

연습삼아 다음 쿼리를 GraphQL explorer에 작성해보세요. 이 첫번째 쿼리는 Wordpress로부터 블로그 포스트 컨텐츠를 가져올 것입니다.

```graphql
query {
  allWordpressPage {
    edges {
      node {
        id
        title
        excerpt
        slug
        date(formatString: "MMMM DD, YYYY")
      }
    }
  }
}
```

다음 쿼리는 정렬된 블로그 포스트를 가져올 것입니다.

```graphql
{
  allWordpressPost(sort: { fields: [date] }) {
    edges {
      node {
        title
        excerpt
        slug
      }
    }
  }
}
```

## 블로그 포스트들을 `index.js`에 출력하기

이제까지 여러분은 원하는 데이터를 가져오는 GraphQL 쿼리들을 작성했습니다. 두번째 쿼리를 사용해서 여러분의 사이트 홈페이지에 정렬된 블로그의 제목 리스트를 보여주겠습니다. 여러분의 `index.js` 는 다음과 같아야 합니다:

```jsx:title=src/pages/index.js
import React from "react";
import { graphql } from "gatsby";
import Layout from "../components/layout";
import SEO from "../components/seo";

export default ({ data }) => {
  //highlight-line
  return (
    <Layout>
      <SEO title="home" />
      //highlight-start
      <h1>My WordPress Blog</h1>
      <h4>Posts</h4>
      {data.allWordpressPost.edges.map(({ node }) => (
        <div>
          <p>{node.title}</p>
          <div dangerouslySetInnerHTML={{ __html: node.excerpt }} />
        </div>
      ))}
      //highlight-end
    </Layout>
  );
};

//highlight-start
export const pageQuery = graphql`
  query {
    allWordpressPost(sort: { fields: [date] }) {
      edges {
        node {
          title
          excerpt
          slug
        }
      }
    }
  }
`;
//highlight-end
```

바꾼 것들을 저장하고 localhost:8000 에 접속해서, 정렬된 블로그 포스트 목록이 보이는 여러분의 새로운 홈페이지를 보세요.

![WordPress home after query](/images/wordpress-source-plugin-home.jpg)

> **NOTE:** 미래의 에디터들에게: 블로그 포스트를 각각의 페이지로 불러오는 예제가 있었으면 도움이 될거 같습니다. 그리고 최종 결과물의 스크린샷을 추가하면 도움이 될거 같습니다.

## 각각의 블로그 포스트 페이지를 만들고 링크를 걸어봅시다.

블로그 포스트 글과 요약이 포함된 index 페이지도 대단합니다만, 각각의 블로그 포스트를 위한 페이지들도 만들고 `index.js` 파일에서 그 페이지들로 연결시켜줘야합니다.

이를 위해, 다음과 같은 것이 필요합니다:

1. 각각의 블로그 포스트를 위한 페이지들을 만들기
2. index 페이지에 있는 제목과 블로그 포스트 페이지를 연결하기

만약 기초 튜토리알 [Part 7](/tutorial/part-seven/)을 아직 읽지 않았다면, Wordpress 대신에 Markdown으로 위 프로세스의 컨셉과 예제를 살펴보니 되도록 읽어주세요.

### 각각의 블로그 포스트를 위한 페이지 작성.

튜토리알의 파트 7에서, 페이지 생성의 첫번째 단계는 markdown 파일을 위한 slug를 만드는 것입니다. 여러분은 Markdown 파일을 사용하지 않고 워드프레스를 사용하기 때문에, API 호출로부터 Wordpress source로 반환되는 slug를 사용할 수 있습니다. 여러분은 slug를 이미 가지고 있기 때문에 slug 생성 부분은 건너 뛸 수 있습니다.

프로젝트 최상단에 있는 `gatsby-node.js` 파일(몇개의 주석을 제외하고는 비어있어야 합니다.)에 다음의 것을 추가하세요:

```js:title=gatsby-node.js
const path = require(`path`);

exports.createPages = ({ graphql, actions }) => {
  const { createPage } = actions;
  return graphql(`
    {
      allWordpressPost(sort: { fields: [date] }) {
        edges {
          node {
            title
            excerpt
            content
            slug
          }
        }
      }
    }
  `).then(result => {
    console.log(JSON.stringify(result, null, 4));
  });
};
```

다음으로, `gatsby develop` 환경을 멈추고 다시 시작하세요. 터미널에 두개의 Post 오브젝트 로그가 표시되어야 합니다:

![Two posts logged to the terminal](/images/wordpress-source-plugin-log.jpg)

훌륭합니다! 튜토리알 파트 7에서 설명했다시피, `createPages` export가 Gatsby의 "일꾼" 중의 하나이고, 워드프레스로부터 블로그 포스트(또는 페이지, 커스텀 포스트 타입, 기타등등)를 만들게 해줍니다.

하지만 블로그 포스트를 만들기전에, 페이지를 만들기 위한 템플릿을 지정해야합니다.

`src` 디렉토리안에, `templates` 디렉토리를 만들고, 그 안에 `blog-post.js` 파일을 만드세요. 그 파일에 다음 내용을 붙여넣기하세요:

```jsx:title=src/tempates/blog-post.js
import React from "react";
import Layout from "../components/layout";
import { graphql } from "gatsby";

export default ({ data }) => {
  const post = data.allWordpressPost.edges[0].node;
  console.log(post);
  return (
    <Layout>
      <div>
        <h1>{post.title}</h1>
        <div dangerouslySetInnerHTML={{ __html: post.content }} />
      </div>
    </Layout>
  );
};
export const query = graphql`
  query($slug: String!) {
    allWordpressPost(filter: { slug: { eq: $slug } }) {
      edges {
        node {
          title
          content
        }
      }
    }
  }
`;
```

이 파일이 하는 일이 무엇일가요? 의존성을
What is this file doing? After importing your dependencies, it constructs the layout of the post with JSX. It wraps everything in the `Layout` component, so the style is the same throughout the site. Then, it simply adds the post title and the post content. You can add anything you want and can query for here (e.g. feature image, post meta, custom fields, etc.).

Below that, you can see the GraphQL query calling the specific post based on the `$slug`. This variable is passed to the `blog-post.js` template when the page is created in `gatsby-node.js`. To accomplish this, add the following code to the `gatsby-node.js` file:

```js:title=gatsby-node.js
const path = require(`path`);

exports.createPages = ({ graphql, actions }) => {
  const { createPage } = actions;
  return graphql(`
    {
      allWordpressPost(sort: { fields: [date] }) {
        edges {
          node {
            title
            excerpt
            content
            slug
          }
        }
      }
    }
  `).then(result => {
    //highlight-start
    result.data.allWordpressPost.edges.forEach(({ node }) => {
      createPage({
        path: node.slug,
        component: path.resolve(`./src/templates/blog-post.js`),
        context: {
          // This is the $slug variable
          // passed to blog-post.js
          slug: node.slug
        }
      });
    });
    //highlight-end
  });
};
```

`gatsby develop` 를 사용해서 환경을 재 시작해줘야 하고요. 그렇게 했을 때, index 페이지는 바뀌지 않았지만, [http://localhost:8000/asdf](http://localhost:8001/asdf)같은 404 페이지로 가보면, 두개의 예제 포스트를 볼 수 있고 클릭해서 그 포스트로 갈 수 있을 것입니다:

You will need to stop and start your environment again using `gatsby develop`. When you do, you will not see a change on the index page of the site, but if you navigate to a 404 page, like [http://localhost:8000/asdf](http://localhost:8001/asdf), you should see the two sample posts created and be able to click on them to go to the sample posts:

![Sample post links](/images/wordpress-source-plugin-sample-post-links.gif)

하지만 누구도 블로그 글을 찾기위해 404 페이지로 가고 싶어하지 않아요! 그러니 홈페이지에서 링크를 걸어봅시다.

But nobody likes to go to a 404 page to find a blog post! So, let's link these up from the home page.

### 홈페이지에서 포스트로 링크걸기. Linking to posts from the homepage.

`index.js` 페이지에 이미 여러분의 구조와 쿼리를 완료했기에, `Link` 컴포넌트를 사용해서 타이틀을 감싸기만 하면 모든 준비가 완료됩니다.

Since you already have your structure and query done for the `index.js` page, all you need to do is use the `Link` component to wrap your titles and you should be good to go.

`index.js` 파일을 열고 다음의 내용을 추가하세요:

Open up your `index.js` file and add the following:

```jsx:title=src/pages/index.js
import React from "react";
import { Link, graphql } from "gatsby"; //highlight-line
import Layout from "../components/layout";
import SEO from "../components/seo";

export default ({ data }) => {
  return (
    <Layout>
      <SEO title="home" />
      <h1>My WordPress Blog</h1>
      <h4>Posts</h4>
      {data.allWordpressPost.edges.map(({ node }) => (
        <div>
          //highlight-start
          <Link to={node.slug}>
            <p>{node.title}</p>
          </Link>
          //highlight-end
          <div dangerouslySetInnerHTML={{ __html: node.excerpt }} />
        </div>
      ))}
    </Layout>
  );
};

export const pageQuery = graphql`
  query {
    allWordpressPost(sort: { fields: [date] }) {
      edges {
        node {
          title
          excerpt
          slug
        }
      }
    }
  }
`;
```

이게 전부입니다~ `Link` 컴포넌트로 타이틀을 감싸고 포스트의 slug 를 참조하면, Gatsby가 마법과도 같이 link를 추가하고, 미리 로딩하고, 완전 빠르게 페이지 간의 전환을 만들어줍니다:

And that's it! When you wrap the title in the `Link` component and reference the slug of the post, Gatsby will add some magic to the link, preload it, and make the transition between pages incredibly fast:

![Final product with links from the home page to the blog posts](/images/wordpress-source-plugin-home-to-post-links.gif)

### 마무리 Wrapping up.

여러분은 동일한 절차를 페이지, 커스텀 포스트 타입, 커스텀 필드, 텍사노미 및 WordPress가 알고 있는 모든 유연하고 재미있는 컨텐츠를 호출하거나 생성하는데 적용할 수 있습니다. 이는 여러분이 원하는 만큼 쉬울수도 복잡할 수 있으니, 탐험하고 즐기세요!

You can apply the same procedure to calling and creating pages, custom post types, custom fields, taxonomies, and all the fun and flexible content WordPress is known for. This can be as simple or as complex as you would like it to be, so explore and have fun with it!
