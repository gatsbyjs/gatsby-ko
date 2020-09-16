---
title: Browser Support
---

Gatsby supports [the same browsers as the current stable version of React.js](https://facebook.github.io/react/docs/react-dom.html#browser-support) which is currently IE9+ as well as the most recent versions of other popular browsers.

## Polyfills

Gatsby leverages Babel 7's ability to automatically add polyfills for your target browsers.

Newer browsers support more JavaScript APIs than older browsers. For older versions, Gatsby (via Babel) automatically adds the minimum "polyfills" necessary for your code to work in those browsers.

If you start using a newer JavaScript API like `[].includes` that isn't supported by some of your targeted browsers, you won't have to worry about it breaking the older browsers as Babel will automatically add the needed polyfill `core-js/modules/es7.array.includes`.

## Specify what browsers your project supports using "Browserslist"

You may customize your list of supported browser versions by declaring a [`"browserslist"`](https://github.com/ai/browserslist) key within your `package.json`. Changing these values will modify your JavaScript (via[`babel-preset-env`](https://github.com/babel/babel-preset-env#targetsbrowsers)) and your CSS (via [`autoprefixer`](https://github.com/postcss/autoprefixer)) output.

This article is a good introduction to the growing community of tools around Browserslist — https://css-tricks.com/browserlist-good-idea/

By default, Gatsby emulates the following config:

```json:title=package.json
{
  "browserslist": [">0.25%", "not dead"]
}
```

If you only support newer browsers, make sure to specify this in your `package.json`. This will often enable you to ship smaller JavaScript files.
