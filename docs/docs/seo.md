---
title: SEO with Gatsby
---

Gatsby can help your site rank and perform better in search engines. Using Gatsby makes your site fast and efficient for search engine crawlers, like Googlebot, to crawl your site and index your pages. Some advantages, like speed, come out of the box and others require configuration.

## Server rendering

Because Gatsby pages are server-side rendered, all the page content is available to Googlebot and other search engine crawlers.
You can see this by viewing the source for this page in your browser, Right-Click => View source. You'll see the fully rendered HTML document.

When you've installed [`gatsby-plugin-offline`](/packages/gatsby-plugin-offline/), you'll see a partial HTML document that does not contain the HTML you were hoping for. By using `gatsby-plugin-offline`, we can optimize bandwidth consumption and not let your users download too much data. Serving a partial HTML document is okay. Google and other search engines will still see the full HTML because `gatsby-plugin-offline` only starts working on the second-page load. A search engine always runs a page in Sandbox mode, which essentially is the first visit.

As a website owner, how do I test my site is serving its HTML correctly when `gatsby-plugin-offline` is being used? It would be best if you used your terminal of choice to visit your website. You can crawl your site by running the following command:

**on Windows (using powershell):**

```shell
Invoke-WebRequest https://www.gatsbyjs.org/docs/seo | Select -ExpandProperty Content
```

**on Mac OS/Linux:**

```shell
curl https://www.gatsbyjs.org/docs/seo
```

## Speed boost

Gatsby's many built-in performance optimizations, such as rendering to static files, progressive image loading, and the [PRPL pattern](/docs/prpl-pattern/)—all help your site be lightning-fast by default.

In July 2018, [Google announced a new ranking factor for site speed](https://webmasters.googleblog.com/2018/01/using-page-speed-in-mobile-search.html), calling the algorithm update the "Speed Update". Google will possibly rank pages higher in the search results for faster loading times, however, the intent of the search query is still very relevant and a slower page can rank higher if the content is more relevant.

## Page metadata

Adding metadata to pages, such as page title, meta description, alt text and structured data using JSON-LD, helps search engines understand your content and when to show your pages in search results.

A common way to add metadata to pages is to add [react-helmet](https://github.com/nfl/react-helmet) components (together with the [Gatsby React Helmet plugin](/packages/gatsby-plugin-react-helmet) for SSR support) to your page components. Here's a [guide on how to add an SEO component](https://www.gatsbyjs.org/docs/add-seo-component/) to your Gatsby app.

Some examples using react-helmet:

- [Official GatsbyJS.org site](https://github.com/gatsbyjs/gatsby/blob/87ad6e81b9bd78b25d089434600750f5903baaee/www/src/components/package-readme.js#L16-L25)
- [Official GatsbyJS default starter](https://github.com/gatsbyjs/gatsby/blob/776dc1d6fe8d5ce7b5ea6d884736bb3c76280975/starters/default/src/components/seo.js)
- [Gatsby Mail](https://github.com/DSchau/gatsby-mail/blob/89b467e5654619ffe3073133ef0ae48b4d7502e3/src/components/meta.js)
- [Jason Lengstorf’s personal blog](https://github.com/jlengstorf/gatsby-theme-jason-blog/blob/e6d25ca927afdc75c759e611d4ba6ba086452bb8/src/components/SEO/SEO.js)

## Generate rich snippets in search engines using structured data

Google uses structured data that it finds on the web to understand the content of the page, as well as to gather information about the web and the world in general.

For example, here is a structured data snippet in the [JSON-LD format](https://developers.google.com/search/docs/guides/intro-structured-data) (JavaScript Object Notation for Linked Data) that might appear on the contact page of a company called Spooky Technologies, describing their contact information:

```html
<script type="application/ld+json">
  {
    "@context": "https://schema.org",
    "@type": "Organization",
    "url": "http://www.spookytech.com",
    "name": "Spooky technologies",
    "contactPoint": {
      "@type": "ContactPoint",
      "telephone": "+5-601-785-8543",
      "contactType": "Customer Support"
    }
  }
</script>
```

When using structured data, you'll need to test during development and the [Structured Data Testing Tool](https://search.google.com/structured-data/testing-tool) from Google is one recommended method.

After deployment, their [Rich result status reports](https://support.google.com/webmasters/answer/7552505?hl=en) may help to monitor the health of your pages and mitigate any templating or serving issues.

## Additional resources

You might also be interested in [blog posts about SEO in Gatsby](/blog/tags/seo/).
