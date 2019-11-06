---
title: "WordPress Source Plugin Tutorial"
---

## 워드프레스로부터 데이터를 가져와서 사이트를 만드는 방법

### 이번 튜토리알이 다루는 점들:

이번 튜토리알에서, 설치된 워드프레스로부터 블로그와 이미지 데이터를 Gatsby 사이트로 가져와서 데이터를 출력하기 위해서 `gatsby-source-wordpress` 플러그인을 설치할 것입니다. [Gatsby + WordPress demo site](https://github.com/gatsbyjs/gatsby/tree/master/examples/using-wordpress) 는 이번 튜토리알에서 만들 것과 유사한 예제 사이트의 소스코드를 보여줍니다. 이번 튜토리알의 다음 파트, [Adding Images to a WordPress Site](/tutorial/wordpress-image-tutorial/), 에서 다룰 예정인 멋진 이미지들이 빠져있지만이요. :D

In this tutorial, you will install the `gatsby-source-wordpress` plugin in order to pull blog and image data from a WordPress install into your Gatsby site and render that data. This [Gatsby + WordPress demo site](https://github.com/gatsbyjs/gatsby/tree/master/examples/using-wordpress) shows you the source code for an example site similar to what you’re going to be building in this tutorial, although it’s missing the cool images you’ll be adding in the next part of this tutorial, [Adding Images to a WordPress Site](/tutorial/wordpress-image-tutorial/). :D

#### GraphQL을 선호한다면? But do you prefer GraphQL?

GraphQL을 사용하는 것을 선호한다면, 워드프레스의 default 와 custom 데이터를 간단히 공개하는 [wp-graphql](https://github.com/wp-graphql/wp-graphql) 플러그인이 있습니다.

If you prefer using GraphQL, there's a [wp-graphql](https://github.com/wp-graphql/wp-graphql) plugin to easily expose both default and custom data in WordPress.

WP-API 에 의해 지원되는 것과 동일한 인증 스키마가 wp-graphql 에서 지원되며, [gatsby-source-graphql](/packages/gatsby-source-graphql/) 플러그인과 함께 사용될 수 있습니다.

The same authentication schemes supported by the WP-API are supported in wp-graphql, which can be used with the [gatsby-source-graphql](/packages/gatsby-source-graphql/) plugin.

## 왜 이 튜토리알을 살펴봐야할까요? Why go through this tutorial?

각각의 source 플러그인이 작동하는 방법은 각각 다를 수 있지만, 여러분이 앞으로 만들게될 거의 대부분의 Gtasby 사이트에서 source 플러그인을 사용하게 될 것이기 때문에 이번 튜토리알을 살펴볼 가치가 있습니다. 이번 튜토리알에서 여러분은 Gtasby 사이트와 CMS의 기본적인 연결과, 데이터를 가져오고, 그 데이터를 React를 사용해서 여러분의 사이트에 우아한 방법으로 출력할 것입니다.

While each source plugin may operate differently from others, it’s worth going through this tutorial because you will almost definitely be using a source plugin in most Gatsby sites you build. This tutorial will walk you through the basics of connecting your Gatsby site to a CMS, pulling in data, and using React to render that data in beautiful ways on your site.

만약 여러분이 사용할 수 있는 플러그인을 보고 싶다면, [Gatsby plugin library](/plugins/?=source) 에서 “source” 로 검색 해보세요.

If you’d like to look at the growing number of source plugins available to you, search for “source” in the [Gatsby plugin library](/plugins/?=source).

### Creating a site with the `gatsby-source-wordpress` plugin

Create a new Gatsby project and change directories into the new project you just created:

```shell
 gatsby new wordpress-tutorial-site
cd wordpress-tutorial-site
```

Install the `gatsby-source-wordpress` plugin. For extra reading on the plugin’s features and examples of GraphQL queries not included in this tutorial, see the [`gatsby-source-wordpress` plugin’s README file](/packages/gatsby-source-wordpress/?=wordpress).

```shell
npm install --save gatsby-source-wordpress
```

Add the `gatsby-source-wordpress` plugin to `gatsby-config.js` using the following code, which you can also find in the [demo site’s source code](https://github.com/gatsbyjs/gatsby/blob/master/examples/using-wordpress/gatsby-config.js).

```js:title=gatsby-config.js
module.exports = {
  siteMetadata: {
    title: "Gatsby WordPress Tutorial",
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
        useACF: true,
      },
    },
    // highlight-end
  ],
}
```

### Creating GraphQL queries that pull data from WordPress

Now you are ready to create a GraphQL query to pull in some data from the WordPress site. You will create a query that pulls in the title of the blog posts, date they were posted, and blogpost content.

Run:

```shell
gatsby develop
```

In your browser, open localhost:8000 to see your site, and open localhost:8000/\_\_\_graphql so that you can create your GraphQL queries.

As an exercise, try re-creating the following queries in your GraphiQL explorer. This first query will pull in the blogpost content from WordPress:

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

This next query will pull in a sorted list of the blog posts:

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

## Rendering the blog posts to `index.js`

Now that you've created GraphQL queries that pull in the data you want, you'll use that second query to create a list of sorted blogpost titles on your site's homepage. Here is what your `index.js` should look like:

```jsx:title=src/pages/index.js
import React from "react"
import { graphql } from "gatsby"
import Layout from "../components/layout"
import SEO from "../components/seo"

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
  )
}

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
`
//highlight-end
```

Save these changes and look at localhost:8000 to see your new homepage with a list of sorted blog posts!

![WordPress home after query](/images/wordpress-source-plugin-home.jpg)

> **NOTE:** to future editors: it would be useful to also have examples of how to load blog posts to their own individual pages. And helpful to insert a screenshot of the final result here

## Create pages for each blog post and link to them

An index page with a post title and excerpt is great, but you should also build pages out for each of the blog posts, and link to them from your `index.js` file.

To do this, you need to:

1. Create pages for each blog post
2. Link up the title on the index page with the post page.

If you haven't already, please read through [Part 7](/tutorial/part-seven/) of the foundational tutorial, as it goes through the concept and examples of this process with Markdown instead of WordPress.

### Creating pages for each blog post.

In Part 7 of the tutorial, the first step in creating pages is creating slugs for the markdown files. Since you are using WordPress and not Markdown files, you can grab the slugs that get returned from your API call to the WordPress source. You can skip creating slugs, since you already have them.

Open up your `gatsby-node.js` file in the root of your project (it should be blank except for some comments) and add the following:

```js:title=gatsby-node.js
const path = require(`path`)

exports.createPages = ({ graphql, actions }) => {
  const { createPage } = actions
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
    console.log(JSON.stringify(result, null, 4))
  })
}
```

Next, stop and restart the `gatsby develop` environment. As you watch the terminal you should see two Post objects log to the terminal:

![Two posts logged to the terminal](/images/wordpress-source-plugin-log.jpg)

Excellent! As explained in Part 7 of the tutorial, this `createPages` export is one of the Gatsby "workhorses" and allows us to create your blog posts (or pages, or custom post types, etc.) from your WordPress install.

Before you can create the blog posts, however, you need to specify a template to build the pages.

In your `src` directory, create a directory called `templates` and in the newly created `templates` folder, create a filed named `blog-post.js`. In that new file, paste the following:

```jsx:title=src/tempates/blog-post.js
import React from "react"
import Layout from "../components/layout"
import { graphql } from "gatsby"

export default ({ data }) => {
  const post = data.allWordpressPost.edges[0].node
  console.log(post)
  return (
    <Layout>
      <div>
        <h1>{post.title}</h1>
        <div dangerouslySetInnerHTML={{ __html: post.content }} />
      </div>
    </Layout>
  )
}
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
`
```

What is this file doing? After importing your dependencies, it constructs the layout of the post with JSX. It wraps everything in the `Layout` component, so the style is the same throughout the site. Then, it simply adds the post title and the post content. You can add anything you want and can query for here (e.g. feature image, post meta, custom fields, etc.).

Below that, you can see the GraphQL query calling the specific post based on the `$slug`. This variable is passed to the `blog-post.js` template when the page is created in `gatsby-node.js`. To accomplish this, add the following code to the `gatsby-node.js` file:

```js:title=gatsby-node.js
const path = require(`path`)

exports.createPages = ({ graphql, actions }) => {
  const { createPage } = actions
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
          slug: node.slug,
        },
      })
    })
    //highlight-end
  })
}
```

You will need to stop and start your environment again using `gatsby develop`. When you do, you will not see a change on the index page of the site, but if you navigate to a 404 page, like [http://localhost:8000/asdf](http://localhost:8001/asdf), you should see the two sample posts created and be able to click on them to go to the sample posts:

![Sample post links](/images/wordpress-source-plugin-sample-post-links.gif)

But nobody likes to go to a 404 page to find a blog post! So, let's link these up from the home page.

### Linking to posts from the homepage.

Since you already have your structure and query done for the `index.js` page, all you need to do is use the `Link` component to wrap your titles and you should be good to go.

Open up your `index.js` file and add the following:

```jsx:title=src/pages/index.js
import React from "react"
import { Link, graphql } from "gatsby" //highlight-line
import Layout from "../components/layout"
import SEO from "../components/seo"

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
  )
}

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
`
```

And that's it! When you wrap the title in the `Link` component and reference the slug of the post, Gatsby will add some magic to the link, preload it, and make the transition between pages incredibly fast:

![Final product with links from the home page to the blog posts](/images/wordpress-source-plugin-home-to-post-links.gif)

### Wrapping up.

You can apply the same procedure to calling and creating pages, custom post types, custom fields, taxonomies, and all the fun and flexible content WordPress is known for. This can be as simple or as complex as you would like it to be, so explore and have fun with it!
