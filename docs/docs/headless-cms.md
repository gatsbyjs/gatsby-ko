---
title: What is a Headless CMS and How to Source Content from One
overview: true
---

A _headless CMS_ is content management software that enables writers to produce and organize content, while providing developers with structured data that can be displayed using a separate system on the frontend of a website or app.

A traditional, monolithic CMS is responsible for both the backend management of content, and serving that content to end users. In contrast, a headless CMS is decoupled from frontend concerns, which frees developers to build rich experiences for end users, using the best technology available.

Many content management systems (CMS) now support a “headless” or “decoupled” mode, which is perfect for Gatsby sites.

Through use of [source plugins](/plugins/?=source), Gatsby has support for dozens of headless CMS options, allowing your content team the comfort and familiarity of its preferred admin interface, and your development team the improved developer experience and performance gains of using Gatsby, GraphQL, and React to build the frontend.

The guides in this section will walk through the process of setting up content sourcing from some of the most popular headless CMSes in use today.

<GuideList slug={props.slug} />

<!--
  Ordering in this section is driven by Gatsby plugin downloads (/plugins/?=gatsby-source-) & CMS vendor size/adoption.
-->

Here are more resources for guides, plugins, and starters for CMS systems you can connect to:

| CMS                                           | Guides                                                                           | Plugin Docs                                          | Official Starter                                                    |
| --------------------------------------------- | -------------------------------------------------------------------------------- | ---------------------------------------------------- | ------------------------------------------------------------------- |
| [Contentful](https://www.contentful.com/)     | [guide](/docs/sourcing-from-contentful/)                                         | [docs](/packages/gatsby-source-contentful)           | [starter](/starters/contentful-userland/gatsby-contentful-starter/) |
| [NetlifyCMS](https://www.netlifycms.org/)     | [guide](/docs/sourcing-from-netlify-cms/)                                        | [docs](/packages/gatsby-plugin-netlify-cms)          | [starter](/starters/netlify-templates/gatsby-starter-netlify-cms/)  |
| [WordPress](https://www.wordpress.com/)       | [guide](/docs/sourcing-from-wordpress/)                                          | [docs](/packages/gatsby-source-wordpress)            |                                                                     |
| [Prismic](https://www.prismic.io/)            | [guide](/docs/sourcing-from-prismic/)                                            | [docs](/packages/gatsby-source-prismic)              |                                                                     |
| [Strapi](https://strapi.io/)                  | [guide](/blog/2018-1-18-strapi-and-gatsby/)                                      | [docs](/packages/gatsby-source-strapi)               |
| [DatoCMS](https://www.datocms.com/)           | [guide](https://www.gatsbyjs.com/guides/datocms/)                                | [docs](/packages/gatsby-source-datocms)              | [starter](/starters/datocms/gatsby-portfolio/)                      |
| [Sanity](https://www.sanity.io/)              | [guide](/docs/sourcing-from-sanity)                                              | [docs](/packages/gatsby-source-sanity/)              |
| [Drupal](https://www.drupal.com/)             | [guide](/docs/sourcing-from-drupal/)                                             | [docs](/packages/gatsby-source-drupal)               |                                                                     |
| [Shopify](https://www.shopify.com/)           |                                                                                  | [docs](/packages/gatsby-source-shopify)              |                                                                     |
| [CosmicJS](https://cosmicjs.com/)             | [guide](/blog/2018-06-07-build-a-gatsby-blog-using-the-cosmic-js-source-plugin/) | [docs](/packages/gatsby-source-cosmicjs)             | [starters](/starters/?s=cosmicjs&v=2)                               |
| [Contentstack](https://www.contentstack.com/) | [guide](/docs/sourcing-from-contentstack)                                        | [docs](/packages/gatsby-source-contentstack)         | [starter](/starters/contentstack/gatsby-starter-contentstack/)      |
| [ButterCMS](https://buttercms.com/)           | [guide](/docs/sourcing-from-buttercms/)                                          | [docs](/packages/gatsby-source-buttercms)            | [starter](/starters/ButterCMS/gatsby-starter-buttercms/)            |
| [Ghost](https://ghost.org/)                   | [guide](/docs/sourcing-from-ghost/)                                              | [docs](/packages/gatsby-source-ghost/)               | [starter](/starters/TryGhost/gatsby-starter-ghost/)                 |
| [Kentico Kontent](https://kontent.ai/)        | [guide](/docs/sourcing-from-kentico-kontent)                                     | [docs](/packages/@kentico/gatsby-source-kontent)     | [starter](/starters/Kentico/gatsby-starter-kontent/)                |
| [Directus](https://directus.io/)              |                                                                                  | [docs](/packages/gatsby-source-directus)             |
| [GraphCMS](https://graphcms.com/)             | [guide](/docs/sourcing-from-graphcms)                                            | [docs](/packages/gatsby-source-graphql)              | [starter](/starters/GraphCMS/gatsby-graphcms-tailwindcss-example/)  |
| [Storyblok](https://www.storyblok.com/)       |                                                                                  | [docs](/packages/gatsby-source-storyblok)            |
| [Cockpit](https://getcockpit.com/)            |                                                                                  | [docs](/packages/gatsby-plugin-cockpit)              |
| [CraftCMS](https://craftcms.com/)             |                                                                                  | [docs](/packages/gatsby-source-craftcms)             |
| [AgilityCMS](https://agilitycms.com/)         | [guide](/docs/sourcing-from-agilitycms/)                                         | [docs](/packages/@agility/gatsby-source-agilitycms/) | [starter](/starters/agility/agility-gatsby-starter/)                |
| [Forestry](https://forestry.io/)              | [guide](/docs/sourcing-from-forestry/)                                           |                                                      |                                                                     |
| [Gentics Mesh](https://getmesh.io)            | [guide](/docs/sourcing-from-gentics-mesh)                                        |                                                      |                                                                     |
| [Seams-CMS](https://seams-cms.com/)           | [guide](/docs/sourcing-from-seams-cms)                                           |                                                      |                                                                     |

## How to add new guides to this section

If you don’t see your preferred CMS in this list, you can [write a new guide yourself](/contributing/how-to-contribute/) or [open an issue to request it](https://github.com/gatsbyjs/gatsby/issues/new/choose).

You can also [write your own source plugin](/docs/creating-a-source-plugin/) to integrate Gatsby with a CMS that is not in the list.
