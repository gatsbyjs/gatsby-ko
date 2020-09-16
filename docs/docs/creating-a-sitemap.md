---
title: Creating a Sitemap
---

## What is a sitemap?

An [XML sitemap](https://support.google.com/webmasters/answer/156184?hl=en) lists a website’s important pages, making sure search engines (such as Google) can find and crawl them all. In effect, a sitemap helps a search engine understand your website structure.

Think of it as a map for your website. It shows what all of the pages are on your website.

## Using [gatsby-plugin-sitemap](/packages/gatsby-plugin-sitemap/)

To generate an XML sitemap, you will use the [`gatsby-plugin-sitemap`](/packages/gatsby-plugin-sitemap/) package.

Install the package by running the following command:
`npm install --save gatsby-plugin-sitemap`

### How to configure

Once installation is complete, you can now add this plugin to your `gatsby-config.js`, like so:

```javascript:title=gatsby-config.js
module.exports = {
  siteMetadata: {
    siteUrl: `https://www.example.com`,
  },
  plugins: [`gatsby-plugin-sitemap`],
}
```

**Note:** The siteUrl property must be defined and not left empty.

Next run a build (`npm run build`) since the sitemap generation will only happen for production builds. This is all that's required to get a working sitemap with Gatsby! By default, the generated sitemap path is /sitemap.xml and will include all of your site’s pages, but of course the plugin exposes options to configure this default functionality.

### Additional modifications

Additional modification steps are available in the [`gatsby-plugin-sitemap` documentation](/packages/gatsby-plugin-sitemap)

## More information

- Also check out a post on [gatsby-plugin-advanced-sitemap](/blog/2019-05-07-advanced-sitemap-plugin-for-seo/) from the Gatsby blog
